First sign up [here](https://appcenter.ms/create-account) 
When logged in click on `Add New` to add a new app as android or ios, select production as release type and react native;
After creating an app, click on it  Go to Distribute => CodePush and then click on the Create standard deployments button. App Center will create two environments, Staging and Production, we will only use Production in this guide.

It tells you install appcenter cli with `npm install -g appcenter-cli` then log in like so 
- Run appcenter login. This will open a browser and generate a new API token.
- Copy the API token from the browser, and paste it into the command window.
- The command window will display Logged in as {user-name}.
Congratulations! You’ve successfully logged in and can run CLI commands.