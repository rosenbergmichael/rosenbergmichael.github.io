---
layout: post
title:      "Redux Overview: Action -> Reducer -> Updated State"
date:       2020-11-13 22:58:58 +0000
permalink:  redux_overview_action_-_reducer_-_updated_state
---


I wanted to follow up my last blog post with a high-level overview of Redux, one of the most popular JS libraries for managing an application’s state, and it’s general flow: an **action** sent to a **reducer** which updates **state**. Redux can be a bit overwhelming to learn at first, as I’m sure many others would agree, and so I wanted to break down the main pieces of the Redux puzzle by explaining each individual piece further as well as how they’re all connected. I hope that this overview helps anyone out there that is being introduced to Redux for the first time!

## Store and State
In order to understand how Redux works, the first thing to wrap your head around is the above two words.

To put it as simply as possible, **store** is what brings together all of the other pieces of the puzzle. It’s an object that holds the current application state and allows the state to be updated via actions (representing what should happen) and reducers (the function that updates state based on the action). There is only a single store in a Redux application (the reducer function is what updates it).

As for **state**, this is just a plain JavaScript object that holds data and represents the condition of the app at a point in time. The UI that is rendered is based on the state. As an example, in my React/Redux app that I just built, at one point state represents a list of all user entries and then at another point state is updated to reflect all entries including a new entry that a user has created.

## Reducers and Actions
As I touched on above, the way that state gets updated is via reducers and actions.

A **reducer** is a function that updates state/returns a new state based on previous state and an action as arguments. It is called a reducer function because it combines two pieces of information (previous state and an action), and reduces them into one updated state. Reducers must be *pure functions* meaning they always return the same output for any given inputs, and they never update the previous state but rather create a new state object. To recap, a reducer is what specifies how to change the state of the app based on the actions that are dispatched to the store. Reducer functions can be set up using any type of “decision logic” you choose to update the state depending on the action- *if/else, switch/case, etc*.

An **action** is a plain JS object that is passed as an argument to a reducer, representing how the state should be updated. Actions have a **type** key, usually a string that describes what the action is intended to do, as well as an optional **payload** key that can contain additional information used for updating the state. In the Calorease app that I just built, for example, some of my actions were ‘FETCH_TRACKERS’, ‘ADD_TRACKER’, ‘ADD_FOOD’ and ‘DELETE_FOOD’.

Ok, now that you have a general understanding of some of the main Redux puzzle pieces, below we’ll go through a summary/recap of how they all work together.

## Putting the Pieces Together
At the beginning, you will set up an initial **state** for your app. A simple counter app, for example, might set initial state as an object with a key value of 0. For my app, my initial state was an empty array of trackers (as a reminder, my models on the backend were Tracker and Food):

```
state = {
  trackers: []
}
```

Next, you’ll get your **reducer** function set up. As a reminder, the reducer function is defined with two arguments, current state and action. When the app starts up, it doesn’t have any state yet, so initial state is usually provided as the default value for the reducer:

```
export default function trackerReducer(state = {trackers: []}, action) {
    
   switch (action.type) {
    case 'FETCH_TRACKERS':
      return {trackers: action.payload} 
    case 'ADD_TRACKER':
      return {...state, trackers: [...state.trackers,     action.payload]}
    
    default:
      return state
   }
}
```

After the reducer function is defined, you’ll create **store** with **createStore**, provided by the Redux library. Your reducer function will be passed into createStore, which will use the reducer to generate initial state and as well as updated states depending on the actions in the reducer:

```
let store = createStore(trackerReducer)
```

Once these main pieces are in place, the Redux skeleton is pretty much all set up for you to continue building your app! As a final summary of the Redux flow:

1. Actions are dispatched in response to user interactions
2. The store runs the reducer function to update state depending on which action was passed in
3. The UI displays the new state

I hope this helps clarify some things for any Redux newbies out there, and I’m excited to continue working with Redux in my future endeavors!
