# FICOFI API Documentation

**API Publisher**: `tvisiontech`
**API Group**: `ficofiweb`
**API Base URL**: `/api/tvisiontech/ficofiweb/v2.0`

---

## Table of Contents

1. [Sales APIs](#sales-apis)
2. [Customer & Contact APIs](#customer--contact-apis)
3. [Item APIs](#item-apis)
4. [Subscription APIs](#subscription-apis)
5. [UPR (Unpaid Reserves) APIs](#upr-unpaid-reserves-apis)
6. [Address APIs](#address-apis)

---

## Sales APIs

### Sales Header API

**Entity Name**: `salesHeaderFic`
**Entity Set Name**: `salesHeadersFic`
**Page ID**: 81000

#### Purpose
Manages sales header records (quotes, orders, invoices). Provides read-only access to sales document headers with related sales line items.

#### CRUD Operations
- **Create**: ❌ Not allowed
- **Read**: ✅ Allowed
- **Update**: ✅ Allowed (ModifyAllowed)
- **Delete**: ❌ Not allowed

#### Fields

| Field Name | Type | Description | Writable |
|---|---|---|---|
| id | GUID | System ID (unique identifier) | ❌ |
| documentType | Code[10] | Sales document type (Quote, Order, Invoice, etc.) | ❌ |
| no | Code[20] | Document number | ❌ |
| quoteNo | Code[20] | Related quote number | ❌ |
| status | Option | Document status | ❌ |
| sellToCustomerNo | Code[20] | Customer number for ship-to | ❌ |
| sellToCustomerId | GUID | Related customer system ID | ❌ |
| sellToCustomerName | Text[100] | Customer name | ❌ |
| billToCustomerNo | Code[20] | Bill-to customer number | ❌ |
| billToCustomerId | GUID | Related bill-to customer system ID | ❌ |
| billToName | Text[100] | Bill-to name | ❌ |
| shipToCode | Code[10] | Ship-to address code | ❌ |
| documentDate | Date | Document date | ❌ |
| externalDocumentNo | Code[35] | Customer's reference number | ❌ |
| salespersonCode | Code[20] | Salesperson code | ❌ |
| currencyCode | Code[10] | Currency code | ❌ |
| pricesIncludingVAT | Boolean | Prices include VAT indicator | ❌ |
| paymentTermsCode | Code[10] | Payment terms code | ❌ |
| paymentMethodCode | Code[10] | Payment method code | ❌ |
| shipmentMethodCode | Code[10] | Shipment method code | ❌ |
| locationCode | Code[10] | Warehouse location code | ❌ |
| campaignNo | Code[10] | Campaign number | ❌ |
| campaignId | GUID | Related campaign system ID | ❌ |
| opportunityNo | Code[20] | Opportunity number | ❌ |
| tvtDutyStatus | Code[10] | TVT duty status code | ❌ |
| tvtOrderSource | Code[20] | TVT order source | ❌ |
| tvtbeReservationStatus | Code[20] | TVTBE reservation status | ❌ |
| tvtfiOfferType | Code[20] | TVTFI offer type | ❌ |
| tvtfiOfferTag | Code[20] | TVTFI offer tag | ❌ |
| tvtfiOfferID | Code[20] | TVTFI offer identifier | ❌ |
| tvtfiOfferNameSC | Text[100] | TVTFI offer name (Simplified Chinese) | ❌ |
| tvtfiOfferNameTC | Text[100] | TVTFI offer name (Traditional Chinese) | ❌ |
| tvtfiOfferDescription | Text[250] | TVTFI offer description | ❌ |
| tvtfiOfferStatus | Code[20] | TVTFI offer status | ✅ |
| tvtfiMemberServiceManager | Code[20] | TVTFI member service manager | ❌ |
| tvtfiExpDeliveryInstr | Code[20] | TVTFI expected delivery instructions | ❌ |
| tvtfiExpDeliveryShipTo | Code[10] | TVTFI expected delivery ship-to address | ❌ |
| tvtfiExpDeliveryLocCode | Code[10] | TVTFI expected delivery location code | ❌ |
| quoteValidUntilDate | Date | Quote validity expiration date | ❌ |
| quoteAcceptedDate | Date | Quote acceptance date | ❌ |
| yourReference | Code[35] | Customer reference | ❌ |
| tvtfiYourReferenceSC | Text[35] | Customer reference (Simplified Chinese) | ❌ |
| tvtfiYourReferenceTC | Text[35] | Customer reference (Traditional Chinese) | ❌ |
| amount | Decimal | Document amount (excluding VAT) | ❌ |
| amountIncludingVAT | Decimal | Document amount (including VAT) | ❌ |
| systemModifiedAt | DateTime | Last system modification timestamp | ❌ |

#### Related Resources
- **Sales Lines**: Sub-collection accessible via `salesLinesFic` entity

---

### Sales Line API

**Entity Name**: `salesLineFic`
**Entity Set Name**: `salesLinesFic`
**Page ID**: 81001
**Source Table**: TVTFIC Sales Line API (Temporary)

#### Purpose
Manages sales line items within sales documents. Provides read/write access to individual line items with quantity and delivery management.

#### CRUD Operations
- **Create**: ❌ Not allowed
- **Read**: ✅ Allowed
- **Update**: ✅ Allowed (ModifyAllowed)
- **Delete**: ❌ Not allowed

#### Fields

| Field Name | Type | Description | Writable |
|---|---|---|---|
| id | GUID | System ID (unique identifier) | ❌ |
| documentId | GUID | Parent sales header system ID | ❌ |
| documentNo | Code[20] | Sales document number | ❌ |
| lineNo | Integer | Line sequence number | ❌ |
| type | Code[10] | Line type (Item, Resource, G/L Account) | ❌ |
| no | Code[20] | Item, resource, or account number | ❌ |
| variantCode | Code[10] | Item variant code | ❌ |
| description | Text[100] | Line description | ❌ |
| locationCode | Code[10] | Warehouse location code | ❌ |
| quantity | Decimal | Quantity ordered | ✅ |
| quantityBase | Decimal | Quantity in base unit of measure | ✅ |
| unitOfMeasureCode | Code[10] | Unit of measure code | ❌ |
| qtyPerUnitOfMeasure | Decimal | Qty per unit of measure conversion factor | ❌ |
| unitPrice | Decimal | Unit price | ❌ |
| tvtfiBaseUnitPrice | Decimal | TVTFI base unit price | ❌ |
| lineDiscountPerc | Decimal | Line discount percentage | ❌ |
| lineAmount | Decimal | Line total amount | ❌ |
| tvtfiExpDeliveryShipTo | Code[10] | TVTFI expected delivery ship-to address | ✅ |
| tvtfiOfferQuantity | Decimal | TVTFI offer quantity restriction | ❌ |
| tvtfiMaximumQuantity | Decimal | TVTFI maximum allowed quantity | ❌ |
| tvtfiSoftReservedQtySR | Decimal | TVTFI soft reserved quantity | ✅ |
| stockForecastAfterUpdate | Decimal | Stock forecast after updates (read-only calculated) | ❌ |

#### Filters & Usage Note
Must be filtered by either:
- `id` (individual line ID), or
- `documentId` (parent document ID)

---

### Sales Invoice Header API

**Entity Name**: `salesInvoiceHeaderFic`
**Entity Set Name**: `salesInvoiceHeadersFic`
**Page ID**: 81011

#### Purpose
Provides read-only access to posted sales invoice headers with line items.

#### CRUD Operations
- **Create**: ❌ Not allowed
- **Read**: ✅ Allowed
- **Update**: ❌ Not allowed
- **Delete**: ❌ Not allowed

#### Key Fields

| Field Name | Type | Description |
|---|---|---|
| id | GUID | System ID |
| no | Code[20] | Invoice number |
| quoteNo | Code[20] | Original quote number |
| orderNo | Code[20] | Original order number |
| sellToCustomerNo | Code[20] | Customer number |
| sellToCustomerId | GUID | Related customer system ID |
| sellToCustomerName | Text[100] | Customer name |
| billToCustomerNo | Code[20] | Bill-to customer number |
| billToCustomerId | GUID | Related bill-to customer system ID |
| billToName | Text[100] | Bill-to name |
| shipToCode | Code[10] | Ship-to address code |
| documentDate | Date | Invoice date |
| externalDocumentNo | Code[35] | Customer reference |
| salespersonCode | Code[20] | Salesperson code |
| currencyCode | Code[10] | Currency code |
| pricesIncludingVAT | Boolean | Prices include VAT |
| paymentTermsCode | Code[10] | Payment terms |
| paymentMethodCode | Code[10] | Payment method |
| shipmentMethodCode | Code[10] | Shipment method |
| locationCode | Code[10] | Location code |
| amount | Decimal | Amount excluding VAT |
| amountIncludingVAT | Decimal | Amount including VAT |

#### Related Resources
- **Sales Invoice Lines**: Sub-collection via `salesInvoiceLinesFic` entity

---

### Sales Invoice Line API

**Entity Name**: `salesInvoiceLineFic`
**Entity Set Name**: `salesInvoiceLinesFic`
**Page ID**: 81012

#### Purpose
Provides read-only access to posted sales invoice line items.

#### CRUD Operations
- **Create**: ❌ Not allowed
- **Read**: ✅ Allowed
- **Update**: ❌ Not allowed
- **Delete**: ❌ Not allowed

#### Key Fields

| Field Name | Type | Description |
|---|---|---|
| id | GUID | System ID |
| documentNo | Code[20] | Invoice number |
| lineNo | Integer | Line sequence number |
| type | Code[10] | Line type |
| no | Code[20] | Item/Account number |
| description | Text[100] | Line description |
| quantity | Decimal | Quantity invoiced |
| unitOfMeasureCode | Code[10] | Unit of measure code |
| unitPrice | Decimal | Unit price |
| lineDiscountPerc | Decimal | Line discount % |
| lineAmount | Decimal | Line total amount |
| tvtfiExpDeliveryShipTo | Code[10] | Expected delivery ship-to |

---

### Sales Header Archive API

**Entity Name**: `salesHeaderArchiveFic`
**Entity Set Name**: `salesHeaderArchivesFic`
**Page ID**: 81013

#### Purpose
Provides read-only access to archived sales of sales headers with all versions maintained for historical records.

#### CRUD Operations
- **Create**: ❌ Not allowed
- **Read**: ✅ Allowed
- **Update**: ❌ Not allowed
- **Delete**: ❌ Not allowed

#### Related Resources
- **Sales Line Archives**: Sub-collection via `salesLineArchivesFic` entity

---

### Sales Line Archive API

**Entity Name**: `salesLineArchiveFic`
**Entity Set Name**: `salesLineArchivesFic`
**Page ID**: 81014

#### Purpose
Provides read-only access to archived sales line items from historical sales document versions.

#### CRUD Operations
- **Create**: ❌ Not allowed
- **Read**: ✅ Allowed
- **Update**: ❌ Not allowed
- **Delete**: ❌ Not allowed

#### Key Fields

| Field Name | Type | Description |
|---|---|---|
| id | GUID | System ID |
| documentNo | Code | Document number |
| lineNo | Integer | Line sequence number |
| type | Option | Line type |
| no | Code | Item/Account number |
| variantCode | Code | Item variant code |
| description | Text | Line description |
| locationCode | Code | Location code |
| quantity | Decimal | Quantity |
| quantityBase | Decimal | Quantity in base UOM |
| unitOfMeasureCode | Code | Unit of measure |
| qtyPerUnitOfMeasure | Decimal | Qty per UOM |
| unitPrice | Decimal | Unit price |
| lineDiscountPerc | Decimal | Line discount % |
| lineAmount | Decimal | Line total |

---

## Customer & Contact APIs

### Customer API

**Entity Name**: `customerFic`
**Entity Set Name**: `customersFic`
**Page ID**: 81003

#### Purpose
Manages customer master data including addresses, payment terms, credit limits, and business relationships.

#### CRUD Operations
- **Create**: ✅ Allowed
- **Read**: ✅ Allowed
- **Update**: ✅ Allowed
- **Delete**: ❌ Not allowed

#### Notable Fields

| Field Name | Type | Description | Writable |
|---|---|---|---|
| address | Text[100] | Primary address | ✅ |
| address2 | Text[50] | Address line 2 | ✅ |
| applicationMethod | Option | Application method for payments | ✅ |
| billToCustomerNo | Code[20] | Bill-to customer number | ✅ |
| blocked | Option | Blocked status | ✅ |
| chainName | Text[100] | Chain/Group name | ✅ |
| city | Text[30] | City | ✅ |
| collectionMethod | Option | Collection method | ✅ |
| combineShipments | Boolean | Combine shipments | ✅ |
| contact | Text[100] | Primary contact person | ✅ |
| contactType | Option | Contact type | ✅ |
| countryRegionCode | Code[10] | Country/Region code | ✅ |
| county | Text[30] | County/State | ✅ |
| creditLimitLCY | Decimal | Credit limit in local currency | ✅ |
| currencyCode | Code[10] | Default currency | ✅ |
| currencyId | GUID | Currency system ID | ✅ |
| customerDiscGroup | Code[20] | Customer discount group | ✅ |
| customerPostingGroup | Code[20] | Customer posting group | ✅ |
| customerPriceGroup | Code[10] | Customer price group | ✅ |

---

### Contact API

**Entity Name**: `contactFic`
**Entity Set Name**: `contactsFic`
**Page ID**: 81004

#### Purpose
Manages contact information for individuals and organizations associated with sales activities.

#### CRUD Operations
- **Create**: ✅ Allowed
- **Read**: ✅ Allowed
- **Update**: ✅ Allowed
- **Delete**: ❌ Not allowed

#### Fields

| Field Name | Type | Description | Writable |
|---|---|---|---|
| no | Code[20] | Contact number | ✅ |
| name | Text[100] | Primary name | ✅ |
| name2 | Text[50] | Name line 2 | ✅ |
| address | Text[100] | Primary address | ✅ |
| address2 | Text[50] | Address line 2 | ✅ |
| city | Text[30] | City | ✅ |
| postCode | Text[20] | Post code | ✅ |
| county | Text[30] | County/State | ✅ |
| countryRegionCode | Code[10] | Country/Region code | ✅ |
| phoneNo | Text[30] | Phone number | ✅ |
| mobilePhoneNo | Text[30] | Mobile phone number | ✅ |
| eMail | Text[80] | Email address | ✅ |
| faxNo | Text[30] | Fax number | ✅ |
| telexNo | Text[20] | Telex number | ✅ |
| telexAnswerBack | Text[20] | Telex answer back code | ✅ |
| languageCode | Code[5] | Language code | ✅ |
| salespersonCode | Code[20] | Assigned salesperson | ✅ |
| territoryCode | Code[3] | Territory code | ✅ |
| currencyCode | Code[10] | Currency code | ✅ |

#### [ServiceEnabled] Procedures

##### LinkToCustomerNo

**Description**: Links a contact to a customer by validating the contact exists and setting the contact's "Company No." field to the company contact associated with the specified customer.

| Aspect | Value |
|--------|-------|
| **Parameters** | `CustomerNo: Code[20]` |
| **Return Type** | None (void) |
| **Notes** | Throws an error if no matching company contact is found for the customer. |

##### LinkToCustomerId

**Description**: Links a contact to a customer using the customer's system ID (GUID). Loads the customer record by its system ID and delegates to `LinkToCustomerNo` to perform the actual linking.

| Aspect | Value |
|--------|-------|
| **Parameters** | `CustomerId: Guid` |
| **Return Type** | None (void) |
| **Notes** | This is a convenience method that accepts GUID instead of Code. |

---

## Item APIs

### Item API

**Entity Name**: `itemFic`
**Entity Set Name**: `itemsFic`
**Page ID**: 81005

#### Purpose
Provides access to item master data including product descriptions, classifications, and wine-specific attributes.

#### CRUD Operations
- **Create**: ✅ Allowed
- **Read**: ✅ Allowed
- **Update**: ✅ Allowed
- **Delete**: ❌ Not allowed

#### Key Fields

| Field Name | Type | Description | Writable |
|---|---|---|---|
| no | Code[20] | Item number | ✅ |
| description | Text[100] | Item description | ✅ |
| type | Code[10] | Item type | ✅ |
| tvtMarketingDescription | Text[250] | Marketing description | ✅ |
| baseUnitOfMeasure | Code[10] | Base unit of measure | ✅ |
| countryRegionOfOriginCode | Code[10] | Country of origin | ✅ |
| tvtRegionCode | Code[20] | Wine region | ✅ |
| tvtSubRegionCode | Code[20] | Wine sub-region | ✅ |
| tvtAppellationCode | Code[20] | Wine appellation | ✅ |
| tvtBrandCode | Code[20] | Brand code | ✅ |
| tvtClassificationCode | Code[20] | Classification code | ✅ |
| tvtStyleCode | Code[20] | Wine style code | ✅ |
| tvtVintage | Code[20] | Wine vintage year | ✅ |
| systemCreatedAt | DateTime | Creation timestamp | ❌ |
| systemCreatedBy | GUID | Creator ID | ❌ |
| systemId | GUID | System ID | ❌ |
| systemModifiedAt | DateTime | Last modification timestamp | ❌ |
| systemModifiedBy | GUID | Last modifier ID | ❌ |

#### [ServiceEnabled] Procedures

##### GetStockForecast

**Description**: Calculates the stock forecast for an item based on variant and location filters. Uses the available stock calculation logic, taking soft reservations into account.

| Aspect | Value |
|--------|-------|
| **Parameters** | `VariantFilter: Text`, `LocationFilter: Text` |
| **Return Type** | `Decimal` (assigned to `StockForecast` field) |
| **Notes** | Returns 0 if the item number is empty. Integrates with soft reservation management for accurate stock planning. |

---

### Item Translation API

**Entity Name**: `itemTranslationFic`
**Entity Set Name**: `itemsTranslationFic`
**Page ID**: 81007

#### Purpose
Manages multilingual item descriptions and marketing information for different languages.

#### CRUD Operations
- **Create**: ✅ Allowed
- **Read**: ✅ Allowed
- **Update**: ✅ Allowed
- **Delete**: ❌ Not allowed

#### Fields

| Field Name | Type | Description | Writable |
|---|---|---|---|
| itemNo | Code[20] | Item number | ✅ |
| description | Text[100] | Translated description | ✅ |
| languageCode | Code[5] | Language code | ✅ |
| tvtfiMarketingDescription | Text[250] | Marketing description (translated) | ✅ |
| systemCreatedAt | DateTime | Creation timestamp | ❌ |
| systemCreatedBy | GUID | Creator ID | ❌ |
| systemId | GUID | System ID | ❌ |
| systemModifiedAt | DateTime | Last modification timestamp | ❌ |
| systemModifiedBy | GUID | Last modifier ID | ❌ |

---

### Item Unit of Measure API

**Entity Name**: `itemUnitOfMeasureFic`
**Entity Set Name**: `itemsUnitOfMeasureFic`
**Page ID**: 81006

#### Purpose
Manages alternative units of measure for items and their conversion factors.

#### CRUD Operations
- **Create**: ✅ Allowed
- **Read**: ✅ Allowed
- **Update**: ✅ Allowed
- **Delete**: ❌ Not allowed

#### Fields

| Field Name | Type | Description | Writable |
|---|---|---|---|
| itemNo | Code[20] | Item number | ✅ |
| code | Code[10] | Unit of measure code | ✅ |
| systemCreatedAt | DateTime | Creation timestamp | ❌ |
| systemCreatedBy | GUID | Creator ID | ❌ |
| systemId | GUID | System ID | ❌ |
| systemModifiedAt | DateTime | Last modification timestamp | ❌ |
| systemModifiedBy | GUID | Last modifier ID | ❌ |

---

### Item Attribute Value API

**Entity Name**: `itemAttributeValueFic`
**Entity Set Name**: `itemAttributeValuesFic`
**Page ID**: 81017
**Source Table**: Item Attribute Value (Temporary)

#### Purpose
Provides read-only access to item attributes including product characteristics, specifications, and custom properties.

#### CRUD Operations
- **Create**: ❌ Not allowed
- **Read**: ✅ Allowed
- **Update**: ❌ Not allowed
- **Delete**: ❌ Not allowed

#### Fields

| Field Name | Type | Description |
|---|---|---|
| id | GUID | System ID |
| attributeId | Integer | Attribute identifier |
| attributeName | Text | Attribute name |
| value | Text | Attribute value |
| numericValue | Decimal | Numeric value (if applicable) |
| dateValue | Date | Date value (if applicable) |
| blocked | Boolean | Blocked status |
| primary | Boolean | Primary attribute indicator |
| createdAt | DateTime | Creation timestamp |
| createdBy | GUID | Creator ID |
| modifiedAt | DateTime | Last modification timestamp |
| modifiedBy | GUID | Last modifier ID |

---

## Subscription APIs

### Subscription Header API

**Entity Name**: `subscriptionHeaderFic`
**Entity Set Name**: `subscriptionHeadersFic`
**Page ID**: 81015

#### Purpose
Manages subscription contracts including billing information, service terms, and customer details.

#### CRUD Operations
- **Create**: ❌ Not allowed
- **Read**: ✅ Allowed
- **Update**: ❌ Not allowed
- **Delete**: ❌ Not allowed

#### Key Fields

| Field Name | Type | Description |
|---|---|---|
| id | GUID | System ID |
| customerNo | Code | End-user customer number |
| no | Code | Subscription number |
| billToCustomerNo | Code | Bill-to customer |
| billToName | Text | Bill-to name |
| billToName2 | Text | Bill-to name line 2 |
| billToAddress | Text | Bill-to address |
| billToAddress2 | Text | Bill-to address line 2 |
| billToCity | Text | Bill-to city |
| billToContact | Text | Bill-to contact |
| shipToCode | Code | Ship-to address code |
| shipToName | Text | Ship-to name |
| shipToName2 | Text | Ship-to name line 2 |
| shipToAddress | Text | Ship-to address |
| shipToAddress2 | Text | Ship-to address line 2 |
| shipToCity | Text | Ship-to city |
| shipToContact | Text | Ship-to contact |
| description | Text | Subscription description |
| serialNo | Code | Serial number |
| version | Integer | Version number |
| key | Code | Subscription key |
| provisionStartDate | Date | Start date |
| provisionEndDate | Date | End date |
| quantity | Decimal | Quantity |
| customerPriceGroup | Code | Customer price group |
| type | Option | Subscription type |
| sourceNo | Code | Source document number |
| createdInContractLine | Boolean | Created from contract line |
| customerName | Text | End-user customer name |
| customerName2 | Text | End-user customer name line 2 |
| address | Text | End-user address |
| address2 | Text | End-user address line 2 |
| city | Text | End-user city |
| contact | Text | End-user contact |
| customerReference | Code | Customer reference |
| variantCode | Code | Item variant code |
| plannedSubscriptionLinesExist | Boolean | Has planned lines |
| contactNo | Code | End-user contact number |
| unitOfMeasure | Code | Unit of measure |
| createdAt | DateTime | Creation timestamp |
| createdBy | GUID | Creator ID |
| modifiedAt | DateTime | Last modification timestamp |
| modifiedBy | GUID | Last modifier ID |

#### Related Resources
- **Subscription Lines**: Sub-collection via `subscriptionLinesFic` entity
- **Item Attributes**: Sub-collection via `itemAttributeValuesFic` entity
- **Subscription Attributes**: Sub-collection via `subscriptionAttributeValuesFic` entity

---

### Subscription Line API

**Entity Name**: `subscriptionLineFic`
**Entity Set Name**: `subscriptionLinesFic`
**Page ID**: 81016

#### Purpose
Provides read-only access to detailed subscription line items including pricing, billing terms, and renewal information.

#### CRUD Operations
- **Create**: ❌ Not allowed
- **Read**: ✅ Allowed
- **Update**: ❌ Not allowed
- **Delete**: ❌ Not allowed

#### Fields

| Field Name | Type | Description |
|---|---|---|
| id | GUID | System ID |
| subscriptionNo | Code | Parent subscription number |
| entryNo | Integer | Entry number |
| subscriptionPackageCode | Code | Package code |
| template | Code | Template code |
| description | Text | Line description |
| subscriptionLineStartDate | Date | Start date |
| subscriptionLineEndDate | Date | End date |
| nextBillingDate | Date | Next billing date |
| calculationBaseAmount | Decimal | Calculation base amount |
| calculationBasePercent | Decimal | Calculation base percentage |
| price | Decimal | Price |
| discountPercent | Decimal | Discount percentage |
| discountAmount | Decimal | Discount amount |
| amount | Decimal | Line amount |
| billingBasePeriod | Code | Billing period |
| invoicingVia | Option | Invoicing method |
| invoicingItemNo | Code | Invoicing item number |
| partner | Code | Partner |
| subscriptionContractNo | Code | Contract number |
| noticePeriod | Code | Notice period |
| initialTerm | Code | Initial term |
| subsequentTerm | Code | Subsequent/Extension term |
| billingRhythm | Code | Billing rhythm |
| cancellationPossibleUntil | Date | Cancellation deadline |
| termUntil | Date | Term end date |
| subscriptionCustomerNo | Code | Subscription customer |
| subscriptionContractLineNo | Integer | Contract line number |
| shortcutDimension1Code | Code | Dimension 1 code |
| shortcutDimension2Code | Code | Dimension 2 code |
| priceLCY | Decimal | Price in local currency |
| discountAmountLCY | Decimal | Discount amount (LCY) |
| amountLCY | Decimal | Amount (LCY) |
| currencyCode | Code | Currency code |
| currencyFactor | Decimal | Currency conversion factor |
| currencyFactorDate | Date | Currency factor date |
| calculationBaseAmountLCY | Decimal | Base amount (LCY) |
| discount | Decimal | Discount amount |
| quantity | Decimal | Quantity |
| createContractDeferrals | Boolean | Create deferrals |
| customerPriceGroup | Code | Customer price group |
| nextPriceUpdate | Date | Next price update date |
| excludeFromPriceUpdate | Boolean | Exclude from price updates |
| priceBindingPeriod | Code | Price binding period |
| periodCalculation | Option | Period calculation method |
| unitCost | Decimal | Unit cost |
| unitCostLCY | Decimal | Unit cost (LCY) |
| closed | Boolean | Closed status |
| renewalTerm | Code | Renewal term |
| selected | Boolean | Selected indicator |
| indent | Integer | Indentation level |
| dimensionSetID | Integer | Dimension set ID |
| usageBasedBilling | Boolean | Usage-based billing |
| usageBasedPricing | Boolean | Usage-based pricing |
| pricingUnitCostSurchargePercent | Decimal | Surcharge percentage |
| supplierReferenceEntryNo | Integer | Supplier reference entry |
| createdInContractLine | Boolean | Created from contract |
| sourceType | Option | Source type |
| sourceNo | Code | Source document number |
| subscriptionDescription | Text | Subscription description |

---

### Subscription Attribute Value API

**Entity Name**: `subscriptionAttributeValueFic`
**Entity Set Name**: `subscriptionAttributeValuesFic`
**Page ID**: 81018
**Source Table**: Item Attribute Value (Temporary)

#### Purpose
Provides read-only access to subscription attributes and product characteristics.

#### CRUD Operations
- **Create**: ❌ Not allowed
- **Read**: ✅ Allowed
- **Update**: ❌ Not allowed
- **Delete**: ❌ Not allowed

#### Fields

| Field Name | Type | Description |
|---|---|---|
| id | GUID | System ID |
| attributeId | Integer | Attribute identifier |
| attributeName | Text | Attribute name |
| value | Text | Attribute value |
| numericValue | Decimal | Numeric value |
| dateValue | Date | Date value |
| blocked | Boolean | Blocked status |
| primary | Boolean | Primary indicator |
| createdAt | DateTime | Creation timestamp |
| createdBy | GUID | Creator ID |
| modifiedAt | DateTime | Last modification timestamp |
| modifiedBy | GUID | Last modifier ID |

---

## UPR (Unpaid Reserves) APIs

### UPR Group API

**Entity Name**: `uPRGroupFic`
**Entity Set Name**: `uPRGroupsFic`
**Page ID**: 81008

#### Purpose
Manages Unpaid Reserves (UPR) groupings for customer categories and pricing strategies. Read-only API for group management.

#### CRUD Operations
- **Create**: ❌ Not allowed
- **Read**: ✅ Allowed
- **Update**: ❌ Not allowed
- **Delete**: ❌ Not allowed

#### Fields

| Field Name | Type | Description |
|---|---|---|
| code | Code[20] | UPR group code |
| name | Text[100] | Group name |
| noCustomers | Integer | Number of customers in group |
| noItemUPRLines | Integer | Number of item UPR lines |
| tvtfiCategory | Code[20] | TVTFI category |
| tvtfiSalespersonCode | Code[20] | Associated salesperson |
| systemCreatedAt | DateTime | Creation timestamp |
| systemCreatedBy | GUID | Creator ID |
| systemId | GUID | System ID |
| systemModifiedAt | DateTime | Last modification timestamp |
| systemModifiedBy | GUID | Last modifier ID |

---

### UPR Information API

**Entity Name**: `uPReserveFic`
**Entity Set Name**: `uPReservesFic`
**Page ID**: 81009

#### Purpose
Provides detailed information on unpaid reserves including quantity tracking, posting history, and assignment status.

#### CRUD Operations
- **Create**: ❌ Not allowed
- **Read**: ✅ Allowed
- **Update**: ❌ Not allowed
- **Delete**: ❌ Not allowed

#### Key Fields

| Field Name | Type | Description |
|---|---|---|
| no | Code[20] | UPR record number |
| uprGroupCode | Code[20] | UPR group reference |
| salespersonCode | Code[20] | Salesperson code |
| itemNo | Code[20] | Item number |
| quantityBase | Decimal | Quantity in base UOM |
| noSeries | Code[20] | Number series |
| qtyRemainingBase | Decimal | Remaining quantity (base) |
| quantity | Decimal | Quantity |
| quantityRemaining | Decimal | Quantity remaining |
| appliedQtyBase | Decimal | Applied quantity (base) |
| appliedQuantity | Decimal | Applied quantity |
| postedQuantity | Decimal | Posted quantity |
| postedQtyBase | Decimal | Posted quantity (base) |
| qtyOnSalesOrder | Decimal | Quantity on sales order |
| qtyOnSalesOrderBase | Decimal | Quantity on sales order (base) |
| qtyOnPostedShipmentBase | Decimal | Quantity on posted shipments (base) |
| createdDate | Date | Creation date |
| reviewDate | Date | Review date |
| assignedUserID | Code[50] | Assigned user ID |

---

## Address APIs

### Ship-to Address API

**Entity Name**: `shipToAddressFic`
**Entity Set Name**: `shipToAddressesFic`
**Page ID**: 81010

#### Purpose
Manages alternative shipping addresses for customers with multilingual support for Chinese markets.

#### CRUD Operations
- **Create**: ❌ Not allowed
- **Read**: ✅ Allowed
- **Update**: ❌ Not allowed
- **Delete**: ❌ Not allowed

#### Key Fields

| Field Name | Type | Description |
|---|---|---|
| systemId | GUID | System ID |
| customerNo | Code[20] | Related customer |
| customerId | GUID | Related customer system ID |
| code | Code[10] | Address code |
| name | Text[100] | Address name |
| name2 | Text[50] | Address name line 2 |
| address | Text[100] | Street address |
| address2 | Text[50] | Address line 2 |
| city | Text[30] | City |
| county | Text[30] | County/State |
| postCode | Text[20] | Post code |
| countryRegionCode | Code[10] | Country/Region code |
| contact | Text[100] | Contact person |
| eMail | Text[80] | Email address |
| homePage | Text[100] | Home page URL |
| faxNo | Text[30] | Fax number |
| gln | Text[13] | GLN (Global Location Number) |
| locationCode | Code[10] | Warehouse location |
| phoneNo | Text[30] | Phone number |
| placeOfExport | Code[10] | Export place code |
| salespersonCode | Code[20] | Salesperson code |
| shipmentMethodCode | Code[10] | Shipment method |
| shippingAgentCode | Code[10] | Shipping agent code |
| shippingAgentServiceCode | Code[10] | Shipping agent service |
| taxAreaCode | Code[20] | Tax area code |
| taxLiable | Boolean | Tax liable indicator |
| systemCreatedAt | DateTime | Creation timestamp |
| systemModifiedAt | DateTime | Last modification timestamp |

#### Multilingual Fields (Simplified Chinese)
- tvtfiChineseNameSC: Chinese name
- tvtfiAddressSC: Address (Simplified Chinese)
- tvtfiAddress2SC: Address 2 (Simplified Chinese)
- tvtfiCitySC: City (Simplified Chinese)
- tvtfiCountySC: County (Simplified Chinese)
- tvtfiCountryRegionCodeSC: Country code (Simplified Chinese)
- tvtfiPostCodeSC: Post code (Simplified Chinese)

#### Multilingual Fields (Traditional Chinese)
- tvtfiChineseNameTC: Chinese name (Traditional)
- tvtfiAddressTC: Address (Traditional Chinese)
- tvtfiAddress2TC: Address 2 (Traditional Chinese)
- tvtfiCityTC: City (Traditional Chinese)
- tvtfiCountyTC: County (Traditional Chinese)
- tvtfiCountryRegionCodeTC: Country code (Traditional Chinese)
- tvtfiPostCodeTC: Post code (Traditional Chinese)

---

### Payment Address API

**Entity Name**: `paymentAddressFic`
**Entity Set Name**: `paymentAddressesFic`
**Page ID**: 81002

#### Purpose
Manages alternative payment addresses for customers and vendors with multilingual support.

#### CRUD Operations
- **Create**: ✅ Allowed
- **Read**: ✅ Allowed
- **Update**: ✅ Allowed (via DelayedInsert)
- **Delete**: ❌ Not allowed

#### Key Fields

| Field Name | Type | Description | Writable |
|---|---|---|---|
| id | GUID | System ID | ❌ |
| accountType | Code[10] | Customer or Vendor | ✅ |
| accountNo | Code[20] | Account number | ✅ |
| code | Code[10] | Address code | ✅ |
| name | Text[100] | Address name | ✅ |
| name2 | Text[50] | Name line 2 | ✅ |
| address | Text[100] | Street address | ✅ |
| address2 | Text[50] | Address line 2 | ✅ |
| city | Text[30] | City | ✅ |
| contact | Text[100] | Contact person | ✅ |

#### Multilingual Fields (Simplified Chinese)
- tvtfiChineseNameSC: Chinese name
- tvtfiAddressSC: Address (SC)
- tvtfiAddress2SC: Address 2 (SC)
- tvtfiCitySC: City (SC)
- tvtfiCountryRegionCodeSC: Country code (SC)

---

## API Version Support

- **Current Version**: v2.0 (Primary)
- **Legacy Version**: v1.0 (UPR APIs only)

All APIs use OData protocol for RESTful access. The base URL follows the pattern:
```
/api/{APIPublisher}/{APIGroup}/{APIVersion}/{EntitySetName}
```

Example:
```
https://[server]/api/tvisiontech/ficofiweb/v2.0/salesHeadersFic
```

---

## Authentication & Authorization

All API endpoints require appropriate Business Central user authentication. Users must have the necessary permission sets assigned to access specific API entities.

---

## Last Updated

March 26, 2026
