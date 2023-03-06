# VIRTUAL CARD
For the virtual card API communication, an API key and token will be
generated from the account that is attempting the communication.
This token will have the Account ID


## CARD ENDPOINTS
Snippets of card endpoints
All endpoints return success if there are no errors, otherwise
{"resource": "name_of_the_resource",
 "status": "failed",
 "message" "error_message"
}
### CREATE A VIRTUAL CARD
- **POST /v1/card**
This endpoint creates a new virtual card
| REQUEST DATA	|	Supported values |
| -----------   | ---------------------- |
| card_brand (str) | (required) mastercard, visa, verve |
| card_currency (str) | (required) NGN, USD |
| name_on_card (str) | (required) _delimited by whitespace_ |
| PIN (str) | (required) _four digits_ |
RESPONSE DATA
{"resource": "create_virtual_card",
 "status" : "success",
 "data" : {"card_id": "", "card_brand": "VISA", "card_currency": "NGN",
	   "card_number": "", "name_on_card": "", "expiry_month": "",
	   "expiry_year": "", "cvv": "", "PIN": "", "status": "inactive",
	   "balance": "0.00", "date_created": <datetime>, "last_updated": <datetime>
	  }
}

### List all virtual cards under an account
- ** GET /v1/card/
This endpoint gets all virtual cards registered under an account, [] if otherwise
| REQUEST DATA  |       Supported values |
| -----------   | ---------------------- |
| card_currency (str) | (optional) NGN, USD |
RESPONSE DATA
{"resource": "list_virtual_card",
 "status" : "success",
 "data" : [{"card_id": "", "card_brand": "VISA", "card_currency": "NGN",
           "card_number": "", "name_on_card": "", "expiry_month": "",
           "expiry_year": "", "cvv": "", "PIN": "", "status": "inactive",
           "balance": "0.00", "date_created": <datetime>, "last_updated": <datetime>
           },
 
	   {"card_id": "", "card_brand": "VISA", "card_currency": "NGN",
           "card_number": "", "name_on_card": "", "expiry_month": "",
           "expiry_year": "", "cvv": "", "PIN": "", "status": "inactive",
           "balance": "0.00", "date_created": <datetime>, "last_updated": <datetime>
           }]
}

### List a virtual card details
- **GET /v1/card/:card_id**
This endpoint retrieves the details of a virtual card with card_id specified
RESPONSE DATA
{"resource": "get_virtual_card",
 "status" : "success",
 "data" : {"card_id": "", "card_brand": "VISA", "card_currency": "NGN",
           "card_number": "", "name_on_card": "", "expiry_month": "",
           "expiry_year": "", "cvv": "", "PIN": "", "status": "inactive",
           "balance": "0.00", "date_created": <datetime>, "last_updated": <datetime>
          }
}

### UPDATE virtual card details
To block/unblock a card
- **PUT /v1/card/**
This endpoint changes a card status to either active or inactive
| REQUEST DATA  |       Supported values |
| -----------   | ---------------------- |
| card_id (str) | (required) |
| status (str) | (required) |
RESPONSE DATA
{"resource": "change_virtual_card_status",
 "status" : "success",
 "data" : {"card_id": "", "status": "new_status",
           "date_created": <datetime>, "last_updated": <datetime>
          }
}

To change card PIN
- **PUT /v1/card/**
This endpoint changes the PIN of a virtual card
| REQUEST DATA  |       Supported values |
| -----------   | ---------------------- |
| card_id (str) | (required) |
| PIN (str) | (required) |
RESPONSE DATA
{"resource": "change_virtual_card_PIN",
 "status" : "success",
 "data" : {"card_id": "", "PIN": "new_PIN",
           "date_created": <datetime>, "last_updated": <datetime>
          }
}

### DELETE a virtual card
To delete a card
- **DELETE /v1/card/:card_id**
This endpoint deletes a card and changes its status to terminated
| REQUEST DATA  |       Supported values |
| -----------   | ---------------------- |
| card_id (str) | (required) |
RESPONSE DATA
{"resource": "delete_virtual_card",
 "status" : "success",
 "data" : {"card_id": "", "status": "terminated",
           "date_created": <datetime>, "last_updated": <datetime>
          }
}

## CARD TRANSACTIONS
Snippet of card transactions endpoints

### FUND a virtual card from a Nairalink account
- **POST /v1/card/transactions**
This endpoint funds a virtual card from a nairalink account.
| REQUEST DATA  |       Supported values |
| -----------   | ---------------------- |
| card_id (str) | (required) |
| amount (decimal) | (required) to be converted to cents/kobos |
| currency (str) | (required) NGN, USD |
| narration (str) | (optional) |
RESPONSE DATA
{"resource": "fund_virtual_card",
 "status" : "success",
 "data" : {"transaction_id": "", "card_id": "", "amount": "amount", "narration": "",
	   "type": "credit", "currency: "NGN", "datetime_of_transaction": <datetime>
	   "status": ""
          }
}

### GET transactions on a virtual_card filtered by (currency/type/status)
- **GET /v1/card/transactions/**
This endpoints gets transactions on a virtual card filtered by either currency, type, datetime or status
Otherwise, it returns all transactions done a particular virtual card
| REQUEST DATA  |       Supported values |
| -----------   | ---------------------- |
| card_id (str) | (required) |
| type (str) | (optional) either debit or credit transactions |
| currency (str) | (optional) either NGN or USD transactions |
| status (str) | (optional) either failed or successful transactions |
| limit_datetime (datetime) | (optional) datetime to limit the search (DD-MM-YYYY HH:MM:SS) |
RESPONSE DATA
{"resource": "fund_virtual_card",
 "status" : "success",
 "data" : [{"transaction_id": "", "card_id": "", "amount": "amount",
           "type": "debit", "currency: "NGN", "narration": "",
           "datetime_of_transaction": <datetime>, "status": "successful"
          },
	  {"transaction_id": "", "card_id": "", "amount": "amount",
           "type": "credit", "currency: "NGN", "narration": "",
           "datetime_of_transaction": <datetime>, "status": "failed"
          },
	  {"transaction_id": "", "card_id": "", "amount": "amount",
           "type": "credit", "currency: "NGN", "narration": "",
           "datetime_of_transaction": <datetime>, "status": "successful"
          }]
}



## CARD TABLE
| Fields | Data type | required | default | primary key | foriegn key | index |
| ------ | --------- | -------- | ------- | ----------- | ----------- | ----- |
| account_id     | VARCHAR   | yes      |  | no | yes |  |
| card_id | VARCHAR | yes |  | yes | no |  |
| card_brand | VARCHAR | yes | VISA / VERVE / MASTERCARD |  |  |  |
| card_currency | VARCHAR | yes | NGN / USD |  |  |  |
| card_number | VARCHAR | yes |  |  |  |  |
| name_on_card | VARCHAR | yes |  |  |  |  |
| expiry_month | VARCHAR | yes |  |  |  |  |
| expiry_year  | VARCHAR | yes |  |  |  |  |
| cvv  | VARCHAR | yes |  |  |  |  |
| PIN  | VARCHAR | yes |  |  |  |  |
| balance  | VARCHAR | yes | 0.00 |  |  |  |
| status  | VARCHAR | yes | active / inactive / terminated |  |  |  |
| date_created  | DATETIME | yes | utc iso format |  |  |  |
| last_updated  | DATETIME | yes | utc iso format |  |  |  |


## CARD TRANSACTIONS TABLE
| Fields | Data type | required | default | primary key | foriegn key | index |
| ------ | --------- | -------- | ------- | ----------- | ----------- | ----- |
| transaction_id     | VARCHAR   | yes | card_id+uuid4   | yes | no |  |
| card_id | VARCHAR | yes |  | no | yes |  |
| type | VARCHAR | yes | debit/credit |  |  |  |
| narration | VARCHAR | no |   |  |  |  |
| currency | VARCHAR | yes | NGN / USD |  |  |  |
| amount | VARCHAR | yes |  |  |  |  |
| datetime_of_transaction | Datetime | yes | utc iso format |  |  |  |
| status | VARCHAR | yes | failed / successful |  |  |  |

