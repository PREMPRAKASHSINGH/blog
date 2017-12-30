---
layout:     post
title:      "Building Freecodecamp Twitch Tv App HowToCoder Weekly Project Series."
subtitle:   "4th project of HowToCoder weekly project series."
description: "HowToCoder - Building Freecodecamp Twitch Tv App Project Part of HowToCoder Weekly Project Series. Want to learn how to build your own Twitch Tv App? Let's learn to code together."
comments:   true
date:       '2017-12-30 12:00:00'
author:     "Prem Singh"
header-img: "img/post-bg-7.png"
---

<img  src="{{ site.baseurl }}/img/Tw/Twitch.PNG" style="width: 100%" alt="freecodecamp Twitch Tv App Project by HowToCoder" title="Twitch Tv App Project by HowToCoder"/>

Building the Freecodecamp 4th Intermediate Front-end Project - Build a Twitch Tv App.

This project is part of <a href="{{ site.baseurl }}/blog/starting-howtocoder-weekly-project-series" title="HowToCoder weekly project series" target="_blank">HowToCoder weekly project series</a>.

Checkout the previous posts if you haven't read
* <a href="{{ site.baseurl }}/blog/Building-Freecodecamp-Wikipedia-Viewer-App" title="Building Freecodecamp Wikipedia Viewer App" target="_blank">Building Freecodecamp Wikipedia Viewer App</a>

* <a href="{{ site.baseurl }}/blog/Building-Freecodecamp-Random-Quote-Machine-HowToCoder-Weekly-Project-Series" title="Building Freecodecamp Random Quote Machine" target="_blank">Building Freecodecamp Random Quote Machine</a>

* <a href="{{ site.baseurl }}/blog/Building-Freecodecamp-Weather-App-HowToCoder-Weekly-Project-Series" title="Building Freecodecamp Weather App Project" target="_blank">Building Freecodecamp Weather App</a>


I know i am publishing this article very late so apologies for that and to make up for that I will publish 2 tutorials next week since now i have some time. Next will be the portfolio project that will be published tomorrow 1st Jan 2018. In that you can show case your projects that you have build.

{% include ad_in_article.html %}

Alright then let's do this!

# Twitch Tv app
The user stories are as follows:
* I can see whether Free Code Camp is currently streaming on Twitch.tv.
* I can see whether Free Code Camp is currently streaming on Twitch.tv.
* if a Twitch user is currently streaming, I can see additional details about what they are streaming.

Hints:
* Here's an array of the Twitch.tv usernames of people who regularly stream: ```["ESL_SC2", "OgamingSC2", "cretetion", "freecodecamp", "storbeck", "habathcx", "RobotCaleb", "noobs2ninjas"]```
* Use ```https://wind-bow.glitch.me/twitch-api``` instead of twitch's API base URL (i.e.``` https://api.twitch.tv/kraken``` ) and you'll still be able to get account information, without needing to sign up for an API key.

The Twitch API now requires an API key to use their API But since the API can be seen in the frontend it is not good.

The freecodecamp has a solution for this Problem. We just need to change the Twitch Tv API baseurl to above mentioned in Hints and everything will work fine without any API key needed.

You can visit the entry page of API and read about the documentation <a href="https://dev.twitch.tv/docs/v5/reference/streams#get-stream-by-user" target="_blank">here</a>.

I already read the documentation and following are the 2 url endpoints for twitch stream API and channel API.

```
https://wind-bow.glitch.me/twitch-api/streams/{channel}&&callback=?
```
```
https://wind-bow.glitch.me/twitch-api/channels/{channel}&&callback=?
```

I have replaced the baseurl from
```
https://api.twitch.tv/kraken
```
to

```
https://wind-bow.glitch.me/twitch-api/
```


How will it look like after the completion of this project?

<a href="https://howtocoder.github.io/Twitch-Tv-App" title="View twitch tv app live" target="_blank">View Live</a>

<a href="https://github.com/HowToCoder/Twitch-Tv-App" title="Check the source code of twitch tv app on github" target="_blank">Code On Github</a>

flow of app or how the app will work?

[app] -> making call to twitch stream channel API to get stream information (the stream object) for a specified user.

[app] <- got the json response from the twitch API like status, what streaming and description of what playing based on whether the channel is live or not.

[app] -> making call to twitch channel API to get channel information like channel display name, logo and channel link.

And then if online then prepend and if offline then append the result in list and display them in html.

Above online will be prepended so that the live channels can be seen above and offline ones below them in the lists.

{% include ad_in_feed.html %}

## Prerequisites?
Basics of Bootstrap but not mandatory.

Basics of Jquery ajax but I am gonna explain anyway.

Git bash shell installed. If not then install it from <a title="git download" href="//git-scm.com/downloads" target="_blank">here</a>

Don't worry if you don't know Bootstrap, it's a css framework, you can use plain css for styling if you want.

So let's start building it.

# Project Setup
## Directory Structure
```
--css/
----style.css
--js/
----script.js
index.html
```

### index.html
Open index.html and paste the following code.
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>FCC Twitch Tv Project</title>
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css">
  <link href='https://fonts.googleapis.com/css?family=Amaranth:700' rel='stylesheet' type='text/css'>
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.5.0/css/font-awesome.min.css">
  <link rel="stylesheet" href="css/style.css">
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script>
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/js/bootstrap.min.js"></script>
</head>
<body>
  <div class="container">
    <h1 class="text-center"><i class="fa fa-twitch"></i> Twitch.tv streamers</h1>
  </div>
  <script src="js/script.js"></script>
</body>
</html>
```

Above code is basic skeleton of an html page. I have included all the required css and js files inside the head tag.

And included the script.js file at the end of closing body tag.

I have added a center aligned heading with class ```text-center``` as "Twitch.tv streamers".

The &lt;i&gt; tag is an icon from font-awesome and has class as ```fa fa-twitch```. The font awesome css file is included in the header of the index page.

Let's add the 3 buttons to show all channels, online channels and offline channels.

And a placeholder div where all the search results will be populated.

```html
<body>
  <div class="container">
    <h1 class="text-center"><i class="fa fa-twitch"></i> Twitch.tv streamers</h1>
    <div class="row">
      <div class="btn-group btn-group-justified">
        <div class="btn-group">
          <button type="button" class="btn btn-primary" id="all">All</button>
        </div>
        <div class="btn-group">
          <button type="button" class="btn btn-primary" id="online">Online</button>
        </div>
        <div class="btn-group">
          <button type="button" class="btn btn-primary" id="offline">Offline</button>
        </div>
      </div>
    </div>
    <div class="res">
    </div>
  </div>
  <script src="js/script.js"></script>
</body>
```

Here you added a div with class ```row``` that contains a button group with class ```btn-group```.

There are 3 buttons as ```All``` ```Online``` ```Offline``` with bootstrap button class ```btn btn-primary``` and have id attribute as all, online and offline respectively.

These 3 buttons will act like a filter to show the channel stream results based on the status.

At the end you added a div with class ```res``` that will render the results.

Now if you open index.html in your browser you will see following output.

<img src="{{ site.baseurl }}/img/Tw/Twitch-1.PNG" style="width: 100%" alt="freecodecamp Twitch Tv App Project by HowToCoder"/>

The styles like background colors are not appearing because we haven't added the css yet.

let's add some css to it to make it look good.

Add following css to your css/style.css file.

### css/style.css
```css
body {
  padding-bottom: 40px;
  font-family: 'Amaranth', sans-serif;
  background: rgba(241, 169, 160, 0.5); }

a {
  text-decoration: none;
  color: rgba(241, 255, 255, 0.9); }

#all, #online, #offline {
  width: 100%; }

.res {
  padding: 10px 20px;
  border: 8px solid;
  border-color: rgba(255, 255, 255, 0.8);
  border-radius: 10px;
  background-color: #FF6766;
  color: rgba(241, 255, 255, 0.9); }
```

Now just refresh your index.html page in your browser and you will see following output.

<img src="{{ site.baseurl }}/img/Tw/Twitch-2.PNG" style="width: 100%" alt="freecodecamp Twitch Tv App Project by HowToCoder"/>

Looks beautiful isn't it?

<div style="width:100%;height:0;padding-bottom:77%;position:relative;"><iframe src="https://giphy.com/embed/AVBo5eqFXd3SU" width="100%" height="100%" style="position:absolute" frameBorder="0" class="giphy-embed" allowFullScreen></iframe></div>

Now the body background color is changed and you can also see background color and border colors for div with class ```res``` where the results will populated.

But there are no results for streaming channels and that's because we haven't added the javascript code yet.

Time to write javascript people..

### js/script.js
```js
var streamapi="https://wind-bow.glitch.me/twitch-api/streams/";
var channelapi="https://wind-bow.glitch.me/twitch-api/channels/";
var channels=["freecodecamp", "storbeck", "terakilobyte", "habathcx","RobotCaleb","thomasballinger","noobs2ninjas","beohoff","ESL_SC2","OgamingSC2","comster404","brunofin"];

$(document).ready(function(){
  /**
   * Calling allStreamCall function on every channel
   */
  channels.forEach(function(channel){
    allStreamCall(channel);
  });
});
```

In above code I declared 3 variables. The streamapi and channelapi variables are the urls for the twitch API and the variable channels is an array of the Twitch.tv usernames of people who regularly stream.

The ```$(document).ready(function(){})``` function is jquery way of saying run the js code inside of this function after the page loads.

Then we run a ```forEach``` loop on the channels array and call a function ```allStreamCall(channel)``` passing the channel as the parameter for each channel.

What the ```allStreamCall(channel)``` function will do? Yes you guessed it right, it will call twitch APIs to get streaming and channel information about that channel.

Let's add the ```allStreamCall(channel)``` function.

```js
/* continue js/script.js */
function allStreamCall(streamchannel){
  var logo,name,game,status,statusdesc,channel_link;

  var streamchannel_url=streamapi+streamchannel+"?callback=?";
  var channel_url=channelapi+streamchannel+"?callback=?";

  /**
   * call streaming channels API to see if it is streaming or not and if yes then what it is streaming
   */
  $.getJSON(streamchannel_url,function(data){
    if(data.status=='404'){ /* if user not found */
      game=data.message;
      status="offline";
      statusdesc="";
    }
    else if(data.status=='422'){ /* if user unavailable or closed their account */
      game=data.message;
      status="offline";
      statusdesc="";
    }
    else{
      data=data.stream;
      if(data===null){ /* means user is offline */
        game="offline";
        status="offline";
        statusdesc="";
        logo="http://www.gravatar.com/avatar/3c069b221c94e08e84aafdefb3228346?s=47&d=http%3A%2F%2Fwww.techrepublic.com%2Fbundles%2Ftechrepubliccore%2Fimages%2Ficons%2Fstandard%2Ficon-user-default.png";
      }
      else{
        game=data.channel.game;
        status="online";
        statusdesc=":"+data.channel.status;
      }
    }

    /**
     * call channels api to get channel informations like channel display name, logo and link url etc.
     */
    $.getJSON(channel_url,function(data){
      name=data.display_name;
      logo=data.logo;
      channel_link=data.url;
      if(data.status=='404'){ /* if channel not found */
        name=streamchannel;
        channel_link="#";
        logo="https://openclipart.org/image/2400px/svg_to_png/211821/matt-icons_preferences-desktop-personal.png";
      }
      else if(data.status=='422'){ /* if channel unavailable or closed their account */
        name=streamchannel;
        channel_link="#";
        logo="https://openclipart.org/image/2400px/svg_to_png/211821/matt-icons_preferences-desktop-personal.png";
      }
      else if(logo===null){ /* if channel does not have a logo then show the following logo */
        logo="https://openclipart.org/image/2400px/svg_to_png/211821/matt-icons_preferences-desktop-personal.png";
      }

      /* prepare a row for the result in html */
      var result="
			<div class='row' id='"+status+"'>
				<div class='col-md-3 col-xs-4'>
					<span class='logo'><img class='img img-circle' src='"+logo+"'></span>
					<a href='"+channel_link+"'>
						<span class='name text-center'>"+name+"</span>
					</a>
				</div>
				<div class='col-md-9 col-xs-8 text-center' id='statusdescription'>
					<span class='game'>"+game+"</span>
					<span class='status'>"+statusdesc+"</span>
				</div>
			</div>";

      if(status=='offline')
        $('.res').append(result);
      else
        $('.res').prepend(result);
    });
  });
};
```
<br/>
## Explanations

* In the above code I'm first making an API call to the stream API to check what the channel is streaming about?
The response I'm getting is ``channel game`` i.e what the channel streaming and a description about the same and I'm making ```status``` as online/offline based on the results.

* Once I got the response from stream API then I'm calling the channels API to get channels information like logo, display name and channel link etc.

* And then I am creating a string in html to show in results. If the status is 'online' then it will appear first else it will be get appended in the result.

If there is no logo then I'm giving an hard code url for the channel logo image.

{% include ad_in_article.html %}

For the understanding the call goes something like the following pseudocode:
```
call_stream_api(
  /* got response from the stream API */
  call_channel_api(
    /* got response from channel API */
    /* create a string variable named result using the responses received from API to populate in results as html */
  )
)
```

Now In above code variable result does not look pretty or properly indented. So here it is :
```
<div class='row' id='"+status+"'>
  <div class='col-md-3 col-xs-4'>
    <span class='logo'><img class='img img-circle' src='"+logo+"'></span>
    <a href='"+channel_link+"'>
      <span class='name text-center'>"+name+"</span>
    </a>
  </div>
  <div class='col-md-9 col-xs-8 text-center' id='statusdescription'>
    <span class='game'>"+game+"</span>
    <span class='status'>"+statusdesc+"</span>
  </div>
</div>
```

All these variables are declared above in code. So now when you refresh the page index.html in browser then you should se the results populating in the div like the following output:

<img src="{{ site.baseurl }}/img/Tw/Twitch-3.PNG" style="width: 100%" alt="freecodecamp Twitch Tv App Project by HowToCoder"/>


Let's write some more css to give some height, width and borders to each results and logos.

Add the following css in css/style.css
```css
.res .row:hover {
  background: #ffa9a0;
  color: #C82647;
  border-color: #C82647; }

.res .row:hover img {
  border-color: #C82647; }

.res .row {
  margin: 10px 0px;
  padding: 10px;
  border: 1px solid;
  border-radius: 5px; }

.logo img {
  height: 50px;
  width: 50px;
  border: 1px solid lightgray; }

.name {
  margin-left: 20px; }

#statusdescription {
  padding: 15px 0px; }

```

I gave some margin, padding and borders to each row and height &amp; width of 50px and lightgray border to channel logo.

Added changing border color once you hover over any row or channel logo.

And now refresh the index page again.

<img src="{{ site.baseurl }}/img/Tw/Twitch-4.PNG" style="width: 100%" alt="freecodecamp Twitch Tv App Project by HowToCoder"/>

Yaay we did it!!

<div style="width:100%;height:0;padding-bottom:56%;position:relative;"><iframe src="https://giphy.com/embed/3ov9k8yLSMSaN74sVi" width="100%" height="100%" style="position:absolute" frameBorder="0" class="giphy-embed" allowFullScreen></iframe></div>

Wait It's not done yet we still have to add code to filter the results for offline and online buttons :/

Open your ```js/script.js``` file and write the following code inside of the ```$(document).ready()``` function
```js
/**
 * Show all channels when clicked on All button
 */
$('#all').click(function(){
  var all=$('.res .row'); /* Select all divs with class row in result div */
  all.each(function(index){ /* iterate through each one and show them */
    $(this).css({'display':'block'});
  });
});

/**
 * Show Only online streaming channels and hide the offline ones.
 */
$('#online').click(function(){
  var online=$('.res .row'); /* Select all div with class row in result div*/
  online.each(function(index){
    var toggle=$(this).attr('id'); /* take id attribute of that row to check if it is online or offline. */
    if(toggle=='online'){
      $(this).css({'display':'block'}); /* show */
    }
    else if(toggle=='offline'){
      $(this).css({'display':'none'}); /* hide */
    }
  });
});

/**
 * Show Only offline channels
 */
$('#offline').click(function(){
  var offline=$('.res .row'); /* Select all div with class row in result div*/
  offline.each(function(index){
    var toggle=$(this).attr('id'); /* take id attribute of that row to check if it is online or offline. */
    if(toggle=='online'){
      $(this).css({'display':'none'}); /* hide */
    }
    else if(toggle=='offline'){
      $(this).css({'display':'block'}); /* show */
    }
  });
});
```


In above code we have click event listeners for each buttons "All", "Online" and "Offline".

If you click on Any one button then I Select all the rows in the result div that has class ```res``` and based on what button you clicked I am making then show or hide using ```"display": "block"``` or ```"display": "none"``` css property.

And that's how the results are filtering when you click different buttons. Go ahead and try clicking and you will see the channel results filtered for each button click.

Congratulations you successfully completed building Freecodecamp Twitch Tv App project.

If you have read the previous fcc project articles from HowToCoder project series then you know what's coming next.

<br/>
Deployment!!!
<br/>


Time for pushing code to Github and deploying it on github pages to make it live.

# Deployment

If you have done the previous projects of HowToCoder weekly project series then you know this step very well and how to do it.

## Let's Push your code to Github

Go to <a href="//github.com" target="_blank">github</a> and create a new repository.

<img  src="{{ site.baseurl }}/img/RQM/RQM-3.PNG" title="create new repository" style="width: 300px" alt="Create New Github Repository"/>

Fill up repository name and description. And click on "Create repository" button.

Copy the repository url. It will be ``https://github.com/<your-username>/<Repository-name>.git``.

Now open git shell and navigate to your project folder. And type ``` git init ``` in github shell.

This command initializes the Directory as the repository. And you are now on the master branch of the repository.

<img src="{{ site.baseurl }}/img/Tw/Twitch-5.PNG" style="width: 100%" alt="freecodecamp Twitch Tv App Project by HowToCoder"/>

Now you need to add a remote as your github repository. A remote is where code will be stored i.e the one you above copied.

Type ``` git remote add origin https://github.com/user/repo.git ```.

Now type ``` git status ``` and it will show the untracked files.

You need to add these files and commit the changes.

Type ``` git add . ``` and it will add all untracked files, so time to commit the changes.

Type ``` git commit -m "initial commit - completed project" ```

You have committed the files and now push it to the remote.

Type ``` git push origin master ```. This command pushes the the code on the master branch to the remote named origin.

<img src="{{ site.baseurl }}/img/Tw/Twitch-6.PNG" style="width: 100%" alt="freecodecamp Twitch Tv App Project by HowToCoder"/>

Now navigate to your repository on github and you can see your files there.

## Let's make it live on github pages

Go to ```Settings tab``` on your project repo page, scroll down to ```Github Pages``` section.

Select the source as your ``` master branch``` and click save.

<img  src="{{ site.baseurl }}/img/RQM/RQM-8.PNG" style="width: 100%" alt="Github Pages Deployment"/>

And as you click save you will see a message saying ``` Your site is ready to be published at http://<user>.github.io/<project-name> ```.

And also click on the "Enforce https" to make your site available on https.

Now you can visit that url and see your project live.

Yes!! Did it :)

<div style="width:100%;height:0;padding-bottom:108%;position:relative;"><iframe src="https://giphy.com/embed/l378j8hCc20WOC8hi" width="100%" height="100%" style="position:absolute" frameBorder="0" class="giphy-embed" allowFullScreen></iframe></div>


# Conclusion

* You successfully completed the Freecodecamp Twitch Tv App project.

* You learned about how Ajax and APIs work. You used Twitch API.

* You learned git and github and how to push your code to Github repository.

* And how to deploy your project to github pages.

If you have any doubts or questions, write in the comments. I would love to help.

Let me know your thoughts on this in comment section.

Checkout the previous posts if you haven't read
* <a href="{{ site.baseurl }}/blog/Building-Freecodecamp-Random-Quote-Machine-HowToCoder-Weekly-Project-Series" title="Building Freecodecamp Random Quote Machine" target="_blank">Building Freecodecamp Random Quote Machine</a>

* <a href="{{ site.baseurl }}/blog/Building-Freecodecamp-Weather-App-HowToCoder-Weekly-Project-Series" title="Building Freecodecamp Weather App Project" target="_blank">Building Freecodecamp Weather App</a>

* <a href="{{ site.baseurl }}/blog/Building-Freecodecamp-Wikipedia-Viewer-App" title="Building Freecodecamp Wikipedia Viewer App Project" target="_blank">Building Freecodecamp Wikipedia Viewer App</a>

I'll see you guys in the next project tutorial.

Did you find this article useful? Write your comments below.

Share you project in the comment with me a link to github repo and live url if you completed. And you can get feedback from others well.

If you found this article helpful, do share with your friends.

Let's learn to code together. Thank you :)
