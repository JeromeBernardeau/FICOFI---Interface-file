# Sales Line API - Developer Guide

## Overview
The Sales Line API provides access to sales document line details including items, quantities, pricing, and discounts. It is typically used in conjunction with the Sales Header API to manage complete sales transactions.

## API Endpoint Details
- **Endpoint**: `/salesLinesFic`
- **Entity Name**: salesLineFic
- **Entity Set Name**: salesLinesFic
- **Page ID**: 81001
- **Source Table**: Sales Line
- **API Group**: ficofiweb
- **Publisher**: tvisiontech
- **Version**: v2.0

## Permissions
- **Insert**: Allowed (via DelayedInsert)
- **Modify**: Allowed
- **Delete**: Not Allowed
- **Read**: Allowed

## Base URL
```
https://api.businesscentral.dynamics.com/v2.0/{environment}/api/tvisiontech/ficofiweb/v2.0/salesLinesFic
```

## Key Field Reference

| Field Name | Type | Description |
|------------|------|-------------|
| `id` | GUID | System identifier (OData key) |
| `documentType` | Enum | Document type (Quote, Order, Invoice, etc.) |
| `documentNo` | String | Document number |
| `lineNo` | Integer | Line number |
| `sellToCustomerNo` | String | Sell-to customer number |
| `type` | Enum | Line type (Item, Resource, G/L Account, etc.) |
| `no` | String | Item/Resource/Account number |
| `locationCode` | String | Location code |
| `shipmentDate` | Date | Planned shipment date |
| `description` | String | Line description |
| `description2` | String | Additional description |
| `unitOfMeasure` | String | Unit of measure code |
| `quantity` | Decimal | Quantity |
| `outstandingQuantity` | Decimal | Outstanding quantity |
| `qtyToInvoice` | Decimal | Quantity to invoice |
| `qtyToShip` | Decimal | Quantity to ship |
| `unitPrice` | Decimal | Unit price |
| `unitCostLCY` | Decimal | Unit cost in local currency |
| `vatPercent` | Decimal | VAT percentage |
| `lineDiscountPercent` | Decimal | Line discount percentage |
| `lineDiscountAmount` | Decimal | Line discount amount |
| `amount` | Decimal | Line amount |
| `amountIncludingVAT` | Decimal | Line amount including VAT |

For full field list, see the source API page.

## Common Use Cases

### 1. Get All Lines for a Document
```http
GET /salesLinesFic?$filter=documentType eq 'Order' and documentNo eq 'SO-00001'
```

### 2. Get Lines for Customer
```http
GET /salesLinesFic?$filter=sellToCustomerNo eq 'C00001'
```

### 3. Get Lines for Specific Item
```http
GET /salesLinesFic?$filter=type eq 'Item' and no eq 'WINE001'
```

### 4. Create Sales Line
```http
POST /salesLinesFic
Content-Type: application/json

{
  "documentType": "Order",
  "documentNo": "SO-00001",
  "lineNo": 10000,
  "type": "Item",
  "no": "WINE001",
  "quantity": 12,
  "unitOfMeasure": "BTL",
  "unitPrice": 45.00,
  "lineDiscountPercent": 10
}
```

### 5. Update Sales Line
```http
PATCH /salesLinesFic({id})
Content-Type: application/json

{
  "quantity": 24,
  "shipmentDate": "2026-02-01"
}
```

### 6. Create Line with Description
```http
POST /salesLinesFic
Content-Type: application/json

{
  "documentType": "Order",
  "documentNo": "SO-00001",
  "lineNo": 20000,
  "type": "Item",
  "no": "WINE002",
  "description": "Château Premium 2020",
  "description2": "Bordeaux Red Wine",
  "quantity": 6,
  "unitOfMeasure": "BTL",
  "unitPrice": 85.00
}
```

## Code Examples

### JavaScript
```javascript
class SalesLineAPI {
  constructor(baseUrl, accessToken) {
    this.baseUrl = baseUrl;
    this.headers = {
      'Authorization': `Bearer ${accessToken}`,
      'Content-Type': 'application/json'
    };
  }

  async getDocumentLines(documentType, documentNo) {
    const response = await fetch(
      `${this.baseUrl}/salesLinesFic?$filter=documentType eq '${documentType}' and documentNo eq '${documentNo}'`,
      { headers: this.headers }
    );
    const data = await response.json();
    return data.value;
  }

  async createLine(lineData) {
    const response = await fetch(
      `${this.baseUrl}/salesLinesFic`,
      {
        method: 'POST',
        headers: this.headers,
        body: JSON.stringify(lineData)
      }
    );
    return await response.json();
  }

  async updateLine(id, updates) {
    const response = await fetch(
      `${this.baseUrl}/salesLinesFic(${id})`,
      {
        method: 'PATCH',
        headers: this.headers,
        body: JSON.stringify(updates)
      }
    );
    return await response.json();
  }

  async addItemToOrder(documentNo, itemNo, quantity, unitPrice) {
    // Get existing lines to determine next line number
    const existingLines = await this.getDocumentLines('Order', documentNo);
    const maxLineNo = existingLines.length > 0
      ? Math.max(...existingLines.map(l => l.lineNo))
      : 0;

    return await this.createLine({
      documentType: 'Order',
      documentNo: documentNo,
      lineNo: maxLineNo + 10000,
      type: 'Item',
      no: itemNo,
      quantity: quantity,
      unitPrice: unitPrice
    });
  }
}

// Usage
const api = new SalesLineAPI(baseUrl, accessToken);

// Add items to order
await api.addItemToOrder('SO-00001', 'WINE001', 12, 45.00);
await api.addItemToOrder('SO-00001', 'WINE002', 6, 85.00);

// Get all lines for order
const lines = await api.getDocumentLines('Order', 'SO-00001');
const total = lines.reduce((sum, line) => sum + line.amount, 0);
console.log(`Order total: ${total}`);
```

### Python
```python
class SalesLineAPI:
    def __init__(self, base_url: str, access_token: str):
        self.base_url = base_url
        self.headers = {
            'Authorization': f'Bearer {access_token}',
            'Content-Type': 'application/json'
        }

    def get_document_lines(self, document_type: str, document_no: str) -> list:
        response = requests.get(
            f'{self.base_url}/salesLinesFic',
            headers=self.headers,
            params={'$filter': f"documentType eq '{document_type}' and documentNo eq '{document_no}'"}
        )
        response.raise_for_status()
        return response.json()['value']

    def create_line(self, line_data: dict) -> dict:
        response = requests.post(
            f'{self.base_url}/salesLinesFic',
            headers=self.headers,
            json=line_data
        )
        response.raise_for_status()
        return response.json()

    def update_line(self, id: str, updates: dict) -> dict:
        response = requests.patch(
            f'{self.base_url}/salesLinesFic({id})',
            headers=self.headers,
            json=updates
        )
        response.raise_for_status()
        return response.json()

    def add_item_to_order(self, document_no: str, item_no: str,
                         quantity: float, unit_price: float) -> dict:
        # Get existing lines to determine next line number
        existing_lines = self.get_document_lines('Order', document_no)
        max_line_no = max([l['lineNo'] for l in existing_lines], default=0)

        return self.create_line({
            'documentType': 'Order',
            'documentNo': document_no,
            'lineNo': max_line_no + 10000,
            'type': 'Item',
            'no': item_no,
            'quantity': quantity,
            'unitPrice': unit_price
        })

# Usage
api = SalesLineAPI(base_url, access_token)
line = api.add_item_to_order('SO-00001', 'WINE001', 12, 45.00)
print(f"Created line {line['lineNo']}: {line['description']}")
```

### C#
```csharp
public class SalesLineApiClient
{
    private readonly HttpClient _httpClient;
    private readonly string _baseUrl;

    public SalesLineApiClient(string baseUrl, string accessToken)
    {
        _baseUrl = baseUrl;
        _httpClient = new HttpClient();
        _httpClient.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", accessToken);
    }

    public async Task<List<SalesLine>> GetDocumentLinesAsync(
        string documentType, string documentNo)
    {
        var filter = $"documentType eq '{documentType}' and documentNo eq '{documentNo}'";
        var response = await _httpClient.GetAsync(
            $"{_baseUrl}/salesLinesFic?$filter={filter}");
        response.EnsureSuccessStatusCode();

        var json = await response.Content.ReadAsStringAsync();
        var result = JsonSerializer.Deserialize<ODataResponse<SalesLine>>(json);
        return result.Value;
    }

    public async Task<SalesLine> CreateLineAsync(SalesLine line)
    {
        var json = JsonSerializer.Serialize(line);
        var content = new StringContent(json, Encoding.UTF8, "application/json");
        var response = await _httpClient.PostAsync(
            $"{_baseUrl}/salesLinesFic", content);
        response.EnsureSuccessStatusCode();

        var responseJson = await response.Content.ReadAsStringAsync();
        return JsonSerializer.Deserialize<SalesLine>(responseJson);
    }

    public async Task<SalesLine> AddItemToOrderAsync(
        string documentNo, string itemNo, decimal quantity, decimal unitPrice)
    {
        var existingLines = await GetDocumentLinesAsync("Order", documentNo);
        var maxLineNo = existingLines.Any()
            ? existingLines.Max(l => l.LineNo)
            : 0;

        var line = new SalesLine
        {
            DocumentType = "Order",
            DocumentNo = documentNo,
            LineNo = maxLineNo + 10000,
            Type = "Item",
            No = itemNo,
            Quantity = quantity,
            UnitPrice = unitPrice
        };

        return await CreateLineAsync(line);
    }
}

public class SalesLine
{
    public Guid Id { get; set; }
    public string DocumentType { get; set; }
    public string DocumentNo { get; set; }
    public int LineNo { get; set; }
    public string SellToCustomerNo { get; set; }
    public string Type { get; set; }
    public string No { get; set; }
    public string LocationCode { get; set; }
    public DateTime? ShipmentDate { get; set; }
    public string Description { get; set; }
    public string Description2 { get; set; }
    public string UnitOfMeasure { get; set; }
    public decimal Quantity { get; set; }
    public decimal OutstandingQuantity { get; set; }
    public decimal UnitPrice { get; set; }
    public decimal LineDiscountPercent { get; set; }
    public decimal Amount { get; set; }
    public decimal AmountIncludingVAT { get; set; }
}
```

## Business Logic Notes

- **Line Numbering**: Lines typically numbered in increments of 10000 (10000, 20000, 30000, etc.)
- **Line Types**: Item, Resource, G/L Account, Fixed Asset, Charge (Item)
- **Quantity Flow**: `Quantity` -> `OutstandingQuantity` -> `QtyToShip` -> shipped
- **Price Calculation**: Amount = (Quantity × Unit Price) - Line Discount Amount
- **VAT Calculation**: Amount Including VAT = Amount + (Amount × VAT%)
- **Parent Relationship**: Must reference existing Sales Header
- **Delayed Insert**: Lines created without immediate commit for batch entry

## Filtering Tips

```http
# All lines for a document
$filter=documentType eq 'Order' and documentNo eq 'SO-00001'

# Lines for an item
$filter=type eq 'Item' and no eq 'WINE001'

# Lines with outstanding quantity
$filter=outstandingQuantity gt 0

# Lines for customer
$filter=sellToCustomerNo eq 'C00001'

# Lines with discount
$filter=lineDiscountPercent gt 0

# Large orders
$filter=amount gt 1000
```

## Error Handling

Common errors:
- **401 Unauthorized**: Invalid access token
- **400 Bad Request**: Invalid item, document doesn't exist, or validation errors
- **404 Not Found**: Sales header doesn't exist
- **409 Conflict**: Line number already exists

## Performance Tips

1. **Batch Line Creation**: Create multiple lines efficiently by reusing connections
2. **Filter by Document**: Always filter by documentType and documentNo
3. **Use $select**: Select only needed fields to reduce payload
4. **Line Number Strategy**: Use consistent increments (10000) for maintainability

## Related APIs
- **SalesHeader**: Parent sales document
- **Item**: For item details and availability
- **ItemUnitOfMeasure**: For unit of measure conversions

## Integration Patterns

### Complete Order Creation
```javascript
async function createOrderWithLines(customer, items) {
  const headerAPI = new SalesHeaderAPI(baseUrl, accessToken);
  const lineAPI = new SalesLineAPI(baseUrl, accessToken);

  // Create header
  const header = await headerAPI.createOrder({
    sellToCustomerNo: customer.no,
    orderDate: new Date().toISOString().split('T')[0],
    salespersonCode: customer.salespersonCode
  });

  // Create lines
  let lineNo = 10000;
  for (const item of items) {
    await lineAPI.createLine({
      documentType: 'Order',
      documentNo: header.no,
      lineNo: lineNo,
      type: 'Item',
      no: item.itemNo,
      quantity: item.quantity,
      unitPrice: item.unitPrice
    });
    lineNo += 10000;
  }

  return header;
}

// Usage
const order = await createOrderWithLines(
  { no: 'C00001', salespersonCode: 'JD' },
  [
    { itemNo: 'WINE001', quantity: 12, unitPrice: 45.00 },
    { itemNo: 'WINE002', quantity: 6, unitPrice: 85.00 }
  ]
);
```

---

**Last Updated**: January 7, 2026
**API Version**: 2.0
