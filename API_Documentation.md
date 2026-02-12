# API Documentation

> Generated from the provided metadata file. Navigation properties that do not include a `Partner` attribute were omitted as requested.

---

## entityMetadata (EntitySet: entityDefinitions)

### Operations
Not specified in metadata (no Insert/Update/Delete annotations present).

### Fields
- entityName — string (Edm.String, Nullable=false)
- entitySetName — string (Edm.String)
- entityCaptions — Collection(Microsoft.NAV.entityMetadataLabel)
- entitySetCaptions — Collection(Microsoft.NAV.entityMetadataLabel)
- properties — Collection(Microsoft.NAV.entityMetadataField)
- actions — Collection(Microsoft.NAV.entityMetadataAction)
- enumMembers — Collection(Microsoft.NAV.entityMetadataEnumMember)

---

## company (EntitySet: companies)

### Operations
Read-only (Insertable=false, Updatable=false, Deletable=false).

### Fields
- id — Guid (Edm.Guid, Nullable=false)
- systemVersion — string (Edm.String)
- timestamp — Int64 (Edm.Int64)
- name — string (Edm.String, MaxLength=30)
- displayName — string (Edm.String, MaxLength=250)
- businessProfileId — string (Edm.String, MaxLength=250)
- systemCreatedAt — DateTimeOffset (Edm.DateTimeOffset)
- systemCreatedBy — Guid (Edm.Guid)
- systemModifiedAt — DateTimeOffset (Edm.DateTimeOffset)
- systemModifiedBy — Guid (Edm.Guid)

---

## subscriptions (EntitySet: subscriptions)

### Operations
Create, Read, Update, Delete (Insertable=true, Updatable=true, Deletable=true).

### Fields
- subscriptionId — string (Edm.String, Nullable=false, MaxLength=150)
- notificationUrl — string (Edm.String, Nullable=false)
- resource — string (Edm.String, Nullable=false)
- timestamp — Int64 (Edm.Int64)
- userId — Guid (Edm.Guid)
- lastModifiedDateTime — DateTimeOffset (Edm.DateTimeOffset)
- clientState — string (Edm.String, MaxLength=2048)
- expirationDateTime — DateTimeOffset (Edm.DateTimeOffset)
- systemCreatedAt — DateTimeOffset (Edm.DateTimeOffset)
- systemCreatedBy — Guid (Edm.Guid)
- systemModifiedAt — DateTimeOffset (Edm.DateTimeOffset)
- systemModifiedBy — Guid (Edm.Guid)

---

## externaleventsubscriptions (EntitySet: externaleventsubscriptions)

### Operations
Create, Read, Update, Delete (Insertable=true, Updatable=true, Deletable=true).

### Fields
- id — Guid (Edm.Guid, Nullable=false)
- companyId — Guid (Edm.Guid)
- timestamp — Int64 (Edm.Int64)
- appId — Guid (Edm.Guid)
- eventName — string (Edm.String, MaxLength=80)
- companyName — string (Edm.String, MaxLength=30)
- userId — Guid (Edm.Guid)
- notificationUrl — string (Edm.String, MaxLength=2048)
- lastModifiedDateTime — DateTimeOffset (Edm.DateTimeOffset)
- clientState — string (Edm.String, MaxLength=2048)
- subscriptionType — string (Edm.String)
- eventVersion — string (Edm.String, MaxLength=43)
- subscriptionState — string (Edm.String)
- systemCreatedAt — DateTimeOffset (Edm.DateTimeOffset)
- systemCreatedBy — Guid (Edm.Guid)
- systemModifiedAt — DateTimeOffset (Edm.DateTimeOffset)
- systemModifiedBy — Guid (Edm.Guid)

---

## externalbusinesseventdefinitions (EntitySet: externalbusinesseventdefinitions)

### Operations
Read-only (Insertable=false, Updatable=false, Deletable=false).

### Fields
- appId — Guid (Edm.Guid, Nullable=false)
- name — string (Edm.String, Nullable=false, MaxLength=80)
- eventVersion — string (Edm.String, Nullable=false, MaxLength=43)
- isObsolete — boolean (Edm.Boolean)
- obsoleteReason — string (Edm.String, MaxLength=250)
- obsoleteTag — string (Edm.String, MaxLength=43)
- payload — string (Edm.String, Nullable=false)
- displayName — string (Edm.String, MaxLength=250)
- description — string (Edm.String, MaxLength=1024)
- category — string (Edm.String, MaxLength=250)
- appName — string (Edm.String, MaxLength=250)
- appPublisher — string (Edm.String, MaxLength=250)
- appVersion — string (Edm.String, MaxLength=43)

---

## apicategoryroutes (EntitySet: apicategoryroutes)

### Operations
Read-only (Insertable=false, Updatable=false, Deletable=false).

### Fields
- route — string (Edm.String, Nullable=false)
- publisher — string (Edm.String, MaxLength=40)
- group — string (Edm.String, MaxLength=40)
- version — string (Edm.String, MaxLength=10)

---

## salesHeaderFic (EntitySet: salesHeadersFic)

### Operations
Read-only (Insertable=false, Updatable=false, Deletable=false).

### Fields
- id — Guid (Edm.Guid, Nullable=false)
- documentType — Microsoft.NAV.salesDocumentType
- no — string (Edm.String, MaxLength=20)
- status — Microsoft.NAV.salesDocumentStatus
- sellToCustomerNo — string (Edm.String, MaxLength=20)
- sellToCustomerId — Guid (Edm.Guid)
- sellToCustomerName — string (Edm.String, MaxLength=100)
- billToCustomerNo — string (Edm.String, Nullable=false, MaxLength=20)
- billToCustomerId — Guid (Edm.Guid)
- billToName — string (Edm.String, MaxLength=100)
- shipToCode — string (Edm.String, MaxLength=10)
- documentDate — Date (Edm.Date)
- externalDocumentNo — string (Edm.String, MaxLength=35)
- salespersonCode — string (Edm.String, MaxLength=20)
- currencyCode — string (Edm.String, MaxLength=10)
- pricesIncludingVAT — boolean (Edm.Boolean)
- paymentTermsCode — string (Edm.String, MaxLength=10)
- paymentMethodCode — string (Edm.String, MaxLength=10)
- shipmentMethodCode — string (Edm.String, MaxLength=10)
- locationCode — string (Edm.String, MaxLength=10)
- campaignNo — string (Edm.String, MaxLength=20)
- campaignId — Guid (Edm.Guid)
- opportunityNo — string (Edm.String, MaxLength=20)
- tvtDutyStatus — string (Edm.String, MaxLength=10)
- tvtOrderSource — string (Edm.String, MaxLength=20)
- tvtbeReservationStatus — Microsoft.NAV.tvtbeReservationStatus
- tvtfiOfferType — string (Edm.String, MaxLength=20)
- tvtfiOfferTag — string (Edm.String, MaxLength=20)
- tvtfiOfferID — string (Edm.String, MaxLength=20)
- tvtfiOfferDescription — string (Edm.String, MaxLength=250)
- tvtfiOfferStatus — string (Edm.String, MaxLength=20)
- tvtfiMemberServiceManager — string (Edm.String, MaxLength=20)
- tvtfiExpDeliveryInstr — Microsoft.NAV.tvtExpDeliveryInstr
- tvtfiExpDeliveryShipTo — string (Edm.String, MaxLength=10)
- tvtfiExpDeliveryLocCode — string (Edm.String, MaxLength=10)
- quoteValidUntilDate — Date (Edm.Date)
- quoteAcceptedDate — Date (Edm.Date)
- systemModifiedAt — DateTimeOffset (Edm.DateTimeOffset)

### Partnered Navigations
- salesLinesFic — Collection(Microsoft.NAV.salesLineFic), Partner="salesHeaderFic" (ReferentialConstraint: id -> documentId)

---

## salesLineFic (EntitySet: salesLinesFic)

### Operations
Read + Update allowed (Insertable=false, Updatable=true, Deletable=false).

### Fields
- id — Guid (Edm.Guid, Nullable=false)
- documentId — Guid (Edm.Guid)
- documentNo — string (Edm.String, MaxLength=20)
- lineNo — Int32 (Edm.Int32)
- type — Microsoft.NAV.salesLineType
- no — string (Edm.String, MaxLength=20)
- description — string (Edm.String, MaxLength=100)
- quantity — decimal (Edm.Decimal, Scale=Variable)
- unitOfMeasureCode — string (Edm.String, MaxLength=10)
- unitPrice — decimal (Edm.Decimal, Scale=Variable)
- lineDiscountPerc — decimal (Edm.Decimal, Scale=Variable)
- lineAmount — decimal (Edm.Decimal, Scale=Variable)
- tvtfiExpDeliveryShipTo — string (Edm.String, MaxLength=10)
- tvtfiOfferQuantity — decimal (Edm.Decimal, Scale=Variable)
- tvtfiMaximumQuantity — decimal (Edm.Decimal, Scale=Variable)
- tvtfiSoftReservedQtySR — decimal (Edm.Decimal, Scale=Variable)

### Partnered Navigations
- salesHeaderFic — Microsoft.NAV.salesHeaderFic, Partner="salesLinesFic" (ReferentialConstraint: documentId -> id)

---

## paymentAddressFic (EntitySet: paymentAddressesFic)

### Operations
Create, Read, Update, Delete (Insertable=true, Updatable=true, Deletable=true).

### Fields
- id — Guid (Edm.Guid, Nullable=false)
- accountType — Microsoft.NAV.genJournalAccountType
- accountNo — string (Edm.String, MaxLength=20)
- code — string (Edm.String, MaxLength=10)
- name — string (Edm.String, MaxLength=50)
- name2 — string (Edm.String, MaxLength=50)
- address — string (Edm.String, MaxLength=50)
- address2 — string (Edm.String, MaxLength=50)
- city — string (Edm.String, MaxLength=30)
- contact — string (Edm.String, MaxLength=50)
- tvtfiChineseNameSC — string (Edm.String, MaxLength=100)
- tvtfiAddressSC — string (Edm.String, MaxLength=100)
- tvtfiAddress2SC — string (Edm.String, MaxLength=50)
- tvtfiCitySC — string (Edm.String, MaxLength=30)
- tvtfiCountryRegionCodeSC — string (Edm.String, MaxLength=10)
- tvtfiCountySC — string (Edm.String, MaxLength=30)
- tvtfiPostCodeSC — string (Edm.String, MaxLength=100)
- tvtfiChineseNameTC — string (Edm.String, MaxLength=100)
- tvtfiAddressTC — string (Edm.String, MaxLength=100)
- tvtfiAddress2TC — string (Edm.String, MaxLength=50)
- tvtfiCityTC — string (Edm.String, MaxLength=30)
- tvtfiCountryRegionCodeTC — string (Edm.String, MaxLength=10)
- tvtfiCountyTC — string (Edm.String, MaxLength=30)
- tvtfiPostCodeTC — string (Edm.String, MaxLength=100)
- systemCreatedAt — DateTimeOffset (Edm.DateTimeOffset)
- systemModifiedAt — DateTimeOffset (Edm.DateTimeOffset)

---

## customerFic (EntitySet: customersFic)

### Operations
Create, Read, Update (Insertable=true, Updatable=true, Deletable=false).

### Fields
- systemId — Guid (Edm.Guid, Nullable=false)
- address — string (Edm.String, MaxLength=100)
- address2 — string (Edm.String, MaxLength=50)
- applicationMethod — Microsoft.NAV.applicationMethod
- billToCustomerNo — string (Edm.String, MaxLength=20)
- blocked — Microsoft.NAV.customerBlocked
- chainName — string (Edm.String, MaxLength=10)
- city — string (Edm.String, MaxLength=30)
- collectionMethod — string (Edm.String, MaxLength=20)
- combineShipments — boolean (Edm.Boolean)
- contact — string (Edm.String, MaxLength=100)
- contactType — Microsoft.NAV.contactType
- countryRegionCode — string (Edm.String, MaxLength=10)
- county — string (Edm.String, MaxLength=30)
- creditLimitLCY — decimal (Edm.Decimal, Scale=Variable)
- currencyCode — string (Edm.String, MaxLength=10)
- currencyId — Guid (Edm.Guid)
- customerDiscGroup — string (Edm.String, MaxLength=20)
- customerPostingGroup — string (Edm.String, MaxLength=20)
- customerPriceGroup — string (Edm.String, MaxLength=10)
- eMail — string (Edm.String, MaxLength=80)
- eoriNumber — string (Edm.String, MaxLength=40)
- faxNo — string (Edm.String, MaxLength=30)
- finChargeMemoAmountsLCY — decimal (Edm.Decimal, Scale=Variable)
- finChargeTermsCode — string (Edm.String, MaxLength=10)
- formatRegion — string (Edm.String, MaxLength=80)
- gln — string (Edm.String, MaxLength=13)
- genBusPostingGroup — string (Edm.String, MaxLength=20)
- globalDimension1Code — string (Edm.String, MaxLength=20)
- globalDimension2Code — string (Edm.String, MaxLength=20)
- homePage — string (Edm.String, MaxLength=255)
- icPartnerCode — string (Edm.String, MaxLength=20)
- image — Guid (Edm.Guid)
- invoiceCopies — Int32 (Edm.Int32)
- invoiceDiscCode — string (Edm.String, MaxLength=20)
- languageCode — string (Edm.String, MaxLength=10)
- lastStatementNo — Int32 (Edm.Int32)
- locationCode — string (Edm.String, MaxLength=10)
- mobilePhoneNo — string (Edm.String, MaxLength=30)
- name — string (Edm.String, MaxLength=100)
- name2 — string (Edm.String, MaxLength=50)
- no — string (Edm.String, MaxLength=20)
- ourAccountNo — string (Edm.String, MaxLength=20)
- partnerType — Microsoft.NAV.partnerType
- paymentMethodCode — string (Edm.String, MaxLength=10)
- paymentMethodId — Guid (Edm.Guid)
- paymentTermsCode — string (Edm.String, MaxLength=10)
- paymentTermsId — Guid (Edm.Guid)
- phoneNo — string (Edm.String, MaxLength=30)
- placeOfExport — string (Edm.String, MaxLength=20)
- postCode — string (Edm.String, MaxLength=20)
- preferredBankAccountCode — string (Edm.String, MaxLength=20)
- pricesIncludingVAT — boolean (Edm.Boolean)
- primaryContactNo — string (Edm.String, MaxLength=20)
- printStatements — boolean (Edm.Boolean)
- privacyBlocked — boolean (Edm.Boolean)
- registrationNumber — string (Edm.String, MaxLength=50)
- responsibilityCenter — string (Edm.String, MaxLength=10)
- salespersonCode — string (Edm.String, MaxLength=20)
- searchName — string (Edm.String, MaxLength=100)
- shipToCode — string (Edm.String, MaxLength=10)
- shipmentMethodCode — string (Edm.String, MaxLength=10)
- shipmentMethodId — Guid (Edm.Guid)
- shippingAdvice — Microsoft.NAV.salesHeaderShippingAdvice
- shippingAgentCode — string (Edm.String, MaxLength=10)
- shippingAgentServiceCode — string (Edm.String, MaxLength=10)
- shippingTime — string (Edm.String)
- systemCreatedAt — DateTimeOffset (Edm.DateTimeOffset)
- systemCreatedBy — Guid (Edm.Guid)
- systemModifiedAt — DateTimeOffset (Edm.DateTimeOffset)
- systemModifiedBy — Guid (Edm.Guid)
- tvtAWRSDate — Date (Edm.Date)
- tvtAWRSURN — string (Edm.String, MaxLength=20)
- tvtContraVendorNo — string (Edm.String, MaxLength=20)
- tvtCreditStatus — string (Edm.String)
- taxAreaCode — string (Edm.String, MaxLength=20)
- taxAreaID — Guid (Edm.Guid)
- taxLiable — boolean (Edm.Boolean)
- telexAnswerBack — string (Edm.String, MaxLength=20)
- telexNo — string (Edm.String, MaxLength=20)
- territoryCode — string (Edm.String, MaxLength=10)
- vatBusPostingGroup — string (Edm.String, MaxLength=20)
- vatRegistrationNo — string (Edm.String, MaxLength=20)
- bssiIcEntity — string (Edm.String, MaxLength=20)
- bssiDefaultIcbEntity — string (Edm.String, MaxLength=20)
- bssiMapNo — string (Edm.String, MaxLength=20)
- bssiMasterSecurity — boolean (Edm.Boolean)
- bssiMasterSecurityFilter — string (Edm.String, MaxLength=20)
- memberServiceManager — string (Edm.String, MaxLength=20)

---

## contactFic (EntitySet: contactsFic)

### Operations
Create, Read, Update (Insertable=true, Updatable=true, Deletable=false).

### Fields
- systemId — Guid (Edm.Guid, Nullable=false)
- no — string (Edm.String, MaxLength=20)
- name — string (Edm.String, MaxLength=100)
- name2 — string (Edm.String, MaxLength=50)
- address — string (Edm.String, MaxLength=100)
- address2 — string (Edm.String, MaxLength=50)
- city — string (Edm.String, MaxLength=30)
- postCode — string (Edm.String, MaxLength=20)
- county — string (Edm.String, MaxLength=30)
- countryRegionCode — string (Edm.String, MaxLength=10)
- phoneNo — string (Edm.String, MaxLength=30)
- mobilePhoneNo — string (Edm.String, MaxLength=30)
- eMail — string (Edm.String, MaxLength=80)
- faxNo — string (Edm.String, MaxLength=30)
- telexNo — string (Edm.String, MaxLength=20)
- telexAnswerBack — string (Edm.String, MaxLength=20)
- languageCode — string (Edm.String, MaxLength=10)
- salespersonCode — string (Edm.String, MaxLength=20)
- territoryCode — string (Edm.String, MaxLength=10)
- currencyCode — string (Edm.String, MaxLength=10)
- searchName — string (Edm.String, MaxLength=100)
- type — Microsoft.NAV.contactType
- companyNo — string (Edm.String, MaxLength=20)
- companyName — string (Edm.String, MaxLength=100)
- firstName — string (Edm.String, MaxLength=30)
- middleName — string (Edm.String, MaxLength=30)
- surname — string (Edm.String, MaxLength=30)
- jobTitle — string (Edm.String, MaxLength=30)
- initials — string (Edm.String, MaxLength=30)
- extensionNo — string (Edm.String, MaxLength=30)
- organizationalLevelCode — string (Edm.String, MaxLength=10)
- excludeFromSegment — boolean (Edm.Boolean)
- dateFilter — string (Edm.String)
- nextTaskDate — Date (Edm.Date)
- lastDateAttempted — Date (Edm.Date)
- dateOfLastInteraction — Date (Edm.Date)
- noOfJobResponsibilities — Int32 (Edm.Int32)
- noOfIndustryGroups — Int32 (Edm.Int32)
- noOfBusinessRelations — Int32 (Edm.Int32)
- noOfMailingGroups — Int32 (Edm.Int32)
- externalId — string (Edm.String, MaxLength=20)
- noOfInteractions — Int32 (Edm.Int32)
- costLcy — decimal (Edm.Decimal, Scale=Variable)
- durationMin — decimal (Edm.Decimal, Scale=Variable)
- noOfOpportunities — Int32 (Edm.Int32)
- estimatedValueLcy — decimal (Edm.Decimal, Scale=Variable)
- calcedCurrentValueLcy — decimal (Edm.Decimal, Scale=Variable)
- opportunityEntryExists — boolean (Edm.Boolean)
- taskEntryExists — boolean (Edm.Boolean)
- salespersonFilter — string (Edm.String, MaxLength=20)
- campaignFilter — string (Edm.String, MaxLength=20)
- actionTakenFilter — string (Edm.String)
- salesCycleFilter — string (Edm.String, MaxLength=10)
- salesCycleStageFilter — string (Edm.String)
- probabilityFilter — string (Edm.String)
- completedFilter — string (Edm.String)
- estimatedValueFilter — string (Edm.String)
- calcedCurrentValueFilter — string (Edm.String)
- chancesOfSuccessFilter — string (Edm.String)
- taskStatusFilter — string (Edm.String)
- taskClosedFilter — string (Edm.String)
- priorityFilter — string (Edm.String)
- teamFilter — string (Edm.String, MaxLength=10)
- correspondenceType — Microsoft.NAV.correspondenceType
- vatRegistrationNo — string (Edm.String, MaxLength=20)
- image — Guid (Edm.Guid)
- privacyBlocked — boolean (Edm.Boolean)
- parentalConsentReceived — boolean (Edm.Boolean)
- registrationNumber — string (Edm.String, MaxLength=50)
- lookupContactNo — string (Edm.String, MaxLength=20)
- xrmId — Guid (Edm.Guid)
- systemCreatedAt — DateTimeOffset (Edm.DateTimeOffset)
- systemCreatedBy — Guid (Edm.Guid)
- systemModifiedAt — DateTimeOffset (Edm.DateTimeOffset)
- systemModifiedBy — Guid (Edm.Guid)

---

## itemFic (EntitySet: itemsFic)

### Operations
Create, Read, Update (Insertable=true, Updatable=true, Deletable=false).

### Fields
- systemId — Guid (Edm.Guid, Nullable=false)
- no — string (Edm.String, MaxLength=20)
- description — string (Edm.String, MaxLength=100)
- type — Microsoft.NAV.itemType
- tvtMarketingDescription — string (Edm.String, MaxLength=250)
- baseUnitOfMeasure — string (Edm.String, MaxLength=10)
- countryRegionOfOriginCode — string (Edm.String, MaxLength=10)
- tvtRegionCode — string (Edm.String, MaxLength=20)
- tvtSubRegionCode — string (Edm.String, MaxLength=20)
- tvtAppellationCode — string (Edm.String, MaxLength=20)
- tvtBrandCode — string (Edm.String, MaxLength=20)
- tvtClassificationCode — string (Edm.String, MaxLength=20)
- tvtStyleCode — string (Edm.String, MaxLength=20)
- tvtVintage — string (Edm.String, MaxLength=10)
- systemCreatedAt — DateTimeOffset (Edm.DateTimeOffset)
- systemCreatedBy — Guid (Edm.Guid)
- systemModifiedAt — DateTimeOffset (Edm.DateTimeOffset)
- systemModifiedBy — Guid (Edm.Guid)

---

## itemUnitOfMeasureFic (EntitySet: itemsUnitOfMeasureFic)

### Operations
Create, Read, Update (Insertable=true, Updatable=true, Deletable=false).

### Fields
- systemId — Guid (Edm.Guid, Nullable=false)
- itemNo — string (Edm.String, Nullable=false, MaxLength=20)
- code — string (Edm.String, Nullable=false, MaxLength=10)
- systemCreatedAt — DateTimeOffset (Edm.DateTimeOffset)
- systemCreatedBy — Guid (Edm.Guid)
- systemModifiedAt — DateTimeOffset (Edm.DateTimeOffset)
- systemModifiedBy — Guid (Edm.Guid)

---

## itemTranslationFic (EntitySet: itemsTranslationFic)

### Operations
Create, Read, Update (Insertable=true, Updatable=true, Deletable=false).

### Fields
- systemId — Guid (Edm.Guid, Nullable=false)
- itemNo — string (Edm.String, Nullable=false, MaxLength=20)
- description — string (Edm.String, MaxLength=100)
- languageCode — string (Edm.String, Nullable=false, MaxLength=10)
- tvtfiMarketingDescription — string (Edm.String, MaxLength=250)
- systemCreatedAt — DateTimeOffset (Edm.DateTimeOffset)
- systemCreatedBy — Guid (Edm.Guid)
- systemModifiedAt — DateTimeOffset (Edm.DateTimeOffset)
- systemModifiedBy — Guid (Edm.Guid)

---

## uPRGroupFic (EntitySet: uPRGroupsFic)

### Operations
Read-only (Insertable=false, Updatable=false, Deletable=false).

### Fields
- code — string (Edm.String, Nullable=false, MaxLength=20)
- name — string (Edm.String, MaxLength=100)
- noCustomers — Int32 (Edm.Int32)
- noItemUPRLines — Int32 (Edm.Int32)
- tvtfiCategory — Microsoft.NAV.tvtfiuprGroups
- tvtfiSalespersonCode — string (Edm.String, MaxLength=20)
- systemCreatedAt — DateTimeOffset (Edm.DateTimeOffset)
- systemCreatedBy — Guid (Edm.Guid)
- systemId — Guid (Edm.Guid)
- systemModifiedAt — DateTimeOffset (Edm.DateTimeOffset)
- systemModifiedBy — Guid (Edm.Guid)

---

## uPReserveFic (EntitySet: uPReservesFic)

### Operations
Read-only (Insertable=false, Updatable=false, Deletable=false).

### Fields
- no — string (Edm.String, Nullable=false, MaxLength=20)
- uprGroupCode — string (Edm.String, MaxLength=20)
- salespersonCode — string (Edm.String)
- itemNo — string (Edm.String, MaxLength=20)
- quantityBase — decimal (Edm.Decimal, Scale=Variable)
- noSeries — string (Edm.String, MaxLength=20)
- qtyRemainingBase — decimal (Edm.Decimal, Scale=Variable)
- quantity — decimal (Edm.Decimal, Scale=Variable)
- quantityRemaining — decimal (Edm.Decimal, Scale=Variable)
- appliedQtyBase — decimal (Edm.Decimal, Scale=Variable)
- appliedQuantity — decimal (Edm.Decimal, Scale=Variable)
- postedQuantity — decimal (Edm.Decimal, Scale=Variable)
- postedQtyBase — decimal (Edm.Decimal, Scale=Variable)
- qtyOnSalesOrder — decimal (Edm.Decimal, Scale=Variable)
- qtyOnSalesOrderBase — decimal (Edm.Decimal, Scale=Variable)
- qtyOnPostedShipmentBase — decimal (Edm.Decimal, Scale=Variable)
- createdDate — Date (Edm.Date)
- reviewDate — Date (Edm.Date)
- assignedUserID — string (Edm.String, MaxLength=50)
- note — string (Edm.String, MaxLength=100)
- itemDescription — string (Edm.String, MaxLength=100)
- uprGroupName — string (Edm.String, MaxLength=100)
- tvtfiUPRCategory — Microsoft.NAV.tvtfiuprGroups
- tvtfiSalesLineId — Guid (Edm.Guid)
- tvtfiSalesQuoteNo — string (Edm.String, MaxLength=20)
- tvtfiSalesQuoteLineNo — Int32 (Edm.Int32)
- tvtfiOfferID — string (Edm.String, MaxLength=20)
- systemCreatedAt — DateTimeOffset (Edm.DateTimeOffset)
- systemCreatedBy — Guid (Edm.Guid)
- systemId — Guid (Edm.Guid)
- systemModifiedAt — DateTimeOffset (Edm.DateTimeOffset)
- systemModifiedBy — Guid (Edm.Guid)

---

## shipToAddressFic (EntitySet: shipToAddressesFic)

### Operations
Read-only (Insertable=false, Updatable=false, Deletable=false).

### Fields
- systemId — Guid (Edm.Guid, Nullable=false)
- customerNo — string (Edm.String, Nullable=false, MaxLength=20)
- customerId — Guid (Edm.Guid)
- code — string (Edm.String, Nullable=false, MaxLength=10)
- name — string (Edm.String, MaxLength=100)
- name2 — string (Edm.String, MaxLength=50)
- address — string (Edm.String, MaxLength=100)
- address2 — string (Edm.String, MaxLength=50)
- city — string (Edm.String, MaxLength=30)
- postCode — string (Edm.String, MaxLength=20)
- countryRegionCode — string (Edm.String, MaxLength=10)
- county — string (Edm.String, MaxLength=30)
- contact — string (Edm.String, MaxLength=100)
- eMail — string (Edm.String, MaxLength=80)
- homePage — string (Edm.String, MaxLength=255)
- faxNo — string (Edm.String, MaxLength=30)
- gln — string (Edm.String, MaxLength=13)
- locationCode — string (Edm.String, MaxLength=10)
- phoneNo — string (Edm.String, MaxLength=30)
- placeOfExport — string (Edm.String, MaxLength=20)
- salespersonCode — string (Edm.String, MaxLength=20)
- shipmentMethodCode — string (Edm.String, MaxLength=10)
- shippingAgentCode — string (Edm.String, MaxLength=10)
- shippingAgentServiceCode — string (Edm.String, MaxLength=10)
- taxAreaCode — string (Edm.String, MaxLength=20)
- taxLiable — boolean (Edm.Boolean)
- systemCreatedAt — DateTimeOffset (Edm.DateTimeOffset)
- systemModifiedAt — DateTimeOffset (Edm.DateTimeOffset)

---

## salesInvoiceHeaderFic (EntitySet: salesInvoiceHeadersFic)

### Operations
Read-only (Insertable=false, Updatable=false, Deletable=false).

### Fields
- id — Guid (Edm.Guid, Nullable=false)
- no — string (Edm.String, MaxLength=20)
- sellToCustomerNo — string (Edm.String, Nullable=false, MaxLength=20)
- sellToCustomerId — Guid (Edm.Guid)
- sellToCustomerName — string (Edm.String, MaxLength=100)
- billToCustomerNo — string (Edm.String, Nullable=false, MaxLength=20)
- billToCustomerId — Guid (Edm.Guid)
- billToName — string (Edm.String, MaxLength=100)
- shipToCode — string (Edm.String, MaxLength=10)
- documentDate — Date (Edm.Date)
- externalDocumentNo — string (Edm.String, MaxLength=35)
- salespersonCode — string (Edm.String, MaxLength=20)
- currencyCode — string (Edm.String, MaxLength=10)
- pricesIncludingVAT — boolean (Edm.Boolean)
- paymentTermsCode — string (Edm.String, MaxLength=10)
- paymentMethodCode — string (Edm.String, MaxLength=10)
- shipmentMethodCode — string (Edm.String, MaxLength=10)
- locationCode — string (Edm.String, MaxLength=10)
- campaignNo — string (Edm.String, MaxLength=20)
- campaignId — Guid (Edm.Guid)
- opportunityNo — string (Edm.String, MaxLength=20)
- tvtDutyStatus — string (Edm.String, MaxLength=10)
- tvtOrderSource — string (Edm.String, MaxLength=20)
- tvtbeReservationStatus — Microsoft.NAV.tvtbeReservationStatus
- tvtfiOfferType — string (Edm.String, MaxLength=20)
- tvtfiOfferID — string (Edm.String, MaxLength=20)
- tvtfiOfferDescription — string (Edm.String, MaxLength=250)
- tvtfiOfferStatus — string (Edm.String, MaxLength=20)
- tvtfiMemberServiceManager — string (Edm.String, MaxLength=20)
- tvtfiExpDeliveryInstr — Microsoft.NAV.tvtExpDeliveryInstr
- tvtfiExpDeliveryShipTo — string (Edm.String, MaxLength=10)
- tvtfiExpDeliveryLocCode — string (Edm.String, MaxLength=10)
- systemModifiedAt — DateTimeOffset (Edm.DateTimeOffset)

---

## salesInvoiceLineFic (EntitySet: salesInvoiceLinesFic)

### Operations
Read-only (Insertable=false, Updatable=false, Deletable=false).

### Fields
- id — Guid (Edm.Guid, Nullable=false)
- documentNo — string (Edm.String, MaxLength=20)
- lineNo — Int32 (Edm.Int32)
- type — Microsoft.NAV.salesLineType
- no — string (Edm.String, MaxLength=20)
- description — string (Edm.String, MaxLength=100)
- quantity — decimal (Edm.Decimal, Scale=Variable)
- unitOfMeasureCode — string (Edm.String, MaxLength=10)
- unitPrice — decimal (Edm.Decimal, Scale=Variable)
- lineDiscountPerc — decimal (Edm.Decimal, Scale=Variable)
- lineAmount — decimal (Edm.Decimal, Scale=Variable)
- tvtfiExpDeliveryShipTo — string (Edm.String, MaxLength=10)

---

> End of documentation. If a different filename or format is required, request the change.
