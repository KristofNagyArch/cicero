# Cicero Server

Exposes the Cicero Engine as a RESTful service.

Templates are loaded from a the root CICERO_DIR. 
Clauses may be instantiated by passing the JSON or MD (i.e. TXT) via the payload Body as, either, `request`, `data`, `state`, `options` or `sample` objects. 

See below CURL examples for details.

## Documentation

http://docs.accordproject.org

## Installation

```
npm install -g @accordproject/cicero-server --save
```

## Run

Assuming you cloned the [Cicero template library](https://github.com/accordproject/cicero-template-library) in directory `<cicero-template-library-dir>`, you can start the server using:
```
export CICERO_DIR=<cicero-template-library-dir>/src
cicero-server
```

The default port for the server is `6001`. You can set a different port as an environment variable `CICERO_PORT`.

Once the server is started, you can sent requests as follows:

### Parse request

```
curl --request POST \
  --url http://localhost:6001/parse/latedeliveryandpenalty \
  --header 'accept: application/json' \
  --header 'content-type: application/json' \
  --data '{
	  "sample": "## Late Delivery and Penalty.\n\n In case of delayed delivery except for Force Majeure cases,\n\"Dan\" (the Seller) shall pay to \"Steve\" (the Buyer) for every 2 days\nof delay penalty amounting to 10.5% of the total value of the Equipment\nwhose delivery has been delayed. Any fractional part of a days is to be\nconsidered a full days. The total amount of penalty shall not however,\nexceed 55% of the total value of the Equipment involved in late delivery.\nIf the delay is more than 15 days, the Buyer is entitled to terminate this Contract." }
'
```

### Draft request

```
curl --request POST \
  --url http://localhost:6001/draft/latedeliveryandpenalty \
  --header 'accept: application/json' \
  --header 'content-type: application/json' \
  --data '{
    "data":{
    "$class": "org.accordproject.latedeliveryandpenalty.LateDeliveryAndPenaltyContract",
    "buyer": "resource:org.accordproject.party.Party#Steve",
    "seller": "resource:org.accordproject.party.Party#Dan",
    "forceMajeure": true,
    "penaltyDuration": {
        "$class": "org.accordproject.time.Duration",
        "amount": 2,
        "unit": "days"
    },
    "penaltyPercentage": 10.5,
    "capPercentage": 55,
    "termination": {
        "$class": "org.accordproject.time.Duration",
        "amount": 15,
        "unit": "days"
    },
    "fractionalPart": "days",
    "contractId": "c538755a-6c9e-43ff-8767-f8915d0336a0",
    "$identifier": "c538755a-6c9e-43ff-8767-f8915d0336a0"
},
    "options": {
        "unquoteVariables": true
    }
}'
```

### Trigger request

```
curl --request POST \
  --url http://localhost:6001/trigger/latedeliveryandpenalty \
  --header 'accept: application/json' \
  --header 'content-type: application/json' \
  --data '{
    "request": {
        "$class": "org.accordproject.latedeliveryandpenalty.LateDeliveryAndPenaltyRequest",
        "forceMajeure": false,
        "agreedDelivery": "December 17, 2017 03:24:00",
        "deliveredAt": null,
        "goodsValue": 200.00
    },
    "state": {
        "$class": "org.accordproject.runtime.State",
        "$identifier": "81fc1750-a77b-11eb-9770-7ddd576a12c2"
    },
    "data": {
        "$class": "org.accordproject.latedeliveryandpenalty.LateDeliveryAndPenaltyContract",
        "buyer": "resource:org.accordproject.party.Party#Steve",
        "seller": "resource:org.accordproject.party.Party#Dan",
        "forceMajeure": true,
        "penaltyDuration": {
            "$class": "org.accordproject.time.Duration",
            "amount": 2,
            "unit": "days"
        },
        "penaltyPercentage": 10.5,
        "capPercentage": 55,
        "termination": {
            "$class": "org.accordproject.time.Duration",
            "amount": 15,
            "unit": "days"
        },
        "fractionalPart": "days",
        "contractId": "c538755a-6c9e-43ff-8767-f8915d0336a0",
        "$identifier": "c538755a-6c9e-43ff-8767-f8915d0336a0"
    }
}'
```

### Trigger request - Stateless (legacy)

Only supported for clauses or contracts without references to contract state

If the body contains an object with the properties 'state', then this is used as the contract state.  If no 'state' property exists then no contract state is used.

Assuming you cloned the [Cicero template library](https://github.com/accordproject/cicero-template-library) in directory `<cicero-template-library-dir>`:

```
curl --request POST \
  --url http://localhost:6001/trigger/latedeliveryandpenalty \
  --header 'accept: application/json' \
  --header 'content-type: application/json' \
  --data '{
    "request": {
        "$class": "org.accordproject.latedeliveryandpenalty.LateDeliveryAndPenaltyRequest",
        "forceMajeure": false,
        "agreedDelivery": "December 17, 2017 03:24:00",
        "deliveredAt": null,
        "goodsValue": 200.00
    },
    "data": {
        "$class": "org.accordproject.latedeliveryandpenalty.LateDeliveryAndPenaltyContract",
        "buyer": "resource:org.accordproject.party.Party#Steve",
        "seller": "resource:org.accordproject.party.Party#Dan",
        "forceMajeure": true,
        "penaltyDuration": {
            "$class": "org.accordproject.time.Duration",
            "amount": 2,
            "unit": "days"
        },
        "penaltyPercentage": 10.5,
        "capPercentage": 55,
        "termination": {
            "$class": "org.accordproject.time.Duration",
            "amount": 15,
            "unit": "days"
        },
        "fractionalPart": "days",
        "contractId": "c538755a-6c9e-43ff-8767-f8915d0336a0",
        "$identifier": "c538755a-6c9e-43ff-8767-f8915d0336a0"
    }
}'
```

Test with example helloworld
```
curl --location 'http://localhost:6001/initialize/helloworld' \
--header 'Content-Type: application/json' \
--data '{"sample":"Name of the person to greet: \"Fred Blogs\".\nThank you!"}'
```
![Screenshot 2024-06-13 at 11.06.13.png](..%2F..%2F..%2F..%2F..%2Fnagykristof%2FDesktop%2FScreenshot%202024-06-13%20at%2011.06.13.png)
## License <a name="license"></a>
Accord Project source code files are made available under the Apache License, Version 2.0 (Apache-2.0), located in the LICENSE file. Accord Project documentation files are made available under the Creative Commons Attribution 4.0 International License (CC-BY-4.0), available at http://creativecommons.org/licenses/by/4.0/.

© 2017-2019 Clause, Inc.
