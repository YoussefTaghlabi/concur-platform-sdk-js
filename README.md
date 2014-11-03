Concur SDK for JavaScript
==============

JavaScript SDK for the [Concur Platform](http://developer.concur.com). For more information on the set of platform services, see the [Web services overview](https://developer.concur.com/get-started/webservices-overview) document on the developer portal.
Register for a [developer Sandbox here](https://developer.concur.com/register).

[![NPM](https://nodei.co/npm/concur-platform.png?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/concur-platform/)

## Installation

    npm install concur-platform

## Usage

All platform services are exposed via a root module which can be imported using the following.

    var concur = require('concur-platform');

## Platform Services

### OAuth

Enables the client to acquire an [OAuth token](https://developer.concur.com/oauth-20).

#### Usage

#####Native Flow

This is for the [native flow Oauth](https://developer.concur.com/oauth-20/native-flow). Use this
to get a token to test your application. This requires username, password and your registered consumerkey.

    var concur = require('concur-platform');

    var options = {
        username:username,
        password:password,
        consumerKey:consumerKey
    }

    concur.oauth.native(options)
    .then(function(token) {
        // token will contain the value, instanceUrl, refreshToken, and expiration details
    })
    .fail(function(error) {
        // error will contain the error message returned
    });

#####AppCenter Flow

This is for the [AppCenter Flow](https://developer.concur.com/oauth-20/app-center-flow).
AppCenter Flow requires the code query parameter from Concur AppCenter, clientID (consumerKey) and clientSecret
for your registered partner application.

    var options = {
        code:code,
        client_id:client_id,
        client_secret:client_secret
    }

    concur.oauth.appCenter(options)
    .then(function(token) {
        // token will contain the value, instanceUrl, refreshToken, and expiration details
    })
    .fail(function(error) {
        // error will contain the error message returned
    });

### Entries

This is for the Expense [Entries API](https://www.concursolutions.com/api/docs/index.html#!/Entries). Entries belong
to an Expense Report.

#### Usage

##### POST

    //This will post an Expense Entry to a given ReportID
    var entry = {
        'Comment': 'Test Mileage Entry',
        'Description': 'Client Meeting',
        'ExchangeRate': '1.234',
        'ExpenseTypeCode': 'MILEG',
        'TransactionDate': '2014-10-27',
        'reportid': 'REPORTID'
    };

    var options = {
        oauthToken:oauthToken,
        contentType:'application/json',
        body:entry
    };

    concur.entries.send(options)
    .then(function(data){
        // In data will be the entry ID & URL to the entry
    })
    .fail(function (error) {
        // Error will contian the error returned by the server
    });

##### GET

    //This will contain a list of expense entries
    var options = {
        oauthToken:oauthToken
    };

    concur.entries.get(options)
    .then(function(data) {
        //Data will contain a list of entries
    })
    .fail(function (error) {
        // Error will contian the error returned by the server
    });

    //This will contain a list of expense entries
    var options = {
        oauthToken:oauthToken,
        id:entriesId
    };

    concur.entries.get(options)
    .then(function(data) {
        //Data will contain an entry
    })
    .fail(function (error) {
        // Error will contain the error returned by the server
    });

##### PUT

    //This will update the entry given by the entryId, and you can use any field support by entries from the link above.
    var entry = {
        'Comment': 'Test put',
    };

    var options = {
        oauthToken:oauthToken,
        contentType:'application/json',
        id:entryId,
    };

    concur.entries.put(options)
    .then(function(data){
        //Contains the response code 204, for a successful resource update
    })
    .fail(function (error) {
        // Error will contain the error returned by the server
    });

##### DELETE

    //This will delete the entry given an ID.
    var options = {
        oauthToken:oauthToken,
        id:entryId
    };

    concur.entries.delete(options)
    .then(function(data) {
        //Contains the response code 204, for a successful resource update
    })
    .fail(function (error) {
        //Contains the error returned
    });

### Quick Expenses

Enables the client to interact with the [quick expense](https://www.concursolutions.com/api/docs/index.html#!/QuickExpenses) web service.

#### Usage

##### POST

    var quickexpenseJSON = {
        "Comment": "I am a Quick Expense",
        "CurrencyCode": "USD",
        "ExpenseTypeCode": "CARMI",
        "LocationCity": "Seattle",
        "LocationCountry": "US",
        "LocationSubdivision": "US-WA",
        "TransactionAmount": "12.23",
        "TransactionDate": "2015-05-10",
        "VendorDescription": "Testing"
    };

    var options = {
        oauthToken:oauthToken,
        contentType:'application/json',
        body:quickexpenseJSON
    };

    concur.quickexpense.send(options)
    .then(function(data){
        //Contains the ID and URI to the resouce
    })
    .fail(function (error) {
        //Error contains the error returned
    });

##### GET

    //Get a list of quick expenses
    var options = {
        oauthToken:oauthToken
    };

    concur.quickexpense.get(options)
    .then(function(data) {
        //Data contains a list of quick expenses
    })
    .fail(function (error) {
        //Error contians the error returned
    });

    //Get a single quick expense, just add quickexpenseId to options
    var options = {
        oauthToken:oauthToken,
        id:quickexpenseId
    };

    concur.quickexpense.get(options)
    .then(function(data) {
        //Contains the single quick expense
    })
    .fail(function (error) {
        //Contains the error returned
    });

##### PUT

    var quickexpenseJSON = {
        "TransactionAmount": "16.23"
    };

    var options = {
        oauthToken:oauthToken,
        contentType:'application/json',
        id:quickexpenseId,
        body:quickexpenseJSON
    };

    concur.quickexpense.put(options)
    .then(function(data){
        //Contains the response code 204, for a successful resource update
    })
    .fail(function (error) {
        //Contains the error returned
    });

##### DELETE

    var options = {
        oauthToken:oauthToken,
        id:quickexpenseId
    };

    concur.quickexpense.delete(options)
    .then(function(data) {
        //Contains the response code 204, for a successful resource update
    })
    .fail(function (error) {
        //Contains the error returned
    });

### Receipt

Enables the client to interact with the [receipt](https://www.concursolutions.com/api/docs/index.html#!/ReceiptImages) and [eReceipt](https://developer.concur.com/api-documentation/more-resources/draft-documentation/e-receipt-service) Web services.

#### Usage

#####POST

    var concur = require('concur-platform');

    //If you have the image locally, this will post it to Concur.
    var options = {
        oauthToken:oauthToken,
        image:image,
        contentType:'image/png'
    }

    // Sending a receipt
    concur.receipt.send(options)
    .then(function(receiptID) {
        //receiptID is returned on success
    })
    .fail(function(error) {
        // error will contain the error message returned
    });


    //If you have a link to the image, using this will get the image, then post it to Concur.
    options = {
        oauthToken:oauthToken,
        imageURL:'http://upload.wikimedia.org/wikipedia/commons/2/22/Turkish_Van_Cat.jpg'
    };

    concur.receipt.send(options)
    .then(function(imageId) {
        //receiptID is returned on success
    })
    .fail(function(error) {
        //error will contain the error message returned
    });

#####GET

    // Getting a lit of receipts
    var options = {
        oauthToken:oauthToken
    }

    concur.receipt.get(options)
    .then(function(data) {
        //data will contain a list of receipts
    })
    .fail(function(error) {
        // error will contain the error message returned
    });

    //Get a receipt image URL by image ID.
    var options = {
        oauthToken:oauthToken,
        id:receiptId
    }

    concur.receipt.get(options)
    .then(function(data) {
        //data will contain the receipt image url
    })
    .fail(function(error) {
        // error will contain the error message returned
    });

#####DELETE

    // Deleting a receipt
    var options = {
        oauthToken:oauthToken,
        id:receiptId
    }

    concur.receipt.delete(options)
    .then(function(data.statusCode) {
        // data.statusCode will be equal to 204, the receipt was deleted
    })
    .fail(function(error) {
        // error will contain the error message returned
    });

###User

Enables the user to interact with the [user](https://developer.concur.com/api-documentation/web-services/user) Web service.

####Usage

    var options = {
        oauthToken:oauthToken,
        loginId:loginId
    }

    concur.user.get(options)
    .then(function(user) {
        // user will contain user data
    })
    .fail(function(error) {
        // error will contain the error message returned
    });

## Tests

To run the client SDK tests, create a default.json file in the config folder which contains the credentials of the Concur account to test with. Template.json can be used as a template. Then, run the following:

    npm test

The test will upload the concur logo to the expense receipt store associated with the OAuth token. It will also upload an E-Receipt to the associated user account. 

## Promises
In order to simplify the asynchronous nature of the platform Web service calls, the client SDK has made use of the Q promises library. More information can be found on the [project's GitHub site](https://github.com/kriskowal/q).

## License

Copyright 2014 [Concur](http://www.concur.com)

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
