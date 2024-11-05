# simple-api-with-expressjs
Creating a simple api with express jd backend. Hopefully will add auth0 authentication
## Create API in express js and add auth0 authentication
### Client Credential Flow Implementation

NOTE - you can write every steps here (https://markdownlivepreview.com/) and later can update readme md

* Go to github and create new repo
* Open gitbash and go to working directory
* Find the Repository URL
* SSH: Looks like git@github.com:username/repository.git
* **git clone git@github.com:MusaPollen/simple-api-with-expressjs.git**
* go to created project directory **cd tab**
* **code .**//opens vscode - minimize gitbash and start working
* Open vscode teminal ctrl+shift+~
## Initial Setup
* Install Node first if not installed
  * Check if all installed first **node -v** and **npm -v**
  * **npm init**//creates a new package.jsonwhich is a crucial component for managing a Node.js application or library
  * Follow through the prompt
  * Accept everything as a beginner
* **npm install express** //install express
* 

## Hello World
* Create new file in root. app.js
* Paste 
```
const express = require('express')
const app = express()
const port = 3001

app.get('/', (req, res) => {
  res.send('Hello World!')
})

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
})
```
* Run ```node app.js```
* Faced some issue. Was not starting. Then changed the port number and was working after that
  * (Sources link - https://expressjs.com/en/starter/installing.html , https://expressjs.com/en/starter/hello-world.html )
  * CHAT GPT SAID THAT IT IS A WORKING API. SO NO EXTRA STEPS
  * Test using - http://localhost:3001/
  * Also use post man
  * 
  
## Integrate AUTH0
(https://auth0.com/docs/quickstart/backend/nodejs/interactive and CHATGPT)
* Create new api in auth0 dashboard
* Ignore permission for now
* ```npm install --save express-oauth2-jwt-bearer``` install
* Add these below part 
```
const { auth, requiredScopes } = require('express-oauth2-jwt-bearer');
// Authorization middleware. When used, the Access Token must
// exist and be verified against the Auth0 JSON Web Key Set.
const checkJwt = auth({
  audience: 'http://localhost:3001/',
  issuerBaseURL: `https://dev-clkrligodugy76hh.us.auth0.com/`,
});
```
* Add these as well - two endpoints. Ignored scoped part
```
// This route doesn't need authentication
app.get('/api/public', function(req, res) {
  res.json({
    message: 'Hello from a public endpoint! You don\'t need to be authenticated to see this.'
  });
});

// This route needs authentication
app.get('/api/private', checkJwt, function(req, res) {
  res.json({
    message: 'Hello from a private endpoint! You need to be authenticated to see this.'
  });
});

```
* Run app and check if api is working
* GO to postman
  * Used postman because it seemed easier.
  * And the steps was suggested by chatgpt. Modified as per my requirement.
* Click on the "New" button or use the "+" tab to create a new request.
* In the request tab, select "POST"
* Enter url - https://dev-clkrligodugy76hh.us.auth0.com/oauth/token //your auth0 token issue link
* Click on headers tab.
  * In Key column enter key = Content-Type
  * And value = application/json
* Click on the "Body" tab
  * Select "raw" and ensure the dropdown next to it is set to "JSON".
  * Paste below
  ```
  {
  "client_id": "uoGdLK6DtiUe7TJObjy2H7qvW3WgSAvU",
  "client_secret": "DxxcOcE8eoj5JZ-DX2DgnBdmbFVrvdfwwm1KDLJWxX9ER8cL5qYhrgFiVh7rJbxP",
  "audience": "http://localhost:3001/",
  "grant_type": "client_credentials"
  }

  ```
  * The values can be found in Test tab on auth0 selected api
  * Moreover upon experiment i saw these values are embedded in Application(LEFT TAB) > your newly created API(Test Application) which is a machine to machine app. I think it was automatically created. Just click this application and you will find the values.
  * Click on the "Send" button to execute the request.
  * Found a response
  ```
  {"access_token":"eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6Ikw5R1RYa3o1MFZtM2VUbGxKamh4dCJ9.eyJpc3MiOiJodHRwczovL2Rldi1jbGtybGlnb2R1Z3k3NmhoLnVzLmF1dGgwLmNvbS8iLCJzdWIiOiJ1b0dkTEs2RHRpVWU3VEpPYmp5Mkg3cXZXM1dnU0F2VUBjbGllbnRzIiwiYXVkIjoiaHR0cDovL2xvY2FsaG9zdDozMDAxLyIsImlhdCI6MTczMDQ1MzEyNSwiZXhwIjoxNzMwNTM5NTI1LCJndHkiOiJjbGllbnQtY3JlZGVudGlhbHMiLCJhenAiOiJ1b0dkTEs2RHRpVWU3VEpPYmp5Mkg3cXZXM1dnU0F2VSJ9.Q-c9pBRvru_0SH9v7OOHKFjvDHRxhjqdNGb6DP88SN0UW3Rj4B_xEJaKQVLh7Cx-DGC9OH2DTBqkmhI4DzK8qjrt0cAqd4_C3wB6RTrTUofn_4OMhGxhBAFpGGhv_cwhUFBbYz_qqjbORuXP9U_4krR3RupT54tDpQzv42rYdhQE9SlfwZxsuBGoKBYrXsyBATYBDvOwYrxY858iWuzB_HW-3Axo0grZbOZursjjPKwYFz6IEvYfMaDE2v1BF-ZDnVsjO1TUoLC0zDV2n-2zQz3Hdhg3DLj73GZssGwJB2ryFyUIS3wwInWkpPoU2aGUbbVBUuxpBP0lb7kS5tVLsA","expires_in":86400,"token_type":"Bearer"}
  ```
 * Now go to postman
 * Plus +
 * Add url - private api link //trying to access private resource now
 * Go to the "Headers" tab.
 * Add a new key-value pair:
   * Key: Authorization
   * Value: Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6Ikw5R1RYa3o1MFZtM2VUbGxK……………..dot dot 
 * Send REQ
 * **Done** . you should get acces of private api now.

 

