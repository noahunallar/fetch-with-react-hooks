# Fetch Data with React Hooks

We will be using the _state_ and _effect_ hooks to fetch popular articles from _Hacker News API_ We'll be also implementing our custom hook for the data fetching that can be reused anywhere in your application or published on npm as standalone node package.

We are going to use axios to fetch data, but its up to you to use another data fetching library or the native fetch API of the browser.

The effect hooks runs when the component mounts but also when the component updates. Because we are setting the state after every data fetch, the component updates and the effect runs again. It fetches the data again and again. That's a bug and needs to be avoided. We only want to fetch data when the component mounts. That's why you can provide an empty array as second argument to the effect hook to avoid activating it on component updates but only for the mounting of the component. The second argument can be used to define all the variables (allocated in this array) on which the hook depends. If one of the variables changes, the hook runs again. If the array with the variables is empty, the hook doesn't run when updating the component at all, because it doesn't have to watch any variables.

Great, we are fetching data once the component mounts. But what about using an input field to tell the API in which topic we're interested in? 'Redux' is taken as default query. But what about topics about 'React'? Let's introduce a new state for the input element.

When you try to type something into the input field, there is no other data fetching after the mounting triggered from the effect. That's because you have provided the empty array as second argument to the effect. The effect depends on no variables, so it is only triggered when the component mounts. However, the effect should depend on the query. Once the query changes, the data request should fire again. But that opens up another problem: on every character you type into the input field, the effect is triggered and executes another data fetching request. How about providing a button that triggers the request and therefore the hook manually?

In order to extract a custom hook for data fetching, move everything that belongs to the data fetching, except for the query state that belongs to the input field, but including the loading indicator and error handling, to its own function. Also make sure you return all the necessary variables from the function that are used in the App component. The hook itself doesn't know anything about the API. It receives all parameters from the outside and only manages necessary states such as the data, loading and error state. It executes the request and returns the data to the component using it as custom data fetching hook.

Let's combine all three of states with a Reducer Hook instead. Now every state transition, decided by the action's type, returns a new state based on the previous state and the optional payload. For instance, in the case of a successful request, the payload is used to set the data of the new state object.

## Bonus: Abort data fetching in Effect Hook

It's a common problem in React that component state is set even though the component got already unmounted(e.g. due to navigating away with React Router).

Every Effect Hook comes with a cleanup function which runs when a component unmounts. The cleanup function is the one function returned from the hook. In our case, we use a boolean flag called _didCancel_ to let our data fetching logic know about state(mounted/unmounted) of the component. If the component did unmount, the flag should be set to true which results in preventing to set the component state after the data fetching has been asyncly resolved.
