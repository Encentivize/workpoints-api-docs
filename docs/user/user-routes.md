## User Routes

- [Query](#querying-users)
- [Create](#creating-a-user)
- [Update](#updating-a-user)
- [Activate](#activating-a-user)
- [Deactivate](#deactivating-a-user)

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

### Creating a user

Users are created asynchronously in the background by Workpoints. This means that they won't be immediately available once you make the request to create them. They should be available after 30 seconds of making the request.

url:
```https://api.encentivize.co.za/api/programs/{programName}/members```

method:
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

[member schema](../../schema/member/member-create-schema.json)

### Updating a user

url:
```https://api.encentivize.co.za/api/programs/{programName}/members/{memberId}```

method:
```PUT```

request body: TODO

### Activating a User

url:
```https://api.encentivize.co.za/api/programs/{programName}/members/{memberId}/activate```

method:
```PUT```

request body: empty

### Deactivating a User

url:
```https://api.encentivize.co.za/api/programs/{programName}/members/{memberId}/deactivate```

method:
```PUT```

request body: (set the resetWallet field to true if you want to reset the persons points to zero)
```json
{
  "resetWallet": false
}
```
