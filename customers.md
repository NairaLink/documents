# Customers Routes

This endpoints will be used for customers' profile, customers' dashboard and some functionalities in the customers' setting

### Create Customer

POST /v1/customers - This route won't be defined.

Response

```json
res.status(500).json({
status: 'success',
message: 'This route is not defined! please use /signup instead',
});
```

### Get All Customer

GET /v1/customers - This route will be used to fetch all customers data

Response

```json
res.status(200).json({
results: customers.length
status: 'success',
data: {
    data: customers
}
})
```

### Get a Customer

GET /v1/getMe - This route will be used to fetch a customers data

Response

```json
res.status(200).json({
status: 'success',
id: mongodb Object
username: 'Joely',
email: 'joe@gmail.com'
})
```

### Update a Customer details

PATCH /v1/updateMe - This route will be used to update editable customer details except password
Request
{
email: 'joe123@gmail.com'
}

Response

```json
res.status(201).json({
status: 'success',
email: 'joe123@gmail.com'
})
```

### Delete a Customer

DELETE /v1/deleteMe - This route will be used to temporarily delete customer. The account can be reclaimed. There will be active field in the customer's table which will be set to False when the customer requested for the closing of the account.

Response

```json
res.status(204).json()
```
