---
title:  "Local State is Ok - Lessons from using Redux"
date:   2016-09-23 21:00:00
category: til
tags: [react, redux]
---

TIL that when using Redux, there are examples of state that are best kept local to a component instead of being managed by the Redux store.

### Local State is Ok

I follow Dan Abramov on twitter, and he published [an article][article]{:target="_blank"} this week titled "You Might not Need Redux." One of the key takeaways I had was that ***local state is perfectly fine.***

When you're a new developer working with Redux, it is tempting to stuff every single bit of state managed by your UI in Redux, eliminating local state from your components entirely. This can often lead to wonky implementations, and weird code smells. We followed our noses today at work and found a few places in particular where we were inappropriately storing state in Redux that was better kept local.

### Creating and Updating objects

We have 2 forms in our application that allow the user to create new "Trips." These trips contain a name, a start date and an end date. Users can create new trips, and then later from a trips detail page, click an edit button to update the start and end dates of a particular trip.

We were originally storing all of this intermediate state in Redux in an object called "newTrip". By intermediate state, I mean an action would be dispatched by our input components whenever the value in the input changed to update the "newTrip" object with the current values in the form. When the user had finished editing the form, we would dispatch an action to POST the date in “newTrip” to our server. We also needed to dispatch an action to "clearNewTrip" if the user navigated away from the form, which would clear the “newTrip” part of the state so it would be blank the next time the user visited.

Phew... that’s a lot of work for a simple form! We took Dan Abramov's words to heart - "Local state is fine" - and decided to refactor the above flow to store state locally in the form container instead of in Redux.

### The Great Refactor

We were able to delete about 5 different actions and reducers. Clearing the state no longer involved dispatching an action, but instead can just be cleared out in `comoponentWillUnmount` (or can be ignored entirely, since in the initial `componentWillMount` method we initialize the state of this form to be an empty object!).

This was true intermediate state in the app: it was state that may or may not be persisted in the database. The state didn’t affect any other view in the app except the current view, and once a user navigated away from the page the state should not persist. For these reasons, I think it makes a lot of sense to store it locally instead of making the effort to store it Redux. It's less effort to store locally, and in this case it actually makes a lot more sense due to the nature of the state.

The code smell that initially got us to this refactor was the fact that we were adding a key in our initialState that was empty until we got to those Create/Update pages. By moving that state local to the form containers, we could remove this key from our state object as well, simplifying the object that got stored in redux.

Coming to this realization was helpful, and I think I'm starting to get a better grasp on what belongs in Redux and what does not!

[article]: https://medium.com/@dan_abramov/you-might-not-need-redux-be46360cf367#.j9q7zrwpi

