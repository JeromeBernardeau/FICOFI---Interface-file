# Payment Address API - Developer Guide

## Overview
The Payment Address API provides access to alternative payment addresses for customer and vendor accounts. This is useful for businesses that need to send invoices or payments to addresses different from the main business address, with support for both simplified and traditional Chinese translations.

## API Endpoint Details
- **Endpoint**: `/paymentAddressesFic`
- **Entity Name**: paymentAddressFic
- **Entity Set Name**: paymentAddressesFic
- **Page ID**: 81002
- **Source Table**: Payment Address
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
https://api.businesscentral.dynamics.com/v2.0/{environment}/api/tvisiontech/ficofiweb/v2.0/paymentAddressesFic
```

## Field Reference

| Field Name | Type | Description |
|------------|------|-------------|
| `id` | GUID | System identifier (OData key) |
| `accountType` | Enum | Account type (Customer, Vendor) |
| `accountNo` | String | Account number |
| `code` | String | Payment address code |
| `name` | String | Name |
| `name2` | String | Name 2 |
| `address` | String | Address line 1 |
| `address2` | String | Address line 2 |
| `city` | String | City |
| `contact` | String | Contact person |
| `tvtfiChineseNameSC` | String | Chinese name (Simplified Chinese) |
| `tvtfiAddressSC` | String | Address (Simplified Chinese) |
| `tvtfiAddress2SC` | String | Address 2 (Simplified Chinese) |
| `tvtfiCitySC` | String | City (Simplified Chinese) |
| `tvtfiCountryRegionCodeSC` | String | Country/Region Code (Simplified Chinese) |
| `tvtfiCountySC` | String | County (Simplified Chinese) |
| `tvtfiPostCodeSC` | String | Post Code (Simplified Chinese) |
| `tvtfiChineseNameTC` | String | Chinese name (Traditional Chinese) |
| `tvtfiAddressTC` | String | Address (Traditional Chinese) |
| `tvtfiAddress2TC` | String | Address 2 (Traditional Chinese) |
| `tvtfiCityTC` | String | City (Traditional Chinese) |
| `tvtfiCountryRegionCodeTC` | String | Country/Region Code (Traditional Chinese) |
| `tvtfiCountyTC` | String | County (Traditional Chinese) |
| `tvtfiPostCodeTC` | String | Post Code (Traditional Chinese) |
| `systemCreatedAt` | DateTime | Creation timestamp (read-only) |
| `systemModifiedAt` | DateTime | Last modification timestamp (read-only) |

## Common Use Cases

### 1. Get All Payment Addresses
```http
GET /paymentAddressesFic
```

### 2. Get Payment Addresses for Customer
```http
GET /paymentAddressesFic?$filter=accountType eq 'Customer' and accountNo eq 'C00001'
```

### 3. Get Specific Payment Address
```http
GET /paymentAddressesFic?$filter=accountNo eq 'C00001' and code eq 'BILLING'
```

### 4. Create Payment Address
```http
POST /paymentAddressesFic
Content-Type: application/json

{
  "accountType": "Customer",
  "accountNo": "C00001",
  "code": "BILLING",
  "name": "Accounts Payable Department",
  "address": "PO Box 123",
  "city": "London",
  "contact": "Finance Team"
}
```

### 5. Create Payment Address with Chinese Translation
```http
POST /paymentAddressesFic
Content-Type: application/json

{
  "accountType": "Customer",
  "accountNo": "C00001",
  "code": "SHANGHAI",
  "name": "Shanghai Office",
  "address": "123 Nanjing Road",
  "city": "Shanghai",
  "tvtfiChineseNameSC": "上海办公室",
  "tvtfiAddressSC": "南京路123号",
  "tvtfiCitySC": "上海市"
}
```

### 6. Update Payment Address
```http
PATCH /paymentAddressesFic({id})
Content-Type: application/json

{
  "name": "Updated Billing Department",
  "contact": "New Contact Name"
}
```

## Code Examples

### JavaScript
```javascript
class PaymentAddressAPI {
  constructor(baseUrl, accessToken) {
    this.baseUrl = baseUrl;
    this.headers = {
      'Authorization': `Bearer ${accessToken}`,
      'Content-Type': 'application/json'
    };
  }

  async getPaymentAddresses(accountNo, accountType = 'Customer') {
    const response = await fetch(
      `${this.baseUrl}/paymentAddressesFic?$filter=accountType eq '${accountType}' and accountNo eq '${accountNo}'`,
      { headers: this.headers }
    );
    const data = await response.json();
    return data.value;
  }

  async getPaymentAddress(accountNo, code, accountType = 'Customer') {
    const response = await fetch(
      `${this.baseUrl}/paymentAddressesFic?$filter=accountType eq '${accountType}' and accountNo eq '${accountNo}' and code eq '${code}'`,
      { headers: this.headers }
    );
    const data = await response.json();
    return data.value[0];
  }

  async createPaymentAddress(addressData) {
    const response = await fetch(
      `${this.baseUrl}/paymentAddressesFic`,
      {
        method: 'POST',
        headers: this.headers,
        body: JSON.stringify(addressData)
      }
    );
    return await response.json();
  }

  async updatePaymentAddress(id, updates) {
    const response = await fetch(
      `${this.baseUrl}/paymentAddressesFic(${id})`,
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
const api = new PaymentAddressAPI(baseUrl, accessToken);

// Create billing address
const billingAddr = await api.createPaymentAddress({
  accountType: 'Customer',
  accountNo: 'C00001',
  code: 'BILLING',
  name: 'Accounts Payable',
  address: 'PO Box 456',
  city: 'London'
});

// Create address with Chinese translation
const chinaAddr = await api.createPaymentAddress({
  accountType: 'Customer',
  accountNo: 'C00001',
  code: 'BEIJING',
  name: 'Beijing Office',
  address: 'Chaoyang District',
  city: 'Beijing',
  tvtfiChineseNameSC: '北京办公室',
  tvtfiAddressSC: '朝阳区',
  tvtfiCitySC: '北京市'
});
```

### Python
```python
class PaymentAddressAPI:
    def __init__(self, base_url: str, access_token: str):
        self.base_url = base_url
        self.headers = {
            'Authorization': f'Bearer {access_token}',
            'Content-Type': 'application/json'
        }

    def get_payment_addresses(self, account_no: str, account_type: str = 'Customer') -> list:
        response = requests.get(
            f'{self.base_url}/paymentAddressesFic',
            headers=self.headers,
            params={'$filter': f"accountType eq '{account_type}' and accountNo eq '{account_no}'"}
        )
        response.raise_for_status()
        return response.json()['value']

    def create_payment_address(self, address_data: dict) -> dict:
        response = requests.post(
            f'{self.base_url}/paymentAddressesFic',
            headers=self.headers,
            json=address_data
        )
        response.raise_for_status()
        return response.json()

    def update_payment_address(self, id: str, updates: dict) -> dict:
        response = requests.patch(
            f'{self.base_url}/paymentAddressesFic({id})',
            headers=self.headers,
            json=updates
        )
        response.raise_for_status()
        return response.json()

# Usage
api = PaymentAddressAPI(base_url, access_token)
addr = api.create_payment_address({
    'accountType': 'Customer',
    'accountNo': 'C00001',
    'code': 'BILLING',
    'name': 'Accounts Payable',
    'address': 'PO Box 456',
    'city': 'London'
})
```

### C#
```csharp
public class PaymentAddressApiClient
{
    private readonly HttpClient _httpClient;
    private readonly string _baseUrl;

    public PaymentAddressApiClient(string baseUrl, string accessToken)
    {
        _baseUrl = baseUrl;
        _httpClient = new HttpClient();
        _httpClient.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", accessToken);
    }

    public async Task<List<PaymentAddress>> GetPaymentAddressesAsync(
        string accountNo, string accountType = "Customer")
    {
        var filter = $"accountType eq '{accountType}' and accountNo eq '{accountNo}'";
        var response = await _httpClient.GetAsync(
            $"{_baseUrl}/paymentAddressesFic?$filter={filter}");
        response.EnsureSuccessStatusCode();

        var json = await response.Content.ReadAsStringAsync();
        var result = JsonSerializer.Deserialize<ODataResponse<PaymentAddress>>(json);
        return result.Value;
    }

    public async Task<PaymentAddress> CreatePaymentAddressAsync(PaymentAddress address)
    {
        var json = JsonSerializer.Serialize(address);
        var content = new StringContent(json, Encoding.UTF8, "application/json");
        var response = await _httpClient.PostAsync(
            $"{_baseUrl}/paymentAddressesFic", content);
        response.EnsureSuccessStatusCode();

        var responseJson = await response.Content.ReadAsStringAsync();
        return JsonSerializer.Deserialize<PaymentAddress>(responseJson);
    }
}

public class PaymentAddress
{
    public Guid Id { get; set; }
    public string AccountType { get; set; }
    public string AccountNo { get; set; }
    public string Code { get; set; }
    public string Name { get; set; }
    public string Name2 { get; set; }
    public string Address { get; set; }
    public string Address2 { get; set; }
    public string City { get; set; }
    public string Contact { get; set; }
    public string TvtfiChineseNameSC { get; set; }
    public string TvtfiAddressSC { get; set; }
    public string TvtfiCitySC { get; set; }
    public DateTime SystemCreatedAt { get; set; }
    public DateTime SystemModifiedAt { get; set; }
}
```

## Business Logic Notes

- **Primary Key**: Combination of Account Type, Account No., and Code
- **Account Types**: Customer or Vendor
- **Chinese Support**: Includes separate fields for Simplified Chinese (SC) and Traditional Chinese (TC) translations
- **Use Cases**: Alternative billing addresses, payment remittance addresses, regional offices
- **Parent Relationship**: Must reference existing Customer or Vendor via `accountNo`

## Filtering Tips

```http
# All addresses for a customer
$filter=accountType eq 'Customer' and accountNo eq 'C00001'

# Specific address
$filter=accountNo eq 'C00001' and code eq 'BILLING'

# All customer payment addresses
$filter=accountType eq 'Customer'

# Addresses with Chinese translation
$filter=tvtfiChineseNameSC ne null

# Recently created
$filter=systemCreatedAt gt 2026-01-01T00:00:00Z
```

## Error Handling

Common errors:
- **401 Unauthorized**: Invalid access token
- **400 Bad Request**: Missing required fields
- **404 Not Found**: Account or address doesn't exist
- **409 Conflict**: Address code already exists for this account

## Performance Tips

1. **Filter by Account**: Always filter by accountNo when retrieving addresses
2. **Use $select**: Only retrieve needed fields, especially if not using Chinese fields
3. **Cache Data**: Payment addresses change infrequently

## Related APIs
- **Customer**: Parent customer records
- **SalesHeader**: Can reference payment addresses in sales transactions

## Integration Patterns

### Multi-Address Management
```javascript
async function setupCustomerAddresses(customerNo) {
  const api = new PaymentAddressAPI(baseUrl, accessToken);

  // Create standard addresses
  const addresses = [
    { code: 'BILLING', name: 'Billing Department', address: 'PO Box 100' },
    { code: 'PAYMENT', name: 'Payment Processing', address: 'Finance Dept' },
    { code: 'DELIVERY', name: 'Delivery Address', address: '123 Main St' }
  ];

  for (const addr of addresses) {
    await api.createPaymentAddress({
      accountType: 'Customer',
      accountNo: customerNo,
      ...addr,
      city: 'London'
    });
  }
}
```

---

**Last Updated**: January 7, 2026
**API Version**: 2.0
