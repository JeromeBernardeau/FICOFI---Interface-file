# Contact API - Developer Guide

## Overview
The Contact API provides full access to contact master data including individuals and companies, contact information, relationships, and CRM integration fields. Supports creating, updating, and reading contact records.

## API Endpoint Details
- **Endpoint**: `/contactsFic`
- **Entity Name**: contactFic
- **Entity Set Name**: contactsFic
- **Page ID**: 81004
- **Source Table**: Contact
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
https://api.businesscentral.dynamics.com/v2.0/{environment}/api/tvisiontech/ficofiweb/v2.0/contactsFic
```

## Key Field Reference

| Field Name | Type | Description |
|------------|------|-------------|
| `systemId` | GUID | System identifier (OData key) |
| `no` | String | Contact number |
| `name` | String | Contact name |
| `name2` | String | Additional name field |
| `type` | Enum | Contact type (Company or Person) |
| `companyNo` | String | Company contact number (for Person contacts) |
| `companyName` | String | Company name |
| `firstName` | String | First name (for Person contacts) |
| `middleName` | String | Middle name |
| `surname` | String | Surname/last name |
| `address` | String | Street address line 1 |
| `address2` | String | Street address line 2 |
| `city` | String | City |
| `county` | String | County/State |
| `countryRegionCode` | String | Country/region code |
| `postCode` | String | Postal/ZIP code |
| `phoneNo` | String | Phone number |
| `mobilePhoneNo` | String | Mobile phone number |
| `eMail` | String | Email address |
| `faxNo` | String | Fax number |
| `homePage` | String | Website URL |
| `salespersonCode` | String | Assigned salesperson |
| `territoryCode` | String | Territory code |
| `languageCode` | String | Language preference |
| `currencyCode` | String | Preferred currency |
| `jobTitle` | String | Job title |

For full field list, see the source API page.

## Common Use Cases

### 1. Get All Contacts
```http
GET /contactsFic
```

### 2. Search Contacts by Name
```http
GET /contactsFic?$filter=contains(name, 'Smith')
```

### 3. Get Contact by Number
```http
GET /contactsFic?$filter=no eq 'CT000001'
```

### 4. Get Person Contacts for a Company
```http
GET /contactsFic?$filter=type eq 'Person' and companyNo eq 'CT000100'
```

### 5. Get Contacts by Email
```http
GET /contactsFic?$filter=eMail eq 'contact@example.com'
```

### 6. Create Company Contact
```http
POST /contactsFic
Content-Type: application/json

{
  "type": "Company",
  "name": "Example Wine Merchants",
  "address": "123 Main Street",
  "city": "London",
  "postCode": "SW1A 1AA",
  "countryRegionCode": "GB",
  "phoneNo": "+44 20 1234 5678",
  "eMail": "info@example.com"
}
```

### 7. Create Person Contact
```http
POST /contactsFic
Content-Type: application/json

{
  "type": "Person",
  "companyNo": "CT000100",
  "firstName": "John",
  "surname": "Smith",
  "jobTitle": "Purchasing Manager",
  "phoneNo": "+44 20 1234 5678",
  "mobilePhoneNo": "+44 7700 900000",
  "eMail": "john.smith@example.com"
}
```

### 8. Update Contact
```http
PATCH /contactsFic({systemId})
Content-Type: application/json

{
  "phoneNo": "+44 20 9999 8888",
  "eMail": "newemail@example.com"
}
```

## Code Examples

### JavaScript
```javascript
class ContactAPI {
  constructor(baseUrl, accessToken) {
    this.baseUrl = baseUrl;
    this.headers = {
      'Authorization': `Bearer ${accessToken}`,
      'Content-Type': 'application/json'
    };
  }

  async getContact(contactNo) {
    const response = await fetch(
      `${this.baseUrl}/contactsFic?$filter=no eq '${contactNo}'`,
      { headers: this.headers }
    );
    const data = await response.json();
    return data.value[0];
  }

  async searchContacts(searchTerm) {
    const response = await fetch(
      `${this.baseUrl}/contactsFic?$filter=contains(name, '${searchTerm}') or contains(eMail, '${searchTerm}')`,
      { headers: this.headers }
    );
    const data = await response.json();
    return data.value;
  }

  async createCompany(companyData) {
    const response = await fetch(
      `${this.baseUrl}/contactsFic`,
      {
        method: 'POST',
        headers: this.headers,
        body: JSON.stringify({ ...companyData, type: 'Company' })
      }
    );
    return await response.json();
  }

  async createPerson(personData) {
    const response = await fetch(
      `${this.baseUrl}/contactsFic`,
      {
        method: 'POST',
        headers: this.headers,
        body: JSON.stringify({ ...personData, type: 'Person' })
      }
    );
    return await response.json();
  }

  async getCompanyContacts(companyNo) {
    const response = await fetch(
      `${this.baseUrl}/contactsFic?$filter=type eq 'Person' and companyNo eq '${companyNo}'`,
      { headers: this.headers }
    );
    const data = await response.json();
    return data.value;
  }
}

// Usage
const api = new ContactAPI(baseUrl, accessToken);

// Create company
const company = await api.createCompany({
  name: 'Example Wine Merchants',
  address: '123 Main Street',
  city: 'London',
  postCode: 'SW1A 1AA',
  countryRegionCode: 'GB',
  eMail: 'info@example.com'
});

// Create person linked to company
const person = await api.createPerson({
  companyNo: company.no,
  firstName: 'John',
  surname: 'Smith',
  jobTitle: 'Buyer',
  eMail: 'john@example.com'
});
```

### Python
```python
class ContactAPI:
    def __init__(self, base_url: str, access_token: str):
        self.base_url = base_url
        self.headers = {
            'Authorization': f'Bearer {access_token}',
            'Content-Type': 'application/json'
        }

    def get_contact(self, contact_no: str) -> dict:
        response = requests.get(
            f'{self.base_url}/contactsFic',
            headers=self.headers,
            params={'$filter': f"no eq '{contact_no}'"}
        )
        response.raise_for_status()
        data = response.json()['value']
        return data[0] if data else None

    def create_company(self, company_data: dict) -> dict:
        company_data['type'] = 'Company'
        response = requests.post(
            f'{self.base_url}/contactsFic',
            headers=self.headers,
            json=company_data
        )
        response.raise_for_status()
        return response.json()

    def create_person(self, person_data: dict) -> dict:
        person_data['type'] = 'Person'
        response = requests.post(
            f'{self.base_url}/contactsFic',
            headers=self.headers,
            json=person_data
        )
        response.raise_for_status()
        return response.json()

    def get_company_contacts(self, company_no: str) -> list:
        response = requests.get(
            f'{self.base_url}/contactsFic',
            headers=self.headers,
            params={'$filter': f"type eq 'Person' and companyNo eq '{company_no}'"}
        )
        response.raise_for_status()
        return response.json()['value']

# Usage
api = ContactAPI(base_url, access_token)
company = api.create_company({
    'name': 'Example Wine Merchants',
    'address': '123 Main Street',
    'city': 'London',
    'postCode': 'SW1A 1AA',
    'countryRegionCode': 'GB',
    'eMail': 'info@example.com'
})
```

## Business Logic Notes

- **Contact Types**: Two types - Company and Person
- **Hierarchical Structure**: Person contacts link to Company contacts via `companyNo`
- **Name Fields**: For Person contacts, use `firstName`, `middleName`, `surname`; for Company, use `name`
- **CRM Integration**: Includes fields for interactions, opportunities, tasks, and campaign tracking
- **Email Validation**: Email field is not validated by API; validate client-side
- **Search Name**: Automatically generated for quick searching

## Filtering Tips

```http
# Company contacts only
$filter=type eq 'Company'

# Person contacts only
$filter=type eq 'Person'

# Contacts with email
$filter=eMail ne null and eMail ne ''

# Contacts by salesperson
$filter=salespersonCode eq 'JD'

# Contacts in territory
$filter=territoryCode eq 'SOUTH'

# Recently modified
$filter=systemModifiedAt gt 2026-01-01T00:00:00Z
```

## Error Handling

Common errors:
- **401 Unauthorized**: Invalid access token
- **400 Bad Request**: Missing required fields or invalid type
- **404 Not Found**: Contact doesn't exist
- **409 Conflict**: Contact number already exists

## Performance Tips

1. **Use $select**: Only retrieve needed fields (Contact table has many fields)
2. **Filter by Type**: Always specify type when searching for companies or persons
3. **Cache Data**: Contact data typically changes infrequently
4. **Pagination**: Use `$top` and `$skip` for large lists

## Related APIs
- **Customer**: For converting contacts to customers
- **SalesHeader**: For sales transactions with contacts

---

**Last Updated**: January 7, 2026
**API Version**: 2.0
