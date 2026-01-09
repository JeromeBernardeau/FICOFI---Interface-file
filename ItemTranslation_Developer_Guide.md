# Item Translation API - Developer Guide

## Overview
The Item Translation API provides multi-language support for item descriptions and marketing content. It allows storing and retrieving item information in different languages to support international markets.

## API Endpoint Details
- **Endpoint**: `/itemsTranslationFic`
- **Entity Name**: itemTranslationFic
- **Entity Set Name**: itemsTranslationFic
- **Page ID**: 81007
- **Source Table**: Item Translation
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
https://api.businesscentral.dynamics.com/v2.0/{environment}/api/tvisiontech/ficofiweb/v2.0/itemsTranslationFic
```

## Field Reference

| Field Name | Type | Description |
|------------|------|-------------|
| `systemId` | GUID | System identifier (OData key) |
| `itemNo` | String | Item number (foreign key to Item) |
| `languageCode` | String | Language code (e.g., 'ENU', 'FRA', 'DEU') |
| `description` | String | Item description in specified language |
| `tvtfiMarketingDescription` | String | Marketing description in specified language |
| `systemCreatedAt` | DateTime | Creation timestamp |
| `systemCreatedBy` | GUID | Created by user ID |
| `systemModifiedAt` | DateTime | Last modification timestamp |
| `systemModifiedBy` | GUID | Modified by user ID |

## Common Use Cases

### 1. Get All Translations for an Item
```http
GET /itemsTranslationFic?$filter=itemNo eq 'WINE001'
```

### 2. Get Specific Language Translation
```http
GET /itemsTranslationFic?$filter=itemNo eq 'WINE001' and languageCode eq 'FRA'
```

### 3. Get All French Translations
```http
GET /itemsTranslationFic?$filter=languageCode eq 'FRA'
```

### 4. Create Translation
```http
POST /itemsTranslationFic
Content-Type: application/json

{
  "itemNo": "WINE001",
  "languageCode": "FRA",
  "description": "Château Example 2020",
  "tvtfiMarketingDescription": "Vin rouge premium de Bordeaux"
}
```

### 5. Update Translation
```http
PATCH /itemsTranslationFic({systemId})
Content-Type: application/json

{
  "description": "Updated French Description",
  "tvtfiMarketingDescription": "Updated French Marketing Content"
}
```

### 6. Get Item with All Translations (via Item API)
```http
GET /itemsFic?$filter=no eq 'WINE001'&$expand=itemsTranslationFic
```

## Code Examples

### JavaScript
```javascript
class ItemTranslationAPI {
  constructor(baseUrl, accessToken) {
    this.baseUrl = baseUrl;
    this.headers = {
      'Authorization': `Bearer ${accessToken}`,
      'Content-Type': 'application/json'
    };
  }

  async getTranslations(itemNo) {
    const response = await fetch(
      `${this.baseUrl}/itemsTranslationFic?$filter=itemNo eq '${itemNo}'`,
      { headers: this.headers }
    );
    const data = await response.json();
    return data.value;
  }

  async getTranslation(itemNo, languageCode) {
    const response = await fetch(
      `${this.baseUrl}/itemsTranslationFic?$filter=itemNo eq '${itemNo}' and languageCode eq '${languageCode}'`,
      { headers: this.headers }
    );
    const data = await response.json();
    return data.value[0];
  }

  async createTranslation(translationData) {
    const response = await fetch(
      `${this.baseUrl}/itemsTranslationFic`,
      {
        method: 'POST',
        headers: this.headers,
        body: JSON.stringify(translationData)
      }
    );
    return await response.json();
  }

  async updateTranslation(systemId, updates) {
    const response = await fetch(
      `${this.baseUrl}/itemsTranslationFic(${systemId})`,
      {
        method: 'PATCH',
        headers: this.headers,
        body: JSON.stringify(updates)
      }
    );
    return await response.json();
  }

  async createMultipleTranslations(itemNo, translations) {
    const promises = translations.map(t =>
      this.createTranslation({
        itemNo,
        languageCode: t.languageCode,
        description: t.description,
        tvtfiMarketingDescription: t.marketingDescription
      })
    );
    return await Promise.all(promises);
  }
}

// Usage
const api = new ItemTranslationAPI(baseUrl, accessToken);

// Get all translations for an item
const translations = await api.getTranslations('WINE001');
translations.forEach(t => {
  console.log(`${t.languageCode}: ${t.description}`);
});

// Create multiple translations
await api.createMultipleTranslations('WINE001', [
  { languageCode: 'FRA', description: 'Vin rouge', marketingDescription: 'Excellent vin' },
  { languageCode: 'DEU', description: 'Rotwein', marketingDescription: 'Ausgezeichneter Wein' }
]);
```

### Python
```python
class ItemTranslationAPI:
    def __init__(self, base_url: str, access_token: str):
        self.base_url = base_url
        self.headers = {
            'Authorization': f'Bearer {access_token}',
            'Content-Type': 'application/json'
        }

    def get_translations(self, item_no: str) -> list:
        response = requests.get(
            f'{self.base_url}/itemsTranslationFic',
            headers=self.headers,
            params={'$filter': f"itemNo eq '{item_no}'"}
        )
        response.raise_for_status()
        return response.json()['value']

    def get_translation(self, item_no: str, language_code: str) -> dict:
        response = requests.get(
            f'{self.base_url}/itemsTranslationFic',
            headers=self.headers,
            params={'$filter': f"itemNo eq '{item_no}' and languageCode eq '{language_code}'"}
        )
        response.raise_for_status()
        data = response.json()['value']
        return data[0] if data else None

    def create_translation(self, translation_data: dict) -> dict:
        response = requests.post(
            f'{self.base_url}/itemsTranslationFic',
            headers=self.headers,
            json=translation_data
        )
        response.raise_for_status()
        return response.json()

    def update_translation(self, system_id: str, updates: dict) -> dict:
        response = requests.patch(
            f'{self.base_url}/itemsTranslationFic({system_id})',
            headers=self.headers,
            json=updates
        )
        response.raise_for_status()
        return response.json()

    def create_multiple_translations(self, item_no: str, translations: list) -> list:
        results = []
        for t in translations:
            data = {
                'itemNo': item_no,
                'languageCode': t['languageCode'],
                'description': t['description'],
                'tvtfiMarketingDescription': t.get('marketingDescription', '')
            }
            results.append(self.create_translation(data))
        return results

# Usage
api = ItemTranslationAPI(base_url, access_token)
translations = api.get_translations('WINE001')
for t in translations:
    print(f"{t['languageCode']}: {t['description']}")
```

### C#
```csharp
public class ItemTranslationApiClient
{
    private readonly HttpClient _httpClient;
    private readonly string _baseUrl;

    public ItemTranslationApiClient(string baseUrl, string accessToken)
    {
        _baseUrl = baseUrl;
        _httpClient = new HttpClient();
        _httpClient.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", accessToken);
    }

    public async Task<List<ItemTranslation>> GetTranslationsAsync(string itemNo)
    {
        var filter = $"itemNo eq '{itemNo}'";
        var response = await _httpClient.GetAsync(
            $"{_baseUrl}/itemsTranslationFic?$filter={filter}");
        response.EnsureSuccessStatusCode();

        var json = await response.Content.ReadAsStringAsync();
        var result = JsonSerializer.Deserialize<ODataResponse<ItemTranslation>>(json);
        return result.Value;
    }

    public async Task<ItemTranslation> GetTranslationAsync(string itemNo, string languageCode)
    {
        var filter = $"itemNo eq '{itemNo}' and languageCode eq '{languageCode}'";
        var response = await _httpClient.GetAsync(
            $"{_baseUrl}/itemsTranslationFic?$filter={filter}");
        response.EnsureSuccessStatusCode();

        var json = await response.Content.ReadAsStringAsync();
        var result = JsonSerializer.Deserialize<ODataResponse<ItemTranslation>>(json);
        return result.Value.FirstOrDefault();
    }

    public async Task<ItemTranslation> CreateTranslationAsync(ItemTranslation translation)
    {
        var json = JsonSerializer.Serialize(translation);
        var content = new StringContent(json, Encoding.UTF8, "application/json");
        var response = await _httpClient.PostAsync(
            $"{_baseUrl}/itemsTranslationFic", content);
        response.EnsureSuccessStatusCode();

        var responseJson = await response.Content.ReadAsStringAsync();
        return JsonSerializer.Deserialize<ItemTranslation>(responseJson);
    }
}

public class ItemTranslation
{
    public Guid SystemId { get; set; }
    public string ItemNo { get; set; }
    public string LanguageCode { get; set; }
    public string Description { get; set; }
    public string TvtfiMarketingDescription { get; set; }
    public DateTime SystemCreatedAt { get; set; }
    public DateTime SystemModifiedAt { get; set; }
}
```

## Business Logic Notes

- **Language Codes**: Use standard Business Central language codes (e.g., 'ENU' = English US, 'FRA' = French, 'DEU' = German)
- **Primary Key**: Combination of Item No., Variant Code (not exposed in API), and Language Code
- **Fallback Logic**: If no translation exists for a language, the base item description should be used
- **Marketing Content**: Separate marketing description field allows different promotional content per language
- **Parent Relationship**: Must reference existing item via `itemNo`

## Filtering Tips

```http
# All translations for specific language
$filter=languageCode eq 'FRA'

# Translations with marketing content
$filter=tvtfiMarketingDescription ne null and tvtfiMarketingDescription ne ''

# Recently modified translations
$filter=systemModifiedAt gt 2025-01-01T00:00:00Z

# Multiple items
$filter=itemNo in ('WINE001', 'WINE002', 'WINE003')

# Items with specific language
$filter=itemNo eq 'WINE001' and languageCode eq 'FRA'
```

## Error Handling

Common errors:
- **401 Unauthorized**: Invalid access token
- **400 Bad Request**: Invalid language code or missing required fields
- **404 Not Found**: Item or translation doesn't exist
- **409 Conflict**: Translation already exists for item/language combination

## Performance Tips

1. **Batch Operations**: When creating translations for multiple languages, consider the overhead
2. **Use Expand**: When fetching items, use `$expand=itemsTranslationFic` instead of separate calls
3. **Cache Translations**: Translation data is relatively static
4. **Filter by Language**: Always filter by language code when retrieving for display
5. **Select Fields**: Use `$select` to retrieve only needed fields

## Related APIs
- **Item**: Parent API for item master data
- **ItemUnitOfMeasure**: For units of measure per item

## Integration Patterns

### Multi-Language Support Pattern
```javascript
// Get item with all translations
async function getItemWithTranslations(itemNo, preferredLanguage) {
  const item = await itemAPI.getItem(itemNo);
  const translations = await translationAPI.getTranslations(itemNo);

  // Find preferred language or fall back to base description
  const translation = translations.find(t => t.languageCode === preferredLanguage);

  return {
    ...item,
    displayDescription: translation?.description || item.description,
    displayMarketing: translation?.tvtfiMarketingDescription || item.tvtMarketingDescription
  };
}
```

---

**Last Updated**: January 7, 2026
**API Version**: 2.0
