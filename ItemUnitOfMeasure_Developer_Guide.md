# Item Unit of Measure API - Developer Guide

## Overview
The Item Unit of Measure API provides access to alternative units of measure for items. This allows items to be sold, purchased, or stored in different quantities (e.g., bottles, cases, pallets) with automatic conversion.

## API Endpoint Details
- **Endpoint**: `/itemsUnitOfMeasureFic`
- **Entity Name**: itemUnitOfMeasureFic
- **Entity Set Name**: itemsUnitOfMeasureFic
- **Page ID**: 81006
- **Source Table**: Item Unit of Measure
- **API Group**: ficofiweb
- **Publisher**: tvisiontech
- **Version**: v2.0

## Permissions
- **Insert**: Allowed
- **Modify**: Allowed
- **Delete**: Not Allowed
- **Read**: Allowed

## Base URL
```
https://api.businesscentral.dynamics.com/v2.0/{environment}/api/tvisiontech/ficofiweb/v2.0/itemsUnitOfMeasureFic
```

## Field Reference

| Field Name | Type | Description |
|------------|------|-------------|
| `systemId` | GUID | System identifier (OData key) |
| `itemNo` | String | Item number (foreign key to Item) |
| `code` | String | Unit of measure code (e.g., 'BTL', 'CS', 'PLT') |
| `systemCreatedAt` | DateTime | Creation timestamp |
| `systemCreatedBy` | GUID | Created by user ID |
| `systemModifiedAt` | DateTime | Last modification timestamp |
| `systemModifiedBy` | GUID | Modified by user ID |

## Common Use Cases

### 1. Get All Units of Measure for an Item
```http
GET /itemsUnitOfMeasureFic?$filter=itemNo eq 'WINE001'
```

### 2. Get Specific Unit of Measure
```http
GET /itemsUnitOfMeasureFic?$filter=itemNo eq 'WINE001' and code eq 'CS'
```

### 3. Create Unit of Measure
```http
POST /itemsUnitOfMeasureFic
Content-Type: application/json

{
  "itemNo": "WINE001",
  "code": "CS"
}
```

### 4. Update Unit of Measure
```http
PATCH /itemsUnitOfMeasureFic({systemId})
Content-Type: application/json

{
  "code": "CASE"
}
```

### 5. Get Item with All Units of Measure (via Item API)
```http
GET /itemsFic?$filter=no eq 'WINE001'&$expand=itemsUnitOfMeasureFic
```

## Code Examples

### JavaScript
```javascript
class ItemUnitOfMeasureAPI {
  constructor(baseUrl, accessToken) {
    this.baseUrl = baseUrl;
    this.headers = {
      'Authorization': `Bearer ${accessToken}`,
      'Content-Type': 'application/json'
    };
  }

  async getUnitsOfMeasure(itemNo) {
    const response = await fetch(
      `${this.baseUrl}/itemsUnitOfMeasureFic?$filter=itemNo eq '${itemNo}'`,
      { headers: this.headers }
    );
    const data = await response.json();
    return data.value;
  }

  async getUnitOfMeasure(itemNo, code) {
    const response = await fetch(
      `${this.baseUrl}/itemsUnitOfMeasureFic?$filter=itemNo eq '${itemNo}' and code eq '${code}'`,
      { headers: this.headers }
    );
    const data = await response.json();
    return data.value[0];
  }

  async createUnitOfMeasure(unitData) {
    const response = await fetch(
      `${this.baseUrl}/itemsUnitOfMeasureFic`,
      {
        method: 'POST',
        headers: this.headers,
        body: JSON.stringify(unitData)
      }
    );
    return await response.json();
  }

  async createMultipleUnits(itemNo, unitCodes) {
    const promises = unitCodes.map(code =>
      this.createUnitOfMeasure({ itemNo, code })
    );
    return await Promise.all(promises);
  }

  async updateUnitOfMeasure(systemId, updates) {
    const response = await fetch(
      `${this.baseUrl}/itemsUnitOfMeasureFic(${systemId})`,
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
const api = new ItemUnitOfMeasureAPI(baseUrl, accessToken);

// Get all units for an item
const units = await api.getUnitsOfMeasure('WINE001');
units.forEach(u => {
  console.log(`Unit: ${u.code}`);
});

// Create multiple units
await api.createMultipleUnits('WINE001', ['BTL', 'CS', 'PLT']);
```

### Python
```python
class ItemUnitOfMeasureAPI:
    def __init__(self, base_url: str, access_token: str):
        self.base_url = base_url
        self.headers = {
            'Authorization': f'Bearer {access_token}',
            'Content-Type': 'application/json'
        }

    def get_units_of_measure(self, item_no: str) -> list:
        response = requests.get(
            f'{self.base_url}/itemsUnitOfMeasureFic',
            headers=self.headers,
            params={'$filter': f"itemNo eq '{item_no}'"}
        )
        response.raise_for_status()
        return response.json()['value']

    def get_unit_of_measure(self, item_no: str, code: str) -> dict:
        response = requests.get(
            f'{self.base_url}/itemsUnitOfMeasureFic',
            headers=self.headers,
            params={'$filter': f"itemNo eq '{item_no}' and code eq '{code}'"}
        )
        response.raise_for_status()
        data = response.json()['value']
        return data[0] if data else None

    def create_unit_of_measure(self, unit_data: dict) -> dict:
        response = requests.post(
            f'{self.base_url}/itemsUnitOfMeasureFic',
            headers=self.headers,
            json=unit_data
        )
        response.raise_for_status()
        return response.json()

    def create_multiple_units(self, item_no: str, unit_codes: list) -> list:
        results = []
        for code in unit_codes:
            data = {'itemNo': item_no, 'code': code}
            results.append(self.create_unit_of_measure(data))
        return results

# Usage
api = ItemUnitOfMeasureAPI(base_url, access_token)
units = api.get_units_of_measure('WINE001')
for u in units:
    print(f"Unit: {u['code']}")

# Create standard wine units
api.create_multiple_units('WINE001', ['BTL', 'CS', 'PLT'])
```

### C#
```csharp
public class ItemUnitOfMeasureApiClient
{
    private readonly HttpClient _httpClient;
    private readonly string _baseUrl;

    public ItemUnitOfMeasureApiClient(string baseUrl, string accessToken)
    {
        _baseUrl = baseUrl;
        _httpClient = new HttpClient();
        _httpClient.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", accessToken);
    }

    public async Task<List<ItemUnitOfMeasure>> GetUnitsOfMeasureAsync(string itemNo)
    {
        var filter = $"itemNo eq '{itemNo}'";
        var response = await _httpClient.GetAsync(
            $"{_baseUrl}/itemsUnitOfMeasureFic?$filter={filter}");
        response.EnsureSuccessStatusCode();

        var json = await response.Content.ReadAsStringAsync();
        var result = JsonSerializer.Deserialize<ODataResponse<ItemUnitOfMeasure>>(json);
        return result.Value;
    }

    public async Task<ItemUnitOfMeasure> GetUnitOfMeasureAsync(string itemNo, string code)
    {
        var filter = $"itemNo eq '{itemNo}' and code eq '{code}'";
        var response = await _httpClient.GetAsync(
            $"{_baseUrl}/itemsUnitOfMeasureFic?$filter={filter}");
        response.EnsureSuccessStatusCode();

        var json = await response.Content.ReadAsStringAsync();
        var result = JsonSerializer.Deserialize<ODataResponse<ItemUnitOfMeasure>>(json);
        return result.Value.FirstOrDefault();
    }

    public async Task<ItemUnitOfMeasure> CreateUnitOfMeasureAsync(ItemUnitOfMeasure unit)
    {
        var json = JsonSerializer.Serialize(unit);
        var content = new StringContent(json, Encoding.UTF8, "application/json");
        var response = await _httpClient.PostAsync(
            $"{_baseUrl}/itemsUnitOfMeasureFic", content);
        response.EnsureSuccessStatusCode();

        var responseJson = await response.Content.ReadAsStringAsync();
        return JsonSerializer.Deserialize<ItemUnitOfMeasure>(responseJson);
    }

    public async Task<List<ItemUnitOfMeasure>> CreateMultipleUnitsAsync(
        string itemNo, List<string> unitCodes)
    {
        var tasks = unitCodes.Select(code =>
            CreateUnitOfMeasureAsync(new ItemUnitOfMeasure
            {
                ItemNo = itemNo,
                Code = code
            })
        );
        return (await Task.WhenAll(tasks)).ToList();
    }
}

public class ItemUnitOfMeasure
{
    public Guid SystemId { get; set; }
    public string ItemNo { get; set; }
    public string Code { get; set; }
    public DateTime SystemCreatedAt { get; set; }
    public DateTime SystemModifiedAt { get; set; }
}
```

## Business Logic Notes

- **Base Unit**: One unit of measure must be designated as the base unit on the Item card
- **Qty. per Unit**: The full Item Unit of Measure table includes a "Qty. per Unit of Measure" field that defines conversion ratios (not exposed in this simplified API)
- **Common Units**:
  - BTL = Bottle
  - CS = Case (typically 6 or 12 bottles)
  - PLT = Pallet (multiple cases)
  - MAG = Magnum (1.5L bottle)
- **Primary Key**: Combination of Item No. and Code
- **Parent Relationship**: Must reference existing item via `itemNo`
- **Code Validation**: Unit of Measure code must exist in the Unit of Measure master table

## Filtering Tips

```http
# All units for an item
$filter=itemNo eq 'WINE001'

# Specific unit
$filter=itemNo eq 'WINE001' and code eq 'CS'

# Multiple items
$filter=itemNo in ('WINE001', 'WINE002')

# Recently created
$filter=systemCreatedAt gt 2025-01-01T00:00:00Z
```

## Error Handling

Common errors:
- **401 Unauthorized**: Invalid access token
- **400 Bad Request**: Invalid unit code or missing required fields
- **404 Not Found**: Item or unit of measure doesn't exist
- **409 Conflict**: Unit of measure already exists for this item

## Performance Tips

1. **Use Expand**: When fetching items, use `$expand=itemsUnitOfMeasureFic` to get units in one call
2. **Batch Creation**: Create multiple units in parallel for better performance
3. **Cache Units**: Unit of measure data is relatively static
4. **Filter by Item**: Always filter by item number when retrieving
5. **Minimal API**: This API exposes minimal fields; consider using standard BC APIs for full unit details

## Related APIs
- **Item**: Parent API for item master data
- **ItemTranslation**: For multi-language item descriptions

## Integration Patterns

### Setup New Item with Units Pattern
```javascript
async function setupNewItemWithUnits(itemData, unitCodes) {
  // Create the item
  const item = await itemAPI.createItem(itemData);

  // Create units of measure
  const units = await Promise.all(
    unitCodes.map(code =>
      unitOfMeasureAPI.createUnitOfMeasure({
        itemNo: item.no,
        code: code
      })
    )
  );

  return { item, units };
}

// Usage - setup wine item with standard units
const result = await setupNewItemWithUnits(
  {
    no: 'WINE001',
    description: 'Château Example 2020',
    baseUnitOfMeasure: 'BTL'
  },
  ['BTL', 'CS', 'PLT']
);
```

## Limitations

- This API provides basic access to item units of measure
- Conversion quantities and other detailed unit properties are not exposed
- For full unit of measure configuration, use standard Business Central APIs or UI
- Delete operation not supported; use standard BC APIs if deletion is required

---

**Last Updated**: January 7, 2026
**API Version**: 2.0
