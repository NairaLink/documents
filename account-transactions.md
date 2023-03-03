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
**Note**: This is an internal endpoint to be called only when the account balance is been modified
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
