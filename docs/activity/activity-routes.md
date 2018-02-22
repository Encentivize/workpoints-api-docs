## Activity routes

- [Query](#querying-activities)
- [Create](#creating-an-activity)
- [Update](#updating-an-activity)
- [Award](#awarding-an-activity)

### Creating an activity

url:
```https://api.encentivize.co.za/api/programs/{programName}/activities```

method:
```POST```

request schema: [activity](../../schema/member/member-create-schema.json)

example request body:
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

[activity schema](../../schema/activity/activity-create-schema.json)

### Updating an activity

url:
```https://api.encentivize.co.za/api/programs/{programName}/activities/{activityName}```

method:
```PUT```

request schema: [activity](../../schema/activity/activity-create-schema.json)

example request body:

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

> If your activity is setup to accept a target date you can pass one into the request url.
{targetDate} is optional.

method: 
```POST```

example request body:
```json
{
  "motivation": 1
}
```

Any custom activity fields that have been created can be filled in in request body.
