
# Firebase Web App Authentication Tutorial

In this tutorial, we'll guide you through setting up authentication for your web application using Google Firebase. We’ll build a simple message board application where users can sign up and sign in securely with either email and password or their Google account.

## Table of Contents

1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Setting Up the Development Environment](#setting-up-the-development-environment)
4. [Creating a Firebase Project](#creating-a-firebase-project)
5. [Setting Up Firebase Authentication](#setting-up-firebase-authentication)
6. [Implementing Authentication in Your App](#implementing-authentication-in-your-app)
7. [Handling User Authentication States](#handling-user-authentication-states)
8. [Testing the Application](#testing-the-application)
9. [Next Steps](#next-steps)

## Introduction

Firebase is a platform by Google that helps you develop, build, and manage mobile and web apps easily by providing tools like authentication, databases, cloud functions, and web hosting. In this tutorial, we'll focus on Firebase Authentication.

## Prerequisites

Before starting, make sure you have the following installed on your machine:
- [Node.js](https://nodejs.org) (latest LTS version)

## Setting Up the Development Environment

1. **Install Vite:**
   ```sh
   npm install -g create-vite
   ```

2. **Create a New Project:**
   ```sh
   npx create-vite@latest firebase-tutorial --template vanilla
   ```

3. **Navigate to the Project Directory:**
   ```sh
   cd firebase-tutorial
   ```

4. **Install Dependencies:**
   ```sh
   npm install
   ```

5. **Start the Development Server:**
   ```sh
   npm run dev
   ```

## Creating a Firebase Project

1. Go to the [Firebase Console](https://console.firebase.google.com/).
2. Click on "Add project" and follow the steps to create a new project. (e.g., `message-app`)
3. Disable Google Analytics (optional) and click "Create Project".

## Setting Up Firebase Authentication

1. In your Firebase project console, click on the web icon (`</>`).
2. Register your app (e.g., `message-app`) and click "Register App".
3. Copy the Firebase SDK configuration code and paste it into your `main.js` file.
4. Install Firebase in your project:
   ```sh
   npm install firebase
   ```

5. In your `main.js` file, initialize Firebase with the provided configuration:

   ```js
   import { initializeApp } from "firebase/app";
   import { getAuth, createUserWithEmailAndPassword, signInWithEmailAndPassword, signInWithPopup, GoogleAuthProvider, onAuthStateChanged, signOut } from "firebase/auth";

   const firebaseConfig = {
       apiKey: "your-api-key",
       authDomain: "your-auth-domain",
       projectId: "your-project-id",
       storageBucket: "your-storage-bucket",
       messagingSenderId: "your-messaging-sender-id",
       appId: "your-app-id"
   };

   const app = initializeApp(firebaseConfig);
   const auth = getAuth(app);
   ```

## Implementing Authentication in Your App

1. **Sign Up with Email and Password:**

   ```js
   const emailSignUpForm = document.getElementById('emailSignUpForm');
   const passwordSignUpForm = document.getElementById('passwordSignUpForm');
   const signUpButton = document.getElementById('signUpButton');

   signUpButton.addEventListener('click', () => {
       const email = emailSignUpForm.value;
       const password = passwordSignUpForm.value;

       createUserWithEmailAndPassword(auth, email, password)
           .then((userCredential) => {
               console.log('User signed up:', userCredential.user);
           })
           .catch((error) => {
               console.error('Error signing up:', error.message);
           });
   });
   ```

2. **Sign In with Email and Password:**

   ```js
   const emailSignInForm = document.getElementById('emailSignInForm');
   const passwordSignInForm = document.getElementById('passwordSignInForm');
   const signInButton = document.getElementById('signInButton');

   signInButton.addEventListener('click', () => {
       const email = emailSignInForm.value;
       const password = passwordSignInForm.value;

       signInWithEmailAndPassword(auth, email, password)
           .then((userCredential) => {
               console.log('User signed in:', userCredential.user);
           })
           .catch((error) => {
               console.error('Error signing in:', error.message);
           });
   });
   ```

3. **Sign In with Google:**

   ```js
   const googleSignInButton = document.getElementById('googleSignInButton');
   const provider = new GoogleAuthProvider();

   googleSignInButton.addEventListener('click', () => {
       signInWithPopup(auth, provider)
           .then((result) => {
               console.log('User signed in with Google:', result.user);
           })
           .catch((error) => {
               console.error('Error signing in with Google:', error.message);
           });
   });
   ```

## Handling User Authentication States

1. **Monitor Auth State Changes:**

   ```js
   const loggedOutView = document.getElementById('loggedOutView');
   const loggedInView = document.getElementById('loggedInView');
   const userEmail = document.getElementById('userEmail');

   onAuthStateChanged(auth, (user) => {
       if (user) {
           loggedInView.style.display = 'block';
           loggedOutView.style.display = 'none';
           userEmail.textContent = `You are logged in as ${user.email}`;
       } else {
           loggedInView.style.display = 'none';
           loggedOutView.style.display = 'block';
       }
   });
   ```

2. **Sign Out:**

   ```js
   const logoutButton = document.getElementById('logoutButton');

   logoutButton.addEventListener('click', () => {
       signOut(auth)
           .then(() => {
               console.log('User signed out');
           })
           .catch((error) => {
               console.error('Error signing out:', error.message);
           });
   });
   ```

## Testing the Application

1. **Run the Development Server:**
   ```sh
   npm run dev
   ```

2. **Open Your App in the Browser:**
   Navigate to `http://localhost:5174` (or the port specified by Vite) and test the sign-up, sign-in, and sign-out functionalities.

## Next Steps

In the next tutorial, we will connect our application to a Firestore database to enable creating and viewing posts. Stay tuned!
