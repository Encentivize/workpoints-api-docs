# Workpoints API

## Table of Contents

- [Neuron Auth](README-v2.md:16)
- [User Routes](README-v2.md:82)
    - [Query](README-v2.md:84)
    - [Create](README-v2.md:131)
    - [Update](README-v2.md:161)
- [Activity Routes](README-v2.md:163)
    - [Query](README-v2.md:226)
    - [Create](README-v2.md:171)
    - [Update]()
    - [Award](README-v2.md:296)

## Getting a neuron token
You can either use the [Neuron Synapse SDK](https://github.com/Encentivize/neuron-synapse) or make the API calls directly. The SDK will take care of the token refreshing. Once you get the token simply use it as a Bearer auth token in the request header.

```javascript
headers: {
    Authorization: 'Bearer ' + token
}
```

### Neuron Synapse SDK

```javascript
const synapse = require('neuron-synapse');

const clientId = 'your-client-name';
const clientSecret = 'xxxxxxxxxxxxxxxxxxx';
synapse.initialise({
    neuronBaseUrl: 'https://neuron.encentivize.co.za/',
    clientId: clientId,
    clientSecret: clientSecret
});

const programName = 'workpoints-program-name';
synapse.getClientToken({
        programName: programName,
        scope: 'pointsRedemption'
    }, function (err, token) {
        // err: error object
        // token: string
    });
```

### Rest Request

If you use this method remember that tokens will need to be refreshed every 24 hours.

```javascript
const request = require('request');

const programName = 'workpoints-program-name';
const url = `https://neuron.encentivize.co.za/${programName}/oauth/token`;
const clientId = 'your-client-name';
const clientSecret = 'xxxxxxxxxxxxxxxxxxx';
const options = {
    method: 'POST',
    uri: url,
    form: {
        'client_id': clientId,
        'client_secret': clientSecret,
        'grant_type': "client_credentials",
        "scope": 'pointsRedemption'
    },
    headers: {
        "Content-Type": "application/x-www-form-urlencoded"
    },
    timeout: 30000
};

request(options, function (error, response, body) {
    const token = JSON.parse(body);
    
    // token.access_token: string
    // token.expires_in: number
});
```

## User Routes

### Querying users

url:
```https://api.encentivize.co.za/api/programs/{programName}/members```

method:
```GET```

optional query params:
```
deactivated={boolean}           // used to return active activities
limit={integer}                 // limit the number of activities returned
skip={integer}                  // start returning from the first element in the match results
sort={member field}             // sort results by the given field
```

### Getting a specific user

url:
```https://api.encentivize.co.za/api/programs/{programName}/members/{memberId}```

method:
```GET```

response example:
```json
{
  "memberId":"1",
  "displayName":"John",
  "surname":"Doe",
  "firstName":"John",
  "primaryEmail":"john.doe@workpoints.co.za",
  "primaryMobile":"0830000000",
  "idNumber":"123456",
  "employeeNumber":"002",
  "jobTitle":"Engineer",
  "birthday":"2000-06-28T00:00:00.000Z",
  "engagementDate":"2020-12-30T00:00:00.000Z",
  "terminationDate":"2021-12-21T00:00:00.000Z",
  "roles":["admin"],
  "profileImageCode":"mavima",
  "isDisabled":false,
  "profilePictureUrl":"https://encentivizev2.blob.core.windows.net/program/profile-1-mavima",
  "isAdministrator":true
}
```

### Creating a Workpoints user

Users are created asynchronously in the background by Workpoints. This means that they won't be immediately available once you make the request to create them. They should be available after 30 seconds of making the request.

url:
```https://api.encentivize.co.za/api/programs/{programName}/members```

http method:
```POST```

request body example:
```json
{
    "displayName": "Test User",
    "firstName": "Test",
    "surname": "User",
    "primaryEmail": "test@user.co.za",
    "primaryMobile": "073000000",
    "roles": ["admin"], // empty array for non-admin users
    "jobTitle": "Developer",
    "idNumber": "4239084597324698",
    "birthday": "2018-01-29T13:02:20.024Z",
    "engagementDate": "2018-01-29T13:02:20.024Z",
    "terminationDate": "2018-01-29T13:02:20.024Z",
    "stores": {} // leave out if you don't have additional profile fields
}
```

[member schema](schema/member/member-schema.json)

### Updating a Workpoints user

url:
```https://api.encentivize.co.za/api/programs/{programName}/members/{memberId}```

http method:
```PUT```

## Activity routes

### Creating an activity

url:
```https://api.encentivize.co.za/api/programs/{programName}/activities```

http method:
```POST```

request body example:
```json
{
    "name": "happyBirthday",
    "title": "Happy Birthday",
    "description": "Activity used by Birthdays Application",
    "category": "humanResourcesGeneral",
    "schema": {
        "type":"object",
        "title":"Fields",
        "properties": {
            "birthdayMessage": {
                "title":"Birthday Message",
                "type":"string"
            }
        }
    },
    "restrictions": {
        "acceptTargetDate": false,
        "applicableGroups": [],
        "assigneeLimit": {
            "maximumValue": 1,
            "period": {
                "baseUnit": "years",
                "length": 1
            }
        },
        "selfAssignment" : {
            "allowedGroups" : []
        }
    },
    "actions": {
        "postToFeed": true,
        "sendComms": true
    },
    "integrations": {
        "slackIncomingWebhook": {
            "isEnabled": false,
            "url": null
        }
    },
    "deactivated": false  
}
```

[activity schema](schema/activity/activity-schema.json)

### Querying activities
url:
```https://api.encentivize.co.za/api/programs/{programName}/activities```

method:
```GET```

optional query params:
```
deactivated=false       // only return active activities
limit=13                // only return 13 activities
skip=0                  // start returning from the first element in the matched results
sort=title              // sort by title
```

response:
```json
{
  "data": [
    {activity object},
    {...},
    {...}
  ]
}
```

### Getting a specific activity
url:
```https://api.encentivize.co.za/api/programs/{programName}/activities/{activityName}```

method:
```GET```

response example:
```json
{
  "name":"achievement",
  "title":"Achievement",
  "description":"Achievement",
  "schema":{
    "type":"object",
    "title":"Fields",
    "properties":{"motivation":{"title":"Motivation","type":"string"}},
    "public":["motivation"]
  },
  "restrictions":{
    "applicableGroups":[],
    "acceptTargetDate":true,
    "selfAssignment":{"allowedGroups":["adminGroup"]},
    "assignerLimit":{"maximumValue":10,"period":{"baseUnit":"months","length":1}}
  },
  "actions":{
    "sendComms":true,
    "postToFeed":true,
    "giveBonusPoints":{
      "points":{"fixed":1000},
      "activityCount":5,
      "period":"day",
      "mode":"rolling"
    },
    "givePoints":{"fixed":100}
  },
  "deactivated":false,
  "category":"peerRecognition",
  "integrations":{"slackIncomingWebhook":{"isEnabled":false,"url":"https://hooks.slack.com/services/xxxx/xxxxx/xxxxxx"}},
  "iconUrl":null,
  "canIassign":true
}
```

### Awarding an activity

url:
```https://api-qa.encentivize.co.za/api/programs/{programName}/members/{memberId}/activities/{activityName}/{targetDate}```

If your activity is setup to accept a target date you can pass one into the request url.
targetDate is optional.

method: 
```POST```

request body :
```json
{
  "yearsWithTheTeam": 1
}
```

Any custom activity fields that have been created can be filled in in request body.
