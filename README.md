# Ember Firebase Google Login

An example Ember app featuring a Google login component for Firebase.

Both these features are tricky for those new to Ember. This example serves as an educational tool with poachable code for your app.

It is explicitly detailed for those new to Ember.

## Working Example

[Firebase hosted app.](https://auth-test-25504.firebaseapp.com)

## Introduction
##### In the end ...
After following *all* the instructions below, your app will:
  * be hosted on Firebase for free
  * be linked to it's own Firebase database
  * look and behave like the example app
  * have poachable code to incorporate into your own app

##### Database Structure
The login feature creates user entries in the database with the following default data and structure:
```JSON
{
  "users" : {
    "$uid" : {
      "items" : [
      {
        "title" : "Item 1",
        "text" : "Text 1"
      }, {
        "title" : "Item 2",
        "text" : "Text 2"
      }, {
        "title" : "Item 3",
        "text" : "Text 3"
      } ],
      "settings" : [ {
        "fontSize" : 10,
        "lastLogin" : "$timeStamp",
        "theme" : "light"
      } ]
    }
  }
}
```

This structure has advantages:
* Users have their own db entry with key $uid that is used by both Firebase's Authentication rules and the app's Torii session.
* Better for NoSQL queries.
* Firebase rules limit each user's db access to only their own data.
* [Firebase documented.](https://firebase.google.com/docs/database/security/user-security#section-structuring-db)

## Installation
### Overview
* Prerequisites
* Get the Code
* Setup and Link to Firebase
* Poach and Customize

###### Versions
* Ember      : 2.10.2
* Ember Data : 2.11.3
* Firebase   : 3.7.1
* EmberFire  : 0.0.0
* jQuery     : 3.1.1

$ - means "at the command prompt in a terminal type this".

### Prerequisites
Ensure the following is properly installed on your computer:
* [Git](https://git-scm.com/)
* [Node.js](https://nodejs.org/) (with NPM)
* [Bower](https://bower.io/)
* [Ember CLI](https://ember-cli.com/)
* [PhantomJS](http://phantomjs.org/)

### Get the Code
Get the Ember code from Github:
1. $ git clone https://github.com/dirkdirk/ember-firebase-google-login.git
<br>or<br>Download from [Github](https://github.com/dirkdirk/ember-firebase-google-login)
1. $ npm install && bower install

### Setup and Link to Firebase
Firebase:
1. Log into your Firebase console.
1. 'Create New Project' with any name.
1. Click 'Add Firebase to your web app' and copy the info.
1. Open `app/config/environment.js` in a text editor and edit `var ENV`'s `firebase` property:
   ```javascript
   firebase: {
      apiKey: "[your pasted info here]",
      authDomain: "[your pasted info here]",
      databaseURL: "[your pasted info here]",
      storageBucket: "[your pasted info here]",
      messagingSenderId: "[your pasted info here]"
   },
   ```
1. Go back to your Firebase console.
1. Click 'Authentication' in left menu.
1. Click 'Sign-In Method' under Authentication title.
1. Enable Google
    * Mouse over Google.
    * Click the pencil.
    * Toggle the 'Enable' switch.
    * Click 'Save'
1. Click 'Rules' under Realtime Database title.
1. Cut and paste the following and click 'Publish'
   ```JSON
   {
     "rules": {
       "users": {
         "$uid": {
           ".read": "auth != null && auth.uid == $uid",
           ".write": "auth != null && auth.uid == $uid"
         }
       }
     }
   }
   ```
1. You should see a working app just like the [working example](https://auth-test-25504.firebaseapp.com) when pointing a browser to: https://localhost:4200
1. If not, review each step above and fix the mistake. If it's unfixable, open a Issue on Github to get help.

### Poach and Customize
1. The best way to incorporate these features into your app is copy and paste. This allows for easy modification the code to fit your specific use case.
1. You'll need everything in:
    * `app/adapters/application.js`
    * `app/components/login-component.js`
    * `app/models/*.js`
    * `app/routes/*`
    * `app/torii-adapters/application.js`
1. You'll need portions of the contents of these files:
    * `app/templates/*`
    * `app/router.js`
    * `app/styles/app.css`
    * `config/environment.js` - Ensure `ENV`'s `firebase` and `torii` properties are configured properly:
      ```javascript
      firebase: {
        apiKey: "[your pasted info here]",
        authDomain: "[your pasted info here]",
        databaseURL: "[your pasted info here]",
        storageBucket: "[your pasted info here]",
        messagingSenderId: "[your pasted info here]"
      },
      torii: {
        sessionServiceName: 'session'
      },
      ```
1. Next edit the code to fit your fantastic app.

### Build and Deploy
##### First time
1. $ npm install -g firebase-tools
1. $ ember build
1. $ firebase login
1. $ firebase init
   * This step is only required once per app Firebase setup.
   * Select Hosting only.
   * Select the database you set up above.
   * Type "dist" as your public directory.
   * Answer "n" to "Configure as a single-page app?"
   * Answer "n" to "File dist/index.html already exists. Overwrite?"
1. $ firebase deploy
   * A Hosting URL will be displayed if all goes well.
   * Point a browser at this URL to see your app in action.

##### Thereafter ...
1. $ ember build
1. $ firebase deploy

### Manual Testing
1. To test repeated initial logins from the same Google account, all traces of the initial login must be destroyed.
   * In the browser click "Log out".
   * Open the browser's dev tools (in most browsers right click and select "inspect element").
   * Find where the cookies and local storage data are viewable and delete all local storage entries.
   * Go to your Firebase console and delete the user from the Authentication users section.
   * Optionally delete the user's data from the Database data section.
1. Go back the app in the browser and click "Log in". A fresh user id and database entry will be created in Firebase. If not, see the console in dev tools and read the errors.
   * `login-component.js` has many `console.log` calls making it easy to follow what the app is attempting to do and track down where errors occur.

### Run Tests
1. $ ember test
1. $ ember test --server

## Help
* If you need help, open an Issue on Github.
* I need help too:
   * I'm not too fond of the `.observes('sessions.isAuthenticated')` call in `routes/application.js`. Is there a better way to force a `refresh()`?
   * Any other advice and suggestions are more than welcome. Please open an Issue or Pull Request.

## Further Reading / Useful Links
* [ember.js](http://emberjs.com/)
* [ember-cli](https://ember-cli.com/)
* [Firebase](https://firebase.com/)
* [Firebase Web Docs](https://firebase.google.com/docs/web/setup)
