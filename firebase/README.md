### Notes

#### Integrating Firebase in JavaFX project
1. User Java Admin SDK
2. Do not create inputstream from json file. It will be easier for hackers to hack your data using this json file.
3. Copy the data from json file and store as string in the application. And [create](https://stackoverflow.com/questions/5720524/how-does-one-create-an-inputstream-from-a-string) inputstream from the string.
4. Enjoy....


#### You may follow the following steps to generate a valid access token using Node.JS in Firestore REST:

1. Download your project's service account's credentials (JSON format).
2. Copy this code block below:
    ```javascript
    var google = require('googleapis');

    var serviceAccount = require("./serviceAccountKey.json");

    var jwtClient = new google.auth.JWT(serviceAccount.client_email, null, serviceAccount.private_key, [
        "https://www.googleapis.com/auth/datastore",
        "https://www.googleapis.com/auth/cloud-platform"
    ]);

    jwtClient.authorize(function(err, tokens) {
     console.log("Token " + tokens.access_token);
    });
    ```
3. To run Node.JS file, execute this command: node <YourNodeFilename>. This should print/return valid access key.
