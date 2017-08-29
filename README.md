## Introduction

This is a Quickstart guide to firebase and how to use it. 

## Things you need to know

* JavaScript
* JSON
* HTML (basics)

## ![alt text](https://www.gstatic.com/mobilesdk/160503_mobilesdk/logo/2x/firebase_28dp.png) Console Setup

First create a [Gmail](https://gmail.com/) account and visit the **Firebase [Console](https://console.firebase.google.com)**. You will be asked to login if you aren't already logged in.

If logged in, Follow these steps: 
* Click on **Create New Project**
* Name your project 
* The *Project ID* is unique and thats what the firebase console uses to identify your project. If your project name is *test123* the *Project ID* will be something like *test123-78d5f*, But you can modify that to your own will. 
* Click on **Create Project**, This will create your project. 

## Initialize firebase 

You need to know basics of HTML and JavaScript from now on. If we go to the **Authentication Tab** on the left hand side of the **Firebase Console**, You can see a button named **Web Setup**. Click on that, We'll see something like: 

```javascript
<script src="https://www.gstatic.com/firebasejs/4.3.0/firebase.js"></script>
<script>
  // Initialize Firebase
  var config = {
    apiKey: "<API-KEY>",
    authDomain: "<PROJECT-ID>.firebaseapp.com",
    databaseURL: "https://<PROJECT-ID>.firebaseio.com",
    projectId: "<PROJECT-ID>",
    storageBucket: "<PROJECT-ID>.appspot.com",
    messagingSenderId: "<MESSAGING SENDER ID>"
  };
  firebase.initializeApp(config);
</script>
```

Create a new `.html` file and paste this in that file. Say you named it: `app.html`, That file's basic set up should look something like: 

```html 
<html>
  <head>
    <title>My App</title>
  </head>
  
  <body>
    <div id="wrapper"></div>
  </body>
  
  <script src="https://www.gstatic.com/firebasejs/4.3.0/firebase.js"></script>
  <script>
    // Initialize Firebase
    var config = {
      apiKey: "<API-KEY>",
      authDomain: "<PROJECT-ID>.firebaseapp.com",
      databaseURL: "https://<PROJECT-ID>.firebaseio.com",
      projectId: "<PROJECT-ID>",
      storageBucket: "<PROJECT-ID>.appspot.com",
      messagingSenderId: "<MESSAGING SENDER ID>"
    };
    firebase.initializeApp(config);
    
    //REST OF THE CODE GOES HERE
    
  </script>
</html>
```

Whatever code that I am going to explain next goes after **REST OF THE CODE GOES HERE** area.

## Simplifying Firebase as much as possible

There are **3 MAIN** functionalities to Firebase. They are: 
* Authentication 
* Database (Real-Time)
* Storage

These are the three main functionalities of Firebase. The line `<script src="https://www.gstatic.com/firebasejs/<version>/firebase.js"></script>` provides us with a javascript file that contains the `firebase` class, Which contains all these 3 methods / functionalities. 

We can access those 3 methods / functionalities by access them through the firebase class.
* For Authentication: 
```javascript
var auth = firebase.auth()
```

* For real-time database
```javascript
var database = firebase.database()
```

* For storage
```javascript
var storage = firebase.storage()
```

Not the variables `auth`, `database` and `storage` stores the 3 main functionalities of firebase.
Now the code will look something like: 

```html 
<html>
  <head>
    <title>My App</title>
  </head>
  
  <body>
    <div id="wrapper"></div>
  </body>
  
  <script src="https://www.gstatic.com/firebasejs/4.3.0/firebase.js"></script>
  <script>
    // Initialize Firebase
    var config = {
      apiKey: "<API-KEY>",
      authDomain: "<PROJECT-ID>.firebaseapp.com",
      databaseURL: "https://<PROJECT-ID>.firebaseio.com",
      projectId: "<PROJECT-ID>",
      storageBucket: "<PROJECT-ID>.appspot.com",
      messagingSenderId: "<MESSAGING SENDER ID>"
    };
    firebase.initializeApp(config);
    
    var auth = firebase.auth(); 
    var database = firebase.database(); 
    var storage = firebase.storage(); 
    
  </script>
</html>
```

## FIREBASE AUTHENTICATION (ONLY EMAIL AND PASSWORD)

The reason why I am only doing the **EMAIL AND PASSWORD** way is because its the most flexible. 
Keep 2 things open, the `app.html` and the **Authentication Tab** in your firebase console. In the `auth` variable that we just created, it has **4 MAIN** functionalites, and their methods are: 
* Sign a user into the website and save the session 
```javascript 
auth.signInWithEmailAndPassword(email, password); 
```
* Create a user and add them to the authentiation tab 
```javascript 
auth.createUserWithEmailAndPassword(email, password); 
```
* Sign a user out of the website and delete the session 
```javascript 
auth.signOut(); 
```
* Keep checking for Authentication changes on the website
```javascript 
auth.onAuthStateChanged(function(u){});
```

#### Here's a simple code for Sign In: 
```
var username = "myname@rijin.com";
var password = "mynewpassword"; 
auth.signInWithEmailAndPassword(username, password);
```

This will sign the user in, **IF** the user exists, If the user doesnt exist, you can see an *uncaught* error on the `console.log` area of your website. You have to *catch* the error by adding. This catch eill catch all the errors including *wrong email format*. 

```javascript
...
auth.signInWithEmailAndPassword(username, password).catch(function(err){
  alert(err.message); 
});
```

#### Here's a simple code for creating a new user: 
```javascript
var email = "ned_stark@gmail.com";
var password = "jonsnowisnotmychild";

auth.createUserWithEmailAndPassword(email,password).catch(function(err){
  console.log(err.message); 
}); 
```

#### What is onAuthStateChanged? 

This is one of the key functions of the `auth` class. This function if there is **ANY CHANGE** to the authetication change. It fires up as soon as the page is loaded, as soon as a user is created, as soon as a person signs in... anything related to authentication this method fires up. This method takes a function as an argument that in-turn has takes another argument of `user` within it. 

```javascript
auth.onAuthStateChanged(function(user){
  if(user){
    //Code for when the user is signed in
  }else{
    //Code for when the user is signed out
  }
}); 
```

The `user` variable, in the scope of the function contains every detail of the user, Their Email-ID, Display Name, Display Image etc, But most importantly it contains the USER ID or the **UID**. You can get the UID by `user.uid`.






 
