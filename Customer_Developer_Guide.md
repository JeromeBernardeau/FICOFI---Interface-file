# Customer API - Developer Guide

## Overview
The Customer API provides full access to customer master data including billing information, payment terms, credit limits, and web integration fields. Supports creating, updating, and reading customer records.

## API Endpoint Details
- **Endpoint**: `/customersFic`
- **Entity Name**: customerFic
- **Entity Set Name**: customersFic
- **Page ID**: 81003
- **Source Table**: Customer
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
https://api.businesscentral.dynamics.com/v2.0/{environment}/api/tvisiontech/ficofiweb/v2.0/customersFic
```

## Key Field Reference

| Field Name | Type | Description |
|------------|------|-------------|
| `systemId` | GUID | System identifier (OData key) |
| `no` | String | Customer number |
| `name` | String | Customer name |
| `name2` | String | Additional name field |
| `address` | String | Street address line 1 |
| `address2` | String | Street address line 2 |
| `city` | String | City |
| `county` | String | County/State |
| `countryRegionCode` | String | Country/region code |
| `postCode` | String | Postal/ZIP code |
| `contact` | String | Contact person name |
| `phoneNo` | String | Phone number |
| `mobilePhoneNo` | String | Mobile phone number |
| `eMail` | String | Email address |
| `homePage` | String | Website URL |
| `blocked` | Enum | Block status (blank, Ship, Invoice, All) |
| `creditLimitLCY` | Decimal | Credit limit in local currency |
| `currencyCode` | String | Default currency |
| `languageCode` | String | Language code |
| `paymentTermsCode` | String | Payment terms |
| `paymentMethodCode` | String | Payment method |
| `salespersonCode` | String | Assigned salesperson |
| `customerPostingGroup` | String | Customer posting group |
| `genBusPostingGroup` | String | General business posting group |
| `vatRegistrationNo` | String | VAT registration number |

For full field list, see the source API page.

## Common Use Cases

### 1. Get All Customers
```http
GET /customersFic
```

### 2. Search Customers by Name
```http
GET /customersFic?$filter=contains(name, 'Wine')
```

### 3. Get Customer by Number
```http
GET /customersFic?$filter=no eq 'C00001'
```

### 4. Get Active Customers
```http
GET /customersFic?$filter=blocked eq ' '
```

### 5. Create Customer
```http
POST /customersFic
Content-Type: application/json

{
  "name": "Example Wine Shop",
  "address": "456 High Street",
  "city": "Manchester",
  "postCode": "M1 1AA",
  "countryRegionCode": "GB",
  "phoneNo": "+44 161 234 5678",
  "eMail": "orders@examplewine.com",
  "paymentTermsCode": "30 DAYS",
  "salespersonCode": "JD"
}
```

### 6. Update Customer
```http
PATCH /customersFic({systemId})
Content-Type: application/json

{
  "eMail": "newemail@examplewine.com",
  "creditLimitLCY": 50000
}
```

### 7. Block Customer
```http
PATCH /customersFic({systemId})
Content-Type: application/json

{
  "blocked": "All"
}
```

## Code Examples

### JavaScript
```javascript
class CustomerAPI {
  constructor(baseUrl, accessToken) {
    this.baseUrl = baseUrl;
    this.headers = {
      'Authorization': `Bearer ${accessToken}`,
      'Content-Type': 'application/json'
    };
  }

  async getCustomer(customerNo) {
    const response = await fetch(
      `${this.baseUrl}/customersFic?$filter=no eq '${customerNo}'`,
      { headers: this.headers }
    );
    const data = await response.json();
    return data.value[0];
  }

  async createCustomer(customerData) {
    const response = await fetch(
      `${this.baseUrl}/customersFic`,
      {
        method: 'POST',
        headers: this.headers,
        body: JSON.stringify(customerData)
      }
    );
    return await response.json();
  }

  async updateCustomer(systemId, updates) {
    const response = await fetch(
      `${this.baseUrl}/customersFic(${systemId})`,
      {
        method: 'PATCH',
        headers: this.headers,
        body: JSON.stringify(updates)
      }
    );
    return await response.json();
  }

  async getActiveCustomers() {
    const response = await fetch(
      `${this.baseUrl}/customersFic?$filter=blocked eq ' '`,
      { headers: this.headers }
    );
    const data = await response.json();
    return data.value;
  }
}

// Usage
const api = new CustomerAPI(baseUrl, accessToken);
const customer = await api.createCustomer({
  name: 'Example Wine Shop',
  address: '456 High Street',
  city: 'Manchester',
  postCode: 'M1 1AA',
  countryRegionCode: 'GB',
  eMail: 'orders@examplewine.com'
});
```

### Python
```python
class CustomerAPI:
    def __init__(self, base_url: str, access_token: str):
        self.base_url = base_url
        self.headers = {
            'Authorization': f'Bearer {access_token}',
            'Content-Type': 'application/json'
        }

    def get_customer(self, customer_no: str) -> dict:
        response = requests.get(
            f'{self.base_url}/customersFic',
            headers=self.headers,
            params={'$filter': f"no eq '{customer_no}'"}
        )
        response.raise_for_status()
        data = response.json()['value']
        return data[0] if data else None

    def create_customer(self, customer_data: dict) -> dict:
        response = requests.post(
            f'{self.base_url}/customersFic',
            headers=self.headers,
            json=customer_data
        )
        response.raise_for_status()
        return response.json()

    def update_customer(self, system_id: str, updates: dict) -> dict:
        response = requests.patch(
            f'{self.base_url}/customersFic({system_id})',
            headers=self.headers,
            json=updates
        )
        response.raise_for_status()
        return response.json()

# Usage
api = CustomerAPI(base_url, access_token)
customer = api.create_customer({
    'name': 'Example Wine Shop',
    'city': 'Manchester',
    'countryRegionCode': 'GB'
})
```

## Business Logic Notes

- **Blocked Status**: Empty string = active, "Ship" = cannot ship, "Invoice" = cannot invoice, "All" = completely blocked
- **Credit Limit**: Enforced at order entry if configured
- **Posting Groups**: Required for financial posting
- **Number Series**: Customer number may be auto-generated based on setup
- **Balance Fields**: Available in full Customer table (not all exposed in this API)

## Filtering Tips

```http
# Active customers
$filter=blocked eq ' '

# Blocked customers
$filter=blocked eq 'All'

# Customers by salesperson
$filter=salespersonCode eq 'JD'

# Customers with email
$filter=eMail ne null and eMail ne ''

# Recently created
$filter=systemCreatedAt gt 2026-01-01T00:00:00Z
```

## Related APIs
- **Contact**: For contact management before customer conversion
- **SalesHeader**: For customer sales transactions
- **PaymentAddress**: For alternative payment addresses

---

**Last Updated**: January 7, 2026
**API Version**: 2.0
