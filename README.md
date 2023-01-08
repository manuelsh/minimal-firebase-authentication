# Minimal Firebase sign in page using FirebaseUI

This is a minimal html and JavaScript code to activate the authorisation functions for signing using Firebase and FirebaseUI.

The motivation of this repo is that the example in the original [Firebase UI repo](https://github.com/firebase/firebaseui-web) does not work by itself.

## Requirements

1. Create a Firebase project and enable the authentication method you want to use.
2. Edit the login.html file by replacing the configuration parameters with your own.

## Integration with webflow

To make it work with webflow, you need to:

1. Add an HTML Embed element to your webflow page, with the following code:

```html
<div id="firebaseui-auth-container"></div>

<script type="text/javascript">
  // FirebaseUI config.
  var uiConfig = {
    signInSuccessUrl: "<url-to-redirect-to-on-success>",
    signInOptions: [
      // Leave the lines as is for the providers you want to offer your users.
      // Remember to activate them in the Firebase console.
      firebase.auth.GoogleAuthProvider.PROVIDER_ID,
      firebase.auth.FacebookAuthProvider.PROVIDER_ID,
      firebase.auth.TwitterAuthProvider.PROVIDER_ID,
      firebase.auth.GithubAuthProvider.PROVIDER_ID,
      firebase.auth.EmailAuthProvider.PROVIDER_ID,
      firebase.auth.PhoneAuthProvider.PROVIDER_ID,
      firebaseui.auth.AnonymousAuthProvider.PROVIDER_ID,
    ],
    // tosUrl and privacyPolicyUrl accept either url string or a callback
    // function.
    // Terms of service url/callback.
    tosUrl: "<your-tos-url>",
    // Privacy policy url/callback.
    privacyPolicyUrl: function () {
      window.location.assign("<your-privacy-policy-url>");
    },
  };
  // App's Firebase configuration
  // Add your own config here
  const firebaseConfig = {
    //...
  };

  // Initialize Firebase
  const app = firebase.initializeApp(firebaseConfig);

  // Initialize the FirebaseUI Widget using Firebase.
  var ui = new firebaseui.auth.AuthUI(firebase.auth());

  // The start method will wait until the DOM is loaded.
  ui.start("#firebaseui-auth-container", uiConfig);
</script>
```

2. In the Project Settings, on the Custom Code tab, add the following code to the `Head Code`:

```html
<script src="https://www.gstatic.com/firebasejs/9.13.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.13.0/firebase-auth-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/ui/6.0.2/firebase-ui-auth.js"></script>
<link
  type="text/css"
  rel="stylesheet"
  href="https://www.gstatic.com/firebasejs/ui/6.0.2/firebase-ui-auth.css"
/>
```

3. In the Project Settings, on the Custom Code tab, add the following code to the `Footer Code` in order to keep track of users logged in in the private area of your website:

```html
<script>
  const firebaseConfig = {
    //...
  };

  // Initialize Firebase
  const app = firebase.initializeApp(firebaseConfig);

  firebase.auth().onAuthStateChanged((user) => {
    // publicElements are elements that sould not be shown when you are private
    let publicElements = document.querySelectorAll("[login_type='public']");
    // privateElements are elements that sould not be shown when you are public (e.g. private area
    let privateElements = document.querySelectorAll("[login_type='private']");

    if (user) {
      // User is signed in, see docs for a list of available properties

      const uid = user.uid;

      privateElements.forEach(function (element) {
        element.style.display = "initial";
      });

      publicElements.forEach(function (element) {
        element.style.display = "none";
      });

      console.log(`The current user's UID is equal to ${uid}`);
      // ...
    } else {
      // User is signed out
      publicElements.forEach(function (element) {
        element.style.display = "initial";
      });

      privateElements.forEach(function (element) {
        element.style.display = "none";
      });
      // ...
    }
  });
</script>
```

4. In order to hide elements that should be shown only to logged in users, add the Custom Attribute `login_type="private"` to the elements you want to hide. To see how to do that in webflow, check [this link](https://university.webflow.com/lesson/custom-attributes).

## Credits

- To Garrett Mack and his [video tutorial](https://www.youtube.com/watch?v=LIuS2L3cIiQ&ab_channel=GarrettMack). The last part of the code to keep track of users logged in is based on his code.
- Firebase UI repo, which doesn't work straight out of the box
- [Webflow](https://webflow.com) for the amazing tool to create websites and specially their great and trully funny tutorials

## Contributing

As I imagine new versions of FirebaseUI will be released, contributions are welcome to keep this repo up to date.

## TODO

- [  ] Add a sign out button
