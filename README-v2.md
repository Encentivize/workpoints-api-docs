# Workpoints API

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

## Getting Workpoints users

### Searching users

url:
```
https://api.encentivize.co.za/api/programs/{programName}/members
```

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
```
https://api.encentivize.co.za/api/programs/{programName}/members/{memberId}
```

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

## Creating a Workpoints user

Users are created asynchronously in the background by Workpoints. This means that they won't be immediately available once you make the request to create them. They should be available after 30 seconds of making the request.

url:
```
https://api.encentivize.co.za/api/programs/{programName}/members
```

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

full member schema:
```json
{
  "title": "Member Create",
  "type": "object",
  "properties": {
    "displayName": {
      "type": "string"
    },
    "primaryMobile": {
      "type": "string"
    },
    "primaryEmail": {
      "type": "string"
    },
    "roles": {
      "type": "array",
      "items": {
        "type": "string",
        "enum": [
          "admin"
        ]
      },
      "maxItems": 1
    },
    "stores": {
      "type": "object"
    },
    "firstName": {
      "type": "string"
    },
    "surname": {
      "type": "string"
    },
    "employeeNumber": {
      "type": "string"
    },
    "idNumber": {
      "type": "string"
    },
    "jobTitle": {
      "type": "string"
    },
    "birthday": {
      "type": [
        "string",
        "null"
      ],
      "format": "date-time"
    },
    "engagementDate": {
      "type": [
        "string",
        "null"
      ],
      "format": "date-time"
    },
    "terminationDate": {
      "type": [
        "string",
        "null"
      ],
      "format": "date-time"
    }
  },
  "additionalProperties": false,
  "required": [
    "displayName",
    "primaryEmail",
    "primaryMobile",
    "roles",
    "firstName",
    "surname"
  ]
}
```

## Updating a Workpoints user

url:
```
https://api.encentivize.co.za/api/programs/{programName}/members/{memberId}
```

http method:
```PUT```

## Creating an activity

url:
```
https://api.encentivize.co.za/api/programs/{programName}/activities
```

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

full activity schema:
```json
{
  "title": "Activity Create",
  "type": "object",
  "properties": {
    "title": {
      "type": "string"
    },
    "description": {
      "type": "string"
    },
    "category": {
      "type": "string",
      "enum": [
        "awards",
        "humanResourcesGeneral",
        "employeeInvolvement",
        "employeeDevelopment",
        "employeeServicesHealthAndWellness",
        "safety",
        "performance",
        "drivingGrowth",
        "corporateSocialResponsibility",
        "peerRecognition"
      ]
    },
    "name": {
      "type": "string",
      "pattern": "^[a-zA-Z][a-zA-Z0-9]*$"
    },
    "iconUrl": {
      "type": ["string", "null"]
    },
    "schema": {
      "type": "object",
      "properties": {
        "type": {
          "type": "string"
        },
        "title": {
          "type": "string"
        },
        "properties": {
          "type": "object"
        },
        "required": {
          "type": "array"
        },
        "public": {
          "type": "array"
        }
      }
    },
    "deactivated": {
      "type": "boolean"
    },
    "restrictions": {
      "type": "object",
      "properties": {
        "acceptTargetDate": {
          "type": "boolean"
        },
        "assigneeLimit": {
          "type": "object",
          "properties": {
            "maximumValue": {
              "type": "integer",
              "minimum": 0
            },
            "period": {
              "type": "object",
              "properties": {
                "baseUnit": {
                  "type": "string",
                  "enum": [
                    "hours",
                    "days",
                    "weeks",
                    "months",
                    "quarters",
                    "years"
                  ]
                },
                "length": {
                  "type": "integer",
                  "minimum": 1
                }
              },
              "required": [
                "baseUnit",
                "length"
              ]
            }
          },
          "required": [
            "maximumValue",
            "period"
          ]
        },
        "assignerLimit": {
          "type": "object",
          "properties": {
            "maximumValue": {
              "type": "integer",
              "minimum": 0
            },
            "period": {
              "type": "object",
              "properties": {
                "baseUnit": {
                  "type": "string",
                  "enum": [
                    "hours",
                    "days",
                    "weeks",
                    "months",
                    "quarters",
                    "years"
                  ]
                },
                "length": {
                  "type": "integer",
                  "minimum": 1
                }
              },
              "required": [
                "baseUnit",
                "length"
              ]
            }
          },
          "required": [
            "maximumValue",
            "period"
          ]
        },
        "validityPeriod": {
          "properties": {
            "start": {
              "type": "string"
            },
            "duration": {
              "type": "string",
              "pattern": "^(\\d+y ?)?(\\d+months ?)?(\\d+d ?)?(\\d+h ?)?(\\d+m)?$"
            }
          }
        },
        "applicableGroups": {
          "type": "array",
          "items": {
            "type": "object",
            "properties": {
              "assignerGroup": {
                "type": [
                  "string",
                  "null"
                ]
              },
              "assigneeGroup": {
                "type": [
                  "string",
                  "null"
                ]
              }
            },
            "required": [
              "assignerGroup",
              "assigneeGroup"
            ],
            "additionalProperties": false
          }
        },
        "selfAssignment": {
          "type": "object",
          "properties": {
            "allowedGroups": {
              "type": "array",
              "items": {
                "type": "string"
              }
            }
          },
          "required": [
            "allowedGroups"
          ]
        }
      },
      "additionalProperties": false,
      "required": [
        "acceptTargetDate",
        "selfAssignment"
      ]
    },
    "actions": {
      "type": "object",
      "properties": {
        "sendComms": {
          "type": "boolean"
        },
        "postToFeed": {
          "type": "boolean"
        },
        "givePoints": {
          "oneOf": [
            {
              "type": "object",
              "properties": {
                "lookup": {
                  "type": "object",
                  "properties": {
                    "name": {
                      "type": "string"
                    },
                    "row": {
                      "type": "object",
                      "properties": {
                        "source": {
                          "type": "string",
                          "enum": [
                            "assignee",
                            "assigner",
                            "activity"
                          ]
                        },
                        "store": {
                          "type": "string"
                        },
                        "field": {
                          "type": "string"
                        }
                      },
                      "required": [
                        "source",
                        "field"
                      ],
                      "additionalProperties": false
                    },
                    "column": {
                      "type": "object",
                      "properties": {
                        "source": {
                          "type": "string",
                          "enum": [
                            "assignee",
                            "assigner",
                            "activity"
                          ]
                        },
                        "store": {
                          "type": "string"
                        },
                        "field": {
                          "type": "string"
                        }
                      },
                      "required": [
                        "source",
                        "field"
                      ],
                      "additionalProperties": false
                    }
                  },
                  "additionalProperties": false,
                  "required": [
                    "name",
                    "row",
                    "column"
                  ]
                }
              },
              "additionalProperties": false,
              "required": [
                "lookup"
              ]
            },
            {
              "type": "object",
              "properties": {
                "activityField": {
                  "type": "string"
                }
              },
              "additionalProperties": false,
              "required": [
                "activityField"
              ]
            },
            {
              "type": "object",
              "properties": {
                "fixed": {
                  "type": "integer"
                }
              },
              "additionalProperties": false,
              "required": [
                "fixed"
              ]
            }
          ]
        },
        "giveBonusPoints": {
          "type": "object",
          "properties": {
            "mode": {
              "type": "string",
              "enum": [
                "rolling",
                "once-off"
              ]
            },
            "period": {
              "type": "string",
              "enum": [
                "hour",
                "day",
                "week",
                "month",
                "quarter",
                "year"
              ]
            },
            "activityCount": {
              "type": "integer",
              "minimum": 1
            },
            "points": {
              "type": "object",
              "properties": {
                "fixed": {
                  "type": "integer",
                  "minimum": 0
                }
              },
              "additionalProperties": false,
              "required": [
                "fixed"
              ]
            }
          },
          "additionalProperties": false,
          "required": [
            "mode",
            "period",
            "activityCount",
            "points"
          ]
        }
      },
      "additionalProperties": false,
      "required": [
        "sendComms",
        "postToFeed"
      ]
    },
    "integrations": {
      "type": "object",
      "properties": {
        "slackIncomingWebhook": {
          "type": "object",
          "properties": {
            "isEnabled": {
              "type": "boolean"
            },
            "url": {
              "type": ["string", "null"]
            }
          },
          "required": [
            "isEnabled",
            "url"
          ]
        }
      },
      "additionalProperties": false,
      "required": [
        "slackIncomingWebhook"
      ]
    },
    "webhooks": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "name": {
            "type": "string",
            "pattern": "^[a-zA-Z][a-zA-Z0-9]*$"
          },
          "title": {
            "type": "string"
          },
          "url": {
            "type": "string"
          }
        },
        "additionalProperties": false,
        "required": [
          "name",
          "title",
          "url"
        ]
      },
      "minItems": 1
    }
  },
  "additionalProperties": false,
  "required": [
    "name",
    "title",
    "description",
    "category",
    "schema",
    "restrictions",
    "integrations",
    "actions",
    "deactivated"
  ],
  "definitions": {
    "cron": {
      "type": "string",
      "pattern": "^(28|\\*) (2|\\*) (7|\\*) (1|\\*) (1|\\*)$"
    }
  }
}
```

## Getting activities

### Searching activities
url:
```
https://api.encentivize.co.za/api/programs/{programName}/activities
```

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
```
https://api.encentivize.co.za/api/programs/{programName}/activities/{activityName}
```

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
