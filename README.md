# react-Notes

2 ways to create react application :

1> npm 
2> npx - node package executor

** Difference : create-react-app ||  crete vite@latest > instead of vite parcel can also be used.

-> create-react-app : has more library files like testing libraries etc. IS slower compare to react with vite

-> crete vite@latest : has less libraries,faster than create-react-app. Has more dev dependencies. It is a bundler that bundles all js files in one file and sends it to display/render.

// create-react-app File structure : 

** package-lock.json : here all the dependencies are locked, for running the application
** manifest.json :  for viewing websites in mobile devices. All the meta tags of our application is viewed in mobile using manifest.json.
** robots.txt : is for search engine.
** index.html :  main page that will be loaded. This is why react is called spa (single page application) , bcus it uploads a single file.


** index.html (name could be anything)  in create-react-app

import React from 'react' // react is the standard core library
import ReactDom from 'react-dom/client' // react-dom is the implementation of react for web , just like react-native is implementation of react for mobile devices.

 * these libraries are used to manipulate dom structure
 * Reactmakes its own DOM => const root = ReactDom.createRoot(docum ..) // here createRoot is method inside ReactDom

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(  uses JSX file that allows custom tags like <App/> in html pages.
  <React.StrictMode> // for safe mode. this tag is optional and can be removed.
    <App />           // App is a function that returns html etc.. and is exported. from file App.js
  </React.StrictMode>
);

** Advantages : we can make function that returns html that is rendered by react.
                using js we can write html file
                we got prograaming capabilities for html files

** but in index.html there is noscript file that loads js in html. This is done by react scripts that can be seen in package.json
    example : right click > view page source on react loaded page  . this is there >  <script defer src="/static/js/bundle.js"></script></head>



// vite-react file structure : 

* index.html : only file that renders, present inside public folder.
* react-scripts are not present in dependencies of package.json which makes it lighter // index.html already has script tag for inserting js into html.  <script type="module" src="/src/main.jsx"></script>
  
* main.js : main entry point

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(  uses JSX file that allows custom tags like <App/> in html pages.
  <React.StrictMode> // for safe mode. this tag is optional and can be removed.
    <App />           // App is a function that returns html etc.. and is exported. from file App.js
  </React.StrictMode>
);

** Custom component :

function Chai() {
  return <h3>This is custom component</h3>;
}

export default Chai;

> create chai.jsx file

** Rules : 
> : in viteReact  component has to be jsx, and its function has to start with capital letter.
 : in create-react-app components can be .js file but function has to be capital.

** Also in app.jsx : jsx file only allows to return one element i.e "<> </> "  return in div. same for App.js in create-react-app too. 



## Why you need hooks and projects

* to update the UI or values of UI.

* Example :

* import { useState } from "react";
import "./App.css";

// let countValue = 15;

function App() {
  let [variableName, method_maanging_the_variable] = useState(1); // useState() returns 2 values in array. Onr is variable affected and other is method updating the variable. 
                                                                   // initial value in useStete can be anything. {} , [] , empty string. 

  const addValue = () => {
    // console.log("value Added ", Math.random()); // to check whethger the button is working and the function is running

    variableName += 1;
    method_maanging_the_variable(variableName);
  };

  const removeValue = () => {
    method_maanging_the_variable((variableName -= 1)); // here doing "variableName--" will reduce the responsiveness.
  };

  return (
    <>
      <h1>first chaiCodeProject</h1>
      <h2>Counter Value {variableName}</h2>
      <button onClick={addValue}>Add value </button>
      <br />
      <br />
      <button onClick={removeValue}>Remove value </button>
    </>
  );
}

export default App;


export default App;


## VirtualDom, Fibre and Reconciliation

* Read the article on react fibre : acdlite/react-fibre-architecture > link : https://github.com/acdlite/react-fiber-architecture

 ->  React fibre is the algo that is now used to update the react-dom
 ->  to increase its suitability for areas like animations , layouts,gestures etc.
 -> instead of updating every request one by one , this updates final value only once.

 => key Features :  the ability to pause, abort, or reuse work as new updates come in; the ability to assign priority to different types of updates; and new concurrency primitives.

* reconciliation
The algorithm React uses to diff one tree with another to determine which parts need to be changed.
* update
A change in the data used to render a React app. Usually the result of `setState`. Eventually results in a re-render.

** Reconciliation is the algorithm behind what is popularly understood as the "virtual DOM." A high-level description goes something like this: when you render a React application, a tree of nodes that describes the app is generated and saved in memory. This tree is then flushed to the rendering environment — for example, in the case of a browser application, it's translated to a set of DOM operations. When the app is updated (usually via setState), a new tree is generated. The new tree is diffed with the previous tree to compute which operations are needed to update the rendered app.

* Diffing of lists is performed using keys. Keys should be "stable, predictable, and unique." > jab bhi arrays,list se values lete hain to each part pe keys banate hain jisse efficiency badhti hai.

*  primary goal of Fiber is to enable React to take advantage of scheduling. Specifically, we need to be able to

-> pause work and come back to it later.
-> assign priority to different types of work.
-> reuse previously completed work.
-> abort work if it's no longer needed.

# Props in react.js
used to share data between components. Each component has access to props.

example : 

* In Card.jsx : card component  > recieving the value


function Card({ username }) {       // here in card function parametre we can use either direactly "{username}" a variable {destructuring is done here}, or " props" and in console.log("props ", props.username)
  console.log("props ", username);
  return (
    <></>)}

* In App.jsx :

-> <card username = "BuildingWithReact" />  or
 
function App() {
  let objectA = {
  name : "rahul",
  pass : "abc"
  }

  return (

  <card user = {objectA} />  // objects like arrays also can only be passed as variable i.e {inside this braces as object reference}.
  )

-> example for different prices :    
    

    




    

    
