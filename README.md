# Introduction

Workpoints uses an OAuth 2.0 server for app integrations, called Neuron. 


Your app may access the following routes. Replace {{programName}} with the name of the program that you app is linked to.

# Program

## Get 

Returns a program details object with info such a name, description and timezone

Verb: GET

url 
https://api.encentivize.co.za/api/programs/:programName

# Members (People)

## Query all members

Returns an array of members on the program

Verb : GET

Url: 
https://api.encentivize.co.za/api/programs/:programName/members

Query string: any field on the member object, and the following optional parameters that assist with pagination:

`skip={number}&limit={number}`


## Get a particular member

Verb: GET

Url: https://api.encentivize.co.za/api/programs/:programName/members/:memberId

Parameters:
memberId : the case-sensitive memberId field from the member object

## Get the current user

Method: GET

Url: https://api.encentivize.co.za/api/programs/:programName/members/me

# Activities

Activities are records of tasks or achievements assigned to programme members. These can be various things, eg. Completing Timesheets, Peer Recognition, Completing a Course, Client Recommendation etc. 

## Query all activities

Method: GET
Url:
https://api.encentivize.co.za/api/programs/:programName/activities
query: any field on the activity object, and the following optional parameters that assist with pagination { skip: number, limit: number }
activity

Method: GET
Url: https://api.encentivize.co.za/api/programs/:programName/activities/:activityName
Parameters:

activityName: case-sensitive name of the activity

## Assign an activity

Verb: POST

Url: https://api.encentivize.co.za/api/programs/:programName/members/:memberId/activities/:activityName

Parameters:

memberId: the memberId field of the member who will receive this activity

activityName: case-sensitive name of the activity

POST body: This is activity specific and user configurable. Fields are defined on the schema.properties field of the activity that you wish to assign