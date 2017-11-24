---
layout:     post
title:      "How to build a Meetupbot for Slack using Node.js"
subtitle:   "A Slack Bot to get to know list of meetups going on nearby your area based on your location and interest."
description: "How to build a Meetupbot for Slack using Node.js. A Slack Bot to get to know list of meetups going on nearby your area based on your location and interest."
comments:   true
date:       '2017-10-01 20:00:00'
author:     "Prem Singh"
header-img: "img/post-bg-1.jpg"
---


<img src="{{ site.baseurl }}/img/meetupbot-logo.png" alt="MeetupBot for Slack logo" />
# What is Slack?
If you are new to <a href="https://slack.com/">Slack</a>, It's a great platform for team collaboration and instant messaging used in and out of organizations to help team communication and collaboration.

I first used slack for a study group. You can create different channels to separate messages and discussions. You can create private channels as well to keep messages private in a team.

The best functionality is that it also allows integrations on it's platform. And this is what makes it different than other messaging and collaboration platforms.

You can integrate google calender, twitter, trello etc. It's also let's you create custom applications like bots.

## Project
In this post, I will walk you through building a <a href="https://meetupbotteam.github.io/meetupbot-landing-page/">MeetupBot</a> for Slack using Nodejs. It will give you list of meetups going on nearby your location based on your interest.

Feeling excited?

<img src="{{ site.baseurl }}/img/gifs/excited.gif" alt="Excited" class="text-center"/>

It will use  Slack's slash commands. You can type <span class="command">/meetupbot</span> from within Slack to call the <a href="https://meetupbotteam.github.io/meetupbot-landing-page/">MeetupBot</a> and it will greet you along with the list of commands.

I built this project as part of chingu cohort with my 2 members [Zameer](https://github.com/zamhaq){:target="_blank"} and [Linus](https://github.com/nusli){:target="_blank"}

<img src="https://meetupbotteam.github.io/meetupbot-landing-page/images/MeetupBotImg.PNG" alt="MeetupBot"/>
<br />
<img src="https://meetupbotteam.github.io/meetupbot-landing-page/images/MeetupBotShow.PNG" alt="MeetupBot Showing Meetup Lists"/>

You will need basic knowledge of Nodejs and how APIs work. Let's get started.

## Steps for building MeetupBot for Slack

### Step 1 - Project Setup
my repo url : [slack-meetup-bot](https://github.com/PREMPRAKASHSINGH/slack-meetup-bot){:target="_blank"}<br/>
glitch : [glitch.com](https://glitch.com){:target="_blank"}<br/>
meetup_api : [meetup.com/meetup_api/](https://www.meetup.com/meetup_api/){:target="_blank"}

* First fork my repository [here](https://github.com/PREMPRAKASHSINGH/slack-meetup-bot){:target="_blank"}

* Then go to [glitch.com](https://glitch.com){:target="_blank"} and create a project and edit the project name to a shorter name.

* Click on project name > advanced options and then click import the repo from github. You first need to grant access to import your repositories into glitch.

* Go to [Meetup Api here](https://www.meetup.com/meetup_api/){:target="_blank"} and click on API Key tab and save that as you will pass it with every request to Meetup API.

* In your glitch project open .env file and set a variable SECRET as Your Meetup API Key as ```SECRET=<MeetupApiKey>```

* Click on Show live in glitch and you will get your glitch project url.

### Step 2 - Create a Slack App

* Go to [https://api.slack.com/](https://api.slack.com/){:target="_blank"} and then click on "Your Apps > Create New App". It will show you following screen.
<br><img src="{{ site.baseurl }}/img/post-meetupbot-create.PNG" alt="Slack Create Your App" /><br/>
Enter "App Name" and select "Development Slack Workspace" and click on Create App.
Now we need to do 3 things to see it working in our slack workspace.
<br>On the next screen you will see your App Configuration page with following things.
1. Activate incoming webhooks.
2. Create Slash commands.
3. Install your app to your workspace.

<img src="{{ site.baseurl }}/img/post-meetupbot-app-features.PNG" alt="App Credentials Page" />

* Now click on "Incoming Webhooks" and activate it. Incoming Webhooks allows you to post messages into slack.
* Next thing click on "Slash Commands" and create one as <span class="command">/meetupbot</span>.
Command as /meetupbot,<br/> Request url as &lt;glitch-project-url&gt;/meetupbot,<br/> and a Short description and a Usage hint.
<img src="{{ site.baseurl }}/img/post-meetupbot-create-command.PNG" alt="Create A New Slash Command" /><br />
By activating incoming webhooks & creating a slash commands you should have already got a green tick on Permissions.

* Now 3rd and last click on install your app to your workspace and that will take you to next screen to confirm and authorize before installing. And now you are good to go.

### Step 3 - Test it in your channel
Open your slack team channel and type  <span class="command">/meetupbot</span> and you should be able to see your commands popping up. Click enter and you will see a greeting message from meetupbot and list of commands that you can use.

Since you have created only one slash command go to your App page and create 1 more commands as <span class="command">/meetupbot-show</span> with request url as &lt;glitch-project-url&gt;/meetupbot-show (Follow step 2 - create slack command).

Now try this command, type <span class="command">/meetupbot-show San Francisco & Javascript</span> hit enter and you will see list of Javascript meetups in San Francisco with details like Name of Event & Meetup Group, Date of Meetup, Status, Venue, Rsvp Count. Click on Event and it will take you to their meetup event page.

So that's it, Congratulations you have successfully created a MeetupBot for Slack using Nodejs.

<img src="{{ site.baseurl }}/img/gifs/yay.gif" alt="Yay did it" class="text-center"/>

## Lets understand the code.
We are using Google Geocode API to get Latitude and Longitude from location/address parameter that is passed in the command. This latlong along with interest parameter is then passed to meetup api to get list of meetups.

Also we are using Express.js and Javascript Promises, npm package "moment" for parsing dates and "request" to make API calls.

What happens when you call <span class="command">/meetupbot</span> ? It makes a <span class="command">Post request</span> to glitch-project-url/meetupbot.

The request body contains user_name, text and other info.
the reply object is the json response format for the slack API.

```javascript
// server.js
// where your node app starts

// init project
var express = require('express');
var app = express();
var bodyParser = require('body-parser');
var request = require('request');
var moment = require('moment');
// we've started you off with Express,
// but feel free to use whatever libs or frameworks you'd like through `package.json`.

app.use(bodyParser.urlencoded({extended: true}));

/*
* /meetupbot calls the meetupbot and response is a greeting along with list of commands
*/
app.post('/meetupbot', function (req, res) {
  var userName = req.body.user_name;
  var reply = {
    "text": "Hello, " +userName+ " I am a MeetupBot. I show list of meetups going on near your location.\n Try following command :",
    "attachments": [
       {
         title: "1) /meetupbot-show <location> & <interest>",
         text: "use this to find meetup-events based on your location and interests \nfor ex: /meetupbot-show Mumbai & Javascript (Don't forget to use ampersand (&).)",
         color: "#764FA5"
       }
    ]
  };
  res.json(reply);
});
```

What happens when you call <span class="command">/meetupbot-show</span> ? It makes a <span class="command">Post request</span> to glitch-project-url/meetupbot. The request body contains user_name, text (i.e location and interest separated by "&") and other info.

We first make sure the location and interest parameters sent with command are not blank.

Then we pass location to getGeocode method which is a JavaScript Promise that make calls to goole geocode api and returns Latitude and Longitude, which is then passed to getMeetupEvents Promise to get list of meetup by making a call to meetup api.

The meetup api returns an array of meetup event object and we iterate through this array to make an array of event objects in slack response format and keep pushing it in attachments array which we created in the start.

And that reply object with event attachments is then returned as response and is shown in your slack.

This response will only be visible to you (i.e to the user who calls the bot ) and won't disturb other members of channel.

```javascript

/*
* /meetupbot-show <Location> <Category/Interest>
* gives you list of meetup events happening nearby your location based on your interest
*/
app.post('/meetupbot-show', function(req, res) {
  var userName = req.body.user_name;
  var commandText = req.body.text;
  var area = commandText.split('&')[0];
  var interest = commandText.split('&')[1];
  var reply = {};
  var location={};
  var attachment=[];

  if(!commandText || commandText == undefined){
    reply.text = 'Please provide a location along with the command @'+userName+'\nFor ex: /meetupbot-show Mumbai & Javascript.';
    return res.json({text: reply.text});
  } else if(interest =='' || interest == undefined){
    reply.text = 'Please provide a location & search term along with the command @'+userName+'\nFor ex: /meetupbot-show Mumbai & Javascript.\nIt helps in filtering the list of meetups according to your interest :blush:.'+'\nActually it will be difficult to show a long list of meetups for me right now :stuck_out_tongue_winking_eye:.';
    res.json(reply);
  } else {
     reply.text = 'Hey '+userName+',\nThis is the list of meetups near '+area+'.';
     getGeoCode(area)
       .then(data => {
         location.lat = data.lat;
         location.lon = data.lng;
         return location;
       })
       .catch(e => {
         console.log("error in geocode as "+e);
         return res.json({text: 'Ops something went wrong. Please try again :blush:'});
       })
       .then(data => getMeetupEvents(data, interest))
       .then(events => {
         if(events.length == 0){
           reply.text = 'No Meetups found in '+area+' :sleuth_or_spy: .\nMake sure the location you entered is correct and try again.:slightly_smiling_face:';
           return res.json(reply);
         }
         events.forEach(event => {
           var status = (event.status != undefined) ? ('Status - '+event.status) : 'Status - visible only to Members';
           var date = new Date(event.time + event.utc_offset);
           date = moment(date).format('lll');
           var venue = (event.venue != undefined) ? event.venue.address_1 : 'Only visible to members';
           attachment.push({
             title: 'Group - '+event.group.name,
             text: '<'+event.link+'| Event - '+event.name+'>',
             author_name: status,
             title_link: 'https://www.meetup.com/'+event.group.urlname,
             color: "#764FA5",
             fields: [
               { "title": "Date", "value": date, "short": true },
               { "title": "Venue", "value": venue, "short": true },
               { "title": "RSVP Count", "value": event.yes_rsvp_count, "short": true }
             ]
           });
         });
         reply.attachments = attachment;
         return res.json(reply);
       })
       .catch(e => {
         console.log("error occured in getMeetupEvents promise as "+e);
         return res.json({text: 'Ops something went wrong. Please try again :blush:'});
       });
  }
});


/*
*function to get the Geocode from google geocode API.
*/
function getGeoCode(location){
  return new Promise(function(resolve, reject) {
    var options = {
      method: 'GET',
      url: 'http://maps.googleapis.com/maps/api/geocode/json',
      qs: { address: location }
    };

    request(options, function (error, res, body) {
      if (error) {
        console.log("Error occured in getGeoCode as "+error);
        reject();
      } else {
        body = JSON.parse(body);
        var loc = body.results[0].geometry.location;
        resolve(loc);
      }
    });
  });
}
/*
*function to get meetups near your city/town/location using meetup API
*/
function getMeetupEvents(location, interest) {
  var key = process.env.SECRET;

  return new Promise((resolve, reject) => {
    var options = { method: 'GET',
      url: 'https://api.meetup.com/find/events',
      qs: {
        key: key,
        lat: location.lat,
        lon: location.lon,
        text: interest,
        radius: 10
      }
    };

    console.log(options);
    request(options, function (error, response, body) {
      if (error) {
        console.log("error occured in getMeetupEvents as "+error);
        reject();
      } else {
        body = JSON.parse(body);
        console.log(body.length);
        resolve(body);
      }
    });
  });
}

// listen for requests :)
var listener = app.listen(process.env.PORT, function () {
  console.log('Your app is listening on port ' + listener.address().port);
});
```
In the above code we have 2 Promises as follows:

* getGeoCode() - This take location as parameter and makes an API call to google geocode API with  location as query string and returns latlong.

* getMeetupEvents() - This takes location and interest as parameters and makes API call to meetup API containing key i.e API Key, Latitude, Longitude, text i.e interest and radius as query string parameters

The above code uses Javascript Promise which is basically used to handle asynchronous operations.
It allows you to write asynchronous code that is similar in style to synchronous code.

Also helps in avoiding nested callbacks by using chainable then.
If you have nested callbacks in code then it looks like pyramid structure also known as "callback hell".

## Official MeetupBot
The official MeetupBot has one more command as <span class="command">/meetupbot-find</span> to get list of meetup group in your location/area and also has Oauth code so that you can install it by clicking add to slack button.

You can find it here [MeetupBot landing page](https://meetupbotteam.github.io/meetupbot-landing-page/){:target="_blank"}, [MeetupBot github repo](https://github.com/MeetupBotTeam/slack-meetup-bot){:target="_blank"}  Start using it now.


Did you find this article useful? Write your comments below.

If you found this article helpful, do share with your friends. Thank you :)
