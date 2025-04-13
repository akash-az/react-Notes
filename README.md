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

## counter interview question : 

function App()
{
  const [counter , setCounter ] = useState(15);

  const addValue = () => {

  setCounter(counter + 1); // changes will be made once. useState performs operation in batches so all below tasks will be sent in one bacth and all below setCounter will have counter value 15.
  setCounter(counter + 1); // counter will be 16
  setCounter(counter + 1); // counter will be 16

  to actually make multiple changes together : 
  
   setCounter( ( prevCounter ) => prevCounter + 1 ) // setCounter method ek call back accept krta hai i.e prevCounter:  last updated state of counter . Also name can be anything "counter" even
                                                   // ab operation batches me nai hoga, next method previous method ki changes staet accept karegi.
                                                    // Also we should not do ( (prevCounter) => {} ) bcus ifwe use curly braces we have to return a value. classic javascript
   setCounter( ( prevCounter ) => prevCounter + 1 )
    setCounter( ( prevCounter ) => prevCounter + 1 )

  
  }



}    


** Note : onClick={() => setColor("red")} // onClick method ko poora function chahiye hota hai , return value nai . 
         ** onClick method in react expects a function reference and parametres cannot be passed directly,INstead we need to pass it as a reference.
         ** refreshing page resets the state to initial state.

    
## React Hooks

** useCallback : it lets us cache a function definition between re-renders. i.e function ko ya uske output ko cache memory me rkhna
                 // aur nai changes ke saath nai output ko previos ke saath combine ya modify kr dena.
 example : const cacheFn = useCallback(fn,dependencies)

 ** useEffect : lets us synchronize component with an external system.
                example : in our password generator project we cant call passwordGenerator method directly.
                there we had to use useEffect bcus we cannot decide when to render , react app decides that.
 ** useRef : whenever we want to use a reference then we use "useRef" hook     

## password generator application
 ```react
import { useCallback, useState, useEffect, useRef } from "react";

function App() {
  const [length, setLength] = useState(8);
  const [hasChar, setHasChar] = useState(false);
  const [hasNum, setHasNum] = useState(false);
  const [password, setPassword] = useState();

  const passwordGenerator = useCallback(() => {
    let pass = "";
    let str = "ABCDEFGHIJKLMNOPQRSTWXYZabcdefghijklmnopqrstwxyz";

    if (hasNum) {
      str += "0123456789";
    }

    if (hasChar) {
      str += "~!@#$%^&*(){}[]|/<>";
    }

    for (let i = 1; i <= length; i++) {
      let char = Math.floor(Math.random() * str.length + 1);

      pass += str.charAt(char);
    }

    setPassword(pass);
  }, [length, hasChar, hasNum, setPassword]); // here if we dont give setPassword code will run fine, but if we use password
  // continuously new passwoed will be generated in an infine loop.setPassword should be saved in cache.
  //useCallBack is not only responsible for runnig function but optimizing it if any changes occur in dependency.
  //this app can be written without useCallback but we using it to optimize the function.

  const passwordRef = useRef(null);

  const copyPassToClip = useCallback(() => {
    passwordRef.current?.select(); // select and highlights copied text to clipboard // Also current? signifies optional bcus value could be null.
    // passwordRef.current?.setSelectionRange(0, 4); // for selecting values within given range.
    // we can do password selection without using useRef but this is used to provide better functionality,like in this case
    // when clicking on copy the text which is copied is highlighted. so useRef is used to access required object and perform
    // additional operations.
    window.navigator.clipboard.writeText(password); // here we can use window objetc bcus we are working in core react but we
    // cannot do this in next js as nextjs uses server side rendering and their is no window.
  }, [password]);

  useEffect(() => {
    passwordGenerator();
  }, [length, hasChar, hasNum, passwordGenerator]); // here if any change occurs in dependeny run the function again.

  return (
    <>
      <div className="w-full  max-w-md mx-auto shadow-md p-4 my-8 rounded-lg text-orange-400   bg-gray-500">
        <h1 className="text-4xl text-center  text-white my-3">
          Password Generator
        </h1>

        <div className="flex shadow rounded-lg overflow-hidden mb-4 my-2 bg-white">
          <input
            type="text"
            value={password}
            className=" outline-none w-full py-1 px-3"
            placeholder="password"
            readOnly
            ref={passwordRef}
          />
          <button
            onClick={copyPassToClip}
            className="outline-none bg-blue-700 text-white px-3 py-0.5 shrink-0"
          >
            copy
          </button>
        </div>
        <div className="flex text-sm gap-x-2">
          <div className="flex items-center gap-x-1">
            <input
              type="range"
              min={6}
              max={100}
              value={length}
              className="cursor-pointer text-black"
              onChange={(e) => {
                // direct method call nai krenge bcus event pass krna hai
                setLength(e.target.value);
              }}
            />
            <label> Length : {length}</label>
          </div>
          <div className="flex items-center gap-x-1">
            <input
              type="checkbox"
              defaultValue={hasNum}
              id="numberInput"
              onChange={() => {
                setHasNum((prev) => !prev);
              }}
            />
            <label htmlFor="numberInput"> Number</label>
          </div>
          <div className="flex items-center gap-x-1">
            <input
              type="checkbox"
              defaultValue={hasChar}
              id="characterInput"
              onChange={() => {
                setHasChar((prev) => !prev); // here we cannot do setChar("true") ,
                //  bcus it will remain true onwards whether we click or not
              }}
            />
            <label htmlFor="characterInput"> Character</label>
          </div>
        </div>
      </div>
    </>
  );
}

export default App;

```


    

    
