## Firebase authentication for Twitter users

## Intro

In this post, I'll show you how to set up an authentication system for Twitter users using Firebase authentication. All the source code for this project can be found on [GitHub](https://github.com/Manaswini1832/react-firebase-twitter-auth). 

If you want to watch a video instead, you can do so on YouTube.

%[https://youtu.be/oh0FbeSNW40]

## Prerequisites

I'll be taking this post up from where I left off in my last post. So if you haven't read that already then make sure to check that out [here](https://manu.hashnode.dev/integrate-firebase-into-a-react-application) or you can also watch the following Part1 video on YouTube.

**Note**: A few things like names of files might be different here, from the video. But they'll be consistent with the names I used in my first post. 

%[https://youtu.be/TymgmAe6WNg]

## Set up routing

We need to go into our App component and set up routing first, so that we have two components that we'll create soon that get rendered when we hit different endpoints.

Let's first install `react-router-dom` by running the following command on the terminal.

```
npm i react-router-dom
``` 
Then, let's change our App component so that the `Home` component is rendered on hitting `/` and the `Private` component is rendered when we hit the endpoint `/private`.

```
import { BrowserRouter as Router, Route } from "react-router-dom";

import Home from "./Home";
import Private from "./Private";

const App = () => {
    return(
        <Router>
            <Route path="/" exact component={Home} />
            <Route path="/private" exact component={Private} />
        </Router>
    )
}

export default App;
``` 
Now let's create the `Home` and the `Private` components in the `components` directory as shown
```
const Home = () => {
    return(
         <h1>Home page</h1>
         <button>Sign in with Twitter</button>
)
}

export default Home;
``` 
```
const Private = () => {
    return(
         <h1>Private page</h1>
         <button>Sign out</button>
)
}

export default Private;
``` 

## Creating the "signUserIn" function in the Firebase.js file

First we need to import Firebase's authentication part from the npm package that we installed with `npm i firebase`. For that we'll type the following line into our Firebase.js file.
```
import firebase from "firebase/app";

//This is the new line
import "firebase/auth";

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
After that, we'll need to create a function that we can then use to let our users log into our app using Twitter. So let's go ahead and create a `signUserIn` function in our Firebase.js file.

```
import firebase from "firebase/app";

//This is the new line
import "firebase/auth";

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

//This is the function that can help users log in with Twitter

signUserIn = async() => {
        const provider = new app.auth.TwitterAuthProvider();
        app
        .auth()
        .signInWithPopup(provider);
 }

}

export default Firebase;
``` 
signInWithPopup is provided by Firebase that shows a popup to our users, who can then enter their Twitter credentials and log into our app. You can refer to [Firebase's official documentation](https://firebase.google.com/docs/auth/web/twitter-login) to know more about what I've done above. 

## Using the `signUserIn` function in the Home component

We'll now go into the Home component and with the help of the useContext hook, we'll make use of the BaseContext that contains the Firebase class which in turn houses our `signUserIn` function that we want to make use of.

```
import { useContext } from "react";
import { Redirect } from "react-router-dom";
import { BaseContext } from "../contexts/Firebase/BaseContext";

const Home = () => {

    const firebase = useContext(BaseContext);

    async function signIn(){
        await firebase.signUserIn();
    }

    return(
        <div>
             <h1>Home page</h1>
             <button onClick={signIn}>Sign in with Twitter</button>
        </div>
    )
}

export default Home;
``` 
We attached an `onClick` listener on the `Sign in with Twitter` button on the Home page and when that is clicked we call the `signIn` function which in turn calls the `signUserIn` function that is inside our Firebase Class. 

Now it'll all work well. But there's still a problem. We'll need to redirect authenticated users to a different page (in this case the Private page) on successful login and store the user's details somewhere. So let's tackle that next.

## Redirect to the Private page on successful login and store user details in the AuthContext

For accomplishing this, we'll use the `Redirect` component that comes packaged with `react-router-dom`. We'll have a context ( we'll call it the AuthContext ), that stores the details of the user. If there's nothing stored in the context, it means that there's no user i.e. the user is not authenticated, so they'll stay in the Home page and will not be able to access the `/private` page. They'll be redirected back to the home page, in case they try to access the Private page. But if there's a user, they'll be redirected to the Private page. 

So let's first of all, set up the AuthContext and then we'll do all the other stuff.

Let's go to the `contexts` directory and create a new file called `AuthContext.js` and type in the following things into that file.
```
import React, { useEffect, useState } from "react";
import app from "firebase/app";

export const AuthContext = React.createContext(null);

export const AuthContextProvider = (props) => {

    const [user, setUser] = useState(null);

    return(
        <AuthContext.Provider value={user}>
            {props.children}
        </AuthContext.Provider>
    )
} 
``` 
Firebase has this handy listener called the `onAuthStateChanged` listener. What it does is, whenever that authentication state of a user using our application changes, it gives us back an object which contains the user details. So let's include this too in our AuthContext.js file so that the `user` state holds the user details at any given point of time.

```
import React, { useEffect, useState } from "react";
import app from "firebase/app";

export const AuthContext = React.createContext(null);

export const AuthContextProvider = (props) => {

    const [user, setUser] = useState(null);

    useEffect(() => {
        app.auth().onAuthStateChanged((userDetails) => {
            setUser(userDetails);
        });
    }, []);

    return(
        <AuthContext.Provider value={user}>
            {props.children}
        </AuthContext.Provider>
    )
} 
```

So now let's go into our index.js file and include our `AuthContextProvider`component there. 
```
import React from "react";
import ReactDOM from "react-dom";
import App from "./components/App";

import { AuthContextProvider } from "./contexts/AuthContext";
import { BaseContextProvider } from "./contexts/Firebase/FirebaseContext";

ReactDOM.render(
<BaseContextProvider>
    <AuthContextProvider>
        <App />
    </AuthContextProvider>
</BaseContextProvider>
, document.getElementById("root"));
``` 

Now back into our Home component, we'll use a ternary operator to see if a user exists. If they do, we'll redirect them to the Private page, but if they don't, we'll keep them on the Home page. 
```
import { useContext } from "react";
import { Redirect } from "react-router-dom";

import { AuthContext } from "../../contexts/AuthContext";
import { BaseContext } from "../../contexts/Firebase/FirebaseContext";

const Home = () => {

    const user = useContext(AuthContext);
    const firebase = useContext(BaseContext);

    async function signIn(){
        await firebase.signUserIn();
    }

    return(
        <div>
            {
                user
                ? <Redirect to="/private" />
                : <div>
                    <h1>Home page</h1>
                    <button onClick={signIn}>Sign in with Twitter</button>
                </div>
            }
        </div>
    )
}

export default Home;
``` 
Now this will work. But there's still a problem. Anybody can still access the Private page, even if they are not authenticated to do so, by typing in `/private` in the search bar. To tackle this, let's create a PrivateRoute component. 

## Creating the PrivateRoute component

Let's go to the `components` directory and create a file called `PrivateRoute.js` and include the following lines of code into it.
```
import { useContext } from "react";
import { Route, Redirect } from "react-router-dom";
import { AuthContext } from "../contexts/AuthContext";

const PrivateRoute = ({ component: RouteComponent, ...rest }) => {

    const user = useContext(AuthContext);

    return(
        <Route
        
            {...rest}
            render={routeProps =>
            !!user ? (
                <RouteComponent {...routeProps} />
            ) : (
                <Redirect to={"/"} />
            )
            }

        />
    )

}

export default PrivateRoute;
``` 
Then we'll import it into our App component, where the Router set up resides, and use it as shown in the following code snippet.
```
const App = () => {
    return(
        <Router>
            <Route path="/" exact component={Home} />
            <PrivateRoute path="/private" exact component={Private} />
        </Router>
    )
}
``` 
From the previous two code snippets, the PrivateRoute component when used instead of the Route component will make sure that no one except an authenticated user will be able to access the Private page. If an unauthorized user still tries to access the Private page, they'll be redirected to the Home page and that was exactly what we were trying to achieve. 

## Conclusion

If you have any feedback regarding this post, I'd love to hear from you! You can find me on [Twitter](https://twitter.com/thisismanaswini)

Happy learning :)