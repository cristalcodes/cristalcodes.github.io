---
layout: post
title:      "Passing Data Around in Javascript"
date:       2020-03-31 22:02:02 +0000
permalink:  passing_data_around_in_javascript
---


If you've stumbled across this blog post, it is quite likely that you are a Flatiron School student and are looking for examples of projects and/or blog posts. This particular post is intended to provided some tips for getting through this project.

Friendly Reminder: it is normal to feel overwhelmed and/or anxious as you approach this and any project. You are not alone in this feeling, and it behooves you to reach out to classmates, your cohort lead, and/or your educational coach if you should ever feel this way. The Flatiron and general dev community is very supportive!

The Project
The purpose of this project is to take your Ruby on Rails knowledge, and add a layer of complexity to it by creating a one page application using Vanilla JS as opposed to ActionView.

Using fetch();
So you've built your backend API, and upon running your server, you're successfully displaying json. Perfect! The next step is to retrieve this information.

The boilerplate code for making this request is as follows:
fetch('http://example.com/movies.json') 
//Fetch takes in one argument here:
//the path whose resource we're trying to retrieve information from. 
  .then((response) => {
    return response.json();
  })
//we are returned a promise that contains the response object. This is 
//NOT the information we can use just yet. It is simply the HTTP 
//response.
  .then((myJson) => {
    console.log(myJson);
  });
//This second .then extracts the information we want from that HTTP 
//response. 

To get a feel for the flow of information, we're going to go through the process of fetching information, creating an object from it (in this case, a pin), and then using that object's information to create a child object (a memory).

The models:
class Pin < ActiveRecord::Base
  has_many :memories, dependent: :destroy
end

class Memory < ApplicationRecord
  belongs_to :pin
end
Here is my API, displaying json rendered by my Rails application.
[
  {
    "id": 41,
    "address": "11 Broadway, New York, NY 10004",
    "label": "// Flatiron School <3",
    "latitude": 40.7053111,
    "longitude": -74.0140526
  }
]
What we see is that each pin is stored as an object in an array. Below, jsonData is returning this array, and .forEach is used to comb through each object key for its values below.
    fetch(BASE_URL)
    .then(response => response.json())
    .then(jsonData => {
    //iterates through each location object & sets variables
      jsonData.forEach((location) =>  {
        let pinId = location['id'];
        let pinLabel = location['label'];
        let pinLatitude = location['latitude'];
        let pinlongitude = location['longitude'];
    //creates a pin using above variables
        pinInfo = {
          id: pinId,
          label: pinLabel,
          coords: {
            lat: pinLatitude,
            lng: pinlongitude
          }
        }
        dropPin(pinInfo); //We're going to need this later. 
      })
PASS THE DATA!
We're going to pick up where we left off above. The last function called was dropPin, with an argument of the each pin that was created with data from the fetch function.

Our pin is dropped (code redacted to focus on the important matter at hand - passing data). A dialogue box is created when a user clicks on the pin; one of the options is below.

//Each pin carries the following:
//id, label, coords object(which include lat and lng)


    function dropPin(pin){

    <a href= "#" onclick= 'createMemoryForm(${pin.id});'> Add a Memory </a><br>


  }
Let's say our location is Disneyland. We clicked on the pin, and now we want to jot down a memory for this location. The onClick attribute in the link fires off 'createMemoryForm()', with a passed in parameter of ${pin.id} (which holds the value of id for that location). Where does this function come from?

You. It's you. You need to write the function.

Let's do that.
function createMemoryForm(pinId){
//First, we declare the function. We have it take in a parameter, and we 
//name that parameter in a way that helps us remember what we're passing 
//in. 

console.log("The function createMemoryForm has been triggered. The form should be displayed below the map.")
  console.log(`This pin has an id of ${pinId}`)
//I've logged my parameter and function namein the console for easy 
//debugging!

  let contentContainer = document.getElementById('content-container')
  //grabbed the container I want to put my form in

    contentContainer.innerHTML =  `
      <br>
      Add your memory to this location by filling out the form below:
      <br>
      <br>
      <form onsubmit="createAndDisplayMemory();return false;">
        <label for="date">Date (YYYY-MM-DD)</label><br>
        <input type="text" id="date"><br>
        <label for="description">Description:</label><br>
        <input type="text-area" id="description" ><br>
        <input type="hidden" id="pin_id" value=${pinId} >
        <input type ="submit" value="Add Memory!"><br>
    </form>  `
   //created the form

}

Can you spot the handshake between our dropPin function and the createMemoryForm function?

Let's do that again, but let's only grab the pieces we need.
// in dropPin();
<a href= "#" onclick= 'createMemoryForm(${pin.id});'> Add a Memory </a>
//clicking the link triggers createMemoryForm(); below and hands it pin.id (from above) 

//in createMemoryForm(pinId)
<form onsubmit="createAndDisplayMemory();return false;">
//focus on this next line! 
//the pinId that was handed to this function by dropPin() is given to 
//the hidden field with an id of "pin_id". 
<input type="hidden" id="pin_id" value=${pinId} >

<input type ="submit" value="Add Memory!">

The user goes on and clicks submit. Where are we going onClick? To createAndDisplayMemory();! Again, we're going to break this function into several chunks to try and make it easier to understand. Try to spot the handshake!
function createAndDisplayMemory(){

  let contentContainer = document.getElementById('content-container')
  let date = document.getElementById('date').value
  let description=  document.getElementById('description').value
  let pin_id = document.getElementById('pin_id').value

  const memory = {
    date: date,
    description: description,
    pin_id: pin_id
  }
}
Did you see it? Our form had included
<input type="hidden" id="pin_id" value=${pinId} >

The following line of code grabs that value
let pin_id = document.getElementById('pin_id').value

and then we use all the information from our form to create a memory object.
const memory = {
    date: date,
    description: description,
    pin_id: pin_id
  }
}
Do you see the critical piece? We grabbed pin_id! What is pin_id on our memory table?

A FOREIGN KEY!!!!!!!!!!!!
What this means for us is that our memory will be sent to our database and given its own unique id. That memory will also know who it belongs to. Done over and over again, each new and unique memory will have a foreign key that corresponds to its has_many resource. Cool, right?

Let's finish creating a memory for this pin.
fetch(BASE_URL+'/memories', {
    method: "POST",
    body: JSON.stringify(memory),
    headers: {
      'Content-Type': 'application/json',
      'Accept': 'application/json'
    }
  })

We send a POST request, and hand our newly created memory to the body of the request :
body: JSON.stringify(memory)
 .then(response => response.json())
  .then(memory => {
    contentContainer.innerHTML =""
    contentContainer.innerHTML = `
    <br>
    Date: <br>
    ${memory.date}<br><br>
    Description:<br>
     ${memory.description}<br>
     <a href='#' onClick='editThisMemory(${memory.id})'; return false;>Edit this Memory</a><br>
     <a href= "#" onClick= 'deleteThisMemoryWarning(${memory.id});'> Delete Memory </a>
    `
  })
Then we executed our remaining .then functions to return the newly created memory. We can expect to see that memory's date, and description.

If you're reading closely, you'll also notice that we have two new links with onClick functions:
<a href='#' onClick='editThisMemory(${memory.id})'; return false;>Edit this Memory</a>
and
<a href= "#" onClick= 'deleteThisMemoryWarning(${memory.id});'> Delete Memory </a>
Each function is handed a memory id. Can you guess what happens next?

.....

We write those two functions, and continue the handshake, of course.

Some hot tips:

HOT TIP 1
When POSTing, PATCHing, DELETEing, you don't necessarily need to use the two .then's that are part of the boilerplate fetch code. Only use your .then's if you need data returned to you so that you can display it.

HOT TIP #2
BUILD YOUR CODE AS YOU GO, AND GIVE VARIABLES SEMANTIC MEANING.

Naming things in a way that is easy to read, and that simply makes sense, will save your life. Especially if you build the functions you need as you go.

&& that's all, folks!
As always, thank you for reading, and please feel free to reach out with critiques, comments, suggestions, and/or questions.

Keep it real, y'all!
