# Introduction

Workpoints 

Authentication with neuron-synapse
install neuron-synapse
ROUTES

Your app may access the following routes. Replace {{programName}} with the name of the program that you app is linked to.

# Program

## Get 

Returns a program details object with info such a name, description and timezone

method: 
`GET`
url: 
`https://api.encentivize.co.za/api/programs/{{programName}}`

# Members (People)

## Query all members

Returns an array of members on the program

method GET
url: 
`https://api.encentivize.co.za/api/programs/{{programName}}/members`
query string: any field on the member object, and the following optional parameters that assist with pagination 

`skip={{number}}&limit={{number}}`


## Get a particular member

method: GET
url: `https://api.encentivize.co.za/api/programs/{{programName}}/members/{{memberId}}`
parameters:
`{{memberId}}` : the case-sensitive memberId field from the member object

me
method: GET
url: https://api.encentivize.co.za/api/programs/{{programName}}/members/me

# Activities

query

method: GET
url: https://api.encentivize.co.za/api/programs/{{programName}}/activities
query: any field on the activity object, and the following optional parameters that assist with pagination { skip: number, limit: number }
activity

method: GET
url: https://api.encentivize.co.za/api/programs/{{programName}}/activities/{{activityName}}
parameters:
{{activityName}}: case-sensitive name of the activity
assign

method: POST
url: https://api.encentivize.co.za/api/programs/{{programName}}/members/{{memberId}}/activities/{{activityName}}
parameters:
{{memberId}}: the memberId field of the member who will receive this activity
{{activityName}}: case-sensitive name of the activity
body: defined on the schema.properties field of the activity that you wish to assign