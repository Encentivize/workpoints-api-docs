# Workpoints API

## Table of Contents

- [Neuron Auth](#getting-a-neuron-token)
- [User Routes](docs/user/user-routes.md#user-routes)
    - [Query](docs/user/user-routes.md#querying-users)
    - [Create](docs/user/user-routes.md#creating-a-user)
    - [Update](docs/user/user-routes.md#updating-a-user)
    - [Activate](docs/user/user-routes.md#activating-a-user)
    - [Deactivate](docs/user/user-routes.md#deactivating-a-user)
- [Activity Routes](docs/activity/activity-routes.md#activity-routes)
    - [Query](docs/activity/activity-routes.md#querying-activities)
    - [Create](docs/activity/activity-routes.md#creating-an-activity)
    - [Update](docs/activity/activity-routes.md#updating-an-activity)
    - [Award](docs/activity/activity-routes.md#awarding-an-activity)

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
