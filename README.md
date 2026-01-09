# FICOFI API Documentation

Welcome to the FICOFI API documentation. This collection provides comprehensive developer guides for integrating with Business Central via the FICOFI Web APIs.

## Overview

The FICOFI APIs provide access to Business Central data through RESTful OData endpoints. All APIs use:
- **Publisher**: tvisiontech
- **API Group**: ficofiweb
- **Version**: v2.0
- **Authentication**: OAuth 2.0 Bearer tokens
- **Base URL**: `https://api.businesscentral.dynamics.com/v2.0/{environment}/api/tvisiontech/ficofiweb/v2.0/`

## API Endpoints

### Item Management
- **[Item API](Item_Developer_Guide.md)** - Item master data with classifications, regions, vintages
- **[Item Translation API](ItemTranslation_Developer_Guide.md)** - Multi-language item descriptions and marketing content
- **[Item Unit of Measure API](ItemUnitOfMeasure_Developer_Guide.md)** - Alternative units of measure for items

### Unpaid Reserve (UPR) Management
- **[UPR Group API](UPRGroup_Developer_Guide.md)** - Unpaid reserve groups for managing pre-orders and allocations (read-only)
- **[UPR Information API](UPRInformation_Developer_Guide.md)** - Individual reservation entries and fulfillment tracking (read-only)

### Customer & Contact Management
- **[Contact API](Contact_Developer_Guide.md)** - Contact master data for companies and persons
- **[Customer API](Customer_Developer_Guide.md)** - Customer master data with billing and credit information
- **[Payment Address API](PaymentAddress_Developer_Guide.md)** - Alternative payment addresses with Chinese translation support

### Sales Transaction Management
- **[Sales Header API](SalesHeader_Developer_Guide.md)** - Sales document headers (quotes, orders, invoices)
- **[Sales Line API](SalesLine_Developer_Guide.md)** - Sales document line details with items and pricing

## Quick Start

### Authentication
All API calls require authentication using OAuth 2.0. Obtain an access token from Azure AD:

```http
POST https://login.microsoftonline.com/{tenant-id}/oauth2/v2.0/token
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials
&client_id={client-id}
&client_secret={client-secret}
&scope=https://api.businesscentral.dynamics.com/.default
```

### Example API Call

```javascript
const baseUrl = 'https://api.businesscentral.dynamics.com/v2.0/{environment}/api/tvisiontech/ficofiweb/v2.0';
const accessToken = 'your-access-token';

const response = await fetch(`${baseUrl}/itemsFic`, {
  headers: {
    'Authorization': `Bearer ${accessToken}`,
    'Content-Type': 'application/json'
  }
});

const data = await response.json();
console.log(data.value);
```

## Common Patterns

### OData Query Options

All APIs support standard OData query options:

- **$filter**: Filter results
  ```http
  GET /itemsFic?$filter=tvtVintage eq 2020
  ```

- **$select**: Select specific fields
  ```http
  GET /itemsFic?$select=no,description,tvtVintage
  ```

- **$expand**: Include related entities
  ```http
  GET /itemsFic?$expand=itemsUnitOfMeasureFic,itemsTranslationFic
  ```

- **$orderby**: Sort results
  ```http
  GET /itemsFic?$orderby=description asc
  ```

- **$top / $skip**: Pagination
  ```http
  GET /itemsFic?$top=20&$skip=40
  ```

- **$count**: Get total count
  ```http
  GET /itemsFic?$count=true
  ```

### Creating Records

```http
POST /itemsFic
Content-Type: application/json
Authorization: Bearer {token}

{
  "no": "WINE001",
  "description": "Château Example 2020",
  "type": "Inventory",
  "baseUnitOfMeasure": "BTL",
  "tvtVintage": 2020
}
```

### Updating Records

```http
PATCH /itemsFic({systemId})
Content-Type: application/json
Authorization: Bearer {token}

{
  "description": "Updated Description",
  "tvtMarketingDescription": "New marketing content"
}
```

## API Categories

### Read/Write APIs
These APIs support creating and modifying records:
- Item
- Item Translation
- Item Unit of Measure
- Contact
- Customer
- Payment Address
- Sales Header
- Sales Line

### Read-Only APIs
These APIs are for reading data only:
- UPR Group
- UPR Information

## Best Practices

1. **Use Filters**: Always filter results to reduce payload size
2. **Select Fields**: Use `$select` to retrieve only needed fields
3. **Pagination**: Implement pagination for large datasets
4. **Error Handling**: Handle 401, 400, 404, and 409 errors appropriately
5. **Rate Limiting**: Respect API rate limits and implement retry logic
6. **Caching**: Cache master data that changes infrequently
7. **Batch Operations**: Group related operations when possible

## Error Codes

| Status Code | Description | Common Causes |
|-------------|-------------|---------------|
| 200 OK | Request successful | - |
| 201 Created | Resource created | POST successful |
| 400 Bad Request | Invalid request | Missing fields, invalid syntax |
| 401 Unauthorized | Authentication failed | Invalid/expired token |
| 404 Not Found | Resource not found | Invalid ID or deleted record |
| 409 Conflict | Resource conflict | Duplicate key, version mismatch |
| 429 Too Many Requests | Rate limit exceeded | Too many API calls |
| 500 Internal Server Error | Server error | Contact support |

## Support & Resources

- **Business Central Documentation**: [Microsoft Docs](https://docs.microsoft.com/en-us/dynamics365/business-central/)
- **OData Protocol**: [OData.org](https://www.odata.org/)
- **OAuth 2.0 Guide**: [OAuth Documentation](https://oauth.net/2/)

## Industry-Specific Features

The FICOFI APIs include specialized features for the wine and beverage industry:

- **Wine Classification**: Region, SubRegion, Appellation, Style fields
- **Vintage Tracking**: Vintage year field on items
- **En Primeur Support**: UPR Groups for pre-release wine allocations
- **Multi-Language**: Translation support for international markets
- **Chinese Localization**: Simplified and Traditional Chinese fields on Payment Addresses

## API Reference Summary

| API | Endpoint | Primary Use | Key Features |
|-----|----------|-------------|--------------|
| Item | /itemsFic | Item master data | Wine classifications, vintages |
| Item Translation | /itemsTranslationFic | Multi-language content | Translations per language |
| Item Unit of Measure | /itemsUnitOfMeasureFic | Units of measure | Bottle, case, pallet support |
| UPR Group | /uPRGroupsFic | Pre-order groups | Category, salesperson assignment |
| UPR Information | /uPReservesFic | Reservation tracking | Quantity lifecycle tracking |
| Contact | /contactsFic | Contact records | Company and person types |
| Customer | /customersFic | Customer master | Credit limits, payment terms |
| Payment Address | /paymentAddressesFic | Alt payment addresses | Chinese translation support |
| Sales Header | /salesHeadersFic | Sales documents | Quotes, orders, invoices |
| Sales Line | /salesLinesFic | Sales line items | Pricing, discounts, quantities |

## Version History

- **v2.0** (Current) - January 2026
  - Comprehensive API coverage
  - Enhanced wine industry features
  - Chinese localization support

---

**Last Updated**: January 7, 2026
**API Version**: 2.0

For detailed information on each API endpoint, please refer to the individual developer guides linked above.
