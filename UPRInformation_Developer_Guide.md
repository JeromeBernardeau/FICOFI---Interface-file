# UPR Information API (Read-Only) - Developer Guide

## Overview
The UPR Information API provides read-only access to individual Unpaid Reserve (UPR) entries. These represent customer pre-orders, allocations, or reservations for specific items within UPR Groups. The API tracks quantities through various stages from reservation to fulfillment.

## API Endpoint Details
- **Endpoint**: `/uPReservesFic`
- **Entity Name**: uPReserveFic
- **Entity Set Name**: uPReservesFic
- **Page ID**: 81009
- **Source Table**: TVT UPR Information
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
https://api.businesscentral.dynamics.com/v2.0/{environment}/api/tvisiontech/ficofiweb/v2.0/uPReservesFic
```

## Field Reference

| Field Name | Type | Description |
|------------|------|-------------|
| `no` | String | UPR entry number (OData key) |
| `uprGroupCode` | String | Parent UPR Group code |
| `salespersonCode` | String | Salesperson code (calculated from group) |
| `itemNo` | String | Item number |
| `quantityBase` | Decimal | Total quantity in base unit |
| `noSeries` | String | Number series used |
| `qtyRemainingBase` | Decimal | Remaining quantity in base unit |
| `quantity` | Decimal | Total quantity in unit of measure |
| `quantityRemaining` | Decimal | Remaining quantity in unit of measure |
| `appliedQtyBase` | Decimal | Applied quantity in base unit |
| `appliedQuantity` | Decimal | Applied quantity in unit of measure |
| `postedQuantity` | Decimal | Posted/shipped quantity |
| `postedQtyBase` | Decimal | Posted quantity in base unit |
| `qtyOnSalesOrder` | Decimal | Quantity on sales orders |
| `qtyOnSalesOrderBase` | Decimal | Quantity on sales orders (base) |
| `qtyOnPostedShipmentBase` | Decimal | Quantity on posted shipments (base) |
| `createdDate` | Date | Date UPR entry was created |
| `reviewDate` | Date | Next review date |
| `assignedUserID` | String | User ID assigned to manage this entry |
| `note` | Text | Notes/comments |
| `itemDescription` | String | Item description (from Item) |
| `uprGroupName` | String | UPR Group name (from Group) |
| `tvtfiUPRCategory` | String | UPR category |
| `tvtfiSalesLineId` | GUID | Linked sales line system ID |
| `tvtfiSalesQuoteNo` | String | Linked sales quote number |
| `tvtfiSalesQuoteLineNo` | Integer | Linked sales quote line number |
| `tvtfiOfferID` | String | Offer/promotion ID |
| `systemCreatedAt` | DateTime | Creation timestamp |
| `systemCreatedBy` | GUID | Created by user ID |
| `systemId` | GUID | System identifier |
| `systemModifiedAt` | DateTime | Last modification timestamp |
| `systemModifiedBy` | GUID | Modified by user ID |

## Nested Resources

### UPR Group
Access the parent UPR Group via the `uPRGroupFic` navigation property.

**SubPage Link**: `Code` = `UPR Group Code`

See [UPRGroup_Developer_Guide.md](UPRGroup_Developer_Guide.md) for details.

## Common Use Cases

### 1. Get All UPR Entries
```http
GET /uPReservesFic
```

### 2. Get Specific UPR Entry
```http
GET /uPReservesFic?$filter=no eq 'UPR00001'
```

### 3. Get UPR Entries by Group
```http
GET /uPReservesFic?$filter=uprGroupCode eq 'PREMIUM'
```

### 4. Get UPR Entries by Item
```http
GET /uPReservesFic?$filter=itemNo eq 'WINE001'
```

### 5. Get Entries with Remaining Quantity
```http
GET /uPReservesFic?$filter=qtyRemainingBase gt 0&$orderby=qtyRemainingBase desc
```

### 6. Get Entries by Salesperson (via Group)
```http
GET /uPReservesFic?$filter=salespersonCode eq 'JD'
```

### 7. Get UPR with Group Details
```http
GET /uPReservesFic?$expand=uPRGroupFic
```

### 8. Get Entries Due for Review
```http
GET /uPReservesFic?$filter=reviewDate le 2026-01-31
```

## Code Examples

### JavaScript
```javascript
class UPRInformationAPI {
  constructor(baseUrl, accessToken) {
    this.baseUrl = baseUrl;
    this.headers = {
      'Authorization': `Bearer ${accessToken}`,
      'Content-Type': 'application/json'
    };
  }

  async getUPREntry(no) {
    const response = await fetch(
      `${this.baseUrl}/uPReservesFic?$filter=no eq '${no}'&$expand=uPRGroupFic`,
      { headers: this.headers }
    );
    const data = await response.json();
    return data.value[0];
  }

  async getEntriesByGroup(groupCode) {
    const response = await fetch(
      `${this.baseUrl}/uPReservesFic?$filter=uprGroupCode eq '${groupCode}'`,
      { headers: this.headers }
    );
    const data = await response.json();
    return data.value;
  }

  async getEntriesByItem(itemNo) {
    const response = await fetch(
      `${this.baseUrl}/uPReservesFic?$filter=itemNo eq '${itemNo}'`,
      { headers: this.headers }
    );
    const data = await response.json();
    return data.value;
  }

  async getActiveReservations() {
    const response = await fetch(
      `${this.baseUrl}/uPReservesFic?$filter=qtyRemainingBase gt 0`,
      { headers: this.headers }
    );
    const data = await response.json();
    return data.value;
  }

  async getEntriesForReview(beforeDate) {
    const response = await fetch(
      `${this.baseUrl}/uPReservesFic?$filter=reviewDate le ${beforeDate}`,
      { headers: this.headers }
    );
    const data = await response.json();
    return data.value;
  }

  async getReservationStatus(no) {
    const entry = await this.getUPREntry(no);
    return {
      no: entry.no,
      item: entry.itemDescription,
      group: entry.uprGroupName,
      total: entry.quantityBase,
      remaining: entry.qtyRemainingBase,
      applied: entry.appliedQtyBase,
      posted: entry.postedQtyBase,
      onOrder: entry.qtyOnSalesOrderBase,
      fulfillmentPct: ((entry.postedQtyBase / entry.quantityBase) * 100).toFixed(2)
    };
  }
}

// Usage
const api = new UPRInformationAPI(baseUrl, accessToken);

// Get reservation status
const status = await api.getReservationStatus('UPR00001');
console.log(`${status.item}: ${status.fulfillmentPct}% fulfilled`);

// Get all active reservations
const active = await api.getActiveReservations();
console.log(`${active.length} active reservations`);
```

### Python
```python
class UPRInformationAPI:
    def __init__(self, base_url: str, access_token: str):
        self.base_url = base_url
        self.headers = {
            'Authorization': f'Bearer {access_token}',
            'Content-Type': 'application/json'
        }

    def get_upr_entry(self, no: str) -> dict:
        response = requests.get(
            f'{self.base_url}/uPReservesFic',
            headers=self.headers,
            params={
                '$filter': f"no eq '{no}'",
                '$expand': 'uPRGroupFic'
            }
        )
        response.raise_for_status()
        data = response.json()['value']
        return data[0] if data else None

    def get_entries_by_group(self, group_code: str) -> list:
        response = requests.get(
            f'{self.base_url}/uPReservesFic',
            headers=self.headers,
            params={'$filter': f"uprGroupCode eq '{group_code}'"}
        )
        response.raise_for_status()
        return response.json()['value']

    def get_entries_by_item(self, item_no: str) -> list:
        response = requests.get(
            f'{self.base_url}/uPReservesFic',
            headers=self.headers,
            params={'$filter': f"itemNo eq '{item_no}'"}
        )
        response.raise_for_status()
        return response.json()['value']

    def get_active_reservations(self) -> list:
        response = requests.get(
            f'{self.base_url}/uPReservesFic',
            headers=self.headers,
            params={'$filter': 'qtyRemainingBase gt 0'}
        )
        response.raise_for_status()
        return response.json()['value']

    def get_reservation_status(self, no: str) -> dict:
        entry = self.get_upr_entry(no)
        if not entry:
            return None

        fulfillment_pct = (entry['postedQtyBase'] / entry['quantityBase'] * 100) if entry['quantityBase'] > 0 else 0

        return {
            'no': entry['no'],
            'item': entry['itemDescription'],
            'group': entry['uprGroupName'],
            'total': entry['quantityBase'],
            'remaining': entry['qtyRemainingBase'],
            'applied': entry['appliedQtyBase'],
            'posted': entry['postedQtyBase'],
            'on_order': entry['qtyOnSalesOrderBase'],
            'fulfillment_pct': round(fulfillment_pct, 2)
        }

# Usage
api = UPRInformationAPI(base_url, access_token)
status = api.get_reservation_status('UPR00001')
print(f"{status['item']}: {status['fulfillment_pct']}% fulfilled")
```

### C#
```csharp
public class UPRInformationApiClient
{
    private readonly HttpClient _httpClient;
    private readonly string _baseUrl;

    public UPRInformationApiClient(string baseUrl, string accessToken)
    {
        _baseUrl = baseUrl;
        _httpClient = new HttpClient();
        _httpClient.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", accessToken);
    }

    public async Task<UPRInformation> GetUPREntryAsync(string no)
    {
        var filter = $"no eq '{no}'";
        var expand = "uPRGroupFic";
        var response = await _httpClient.GetAsync(
            $"{_baseUrl}/uPReservesFic?$filter={filter}&$expand={expand}");
        response.EnsureSuccessStatusCode();

        var json = await response.Content.ReadAsStringAsync();
        var result = JsonSerializer.Deserialize<ODataResponse<UPRInformation>>(json);
        return result.Value.FirstOrDefault();
    }

    public async Task<List<UPRInformation>> GetEntriesByGroupAsync(string groupCode)
    {
        var filter = $"uprGroupCode eq '{groupCode}'";
        var response = await _httpClient.GetAsync(
            $"{_baseUrl}/uPReservesFic?$filter={filter}");
        response.EnsureSuccessStatusCode();

        var json = await response.Content.ReadAsStringAsync();
        var result = JsonSerializer.Deserialize<ODataResponse<UPRInformation>>(json);
        return result.Value;
    }

    public async Task<List<UPRInformation>> GetActiveReservationsAsync()
    {
        var filter = "qtyRemainingBase gt 0";
        var response = await _httpClient.GetAsync(
            $"{_baseUrl}/uPReservesFic?$filter={filter}");
        response.EnsureSuccessStatusCode();

        var json = await response.Content.ReadAsStringAsync();
        var result = JsonSerializer.Deserialize<ODataResponse<UPRInformation>>(json);
        return result.Value;
    }
}

public class UPRInformation
{
    public string No { get; set; }
    public string UprGroupCode { get; set; }
    public string SalespersonCode { get; set; }
    public string ItemNo { get; set; }
    public decimal QuantityBase { get; set; }
    public decimal QtyRemainingBase { get; set; }
    public decimal Quantity { get; set; }
    public decimal QuantityRemaining { get; set; }
    public decimal AppliedQtyBase { get; set; }
    public decimal AppliedQuantity { get; set; }
    public decimal PostedQuantity { get; set; }
    public decimal PostedQtyBase { get; set; }
    public decimal QtyOnSalesOrder { get; set; }
    public decimal QtyOnSalesOrderBase { get; set; }
    public decimal QtyOnPostedShipmentBase { get; set; }
    public DateTime CreatedDate { get; set; }
    public DateTime? ReviewDate { get; set; }
    public string AssignedUserID { get; set; }
    public string Note { get; set; }
    public string ItemDescription { get; set; }
    public string UprGroupName { get; set; }
    public string TvtfiUPRCategory { get; set; }
    public Guid? TvtfiSalesLineId { get; set; }
    public string TvtfiSalesQuoteNo { get; set; }
    public int? TvtfiSalesQuoteLineNo { get; set; }
    public string TvtfiOfferID { get; set; }
    public Guid SystemId { get; set; }
    public DateTime SystemCreatedAt { get; set; }
    public DateTime SystemModifiedAt { get; set; }
}
```

## Business Logic Notes

- **Read-Only**: This API is for reading data only; UPR entries must be managed through BC UI
- **Quantity Tracking**: Multiple quantity fields track the lifecycle:
  - `quantityBase`: Total reserved quantity
  - `qtyRemainingBase`: Not yet applied to orders
  - `appliedQtyBase`: Applied to sales orders
  - `postedQtyBase`: Shipped/fulfilled
  - `qtyOnSalesOrderBase`: Currently on open sales orders
- **Salesperson**: Calculated from parent UPR Group
- **Review Date**: Used for periodic review of outstanding reservations
- **Offer Tracking**: Can be linked to specific offers/promotions via `tvtfiOfferID`
- **Sales Integration**: Can be linked to sales quotes and lines for traceability

## Filtering Tips

```http
# Active reservations with remaining qty
$filter=qtyRemainingBase gt 0

# Entries by group
$filter=uprGroupCode eq 'PREMIUM'

# Entries by item
$filter=itemNo eq 'WINE001'

# Entries with linked sales quotes
$filter=tvtfiSalesQuoteNo ne null

# Entries due for review
$filter=reviewDate le 2026-01-31

# Entries by category
$filter=tvtfiUPRCategory eq 'EN-PRIMEUR'

# Assigned to specific user
$filter=assignedUserID eq 'ADMIN'

# Combine filters
$filter=uprGroupCode eq 'PREMIUM' and qtyRemainingBase gt 0
```

## Error Handling

Common errors:
- **401 Unauthorized**: Invalid access token
- **404 Not Found**: UPR entry doesn't exist
- **400 Bad Request**: Invalid filter syntax

## Performance Tips

1. **Use $select**: Only retrieve needed fields (many quantity fields available)
2. **Filter Efficiently**: Use `no` or `uprGroupCode` for indexed searches
3. **Expand Judiciously**: Only expand `uPRGroupFic` when group details needed
4. **Pagination**: Use `$top` and `$skip` for large result sets
5. **Cache Group Data**: If querying many entries from same group, cache group info

## Related APIs
- **UPRGroup**: For parent UPR Group information
- **Item**: For item master data
- **SalesLine**: For linked sales order lines

## Integration Patterns

### Reservation Fulfillment Dashboard
```javascript
async function getReservationDashboard(groupCode) {
  const entries = await uprInfoAPI.getEntriesByGroup(groupCode);

  const totalReserved = entries.reduce((sum, e) => sum + e.quantityBase, 0);
  const totalRemaining = entries.reduce((sum, e) => sum + e.qtyRemainingBase, 0);
  const totalFulfilled = entries.reduce((sum, e) => sum + e.postedQtyBase, 0);

  return {
    groupCode,
    entryCount: entries.length,
    totalReserved,
    totalRemaining,
    totalFulfilled,
    fulfillmentRate: (totalFulfilled / totalReserved * 100).toFixed(2),
    entries: entries.map(e => ({
      no: e.no,
      item: e.itemDescription,
      reserved: e.quantityBase,
      remaining: e.qtyRemainingBase,
      fulfilled: e.postedQtyBase
    }))
  };
}
```

### Item Availability Check
```javascript
async function getItemReservations(itemNo) {
  const entries = await uprInfoAPI.getEntriesByItem(itemNo);

  const totalReserved = entries.reduce((sum, e) => sum + e.qtyRemainingBase, 0);

  return {
    itemNo,
    totalReservedQty: totalReserved,
    reservationCount: entries.length,
    byGroup: entries.reduce((acc, e) => {
      if (!acc[e.uprGroupCode]) {
        acc[e.uprGroupCode] = { name: e.uprGroupName, qty: 0 };
      }
      acc[e.uprGroupCode].qty += e.qtyRemainingBase;
      return acc;
    }, {})
  };
}
```

---

**Last Updated**: January 7, 2026
**API Version**: 2.0, 1.0
