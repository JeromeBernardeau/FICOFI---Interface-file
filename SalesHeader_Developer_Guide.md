# Sales Header API - Developer Guide

## Overview
The Sales Header API provides full access to sales document headers including quotes, orders, invoices, and credit memos. It supports creating, updating, and reading sales documents with nested access to sales lines.

## API Endpoint Details
- **Endpoint**: `/salesHeadersFic`
- **Entity Name**: salesHeaderFic
- **Entity Set Name**: salesHeadersFic
- **Page ID**: 81000
- **Source Table**: Sales Header
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
https://api.businesscentral.dynamics.com/v2.0/{environment}/api/tvisiontech/ficofiweb/v2.0/salesHeadersFic
```

## Key Field Reference

| Field Name | Type | Description |
|------------|------|-------------|
| `id` | GUID | System identifier (OData key) |
| `documentType` | Enum | Document type (Quote, Order, Invoice, Credit Memo, etc.) |
| `no` | String | Document number |
| `sellToCustomerNo` | String | Sell-to customer number |
| `sellToCustomerName` | String | Sell-to customer name |
| `sellToAddress` | String | Sell-to address |
| `sellToCity` | String | Sell-to city |
| `sellToPostCode` | String | Sell-to post code |
| `sellToCountryRegionCode` | String | Sell-to country/region code |
| `billToCustomerNo` | String | Bill-to customer number |
| `billToName` | String | Bill-to name |
| `billToAddress` | String | Bill-to address |
| `shipToCode` | String | Ship-to code |
| `shipToName` | String | Ship-to name |
| `shipToAddress` | String | Ship-to address |
| `postingDate` | Date | Posting date |
| `documentDate` | Date | Document date |
| `orderDate` | Date | Order date |
| `dueDate` | Date | Due date |
| `requestedDeliveryDate` | Date | Requested delivery date |
| `shipmentDate` | Date | Shipment date |
| `externalDocumentNo` | String | Customer's reference/PO number |
| `salespersonCode` | String | Salesperson code |
| `currencyCode` | String | Currency code |
| `pricesIncludingVAT` | Boolean | Prices including VAT |
| `paymentTermsCode` | String | Payment terms code |
| `paymentMethodCode` | String | Payment method code |
| `shipmentMethodCode` | String | Shipment method code |
| `locationCode` | String | Location code |

For full field list, see the source API page.

## Nested Resources

### Sales Lines
Access sales lines for the document via the `salesLines` navigation property.

**SubPage Link**: `Document Type` = `Document Type`, `Document No.` = `No.`

See [SalesLine_Developer_Guide.md](SalesLine_Developer_Guide.md) for details.

## Common Use Cases

### 1. Get All Sales Orders
```http
GET /salesHeadersFic?$filter=documentType eq 'Order'
```

### 2. Get Sales Document with Lines
```http
GET /salesHeadersFic?$filter=no eq 'SO-00001'&$expand=salesLines
```

### 3. Get Orders for Customer
```http
GET /salesHeadersFic?$filter=documentType eq 'Order' and sellToCustomerNo eq 'C00001'
```

### 4. Get Orders by Salesperson
```http
GET /salesHeadersFic?$filter=documentType eq 'Order' and salespersonCode eq 'JD'
```

### 5. Create Sales Quote
```http
POST /salesHeadersFic
Content-Type: application/json

{
  "documentType": "Quote",
  "sellToCustomerNo": "C00001",
  "orderDate": "2026-01-15",
  "requestedDeliveryDate": "2026-02-15",
  "salespersonCode": "JD",
  "externalDocumentNo": "CUST-PO-12345"
}
```

### 6. Create Sales Order
```http
POST /salesHeadersFic
Content-Type: application/json

{
  "documentType": "Order",
  "sellToCustomerNo": "C00001",
  "orderDate": "2026-01-15",
  "shipmentDate": "2026-01-20",
  "salespersonCode": "JD",
  "paymentTermsCode": "30 DAYS",
  "shipmentMethodCode": "COURIER"
}
```

### 7. Update Sales Header
```http
PATCH /salesHeadersFic({id})
Content-Type: application/json

{
  "requestedDeliveryDate": "2026-02-20",
  "shipmentMethodCode": "EXPRESS"
}
```

## Code Examples

### JavaScript
```javascript
class SalesHeaderAPI {
  constructor(baseUrl, accessToken) {
    this.baseUrl = baseUrl;
    this.headers = {
      'Authorization': `Bearer ${accessToken}`,
      'Content-Type': 'application/json'
    };
  }

  async getSalesDocument(documentNo) {
    const response = await fetch(
      `${this.baseUrl}/salesHeadersFic?$filter=no eq '${documentNo}'&$expand=salesLines`,
      { headers: this.headers }
    );
    const data = await response.json();
    return data.value[0];
  }

  async getOrdersForCustomer(customerNo) {
    const response = await fetch(
      `${this.baseUrl}/salesHeadersFic?$filter=documentType eq 'Order' and sellToCustomerNo eq '${customerNo}'`,
      { headers: this.headers }
    );
    const data = await response.json();
    return data.value;
  }

  async createQuote(quoteData) {
    const response = await fetch(
      `${this.baseUrl}/salesHeadersFic`,
      {
        method: 'POST',
        headers: this.headers,
        body: JSON.stringify({ ...quoteData, documentType: 'Quote' })
      }
    );
    return await response.json();
  }

  async createOrder(orderData) {
    const response = await fetch(
      `${this.baseUrl}/salesHeadersFic`,
      {
        method: 'POST',
        headers: this.headers,
        body: JSON.stringify({ ...orderData, documentType: 'Order' })
      }
    );
    return await response.json();
  }

  async updateSalesHeader(id, updates) {
    const response = await fetch(
      `${this.baseUrl}/salesHeadersFic(${id})`,
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
const api = new SalesHeaderAPI(baseUrl, accessToken);

// Create sales order
const order = await api.createOrder({
  sellToCustomerNo: 'C00001',
  orderDate: '2026-01-15',
  requestedDeliveryDate: '2026-02-15',
  salespersonCode: 'JD',
  externalDocumentNo: 'CUST-PO-12345'
});

console.log(`Created order: ${order.no}`);
```

### Python
```python
class SalesHeaderAPI:
    def __init__(self, base_url: str, access_token: str):
        self.base_url = base_url
        self.headers = {
            'Authorization': f'Bearer {access_token}',
            'Content-Type': 'application/json'
        }

    def get_sales_document(self, document_no: str) -> dict:
        response = requests.get(
            f'{self.base_url}/salesHeadersFic',
            headers=self.headers,
            params={
                '$filter': f"no eq '{document_no}'",
                '$expand': 'salesLines'
            }
        )
        response.raise_for_status()
        data = response.json()['value']
        return data[0] if data else None

    def create_order(self, order_data: dict) -> dict:
        order_data['documentType'] = 'Order'
        response = requests.post(
            f'{self.base_url}/salesHeadersFic',
            headers=self.headers,
            json=order_data
        )
        response.raise_for_status()
        return response.json()

    def get_orders_for_customer(self, customer_no: str) -> list:
        response = requests.get(
            f'{self.base_url}/salesHeadersFic',
            headers=self.headers,
            params={'$filter': f"documentType eq 'Order' and sellToCustomerNo eq '{customer_no}'"}
        )
        response.raise_for_status()
        return response.json()['value']

# Usage
api = SalesHeaderAPI(base_url, access_token)
order = api.create_order({
    'sellToCustomerNo': 'C00001',
    'orderDate': '2026-01-15',
    'salespersonCode': 'JD'
})
```

### C#
```csharp
public class SalesHeaderApiClient
{
    private readonly HttpClient _httpClient;
    private readonly string _baseUrl;

    public SalesHeaderApiClient(string baseUrl, string accessToken)
    {
        _baseUrl = baseUrl;
        _httpClient = new HttpClient();
        _httpClient.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", accessToken);
    }

    public async Task<SalesHeader> GetSalesDocumentAsync(string documentNo)
    {
        var filter = $"no eq '{documentNo}'";
        var expand = "salesLines";
        var response = await _httpClient.GetAsync(
            $"{_baseUrl}/salesHeadersFic?$filter={filter}&$expand={expand}");
        response.EnsureSuccessStatusCode();

        var json = await response.Content.ReadAsStringAsync();
        var result = JsonSerializer.Deserialize<ODataResponse<SalesHeader>>(json);
        return result.Value.FirstOrDefault();
    }

    public async Task<SalesHeader> CreateOrderAsync(SalesHeader order)
    {
        order.DocumentType = "Order";
        var json = JsonSerializer.Serialize(order);
        var content = new StringContent(json, Encoding.UTF8, "application/json");
        var response = await _httpClient.PostAsync(
            $"{_baseUrl}/salesHeadersFic", content);
        response.EnsureSuccessStatusCode();

        var responseJson = await response.Content.ReadAsStringAsync();
        return JsonSerializer.Deserialize<SalesHeader>(responseJson);
    }
}

public class SalesHeader
{
    public Guid Id { get; set; }
    public string DocumentType { get; set; }
    public string No { get; set; }
    public string SellToCustomerNo { get; set; }
    public string SellToCustomerName { get; set; }
    public DateTime OrderDate { get; set; }
    public DateTime? RequestedDeliveryDate { get; set; }
    public string SalespersonCode { get; set; }
    public string ExternalDocumentNo { get; set; }
    public List<SalesLine> SalesLines { get; set; }
}
```

## Business Logic Notes

- **Document Types**: Quote, Order, Invoice, Credit Memo, Blanket Order, Return Order
- **Number Series**: Document number may be auto-generated based on document type
- **Address Hierarchy**: Sell-to, Bill-to, and Ship-to addresses can be different
- **Customer Validation**: Sell-to customer must exist and not be blocked
- **Date Logic**: Order Date, Document Date, Posting Date, Due Date have specific rules
- **Delayed Insert**: Headers created without immediate commit for line entry

## Filtering Tips

```http
# All orders
$filter=documentType eq 'Order'

# Orders for customer
$filter=documentType eq 'Order' and sellToCustomerNo eq 'C00001'

# Orders by salesperson
$filter=salespersonCode eq 'JD'

# Orders with specific delivery date range
$filter=requestedDeliveryDate ge 2026-01-01 and requestedDeliveryDate le 2026-01-31

# Recent orders
$filter=orderDate ge 2026-01-01

# Orders with external reference
$filter=externalDocumentNo ne null and externalDocumentNo ne ''
```

## Error Handling

Common errors:
- **401 Unauthorized**: Invalid access token
- **400 Bad Request**: Invalid customer, document type, or required fields missing
- **404 Not Found**: Document doesn't exist
- **409 Conflict**: Document number already exists

## Performance Tips

1. **Use $expand carefully**: Only expand salesLines when needed
2. **Filter by Document Type**: Always specify document type when possible
3. **Use $select**: Reduce payload by selecting only needed fields
4. **Pagination**: Use `$top` and `$skip` for large result sets

## Related APIs
- **SalesLine**: For sales document line details
- **Customer**: For customer information
- **Item**: For item details on lines

---

**Last Updated**: January 7, 2026
**API Version**: 2.0
