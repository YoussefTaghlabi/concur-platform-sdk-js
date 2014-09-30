Concur SDK for Javascript
==============

JavaScript SDK for the [Concur Platform](http://developer.concur.com). For more information on the set of platform services, see the [Web services overview](https://developer.concur.com/get-started/webservices-overview) document on the developer portal.

## Installation

    npm install concur-platform

## Usage

All platform services are exposed via a root module which can be imported using the following.

    var concur = require('concur-platform');

## Platform Services

### OAuth

Enables the client to acquire an [OAuth token](https://developer.concur.com/api-documentation/oauth-20-0).

#### Usage

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

### Receipt

Enables the client to interact with the [receipt](https://www.concursolutions.com/api/docs/index.html#!/ReceiptImages) and [eReceipt](https://developer.concur.com/api-documentation/more-resources/draft-documentation/e-receipt-service) Web services.

#### Usage

    var concur = require('concur-platform');

    // Sending a receipt
    concur.receipt.send({oauthToken:oauthToken, image:image, contentType:'image/png'})
    .then(function(receiptID) {
        //receiptID is returned on success
    })
    .fail(function(error) {
        // error will contain the error message returned
    });

    // Getting receipts
    concur.receipt.get({oauthToken:oauthToken})
    .then(function(data) {
        //data will contain a list of receipts
    })
    .fail(function(error) {
        // error will contain the error message returned
    });

    // Deleting a receipt
    concur.receipt.delete({oauthToken:oauthToken, receiptId:receiptId})
    .then(function(data.statusCode) {
        // data.statusCode will be equal to 204, the receipt was deleted
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

Copyright 2014 [Concur Technologies](http://www.concur.com)

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.