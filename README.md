# Putting it All Together: Client-Server Communication

## Learning Goals

- Understand how to communicate between client and server using fetch, and how
  the server will process the request based on the URL, HTTP verb, and request
  body
- Debug common problems that occur as part of the request-response cycle

## Introduction

Just like the last lesson, we've got code for a React frontend and Rails API
backend set up. This time though, it's up to you to use your debugging skills to
find and fix the errors!

To get the backend set up, run:

```console
$ bundle install
$ rails db:migrate db:seed
$ rails s
```

Then, in a new terminal, run the frontend:

```console
$ npm install --prefix client
$ npm start --prefix client
```

Confirm both applications are up and running by visiting
[`localhost:4000`](http://localhost:4000) and viewing the list of toys in your
React application.

## Deliverables

In this application, we have the following features:

- Display a list of all the toys
- Add a new toy when the toy form is submitted
- Update the number of likes for a toy
- Donate a toy to Goodwill (and delete it from our database)

The code is in place for all these features on our frontend, but there are some
problems with our API! We're able to display all the toys, but the other three
features are broken.

Use your debugging tools to find and fix these issues.

There are no tests for this lesson, so you'll need to do your debugging in the
browser and using the Rails server logs and `byebug`.

**Note**: You shouldn't need to modify any of the React code to get the
application working. You should only need to change the code for the Rails API.

As you work on debugging these issues, use the space in this README file to take
notes about your debugging process. Being a strong debugger is all about
developing a process, and it's helpful to document your steps as part of
developing your own process.

## Your Notes Here

- Add a new toy when the toy form is submitted

  - How I debugged:
  I got error **500 (Internal server error)**.

  Via the Network tab, I was able to stack and trace where the error originated. It was found "NameError (uninitialized constant ToysController::Toys):".

  The toy object had been created using `toy = Toys.create(toy_params)`. 
  "Toys" is an inexistent class in stead of just "Toy", and I replaced "Toys" with "Toy" in that statement 

- Update the number of likes for a toy

  - How I debugged:
   When I tried liking a toy, I got the error **Uncaught (in promise) SyntaxError: Unexpected end of JSON input** . I realised the response was not a valid JSON.

   In the last statement in the update action, the Toy instance was not a valid JSON: `toy.update(toy_params)`. I replaced it to `toy.update(toy_params)` with `render json: toy.update(toy_params)` to make the action returns
  a JSON object that is being expected in the front-end.

- Donate a toy to Goodwill (and delete it from our database)

  - How I debugged:

  I got the error; **404 (Not Found)** I tried to "donate a toy to goodwill".
    
  What could have raised the errors could be a route to handle the HTTP verb +path for the request had not been implemented.
  
  Then Network tab guided me to know the destroy route had been implemented to facilitate delete request and I updated it to:
  `resources :toys, only: [:index, :create, :update]` to `resources :toys, only: [:index, :create, :update, :destroy]`
