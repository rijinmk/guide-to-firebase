## Introduction

This is a Quickstart guide to firebase and how to use it. 

## Prerequisites 

* JavaScript
* JSON
* HTML (basics)

## ![alt text](https://www.gstatic.com/mobilesdk/160503_mobilesdk/logo/2x/firebase_28dp.png) Console Setup

First create a [Gmail](https://gmail.com/) account and visit the **Firebase [Console](https://console.firebase.google.com)**. You will be asked to login if you haven't already.

If logged in, Follow these steps: 
* Click on **Create New Project**
* Name your project 
* The *Project ID* is unique and that's what the firebase console uses to identify your project. If your project name is *test123* the *Project ID* will be something like *test123-78d5f*. The password can be modified thereafter. 
* Click on **Create Project** . This will create your project. 

## Initialize firebase 

You need to know the basics of HTML and JavaScript from here on. 
On going to the **Authentication Tab** on the left hand side of the **Firebase Console**, You can see a button named **Web Setup**. Click on that, You'll see something like: 

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

Create a new `.html` file and paste this in that file. Suppose you named it: `app.html`, That file's basic set up should look something like: 

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

The code that I am going to explain next goes after **REST OF THE CODE GOES HERE** area.

## Simplifying Firebase to the maximum

There are **3 MAIN** functionalities to Firebase. They are: 
* Authentication 
* Database (Real-Time)
* Storage

 The line `<script src="https://www.gstatic.com/firebasejs/<version>/firebase.js"></script>` provides us with a javascript file that contains the `firebase` class, Which contains all these 3 methods / functionalities. 

We can access those 3 methods / functionalities by accessing them through the firebase class.
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

Note,  the variables `auth`, `database` and `storage` stores the 3 main functionalities of firebase.
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

The reason I'm showing the **EMAIL AND PASSWORD** way is because it is the most flexible. 
Keep 2 things open, the `app.html` and the **Authentication Tab** on your firebase console. In the `auth` variable that we just created, there are **4 MAIN** functionalites, and its operations are to: 
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

This will sign the user in, and **IF** the user exists, you can see an *uncaught* error on the `console.log` area of your website. You have to *catch* the error by adding. This catch will catch all the errors including *wrong email format*. 

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

This is one of the main functions of the `auth` class. This function is used if there is **ANY CHANGE** to the authetication change. It fires up as soon as the page is loaded, as soon as a user is created, as soon as a person signs in... On coming across anything related to authentication, this method fires up. This method takes a function as an argument that in-turn has takes another argument of `user` within it. 

```javascript
auth.onAuthStateChanged(function(user){
  if(user){
    //Code for when the user is signed in
  }else{
    //Code for when the user is signed out
  }
}); 
```

The `user` variable, in the scope of the function contains every detail of the user - Their Email-ID, Display Name, Display Image etc, But most importantly it contains the USER ID or the **UID**. You can get the UID by `user.uid`.

## FIREBASE REAL-TIME DATABASE

Firebase Real-Time Database is basically a JSON structure which can be **referenced** useing the `.ref('path/to/ref/')` method. Here's the basics of references and usign the `.child('path')` to access different parts of the JSON object in the firebase database. 
![Firebase ref & child basics](http://i.imgur.com/ImjcfQw.jpg)

You might be wondering how this might look in code, the entire database is in the form of JSON. Then you REFERENCE `.ref('object')`, We are actually referencing that JSON object in the database. It looks something like this.. 

```json
{
  "DRAW" : {
    "DRAW1" : {
      "mouse" : {
        "x" : 503,
        "y" : 21
      }
    },
    "DRAW2" : {
      "mouse" : {
        "x" : 509,
        "y" : -3
      }
    }
  },
  "PHILOTES" : {
    "users" : {
      "GUN4jva1CGSSpHDEZr0gFqfEU7G3" : {...},
      "Nlh0ZMLgS5RFgW799O7Lzq0TJNt2" : {
        "chatting" : 0,
        "date" : "18:4:18 - 7 September2017",
        "defaultDisplayImage" : "https://cdn.dribbble.com/users/125948/screenshots/1924815/profileicondribbble_1x.png",
        "displayImage" : "Nlh0ZMLgS5RFgW799O7Lzq0TJNt2/19598973_10155838951560628_5743553805312788830_n.jpg",
        "email" : "rijin.mk9@gmail.com",
        "fullname" : "Rijin Mk",
        "live-drawing" : {
          "mouse" : {
            "x" : -22.5,
            "y" : 370.25
          }
        },
        "live-message" : {
          "GUN4jva1CGSSpHDEZr0gFqfEU7G3" : {
            "current-text" : "HAHAHAAHAH"
          },
          "RKXDC6e6o1bIVijGqKSFsedsf8u2" : {
            "current-text" : "BAIIII. "
          },
          "cHFNau7tvdV4e5jPDyluBKELiuf2" : {
            "current-text" : "Whats up"
          },
          "gt1EGNM7QjVlD6unY9AmRdbMhIC3" : {
            "current-text" : "What................."
          },
          "oJiCU6deKMV5SWpF8dDkV01zERZ2" : {
            "current-text" : "Hey"
          }
        },
        "username" : "rijinmk"
      },
      "RKXDC6e6o1bIVijGqKSFsedsf8u2" : {
        "chatting" : 0,
      ...
 ```
 
 ### GETTING DATA FROM THE FIREBASE DATABASE
 
There are two main methods to get data from the database. `.on('event',callback)` and `.once('event',callback)`. This method should be called on the reference. When you call `.on()` or `.once()` on a specific reference (`.ref('path').on()`) on the database. 
 
#### The "REALTIMEness" of Firebase Database

The realtime-ness of the Firebase Database is all on this magnificient method `.on('event',callback)`. The `callback` is triggered whenever that `event` is happening and `.on()` makes it realtime, `.once()` as its name suggest is only called once. 

To make a call on a specific part of the JSON object in the database, We have the following sytax: 

```javascript
var database = firebase.database(); 
var ref = database.ref('path/to/object');

ref.on('value',function(data){
  console.log(data.val()); 
}); 
```

The `event` here is `value`, There are several other events, You can find it [here](https://www.tutorialspoint.com/firebase/firebase_event_types.htm). The callback takes one argument, That is the `data`, Which contains all the information about the contents on that JSON object that has been pointed by the reference. The `.val()` method used on `data` will give out all the information about the recived data, In the form of a Javascript Object. 








 
