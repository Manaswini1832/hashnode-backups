## Integrate Firebase into a React application

### Quick intro

Firebase integrates very well with a React application. In this post, we'll do exactly that. Provide one instance of Firebase to the entire React application, which can then be used to access other services that Firebase provides, like Cloud Firestore, Firebase Real time Database, Firebase Authentication and so on...

Whatever I've mentioned in this post is also available as a video on YouTube (This post is shown as a video in part 1)

%[https://youtu.be/TymgmAe6WNg]

### What we are trying to achieve

Normally in a Vanilla JavaScript application, we initialize the Firebase SDK by including it in our JavaScript file right at the beginning and then proceed to use the **firebase** namespace to access other Firebase services. This is shown below

```
const firebaseConfig = {
  apiKey: "API_KEY",
  authDomain: "PROJECT_ID.firebaseapp.com",
  databaseURL: "https://PROJECT_ID.firebaseio.com",
  projectId: "PROJECT_ID",
  storageBucket: "PROJECT_ID.appspot.com",
  messagingSenderId: "SENDER_ID",
  appId: "APP_ID",
};

``` 
All this comes from the official [Firebase docs](https://firebase.google.com/docs/web/setup)

Now you might ask, why can't we do the same thing inside a React app, trying to initialize it at the beginning of every component file and then use it ? Fair enough. But doing that gives us this error, glaring at us!


```
Firebase App named '[DEFAULT]' already exists (app/duplicate-app)
``` 
Now that happens because, we are trying to initialize the Firebase SDK more than once and that won't work. 

So what we are trying to solve is to come up with a way to initialize Firebase only once and then be able to use it in our entire React application.

Sounds good? Let's go!

### What we'll be using

1. Facebook's official React boilerplate project : [create-react-app](https://reactjs.org/docs/create-a-new-react-app.html)

2. Functional components in React

3. [React's Context API](https://reactjs.org/docs/context.html)

### Basic Setup

```
npx create-react-app react-firebase
cd react-firebase
``` 
Now open this directory in your text editor of choice. I'm using VS Code here. 

For the sake of this post, I want to keep things simple so I'll be deleting the entire src folder and will create one from scratch. 

After creating a new src folder, navigate into it and type the following commands into the terminal ( Make sure you are inside the react-firebase directory before typing the following commands )

```
touch index.js
mkdir components
cd components
touch App.jsx
``` 
What we did was, inside the src folder, we created an index.js file and a components directory containing the App.jsx file.

Now we enter the following lines of code into the index.js file

```
import React from "react";
import ReactDOM from "react-dom";
import App from "./components/App";

ReactDOM.render(<App />, document.getElementById("root"));
``` 
Then create the App component. Inside the App.jsx file, enter the following

```
import React from "react";

const App = () => {
    return(
        <h1>Hello from the App component</h1>
    );
}

export default App;
``` 
### Firebase setup

Now it's time to get our Firebase project settings. For this, follow only steps 1 and 2 from [this](https://firebase.google.com/docs/web/setup) page. 

Now navigate to your project settings and copy only the config object. No need to copy the extra scripts that are mentioned there. The project settings might be visible on clicking on the settings icon in the left navigation bar as shown below

![project-settings.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1611818068934/-rPzorvqS.png)
We'll need this config object while creating the Firebase instance later on.

### Integrating Firebase with React

Before we go any further, let's install the firebase package using npm or yarn. 

```
npm install firebase --save
``` 
Now, create a directory called **contexts** inside the src folder. This will contain our FirebaseContext which will help us provide a single instance of the Firebase class, which we'll create in a moment, to the entire application.

### Creating the Firebase class

Navigate into the contexts directory and type the following commands into the terminal.

```
mkdir FirebaseContext
cd FirebaseContext
touch Firebase.js
touch FirebaseContext.js
``` 
So we have a folder called FirebaseContext containing the Firebase.js and the FirebaseContext.js files. 

Go into the Firebase.js file and include the following lines of code in it.

```
import firebase from "firebase/app";

//The firebaseConfig object that you copied should be entered below
const firebaseConfig = {
  apiKey: "API_KEY",
  authDomain: "PROJECT_ID.firebaseapp.com",
  databaseURL: "https://PROJECT_ID.firebaseio.com",
  projectId: "PROJECT_ID",
  storageBucket: "PROJECT_ID.appspot.com",
  messagingSenderId: "SENDER_ID",
  appId: "APP_ID",
};

class Firebase {
    constructor() {
        this.app = firebase.initializeApp(firebaseConfig);
    }
}

export default Firebase;
``` 

What we did was to import firebase from "firebase/app", which comes from the firebase package that we installed previously. Then we created a **Firebase** class that has an instance field called **app** and it has been initialized with our project settings, which is then made into a default export. Note : The firebaseConfig object comes from your Firebase project settings that we copied in the previous section.

Now we need to provide an instance of this Firebase class to our entire application. We'll be using React's Context API for this purpose. 

### Using React's Context API to store the Firebase class instance

Now we'll go into the FirebaseContext.js file that we created and enter the following lines into it

```
import React from "react";

export const FirebaseContext = React.createContext(null);

export const FirebaseContextProvider = ( props ) => {
    return(
        <FirebaseContext.Provider>
            {props.children}
        </FirebaseContext.Provider>
    )
}
``` 
Let's slow down here and understand what exactly we are doing here. We have two exports in this file. 

1. FirebaseContext
2. FirebaseContextProvider

FirebaseContext will be used inside all our component files, wherever we want to use the Firebase Class instance.

**FirebaseContextProvider** will **provide** whatever **FirebaseContext** contains, to our entire application. 

The fruit that you want to eat is **FirebaseContext** and the tree that provides you with the fruit because it contains that fruit, is the **FirebaseContextProvider**.

But we haven't yet given anything to the FirebaseContext to hold, i.e. we haven't yet given the fruits to our tree. 

So now, we import the Firebase class that we created and put it inside the FirebaseContextProvider component so that our entire app will be able to access it.

This is done be making the following changes to our FirebaseContext.js file

```
import React from "react";

// Import the Firebase class from the Firebase.js file
import Firebase from "./Firebase";

export const FirebaseContext = React.createContext(null);

export const FirebaseContextProvider = ( props ) => {
    return(
   // Supply an instance of that class to the FirebaseContextProvider
     <FirebaseContext.Provider value={new Firebase()}>
            {props.children}
        </FirebaseContext.Provider>
    )
}
``` 

If you didn't lose me yet, you might have noticed that we created the Context and the Context Provider, but we never told our app how to consume the context and whatever is inside it. It is like having the fruit with you, but not knowing how to eat it 😅 .... As good as not having the fruit at all.

### Providing the FirebaseContext to our entire application

We need to import the FirebaseContextProvider into our index.js file and wrap the App component with the FirebaseContextProvider component and this way we can rest assured that the Firebase class is now available to our entire App component and whatever child components are nested inside it. 

```
import React from "react";
import ReactDOM from "react-dom";

import App from "./components/App";

// Import the FirebaseContextProvider component
import { FirebaseContextProvider } from "./contexts/Firebase/FirebaseContext";

//Provide the Context to our entire application
ReactDOM.render(
<FirebaseContextProvider>
    <App />
</FirebaseContextProvider>
, document.getElementById("root"));
``` 
Now that we have the Firebase class that the FirebaseContextProvider provides to our entire app, we can go ahead and try to access it inside our app. If it works, we've achieved our goal. Else, let's maybe debug?😅

Let's enter our App component and try to log whatever FirebaseContext is providing to our app, to the console. But before that another small thing is left out. To be able to access whatever FirebaseContext contains, we will use the **useContext** hook provided by React. This is illustrated in the following code snippet.

```
import React, { useContext } from "react";

// Import FirebaseContext
import {FirebaseContext} from "../contexts/Firebase/FirebaseContext";

const App = () => {

// useContext hook used to give whatever is inside FirebaseContext ( Firebase class ) 
// to a constant called firebase
    const firebase = useContext(FirebaseContext);

    return(
        <h1>Hello from the App component</h1>
    );
}

export default App;
``` 
Now we can treat the above constant called **firebase** as basically an instance of the Firebase class that any component in our app can use, as long as FirebaseContext has been imported into our app and we use the **useContext** hook to provide the class instance to our component. 

Now let's actually log whatever is inside the **firebase** constant to our console and see what we get.

For this, let's edit our App component's file as shown

```
// Import the useEffect hook
import React, { useEffect, useContext } from "react";

import {FirebaseContext} from "../contexts/Firebase/FirebaseContext";

const App = () => {

    const firebase = useContext(FirebaseContext);

// The first time that <App /> gets rendered, the value of firebase  gets logged to the
// console
    useEffect(() => {
        console.log(firebase);
    }, []);

    return(
        <h1>Hello from the App component</h1>
    );
}

export default App;
``` 

Ok , time for the ultimate test! Go to the terminal and enter the following command from the root of your directory and then go to localhost:3000 and check the console.

```
npm start
``` 
If you see your Firebase credentials being logged into the console, like shown below, it means you've successfully created a Firebase class instance that can be accessed in your entire application. 

![Firebase credentials being logged into the console](https://cdn.hashnode.com/res/hashnode/image/upload/v1611821654711/-iKBNYIrC.png)

### Next Steps

In upcoming posts, I'll write about how we can use other Firebase services like authentication and Firestore using this Firebase class instance that is available to our entire application. If you want to know all that right away and don't want to wait, you can read [this article](https://www.robinwieruch.de/complete-firebase-authentication-react-tutorial) that uses almost the same setup but also shows you additionally, how to integrate Firebase's authentication service too or you can also wait for my post+video on Firebase authentication for Twitter users, that is coming out soon!

### Conclusion

If you have any feedback regarding this post, I'd love to hear from you! You can find me on [Twitter](https://twitter.com/thisismanaswini)

Happy learning :)


