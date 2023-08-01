# Missing Customer Asset

## Steps to Recreate Missing Asset

  Create Contract records first, then all top-level bundle or standalone subscriptions, and then the children of those subscriptions.

### Contract

Need to verify the below fields if these are properly populated
| Field API Name | Type | Notes|
| ----------- | ----------- | --------- |
| AccountId | Lookup(Account) | Lookup to the Contract's parent Account
| StartDate | Date | Contract Start Date
| ContractTerm | Number(4, 0) | Contract Term in months
| Status | Picklist | Contracts cannot initially load as Activated. Update to Activated after the initial insert
| SBQQ__RenewalPricebookId__c | Text(18) | 18-digit SF ID of the price book referenced during contract renewal
| SBQQ__AmendmentPricebookId__c| Text(18)| 18-digit SF ID of the price book referenced during contract amendment
| SBQQ__AmendmentRenewalBehavior__c |Picklist| Either ‘Latest End Date’ (CPQ Default) or ‘Earliest End Date’.The basis for which a Renewal Start Date and Amendment End Date are calculated based on the dates of the contracts subscriptions.

### Subscriptions (standalone or top level of a bundle)

Need to verify the below fields if these are properly populated
| Field API Name | Type | Notes|
| ----------- | ----------- | --------- |
| SBQQ__Account__c | Lookup(Account)| Lookup to the Subscription's parent Account
| SBQQ__Contract__c| Lookup(Contract) | Lookup to the parent Contract record
| SBQQ__RootId__c | Text (18) | 18 Digit SFID of bundle parent's Asset or Subscription record
| SBQQ__Product__c | Lookup(Product) | Lookup to the related Product record
| SBQQ__Number__c| Number(5, 0)| Any number can be used but the field dictates the order in which this product is listed in the quote line editor.  This field is required if SBQQ__QuoteLine__c lookup is not populated
| SBQQ__Quantity__c | Number(10, 2)| The quantity of the subscription
| SBQQ__RenewalQuantity__c| Number(10, 2)| The quantity of the subscription upon renewal. Usually the same as SBQQ__Quantity__c
| SBQQ__PricingMethod__c | Picklist| Indicates how the price for this line item is calculated. "List" = subtracting discount from list price. "Cost" = adding markup to cost.
| SBQQ__ListPrice__c| Currency(12, 2)| Pricebook Entry's unit price of the product sold
| SBQQ__CustomerPrice__c | Currency(12, 2)| Unit price including any systematic and discretionary discounts
| SBQQ__NetPrice__c |Currency(12, 2)| Unit price including any partner and distributor discounts
| SBQQ__ProrateMultiplier__c| Number(8, 4)|Contract term divisible by the product’s default subscription term.  The calculation will depend on your Subscription Proration Settings
| SBQQ__RenewalPrice__c | Currency(12, 2)| The non prorated net unit price.
| SBQQ__DiscountScheduleType__c | Picklist| Required if the product has a discount schedule defined. Must match the type from the discount schedule defined in the lookup.
| SBQQ__DiscountSchedule__c | Lookup(Discount Schedule) | Required if the product has a discount schedule defined or would otherwise be defined in a bundle (i.e. Product Option)
| SBQQ__TermDiscountSchedule__c | Lookup(Discount Schedule) | Required if the product has a term discount schedule defined
| SBQQ__Bundle__c |Checkbox| Required if product is parent to other products

**Note:** If Root ID is not populated correctly, bundle structure will be lost or renewals/amendments will fail during their creation.

### Subscriptions (Additional fields needed for bundle children)

These are the additional fields apart from the above mentioned fields Which we need to verify for bundle children.

| Field API Name | Type | Notes|
| ----------- | ----------- | --------- |
|SBQQ__OptionLevel__c | Number(5, 0)| Indicates nest level of this option: 1 for children of the parent product and 2 for grandchildren of the top-level bundle product.
|SBQQ__BundledQuantity__c| Number(10, 2)|The initial quantity of the option before it was multiplied by the parent's quantity
|SBQQ__ProductOption__c| Lookup(Product Option)| Lookup to the product option that joins the parent product reference to this Subscription's Product
|SBQQ__OptionType__c| Picklist|Must match the type of the referenced Product Option record
|SBQQ__RequiredByProduct__c|Lookup(Product)|The Configured SKU product target of the referenced Product Option
|SBQQ__RequiredById__c|Text(18)|The 18-digit SF ID of the bundle parent's Asset or Subscription record
|SBQQ__ComponentSubscriptionScope__c|Picklist|If the child subscription is a percent of total priced product, this field must be populated to limit the percent of total scope to the bundle containing the product.

### Customer Asset

Need to verify the below fields if these are properly populated
| Field API Name | Type | Notes| Value to Populate |
| ---------- | ------- | --------------- | ---------- |
| Name__c | string (255) | Name of the Product in the Customer asset| subscription.SBQQ__Product__r.Name |
| Account__c | Lookup (Account) | If of the Account to which this Customer asset belongs| subscription.SBQQ__Account__c |
| Product__c | Lookup(Product2) | Id of the Product that is on the customer assest | subscription.SBQQ__Product__c |
| Description__c | textarea (32768) | Description of the Product | subscription.SBQQ__Product__r.Description |
| MLS_Selection__c | Lookup (Account) | MLS Selection from the Subscription  | subscription.MLS_Selection__c |
| Number_of_Agents__c | double (18, 0)| Number of agents Count | subscription.Number_of_Agents__c |
| Pricing_Lookup_Type__c | string (255) | Product level configuration Denotes if this product is priced by active (paid) agent or by total agent | subscription.Pricing_Lookup_Type__c |
| Option_Pricing_Lookup_Type__c | picklist (255), restricted| Product Option specific configuration Denotes if this product is priced by active (paid) agent or by total agent | subscription.Option_Pricing_Lookup_Type__c |
| Migrated__c | boolean required | N/A | subscription.Migrated__c |
| Asset_Status__c | picklist (255) | Status of the Customer Asset | Depends on renewal/ amendment/ net new |
| Quantity__c | double (18, 2) | Quantity of the Customer Asset |  Depends on renewal/ amendment/ net new |
| Steelbrick_Subscription__c | Lookup(subscription) | Id of the Subscription from which this CA is created  | subscription.Id |
| Contract__c | Lookup(Contract) | Id of the Contract from which this CA is generated | subscription.SBQQ__Contract__c |
| CurrencyIsoCode | picklist (3) | Currency ISO Code | subscription.CurrencyIsoCode |
| Term_Start_Date__c | Date | Start date of the Customer Asset | subscription.SBQQ__StartDate__c |
| Term_End_Date__c | Date| End date of the Customer Asset| subscription.SBQQ__EndDate__c|
| Win_Loss_Reason__c | string (255)| Win/Loss reason from the opportunity| |
| Win_Loss_Comments__c | string (255) | Win/Loss Comment from the opportunity| |
| Decommission_Effective_Date__c | Date | Date the decommission will start | subscription.SBQQ__EfectiveStartDate__c|
