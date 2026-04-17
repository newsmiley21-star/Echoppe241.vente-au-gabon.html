// Import the functions you need from the SDKs you need
import { initializeApp } from "firebase/app";
import { getAnalytics } from "firebase/analytics";
// TODO: Add SDKs for Firebase products that you want to use
// https://firebase.google.com/docs/web/setup#available-libraries

// Your web app's Firebase configuration
// For Firebase JS SDK v7.20.0 and later, measurementId is optional
const firebaseConfig = {
  apiKey: "AIzaSyCEJJGhcyYWqmeI9D_lwk_qgE2J2GZhIlg",
  authDomain: "communautedugabon.firebaseapp.com",
  databaseURL: "https://communautedugabon-default-rtdb.firebaseio.com",
  projectId: "communautedugabon",
  storageBucket: "communautedugabon.firebasestorage.app",
  messagingSenderId: "647862371022",
  appId: "1:647862371022:web:b209bfc8eb81accb1fc69f",
  measurementId: "G-T7415FPS91"
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);
const analytics = getAnalytics(app);
