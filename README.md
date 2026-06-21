<div align="center">

# node-quickbooks

Node.js client for Intuit's QuickBooks API.

</div>

## 📌 Overview

`node-quickbooks` is a Node.js client library for working with Intuit's [QuickBooks API](https://developer.intuit.com/app/developer/qbo/docs/develop). It provides callback-based methods for common QuickBooks Online operations, including creating, reading, updating, deleting, querying, reporting, batch requests, change data capture, PDF retrieval/emailing, and file uploads as attachables.

The library supports OAuth 2.0 usage by passing `false` for the token secret and providing an OAuth 2.0 refresh token.

## 📦 Installation

```bash
npm install node-quickbooks
```

## 🚀 Basic Usage

```javascript
var QuickBooks = require('node-quickbooks')

var qbo = new QuickBooks(
  consumerKey,
  consumerSecret,
  oauthToken,
  false,
  realmId,
  false,
  true,
  null,
  '2.0',
  refreshToken
)

qbo.getBillPayment('42', function(err, billPayment) {
  if (err) console.log(err)
  else console.log(billPayment)
})
```

## 🔐 Constructor

```javascript
QuickBooks(
  consumerKey,
  consumerSecret,
  oauth_token,
  oauth_token_secret,
  realmId,
  useSandbox,
  debug,
  minorVer,
  oAuthVer,
  refresh_token
)
```

### Arguments

- `consumerKey` - The application's consumer key.
- `consumerSecret` - The application's consumer secret.
- `oauth_token` - The user's generated token.
- `oauth_token_secret` - The user's generated secret. Use `false` for OAuth 2.0.
- `realmId` - The QuickBooks company ID.
- `useSandbox` - Boolean flag for QuickBooks sandbox usage.
- `debug` - Boolean flag to log HTTP requests, headers, and response bodies.
- `minorVer` - QuickBooks API minor version, or `null` to avoid specifying one.
- `oAuthVer` - Use `'2.0'` for OAuth 2.0.
- `refresh_token` - The user's generated refresh token.

## 🧾 Supported Operations

### Create

The client includes create methods for QuickBooks entities such as:

- `createAccount`
- `createAttachable`
- `createBill`
- `createBillPayment`
- `createClass`
- `createCreditMemo`
- `createCustomer`
- `createDepartment`
- `createDeposit`
- `createEmployee`
- `createEstimate`
- `createInvoice`
- `createItem`
- `createJournalEntry`
- `createPayment`
- `createPurchase`
- `createPurchaseOrder`
- `createRefundReceipt`
- `createSalesReceipt`
- `createTaxAgency`
- `createTerm`
- `createTimeActivity`
- `createTransfer`
- `createVendor`
- `createVendorCredit`

Example:

```javascript
qbo.createAttachable({ Note: 'My File' }, function(err, attachable) {
  if (err) console.log(err)
  else console.log(attachable.Id)
})
```

### Read

Read methods retrieve QuickBooks entities by ID or, where applicable, by options:

- `getAccount`
- `getAttachable`
- `getBill`
- `getBillPayment`
- `getCompanyInfo`
- `getCreditMemo`
- `getCustomer`
- `getDepartment`
- `getDeposit`
- `getEmployee`
- `getEstimate`
- `getExchangeRate`
- `getInvoice`
- `getItem`
- `getJournalEntry`
- `getPayment`
- `getPreferences`
- `getPurchase`
- `getPurchaseOrder`
- `getRefundReceipt`
- `getSalesReceipt`
- `getTaxAgency`
- `getTaxCode`
- `getTaxRate`
- `getTerm`
- `getTimeActivity`
- `getVendor`
- `getVendorCredit`

Example:

```javascript
qbo.getBillPayment('42', function(err, billPayment) {
  console.log(billPayment)
})
```

### Update

Update methods persist changes to existing QuickBooks entities. Updated objects generally need `Id` and `SyncToken` fields.

```javascript
qbo.updateCustomer({
  Id: '42',
  SyncToken: '1',
  sparse: true,
  PrimaryEmailAddr: {
    Address: 'customer@example.com'
  }
}, function(err, customer) {
  if (err) console.log(err)
  else console.log(customer)
})
```

Supported update methods include:

- `updateAccount`
- `updateAttachable`
- `updateBill`
- `updateBillPayment`
- `updateCompanyInfo`
- `updateCreditMemo`
- `updateCustomer`
- `updateDepartment`
- `updateDeposit`
- `updateEmployee`
- `updateEstimate`
- `updateInvoice`
- `updateItem`
- `updateJournalEntry`
- `updatePayment`
- `updatePreferences`
- `updatePurchase`
- `updatePurchaseOrder`
- `updateRefundReceipt`
- `updateSalesReceipt`
- `updateTaxAgency`
- `updateTaxCode`
- `updateTaxRate`
- `updateTerm`
- `updateTimeActivity`
- `updateTransfer`
- `updateVendor`
- `updateVendorCredit`
- `updateExchangeRate`

### Delete

Delete methods accept either an entity or an ID. If an ID is passed, the library retrieves the entity first.

Supported delete methods include:

- `deleteAttachable`
- `deleteBill`
- `deleteBillPayment`
- `deleteCreditMemo`
- `deleteDeposit`
- `deleteEstimate`
- `deleteInvoice`
- `deleteJournalEntry`
- `deletePayment`
- `deletePurchase`
- `deletePurchaseOrder`
- `deleteRefundReceipt`
- `deleteSalesReceipt`
- `deleteTimeActivity`
- `deleteTransfer`
- `deleteVendorCredit`

Example:

```javascript
qbo.deleteAttachable('42', function(err, attachable) {
  if (err) console.log(err)
  else console.log(attachable)
})
```

## 🔎 Queries

Query methods accept optional criteria. Criteria can be supplied as an object or as an array of objects with `field`, `value`, and optional `operator` keys.

### Filters

```javascript
qbo.findAttachables({
  Note: 'My sample note field'
}, function(err, attachables) {
  console.log(attachables)
})
```

Array-style criteria can express operators such as `=`, `IN`, `<`, `>`, `<=`, `>=`, and `LIKE`.

```javascript
qbo.findTimeActivities([
  { field: 'TxnDate', value: '2014-12-01', operator: '>' },
  { field: 'TxnDate', value: '2014-12-03', operator: '<' },
  { field: 'limit', value: 5 }
], function(err, timeActivities) {
  console.log(timeActivities)
})
```

### Sorting

Use `asc` or `desc` in the criteria object.

```javascript
qbo.findAttachables({
  desc: 'MetaData.LastUpdatedTime'
}, function(err, attachables) {
  console.log(attachables)
})
```

### Pagination

Use `limit` and `offset`.

```javascript
qbo.findAttachables({
  limit: 10,
  offset: 10
}, function(err, attachables) {
  console.log(attachables)
})
```

The default and maximum limit is 1000 records per request. Passing `fetchAll: true` issues additional requests as needed to retrieve all available records.

```javascript
qbo.findCustomers({
  fetchAll: true
}, function(err, customers) {
  console.log(customers)
})
```

### Counts

Use `count: true` to request row counts instead of full result sets.

```javascript
qbo.findAttachables({
  count: true
}, function(err, attachables) {
  console.log(attachables)
})
```

## 📊 Reports

The client includes methods for QuickBooks report endpoints, including:

- `reportBalanceSheet`
- `reportProfitAndLoss`
- `reportProfitAndLossDetail`
- `reportTrialBalance`
- `reportCashFlow`
- `reportInventoryValuationSummary`
- `reportCustomerSales`
- `reportItemSales`
- `reportCustomerIncome`
- `reportCustomerBalance`
- `reportCustomerBalanceDetail`
- `reportAgedReceivables`
- `reportAgedReceivableDetail`
- `reportVendorBalance`
- `reportVendorBalanceDetail`
- `reportAgedPayables`
- `reportAgedPayableDetail`
- `reportVendorExpenses`
- `reportTransactionList`
- `reportGeneralLedgerDetail`
- `reportDepartmentSales`
- `reportClassSales`

Example:

```javascript
qbo.reportBalanceSheet({ department: '1,4,7' }, function(err, balanceSheet) {
  console.log(balanceSheet)
})
```

## 📎 Attachments and Uploads

Files can be uploaded as QuickBooks attachables, optionally linked to a QuickBooks entity.

```javascript
qbo.upload(
  'contractor.jpg',
  'image/jpeg',
  fs.createReadStream('contractor.jpg'),
  'Invoice',
  40,
  function(err, data) {
    console.log(err)
    console.log(data)
  }
)
```

The `upload` method accepts:

- `filename` - File name.
- `contentType` - MIME type.
- `stream` - Readable stream of file contents.
- `entityType` - Optional QuickBooks entity type, such as `Invoice`.
- `entityId` - Optional QuickBooks entity ID.
- `callback` - Callback receiving the created attachable.

## 📄 PDFs and Email

The client includes PDF retrieval and email methods for supported QuickBooks documents:

- `getInvoicePdf`
- `getCreditMemoPdf`
- `getSalesReceiptPdf`
- `sendInvoicePdf`
- `sendCreditMemoPdf`
- `sendEstimatePdf`
- `sendSalesReceiptPdf`
- `sendPurchaseOrder`

Email methods can use the email address stored on the QuickBooks document or an optional `sendTo` address.

## 🔁 Batch and Change Data Capture

### Batch

`batch(items, callback)` performs multiple supported operations in one request.

Supported batch item types:

- `create`
- `update`
- `delete`
- `query`

The maximum number of batch items in a single request is 30.

### Change Data Capture

`changeDataCapture(entities, since, callback)` returns entities changed since a specified time.

- `entities` may be a comma-separated list or JavaScript array.
- `since` may be a JavaScript `Date` or a string such as `2012-07-20T22:25:51-07:00`.

## 🧪 Example App

The `example` directory contains a barebones Express application demonstrating the OAuth workflow.

The example workflow includes:

- creating an Intuit Developer application,
- configuring the OAuth consumer key and secret,
- starting the example app,
- visiting `http://localhost:3000/start`,
- completing the Intuit OAuth authorization flow,
- receiving OAuth values through the callback flow.

The original setup notes state that OAuth credentials must be obtained from the Intuit Developer portal and configured in the example application before use.

## ✅ Running Tests

Tests require QuickBooks API credentials in `config.js`.

The original README notes that the following values are needed:

- `consumerKey`
- `consumerSecret`
- `token`
- `tokenSecret`
- `realmId`

After credentials are configured, tests can be run with:

```bash
npm test
```

## 🧭 Common Use Cases

`node-quickbooks` is intended for Node.js applications that need to interact with QuickBooks Online data through Intuit's API. Typical usage includes:

- creating and updating customers, invoices, bills, vendors, and other accounting entities,
- querying QuickBooks records with filters, sorting, pagination, and counts,
- retrieving financial reports such as balance sheets and profit-and-loss reports,
- uploading attachable files and linking them to QuickBooks entities,
- sending or retrieving PDFs for invoices, credit memos, estimates, sales receipts, and purchase orders,
- using batch operations for grouped API work,
- using change data capture to identify entities changed after a given time.

## ❓ FAQ

### Does this package work with OAuth 2.0?

Yes. The documented constructor example uses OAuth 2.0 by passing `false` for the token secret, setting the OAuth version to `'2.0'`, and passing a refresh token.

### Can queries include filters and pagination?

Yes. Query methods can use criteria objects for filters, sorting, limits, offsets, counts, and `fetchAll`.

### Can this client retrieve QuickBooks reports?

Yes. The documented API includes report methods such as balance sheet, profit and loss, trial balance, cash flow, customer reports, vendor reports, and transaction reports.

### Can files be uploaded to QuickBooks?

Yes. The `upload` method uploads a file as an attachable and can optionally link it to a QuickBooks entity.

### Are batch operations supported?

Yes. Batch operations support create, update, delete, and query items, with a documented maximum of 30 batch items per request.

### Are tests available?

The original README documents `npm test`, but tests require QuickBooks API credentials configured in `config.js`.
