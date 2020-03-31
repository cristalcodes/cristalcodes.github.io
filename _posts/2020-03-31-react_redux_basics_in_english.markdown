---
layout: post
title:      "React/Redux Basics... In English"
date:       2020-03-31 22:02:56 +0000
permalink:  react_redux_basics_in_english
---


If you've stumbled across this blog post, it is quite likely that you are a Flatiron School student and are looking for examples of projects and/or blog posts. This particular post is intended to provided some general information that might help you understand some of the necessary concepts and components(pun intended).

Friendly Reminder: it is normal to feel overwhelmed and/or anxious as you approach this and any project. You are not alone in this feeling, and it behooves you to reach out to classmates, your cohort lead, and/or your educational coach if you should ever feel this way. The Flatiron and general dev community is very supportive!

React
Container vs. Functional Component
(AKA Stateful vs Stateless, respectively)
You've likely heard all of these terms (if you're a Flatiron Student, you've read them all in the curriculum).

Let there be no confusion- a container component simply has local state. A functional component, on the other hand, does not.

CONTAINER COMPONENT / STATEFUL
import React from 'react';

class ContainerComponent extends React.Component{

  state = {
    someInfo: ""
  }

  render() {
    return(
      <div>
        <p>Hi! I'm a stateful component! You can hand me props by writing something like this.props.yourPropName!</p>
        <p>Being stateful makes me a "Container Component". Take away my state, and I'm a functional component.</p>
        <p>It doesn't matter that I look different from the functional component below! </p>
      </div>
    )
  }
}

export default ContainerComponent;

Functional Component / Stateless
import React from 'react';

const FunctionalComponent = (props) => {
  return(
    <div>
      <p>Hi! I'm a stateless component! You can hand me props by passing them into the arrow function!
      </p>
    </div>
  )

}

export default FunctionalComponent;
Local vs Global State
Remember how a stateful component (container component) holds state? That is a "stateful component"... its state is local.

Redux (which we'll get to in a moment) allows us to create global state.

What's the difference? Let's go back to our stateful component above...
import React from 'react';
import Button from './components/Button';

class ContainerComponent extends React.Component{

  state = {
    showingButton: false
  }

  showButtons = event => {
    this.setState({
      showingButtons: true
    })
  }

  hideButtons = event => {
    this.setState({
      showingButtons: false
    })
  }

  render() {
    return(
      <div onMouseEnter={this.showButtons} onMouseLeave={this.hideButtons}>
        <p>Hi! I'm a stateful component! You can hand me props by writing something like this.props.yourPropName!</p>
        <p>Being stateful makes me a "Container Component". Take away my state, and I'm a functional component.</p>
        <p>It doesn't matter that I look different from the functional component below! </p>

//look here:
        {this.state.showingButtons ? <Button /> : ""}
//
      </div>
    )
  }
}

export default ContainerComponent;

this.state.showingButons ? is a ternary statement. If true, a button is displayed when the mouse scrolls over the div. If false, nothing is displayed but an empty string.

The component itself has access to it's state BECAUSE IT EXISTS IN THE SAME PLACE. Much the way you have access to your own kitchen, your component has access to what's immediately available in its local state.

So how do we get access to global state? What IS global state anyway?

Global State and Redux
Redux allows us to create a global state that every component has access to.

If local state is the equivalent of you walking into your kitchen and going into your pantry, global state is the equivalent of you taking your grocery cart into the supermarket.

To get access to a global store, we import createStore from react-redux.

createStore takes in an argument, a reducer (or a root reducer that combines all your reducers.... one per resource). It accepts additional arguments, such as applyMiddleware, too.

What in tarnation is a reducer?
A reducer is just a function, my dude. It has two arguments- a state, and an action. A switch statement is involved, and includes an action type, provided by dispatch (don't worry, we'll get to that, too). It looks a little something like this:
export default (state={decks: [], loading: false}, action) => {
  switch(action.type){
    case "LOADING":
      return {
        ...state,
        loading:true
      }

    case "LOADED":
      return {
        ...state,
        resource: action.payload,
        loading: false
      }

    case "ADD":
      return {
        ...state,
        loading:true
      }

    case "ADDED":
      return {
        ...state,
        resource: [...state.resource, action.payload],
        loading: false
      }


    default:
      return state
  }
}
If you're thinking, "well, that's not that scary!" you're absolutely right! All you're doing is telling your application, "Hey! If you get this message (case), give me this thing, ok?"

A fork in the road
To keep things a little more linear, we'll continue on from the return, and how to get these returns, WHICH ARE IN GLOBAL STATE THANKS TO REDUX. We'll come back to dispatch (because a) it's just not that exciting, and b) it's just not that useful if you've got action creator functions!).

Getting Things out of Global State
Let's think back to our first component, the stateful one. It had local state. It could go into its pantry by stating this.state.someInfo.

What if you wanted access to THE store? Well, redux gives us access to the store by giving us connect, which is imported at the top of our file as so:
import { connect } from 'redux';
Importing connect, and then providing the connection when we export the component...
export default connect(mapStateToProps)(YourAppName);
and handing connect a first argument of mapStateToProps, which we can declare like so:
const mapStateToProps = state => {
  return {
    resource: state.resourceReducer.resouce,
  }
}
Now, in our component, we can call this.props.resource and get whatever is in global state. It's like showing up at your house party with the groceries you just bought and yelling "CHECK OUT THIS.VONS.CHIPS&DIP".

How did we get to the point of getting global props?
Okay, so this is where we come to both dispatch and reducers. Our store takes in an argument of a reducer, like so:
const store = createStore(reducer);
Our reducer is something that was imported and passed on to the createStore function provided by redux.

As we saw, our reducer takes in an an initial state, and an action type. Where does this action type come from?

Well, one way to get this is by creating an action creator function. It might look something like this:
export const addResource = (resource) => {
  return(dispatch) => {
    dispatch({type: "ADD"}, resource)
    return fetch(`/resources`, {
      method: 'POST',
      body: JSON.stringify(card),
      headers: {
        'Content-Type': 'application/json'
      }
    })
    .then(resp => resp.json())
    .then(resource => {
      return dispatch({type: "ADDED", payload: resource})
    })
  }
}
If it looks confusing, just focus on the parts that matter:
export const addResource = (resource) => {
    dispatch({type: "ADD"}, resource)
}
What you should be getting from this is:

the function addResource is called, with an argument that accepts a resource.

dispatch, given to us by redux, fires off a message (or action type), and a resource.

our reducer takes the message, and the action ( a resource), and returns something to us.

mapStateToProps gives us access to whatever is returned by simply calling this.props.resource!

The equivalent would be something like having a central market, with anyone and everyone in the area (components) being able to go into the store by using { connect }, {actionFunction}, and mapStateToProps.

In other words, all these things come together to help you program an application where local state helps you deal with the here-and-now (local state) and the always(global state).

I still don't get it
I feel you. If you're not sure how to get everything working together, build a simple application. Just get it to display things on the page. Use fun event handlers and state to get some fun effects. Practice makes better, after all!
