# Item API - Developer Guide

## Overview
The Item API provides access to item master data including descriptions, classifications, and related information. It supports creating and updating items with template application and includes nested access to item translations and units of measure.

## API Endpoint Details
- **Endpoint**: `/itemsFic`
- **Entity Name**: itemFic
- **Entity Set Name**: itemsFic
- **Page ID**: 81005
- **Source Table**: Item
- **API Group**: ficofiweb
- **Publisher**: tvisiontech
- **Version**: v2.0

## Permissions
- **Insert**: Allowed
- **Modify**: Allowed (applies default template on modify)
- **Delete**: Not Allowed
- **Read**: Allowed

## Base URL
```
https://api.businesscentral.dynamics.com/v2.0/{environment}/api/tvisiontech/ficofiweb/v2.0/itemsFic
```

## Field Reference

| Field Name | Type | Description |
|------------|------|-------------|
| `systemId` | GUID | System identifier (OData key) |
| `no` | String | Item number |
| `description` | String | Item description |
| `type` | Enum | Item type (Inventory, Service, Non-Inventory) |
| `tvtMarketingDescription` | String | Marketing description for promotional content |
| `baseUnitOfMeasure` | String | Base unit of measure code |
| `countryRegionOfOriginCode` | String | Country/region of origin |
| `tvtRegionCode` | String | Wine region code |
| `tvtSubRegionCode` | String | Wine subregion code |
| `tvtAppellationCode` | String | Wine appellation code |
| `tvtBrandCode` | String | Brand code |
| `tvtClassificationCode` | String | Classification code |
| `tvtStyleCode` | String | Wine style code |
| `tvtVintage` | Integer | Wine vintage year |
| `systemCreatedAt` | DateTime | Creation timestamp |
| `systemCreatedBy` | GUID | Created by user ID |
| `systemModifiedAt` | DateTime | Last modification timestamp |
| `systemModifiedBy` | GUID | Modified by user ID |

## Nested Resources

### Item Unit of Measure
Access units of measure for the item via the `itemsUnitOfMeasureFic` navigation property.

**SubPage Link**: `Item No.` = `No.`

See [ItemUnitOfMeasure_Developer_Guide.md](ItemUnitOfMeasure_Developer_Guide.md) for details.

### Item Translation
Access item translations for different languages via the `itemsTranslationFic` navigation property.

**SubPage Link**: `Item No.` = `No.`

See [ItemTranslation_Developer_Guide.md](ItemTranslation_Developer_Guide.md) for details.

## Common Use Cases

### 1. Get All Items
```http
GET /itemsFic
```

### 2. Search Items by Description
```http
GET /itemsFic?$filter=contains(description, 'Cabernet')
```

### 3. Get Items by Region
```http
GET /itemsFic?$filter=tvtRegionCode eq 'BORDEAUX'
```

### 4. Get Items by Vintage
```http
GET /itemsFic?$filter=tvtVintage eq 2020
```

### 5. Get Item with Translations and Units
```http
GET /itemsFic?$expand=itemsUnitOfMeasureFic,itemsTranslationFic
```

### 6. Create New Item
```http
POST /itemsFic
Content-Type: application/json

{
  "no": "WINE001",
  "description": "Château Example 2020",
  "type": "Inventory",
  "tvtMarketingDescription": "Premium red wine from Bordeaux",
  "baseUnitOfMeasure": "BTL",
  "countryRegionOfOriginCode": "FR",
  "tvtRegionCode": "BORDEAUX",
  "tvtVintage": 2020
}
```

### 7. Update Item
```http
PATCH /itemsFic({systemId})
Content-Type: application/json

{
  "description": "Updated Description",
  "tvtMarketingDescription": "Updated marketing content"
}
```

## Code Examples

### JavaScript
```javascript
class ItemAPI {
  constructor(baseUrl, accessToken) {
    this.baseUrl = baseUrl;
    this.headers = {
      'Authorization': `Bearer ${accessToken}`,
      'Content-Type': 'application/json'
    };
  }

  async getItem(itemNo) {
    const response = await fetch(
      `${this.baseUrl}/itemsFic?$filter=no eq '${itemNo}'&$expand=itemsUnitOfMeasureFic,itemsTranslationFic`,
      { headers: this.headers }
    );
    const data = await response.json();
    return data.value[0];
  }

  async createItem(itemData) {
    const response = await fetch(
      `${this.baseUrl}/itemsFic`,
      {
        method: 'POST',
        headers: this.headers,
        body: JSON.stringify(itemData)
      }
    );
    return await response.json();
  }

  async searchByVintage(vintage) {
    const response = await fetch(
      `${this.baseUrl}/itemsFic?$filter=tvtVintage eq ${vintage}`,
      { headers: this.headers }
    );
    const data = await response.json();
    return data.value;
  }

  async updateItem(systemId, updates) {
    const response = await fetch(
      `${this.baseUrl}/itemsFic(${systemId})`,
      {
        method: 'PATCH',
        headers: this.headers,
        body: JSON.stringify(updates)
      }
    );
    return await response.json();
  }
}

// Usage
const api = new ItemAPI(baseUrl, accessToken);
const item = await api.getItem('WINE001');
console.log(`${item.description} - Vintage: ${item.tvtVintage}`);
```

### Python
```python
class ItemAPI:
    def __init__(self, base_url: str, access_token: str):
        self.base_url = base_url
        self.headers = {
            'Authorization': f'Bearer {access_token}',
            'Content-Type': 'application/json'
        }

    def get_item(self, item_no: str) -> dict:
        response = requests.get(
            f'{self.base_url}/itemsFic',
            headers=self.headers,
            params={
                '$filter': f"no eq '{item_no}'",
                '$expand': 'itemsUnitOfMeasureFic,itemsTranslationFic'
            }
        )
        response.raise_for_status()
        data = response.json()['value']
        return data[0] if data else None

    def create_item(self, item_data: dict) -> dict:
        response = requests.post(
            f'{self.base_url}/itemsFic',
            headers=self.headers,
            json=item_data
        )
        response.raise_for_status()
        return response.json()

    def search_by_region(self, region_code: str) -> list:
        response = requests.get(
            f'{self.base_url}/itemsFic',
            headers=self.headers,
            params={'$filter': f"tvtRegionCode eq '{region_code}'"}
        )
        response.raise_for_status()
        return response.json()['value']

# Usage
api = ItemAPI(base_url, access_token)
item = api.get_item('WINE001')
print(f"Item: {item['description']}, Vintage: {item['tvtVintage']}")
```

### C#
```csharp
public class ItemApiClient
{
    private readonly HttpClient _httpClient;
    private readonly string _baseUrl;

    public ItemApiClient(string baseUrl, string accessToken)
    {
        _baseUrl = baseUrl;
        _httpClient = new HttpClient();
        _httpClient.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", accessToken);
    }

    public async Task<Item> GetItemAsync(string itemNo)
    {
        var filter = $"no eq '{itemNo}'";
        var expand = "itemsUnitOfMeasureFic,itemsTranslationFic";
        var response = await _httpClient.GetAsync(
            $"{_baseUrl}/itemsFic?$filter={filter}&$expand={expand}");
        response.EnsureSuccessStatusCode();

        var json = await response.Content.ReadAsStringAsync();
        var result = JsonSerializer.Deserialize<ODataResponse<Item>>(json);
        return result.Value.FirstOrDefault();
    }

    public async Task<Item> CreateItemAsync(Item item)
    {
        var json = JsonSerializer.Serialize(item);
        var content = new StringContent(json, Encoding.UTF8, "application/json");
        var response = await _httpClient.PostAsync($"{_baseUrl}/itemsFic", content);
        response.EnsureSuccessStatusCode();

        var responseJson = await response.Content.ReadAsStringAsync();
        return JsonSerializer.Deserialize<Item>(responseJson);
    }

    public async Task<List<Item>> SearchByVintageAsync(int vintage)
    {
        var filter = $"tvtVintage eq {vintage}";
        var response = await _httpClient.GetAsync($"{_baseUrl}/itemsFic?$filter={filter}");
        response.EnsureSuccessStatusCode();

        var json = await response.Content.ReadAsStringAsync();
        var result = JsonSerializer.Deserialize<ODataResponse<Item>>(json);
        return result.Value;
    }
}

public class Item
{
    public Guid SystemId { get; set; }
    public string No { get; set; }
    public string Description { get; set; }
    public string Type { get; set; }
    public string TvtMarketingDescription { get; set; }
    public string BaseUnitOfMeasure { get; set; }
    public string CountryRegionOfOriginCode { get; set; }
    public string TvtRegionCode { get; set; }
    public string TvtSubRegionCode { get; set; }
    public string TvtAppellationCode { get; set; }
    public string TvtBrandCode { get; set; }
    public string TvtClassificationCode { get; set; }
    public string TvtStyleCode { get; set; }
    public int? TvtVintage { get; set; }
    public List<ItemUnitOfMeasure> ItemsUnitOfMeasureFic { get; set; }
    public List<ItemTranslation> ItemsTranslationFic { get; set; }
}
```

## Business Logic Notes

- **Template Application**: When modifying items, if a "Default Item Template" is configured in FICOFI Setup, it will automatically be applied to set default values
- **Nested Resources**: Items can have multiple units of measure and translations
- **Wine-Specific Fields**: The TVT fields (Region, SubRegion, Appellation, Style, Vintage) are specific to wine/beverage industry
- **Marketing Description**: Separate field for promotional content distinct from standard description

## Filtering Tips

```http
# Items with vintage
$filter=tvtVintage ne null

# Items from specific country
$filter=countryRegionOfOriginCode eq 'FR'

# Items with marketing description
$filter=tvtMarketingDescription ne null and tvtMarketingDescription ne ''

# Items by type
$filter=type eq 'Inventory'

# Recently modified items
$filter=systemModifiedAt gt 2025-01-01T00:00:00Z

# Items by brand
$filter=tvtBrandCode eq 'CHATEAUX'
```

## Error Handling

Common errors:
- **401 Unauthorized**: Invalid access token
- **400 Bad Request**: Invalid field values or missing required fields
- **409 Conflict**: Item number already exists
- **404 Not Found**: Item doesn't exist (for PATCH operations)

## Performance Tips

1. **Use $select**: Only retrieve needed fields to reduce payload size
2. **Use $expand judiciously**: Only expand nested resources when needed
3. **Filter efficiently**: Use indexed fields (`no`, `systemId`)
4. **Cache data**: Item master data typically changes infrequently
5. **Pagination**: Use `$top` and `$skip` for large result sets

## Related APIs
- **ItemTranslation**: For multi-language item descriptions
- **ItemUnitOfMeasure**: For alternative units of measure
- **SalesLine**: For item usage in sales transactions

---

**Last Updated**: January 7, 2026
**API Version**: 2.0
