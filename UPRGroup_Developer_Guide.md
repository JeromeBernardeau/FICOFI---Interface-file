# UPR Group API (Read-Only) - Developer Guide

## Overview
The UPR Group API provides read-only access to Unpaid Reserve (UPR) Group master data. UPR Groups organize customer reservations and unpaid orders by category, allowing businesses to manage pre-orders and allocations effectively.

## API Endpoint Details
- **Endpoint**: `/uPRGroupsFic`
- **Entity Name**: uPRGroupFic
- **Entity Set Name**: uPRGroupsFic
- **Page ID**: 81008
- **Source Table**: TVT UPR Group
- **API Group**: ficofiweb
- **Publisher**: tvisiontech
- **Version**: v2.0, v1.0

## Permissions
- **Insert**: Not Allowed
- **Modify**: Not Allowed
- **Delete**: Not Allowed
- **Read**: Allowed

## Base URL
```
https://api.businesscentral.dynamics.com/v2.0/{environment}/api/tvisiontech/ficofiweb/v2.0/uPRGroupsFic
```

## Field Reference

| Field Name | Type | Description |
|------------|------|-------------|
| `code` | String | UPR Group code (OData key) |
| `name` | String | UPR Group name/description |
| `noCustomers` | Integer | Number of customers linked to this group |
| `noItemUPRLines` | Integer | Number of item reservation lines in this group |
| `tvtfiCategory` | String | UPR Group category classification |
| `tvtfiSalespersonCode` | String | Assigned salesperson code |
| `systemCreatedAt` | DateTime | Creation timestamp |
| `systemCreatedBy` | GUID | Created by user ID |
| `systemId` | GUID | System identifier |
| `systemModifiedAt` | DateTime | Last modification timestamp |
| `systemModifiedBy` | GUID | Modified by user ID |

## Common Use Cases

### 1. Get All UPR Groups
```http
GET /uPRGroupsFic
```

### 2. Get Specific UPR Group by Code
```http
GET /uPRGroupsFic?$filter=code eq 'PREMIUM'
```

### 3. Get UPR Groups by Salesperson
```http
GET /uPRGroupsFic?$filter=tvtfiSalespersonCode eq 'JD'
```

### 4. Get UPR Groups by Category
```http
GET /uPRGroupsFic?$filter=tvtfiCategory eq 'EN-PRIMEUR'
```

### 5. Get Groups with Customers
```http
GET /uPRGroupsFic?$filter=noCustomers gt 0&$orderby=noCustomers desc
```

### 6. Get Groups with Active Reservations
```http
GET /uPRGroupsFic?$filter=noItemUPRLines gt 0
```

## Code Examples

### JavaScript
```javascript
class UPRGroupAPI {
  constructor(baseUrl, accessToken) {
    this.baseUrl = baseUrl;
    this.headers = {
      'Authorization': `Bearer ${accessToken}`,
      'Content-Type': 'application/json'
    };
  }

  async getUPRGroup(code) {
    const response = await fetch(
      `${this.baseUrl}/uPRGroupsFic?$filter=code eq '${code}'`,
      { headers: this.headers }
    );
    const data = await response.json();
    return data.value[0];
  }

  async getUPRGroupsBySalesperson(salespersonCode) {
    const response = await fetch(
      `${this.baseUrl}/uPRGroupsFic?$filter=tvtfiSalespersonCode eq '${salespersonCode}'`,
      { headers: this.headers }
    );
    const data = await response.json();
    return data.value;
  }

  async getActiveGroups() {
    const response = await fetch(
      `${this.baseUrl}/uPRGroupsFic?$filter=noItemUPRLines gt 0&$orderby=noItemUPRLines desc`,
      { headers: this.headers }
    );
    const data = await response.json();
    return data.value;
  }

  async getGroupsByCategory(category) {
    const response = await fetch(
      `${this.baseUrl}/uPRGroupsFic?$filter=tvtfiCategory eq '${category}'`,
      { headers: this.headers }
    );
    const data = await response.json();
    return data.value;
  }

  async getGroupStatistics(code) {
    const group = await this.getUPRGroup(code);
    return {
      code: group.code,
      name: group.name,
      customers: group.noCustomers,
      reservations: group.noItemUPRLines,
      salesperson: group.tvtfiSalespersonCode,
      category: group.tvtfiCategory
    };
  }
}

// Usage
const api = new UPRGroupAPI(baseUrl, accessToken);

// Get group details
const group = await api.getUPRGroup('PREMIUM');
console.log(`${group.name}: ${group.noCustomers} customers, ${group.noItemUPRLines} reservations`);

// Get all active groups
const activeGroups = await api.getActiveGroups();
activeGroups.forEach(g => {
  console.log(`${g.code}: ${g.noItemUPRLines} active reservations`);
});
```

### Python
```python
class UPRGroupAPI:
    def __init__(self, base_url: str, access_token: str):
        self.base_url = base_url
        self.headers = {
            'Authorization': f'Bearer {access_token}',
            'Content-Type': 'application/json'
        }

    def get_upr_group(self, code: str) -> dict:
        response = requests.get(
            f'{self.base_url}/uPRGroupsFic',
            headers=self.headers,
            params={'$filter': f"code eq '{code}'"}
        )
        response.raise_for_status()
        data = response.json()['value']
        return data[0] if data else None

    def get_groups_by_salesperson(self, salesperson_code: str) -> list:
        response = requests.get(
            f'{self.base_url}/uPRGroupsFic',
            headers=self.headers,
            params={'$filter': f"tvtfiSalespersonCode eq '{salesperson_code}'"}
        )
        response.raise_for_status()
        return response.json()['value']

    def get_active_groups(self) -> list:
        response = requests.get(
            f'{self.base_url}/uPRGroupsFic',
            headers=self.headers,
            params={
                '$filter': 'noItemUPRLines gt 0',
                '$orderby': 'noItemUPRLines desc'
            }
        )
        response.raise_for_status()
        return response.json()['value']

    def get_groups_by_category(self, category: str) -> list:
        response = requests.get(
            f'{self.base_url}/uPRGroupsFic',
            headers=self.headers,
            params={'$filter': f"tvtfiCategory eq '{category}'"}
        )
        response.raise_for_status()
        return response.json()['value']

# Usage
api = UPRGroupAPI(base_url, access_token)
group = api.get_upr_group('PREMIUM')
print(f"Group: {group['name']}, Customers: {group['noCustomers']}")

# Get active groups
active = api.get_active_groups()
for g in active:
    print(f"{g['code']}: {g['noItemUPRLines']} reservations")
```

### C#
```csharp
public class UPRGroupApiClient
{
    private readonly HttpClient _httpClient;
    private readonly string _baseUrl;

    public UPRGroupApiClient(string baseUrl, string accessToken)
    {
        _baseUrl = baseUrl;
        _httpClient = new HttpClient();
        _httpClient.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", accessToken);
    }

    public async Task<UPRGroup> GetUPRGroupAsync(string code)
    {
        var filter = $"code eq '{code}'";
        var response = await _httpClient.GetAsync(
            $"{_baseUrl}/uPRGroupsFic?$filter={filter}");
        response.EnsureSuccessStatusCode();

        var json = await response.Content.ReadAsStringAsync();
        var result = JsonSerializer.Deserialize<ODataResponse<UPRGroup>>(json);
        return result.Value.FirstOrDefault();
    }

    public async Task<List<UPRGroup>> GetGroupsBySalespersonAsync(string salespersonCode)
    {
        var filter = $"tvtfiSalespersonCode eq '{salespersonCode}'";
        var response = await _httpClient.GetAsync(
            $"{_baseUrl}/uPRGroupsFic?$filter={filter}");
        response.EnsureSuccessStatusCode();

        var json = await response.Content.ReadAsStringAsync();
        var result = JsonSerializer.Deserialize<ODataResponse<UPRGroup>>(json);
        return result.Value;
    }

    public async Task<List<UPRGroup>> GetActiveGroupsAsync()
    {
        var filter = "noItemUPRLines gt 0";
        var orderby = "noItemUPRLines desc";
        var response = await _httpClient.GetAsync(
            $"{_baseUrl}/uPRGroupsFic?$filter={filter}&$orderby={orderby}");
        response.EnsureSuccessStatusCode();

        var json = await response.Content.ReadAsStringAsync();
        var result = JsonSerializer.Deserialize<ODataResponse<UPRGroup>>(json);
        return result.Value;
    }

    public async Task<List<UPRGroup>> GetGroupsByCategoryAsync(string category)
    {
        var filter = $"tvtfiCategory eq '{category}'";
        var response = await _httpClient.GetAsync(
            $"{_baseUrl}/uPRGroupsFic?$filter={filter}");
        response.EnsureSuccessStatusCode();

        var json = await response.Content.ReadAsStringAsync();
        var result = JsonSerializer.Deserialize<ODataResponse<UPRGroup>>(json);
        return result.Value;
    }
}

public class UPRGroup
{
    public string Code { get; set; }
    public string Name { get; set; }
    public int NoCustomers { get; set; }
    public int NoItemUPRLines { get; set; }
    public string TvtfiCategory { get; set; }
    public string TvtfiSalespersonCode { get; set; }
    public Guid SystemId { get; set; }
    public DateTime SystemCreatedAt { get; set; }
    public DateTime SystemModifiedAt { get; set; }
}
```

## Business Logic Notes

- **Read-Only**: This API is for reading data only; UPR Groups must be managed through BC UI
- **Counters**: `noCustomers` and `noItemUPRLines` are calculated fields showing current relationships
- **Categories**: Used to classify different types of reservations (e.g., En Primeur, Pre-Orders)
- **Salesperson Assignment**: Each group can be assigned to a salesperson for management
- **OData Key**: Uses `code` as the primary key (not `systemId`)

## Filtering Tips

```http
# Groups with activity
$filter=noItemUPRLines gt 0

# Groups by salesperson
$filter=tvtfiSalespersonCode eq 'JD'

# Groups by category
$filter=tvtfiCategory eq 'EN-PRIMEUR'

# Groups with customers
$filter=noCustomers gt 0

# Recently modified
$filter=systemModifiedAt gt 2025-01-01T00:00:00Z

# Combine filters
$filter=tvtfiSalespersonCode eq 'JD' and noItemUPRLines gt 0
```

## Error Handling

Common errors:
- **401 Unauthorized**: Invalid access token
- **404 Not Found**: UPR Group doesn't exist
- **400 Bad Request**: Invalid filter syntax

## Performance Tips

1. **Use $select**: Only retrieve needed fields
2. **Filter by Code**: Code is the primary key and most efficient filter
3. **Cache Data**: UPR Group master data changes infrequently
4. **Order Results**: Use `$orderby` for sorted results
5. **Pagination**: Use `$top` and `$skip` for large lists

## Related APIs
- **UPRInformation**: For detailed reservation/pre-order entries within groups
- **Customer**: For customer master data linked to groups
- **Item**: For item information on reservations

## Integration Patterns

### Group Summary Dashboard Pattern
```javascript
async function getGroupSummary() {
  const groups = await uprGroupAPI.getActiveGroups();

  return groups.map(group => ({
    code: group.code,
    name: group.name,
    stats: {
      customers: group.noCustomers,
      reservations: group.noItemUPRLines
    },
    management: {
      salesperson: group.tvtfiSalespersonCode,
      category: group.tvtfiCategory
    }
  }));
}
```

### Salesperson Portfolio Pattern
```javascript
async function getSalespersonPortfolio(salespersonCode) {
  const groups = await uprGroupAPI.getUPRGroupsBySalesperson(salespersonCode);

  const totalCustomers = groups.reduce((sum, g) => sum + g.noCustomers, 0);
  const totalReservations = groups.reduce((sum, g) => sum + g.noItemUPRLines, 0);

  return {
    salesperson: salespersonCode,
    groupCount: groups.length,
    totalCustomers,
    totalReservations,
    groups: groups.map(g => ({
      code: g.code,
      name: g.name,
      category: g.tvtfiCategory
    }))
  };
}
```

## Use Case: En Primeur Wine Management

UPR Groups are commonly used in the wine industry for En Primeur (wine futures) programs:
- **Category**: 'EN-PRIMEUR' groups manage pre-release wine allocations
- **Customers**: Collectors and wine merchants in the group
- **Reservations**: Item lines represent allocated cases/bottles
- **Salesperson**: Manages relationships and allocations

---

**Last Updated**: January 7, 2026
**API Version**: 2.0, 1.0
