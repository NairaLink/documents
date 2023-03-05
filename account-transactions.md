# Customers, Accounts and Transactions

## Customers
Snippets of customer enpoints
### POST a customer
- **/v1/customer**
This endpoint creates a new user
    - First Name
    - Last Name
    - username
    - email address
    - physical address
If the above requirements are met, send a verification mail to the customer's email

### GET verify new customer
- **/v1/customer/verify**
This endpoint verifies a new customer
    - a valid query id generated on success post to `/v1/customer`
- On successfully verification, create a default account for the customer by `POST` to `/v1/accounts/`
- Redirect user to login page

### GET a customer
- **/v1/customer/:id**
This endpoint retrieves all the KYC of a customer
##### API communication requirements
- cors valid domain
- valid request token
- valid request header

### DELETE a cutomer
- **/v1/customer/:id**
This endpoint deletes a customer, customers's account, transactions and all other info
##### API communication requirements
- cors valid domain
- valid request token
- valid request header

## ACCOUNTS
Snippet of accounts endpoint

### POST an account
- **/v1/customer**
This endpoint creates a new account.
- Request Data
    - customer id
Creates an account and populates the database with the following details
    - unique account id
    - account type
    - unique account number
    - account currency
    - zeroed account balance
    - date created
    - date updated
- Response Data
    - account type
    - unique account number
    - account balance
    - account currency
##### API communication requirements
- cors valid domain
- valid request token
- valid request header

### GET an account
- **/v1/account/:id**
This endpoint retrieves an account detail
- Request Data
    - optional query parameters to configure response data
- Response Data: if no query params
    - account type
    - unique account number
    - account balance
    - account currency
##### API communication requirements
- cors valid domain
- valid request token
- valid request header

### PUT an account
- **/v1/account/:id**
This is an internal endpoint to be called only when the account balance is been modified
- Request Data
    - amount to be added/substracted from an account
    - date update (automatically updated by the server or database)
##### API communication requirements
- cors valid domain
- valid request token
- valid request header

### DELETE an account
- **/v1/account/:id**
Deletes an account and all transactions related to this account
##### API communication requirements
- cors valid domain
- valid request token
- valid request header

## Transactions
These endpoints are implicit to the application alone

### POST a transaction
- **/v1/transactions/**
Moves money between two `nairalink` accounts
- Request data
    - from account number
    - to account number
    - transaction amount
    - description
    - transfer pin

- Response Data
    - status
    - message

### GET transaction details
- List a transactions from lastest to earliest
    - **/v1/transaction/:id**
- Response Data:
    - from account number
    - to account number
    - transaction amount
    - description
    - transaction date
    - transaction status

## ACCOUNT TRANSACTIONS Relationship

### GET all transactions for an account
This data will be paginated
- **/v1/accounts/:accountid/transactions**
- This endpoint can be queried as follows
    - type parameter: `/v1/accounts/:accountid/transactions?type=[fundings|internal_credits|internal_deposits|card_credits]`

### POST account funding
- **/v1/account/fund**
This endpoint creates an unfulfilled transaction row in the transaction table
- Request Data
    - accountId
- Response Data
    - email
    - amount
    - transaction id as a reference to paystack
    - currency (NGN)

##### Detailed explaination
- The frontend makes a call to this endpoint with the account id about to be funded
- This endpoint generated a transaction reference id, then writes this transacction reference id to the transactions table with a default status of `pending`. It then returns the user's email, the transaction reference id to the frontend
- The frontend collects the amount the user wants to fund from the user.
- Now, with the `amount`, `transaction reference id` and `user email`, a constant callback function is left.
- Here is the callback function flow
    - When paystack verifies the transaction it should send a `GET` request to this endpoint `v1/account/fund/verify?reference={?}`
    - This endpoint checks the transaction table and if it finds the transaction reference, it sends a `200` status code else 404
    - With a `200` confirmation paystack sends a `POST` request to `v1/account/fund/verify`. This endpoint then updates the transaction and account with the transaction response data from paystack.

### GET & POST funding verification
- **/v1/account/fund/verify**

##### GET **/v1/account/fund/verify?reference=<>**
This endpoint expects a transaction reference to be sure we triggered and approved the funding
    - Response Data: `statuscode: 200`

#### POST **/v1/account/fund/verify**
This endpoint recieves the entire `response` data from paystack
- It updates the transaction status with data from `response.data.status`
- It updates the account balance with the amount that was received from paystack
- Triggers a webhook to send notification to the user of the transaction status
All the above can be done using individual workers asynchronously

## ACCOUNTS TABLE
| Fields | Data type | required | default | primary key | foriegn key | index |
| ------ | --------- | -------- | ------- | ----------- | ----------- | ----- |
| id     | VARCHAR   | yes      | -       | yes | no | yes |
| account type | VARCHAR | yes | standard | no | no | no |
| account currency | VARCHAR | yes | NGN | no | no | no |
| account balance | DECIMAL | yes | 0.00 | no | no | no |
| date created | Datetime | yes | utc iso format | no | no | yes |
| last updated | Datetime | yes | utc iso format | no | no | yes |

## TRANSACTIONS TABLE
| Fields | Data type | required | default | primary key | foriegn key | index |
| ------ | --------- | -------- | ------- | ----------- | ----------- | ----- |
| id     | VARCHAR   | yes      | -       | yes | no | yes |
| transaction type | VARCHAR | yes | account funding / internal / card funding | no | no | yes |
| transaction reference | VARCHAR | yes | accountid+uuid4 | no | no | yes |
| from | VARCHAR | yes | paysatck / internal account number | yes | yes | yes |
| to | VARCHAR | yes | internal / account number / debit card | yes | yes | yes |
| amount | VARCHAR | yes | - | no | no | no |
| initiation date | Datetime | yes | utc iso format | no | no | yes |
| completed date | Datetime | yes | utc iso format | no | no | yes |
| status | VARCHAR | yes | pending / failed / successful | no | no | yes |
