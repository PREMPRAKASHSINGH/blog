---
layout:     post
title:      "Building Freecodecamp Random Quote Machine HowToCoder Weekly Project Series."
subtitle:   "1st project of HowToCoder weekly project series."
description: "HowToCoder - Building Freecodecamp Random Quote Machine HowToCoder Weekly Project Series."
comments:   true
date:       '2017-11-27 12:00:00'
author:     "Prem Singh"
header-img: "img/post-bg-4.jpg"
---


<img  src="{{ site.baseurl }}/img/Random-Quote-Machine.PNG" style="width: 100%" alt="freecodecamp Random Quote Machine Project"/>

I am starting from Intermediate Front-end Projects as i said in the last post.

If you haven't read the last post, read it here <a href="http://howtocoder.com/blog/starting-howtocoder-weekly-project-series" target="_blank">Introducing HowToCoder weekly project series.</a>

Today i will be doing the first intermediate project and that is building the Random Quote Machine.

# Build a Random Quote Machine
The Random Quote Machine will be a single page app.
Showing the quote in a block.
The user stories are as follows:
* I can click a button to show me a new random quote.
* I can press a button to tweet out a quote.

How will it look like after the completion of this project?

<a href="https://howtocoder.github.io/Random-Quote-Machine/" target="_blank">View Live</a>

<a href="https://github.com/HowToCoder/Random-Quote-Machine" target="_blank">Code On Github</a>

flow of app or how the app will work?

[app]->making ajax api call to Random Quote API when "New Quote" button is clicked.

[app]<-got response from Random Quote API in json and render it in html.

## Prerequisites?
Basics of Bootstrap but not mandatory.

Basics of Jquery ajax but i am gonna explain anyway.

Git bash shell installed. If not then install it from <a href="//git-scm.com/downloads" target="_blank">here</a>

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
  <title>Random Quote Machine</title>
  <!-- font-awesome stylesheet for icons -->
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.5.0/css/font-awesome.min.css">
  <!-- bootstrap css stylesheet -->
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css">
  <!-- custom css stylesheet -->
  <link rel="stylesheet" href="./css/screen.css">
  <link href="https://fonts.googleapis.com/css?family=Lobster" rel="stylesheet" type="text/css">
  <!-- jquery and bootstrap cdn js file -->
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js"></script>
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/js/bootstrap.min.js" integrity="sha384-0mSbJDEHialfmuBBQP6A4Qrprq5OVfW37PRR3j5ELqxss1yVqOtnepnHVP9aJ7xS" crossorigin="anonymous"></script>
</head>
<body class="col">
  <div class="container">
    <h1 class="text-center name"><b>The Random Quote Machine</b></h1>
  </div>
  <script src="./js/main.js"></script>
</body>
</html>
```
In above code i have added the basic skeleton of an html web page. I have included all the required css and js files inside the head tag.

In the body, i have a class as ```.col``` which applies a background-color and we have a div with class ```.container``` which basically is like a container for the webpage in bootstrap.

And just before the closing of body tage i have included the ```/js/main.js``` javascript file which will contain the code for the Ajax API call to get random quotes from an API.

Now you need to add a div containing a quote, author and share to twitter button.

So let's add quote with class "quote" and author in a h4 tag inside the container div

```html
<!-- paste inside the body tag -->
<div class="container">
  <h1 class="text-center name"><b>The Random Quote Machine</b></h1>
  <div class="row">
    <div class="col-md-6 col-md-push-3">
      <div class="quote text-center">
        <h2>
          <i class="fa fa-quote-left"></i>
          <span id="data">Click New Quote button to get a quote.</span>
        </h2>
        <div class="pull-right">
          <h4>-author</h4>
        </div>
      </div>
    </div>
  </div>
</div>
```

I am using bootstrap grid system where you divide the page row into 12 columns.

The class ```.row``` creates a row and inside that we have class ```.col-md-6``` which says take space upto the 6 columns out of 12.

Also i have added one more class as ```col-md-push-3``` which pushes the ```.col-md-6``` class 3 columns towards right so that it becomes at the center of the 12 grid columns.

The class ```.pull-right``` which contains the author get's pulled towards right of the page. It is like ```float: right``` property of css.

If you are wondering what an &lt;i&gt; tag is then it's an icon for quote by font awesome. I have already included font awesome css inside the head tag.

Let's add the "twitter button" to tweet the quote and the get "new quote" button.

```html
<!-- paste inside the body tag -->
<div class="container">
  <h1 class="text-center name"><b>The Random Quote Machine</b></h1>
  <div class="row">
    <div class="col-md-6 col-md-push-3">
      <div class="quote text-center">
        <h2>
          <i class="fa fa-quote-left"></i>
          <span id="data">Click New Quote button to get a quote.</span>
        </h2>
        <div class="pull-right">
          <h4>-author</h4>
        </div>
        <!-- Adding twitter button and get new quote button -->
        <br>
        <br>
        <div class="socialmedia pull-left">
          <a href="#" id="twitter" class="btn btn-lg col" target=_blank>
            <i class="fa fa-twitter"></i>
          </a>
        </div>
        <div class="col">
          <a href="javascript:;" id="newquote" class="btn btn-lg pull-right col">New Quote</a>
        </div>
        <br>
      </div>
    </div>
  </div>
  <br>
  <br>
  <p class="text-center"><a href="http://howtocoder.com" target="_blank">by HowToCoder</a></p>
</div>
```

I added a twitter button inside of a div with class as ```.socialmedia pull-left```. And i added a font awesome twitter icon.

The button "New Quote" have an id as ```newquote``` and is placed inside a div with class ```.col```.

Now Open the page in web browser. You will see following output.

<img  src="{{ site.baseurl }}/img/RQM/RQM-1.PNG" style="width: 100%" alt="freecodecamp Random Quote Machine Project"/>

I know it's not looking good right now :( . let's add css for this.

### css/screen.css

```css
.col {
  background-color: rgba(246, 36, 89, 0.8);
}
.color {
  color: rgba(246, 36, 89, 0.8);
}

.name {
  margin-top: 50px;
  font-family: 'Lobster', cursive;
  color: white;
}

.quote {
  background-color: #ECF0F1;
  margin: 0 auto;
  margin-top: 12%;
  padding: 40px;
  border-radius: 4px;
}

.socialmedia a {
  background-color: rgba(246, 36, 89, 0.8);
}

a {
  color: white;
}
```

And now it will look like this.

<img  src="{{ site.baseurl }}/img/RQM/RQM-2.PNG" style="width: 100%" alt="freecodecamp Random Quote Machine Project"/>

What do you think now? Beautiful isn't it? :)

<div style="width:100%;height:0;padding-bottom:77%;position:relative;"><iframe src="https://giphy.com/embed/AVBo5eqFXd3SU" width="100%" height="100%" style="position:absolute" frameBorder="0" class="giphy-embed" allowFullScreen></iframe></div>

Now when you click on New Quote button then it should make and ajax call to random famous quote api and give a random quote as response. And it should update the quote text.

But right now When you click New Quote button it is not doing anything.

Time to add javascript code. I am using Jquery.

### js/main.js

```javascript
/** js/main.js **/
$(document).ready(function(){
  $('#newquote').click(function(){
    var color=['#F64747','#663399','#4183D7','#22313F','#9A12B3','#03A678']; /* array of hex color */
    var index=color[Math.floor(Math.random()*color.length)]; /* random color from color array */
    getRandomQuote();
  });
});

/** ajax function which make api call to random-famous-quote-api **/
function getRandomQuote(){
  var color=['#F64747','#663399','#4183D7','#22313F','#9A12B3','#03A678']; /* array of hex color */
  var index=color[Math.floor(Math.random()*color.length)]; /* random color from color array */
  /* Make ajax call here */
}
```

The ```$(document).ready()``` function says run this function code when the whole document gets loaded.
I am adding a click event on the "New Quote" button which has id as ```newquote```.

```getRandomQuote()``` function will make ajax call.

The color array is an array of hex colors and then i am assigning a variable index a value as random index of color array.

```Math.floor(Math.random()*color.length)```  = random index of color array i.e between 0 to length of color array.

Using this random index the color is selected and stored in the variable called index.

Which then will be applied as background color to body so that on each click it will change the background color of the body.

i am using quotes.stormconsultancy random quote API.

Let's add the code for ```getRandomQuote()``` function ajax call.

```javascript
/** js/main.js **/

/** ajax function which make api call to talaikis random quote api **/
function getRandomQuote(){
  var color=['#F64747','#663399','#4183D7','#22313F','#9A12B3','#03A678']; /* array of hex color */
  var index=color[Math.floor(Math.random()*color.length)]; /* random color from color array */
  $.ajax({
    url: 'https://talaikis.com/api/quotes/random/',
    type: 'GET',
    dataType: 'json',
    success: function(data) {
      alert(JSON.stringify(data));
    },
    error: function(err) {
      $('.quote #data').html("Oops something went wrong.");
    }
  });
}
```

This $.ajax() function takes an object containing following properties:
* url - the url of the api end point.
* type - the type of call like get or post etc.
* datatype - data that you expect to receive from the ajax call
* success - a function which gets called when the ajax call successfully returns the response
* error - a function which gets called in case of any errors in ajax call.

Now refresh index.html and click on the "New Quote" button and you can see the response in an alert.

The response object contains the author and quote as key value pairs.

```javascript
{
  "quote":"Personal beauty is a greater recommendation than any letter of reference.",
  "author":"Aristotle","cat":"beauty"
}
```

It's time to update the quote and author with the response we got from ajax call.

In the success function add following code.

```javascript
success: function(data) {
  var quote=data.quote;
  var author=data.author;
  $('.quote #data').html(quote);
  $('.quote h4').html("-"+author);
  $('body').css('background-color', index);
  $('.col').css('background-color', index);
  $('.socialmedia a').css('background-color', index);
  $('#newquote').css('color','white');
  $('.color').css('color', index);
  $('#twitter').attr('href',"https://twitter.com/intent/tweet?text="+quote+"   "+author);
}
```

I am accessing the quote and author using object dot notation to access object properties.

The ```$('.quote #data').html(quote)``` line updates the html inside the div having id as ```data```.
Similarly i am updating the author which is inside of a h4 tag.

I have a random color hex code from the index variable and i am updating the background-color of body with the random color. And updating some other css properties.

And in the last, i am updating the href attribute of the twitter button. So on click it will take you to twitter to tweet the quote with author.

Now refresh the page and click on get "New Quote" button.
It should update the quote and author each time you click "New Button".

Congratulations, You have successfully completed the Build Random Quote Machine Project.

<img src="{{ site.baseurl }}/img/gifs/did-it.gif" alt="Congratulations"/>

<br>

# Let's Push your code to Github

Now time to push the code to github repo. Go to <a href="//github.com" target="_blank">github</a> and create a new repository.

<img  src="{{ site.baseurl }}/img/RQM/RQM-3.PNG" style="width: 300px" alt="Create New Github Repository"/>

Fill up repository name and description. And click on "Create repository" button.

<img  src="{{ site.baseurl }}/img/RQM/RQM-4.PNG" style="width: 100%" alt="Create New Github Repository"/>

Copy the repository url. It will be ``https://github.com/<your-username>/<Repository-name>.git``.

Now open git shell and navigate to your project folder. And type ``` git init ``` in github shell.

This command initializes the Directory as the repository. And you are now on the master branch of the repository.

<img  src="{{ site.baseurl }}/img/RQM/RQM-5.PNG" style="width: 100%" alt="Initialize Github Repository"/>

Now you need to add a remote as your github repository. A remote is where code will be stored i.e the one you above copied.

Type ``` git remote add origin https://github.com/user/repo.git ```.

Now type ``` git status ``` and it will show the untracked files.

You need to add these files and commit the changes.

Type ``` git add . ``` and it will add all untracked files, so time to commit the changes.

Type ``` git commit -m "initial commit - completed project" ```

You have committed the files and now push it to the remote.

Type ``` git push origin master ```. This command pushes the the code on the master branch to the remote named origin.

<img  src="{{ site.baseurl }}/img/RQM/RQM-6.PNG" style="width: 100%" alt="Git commands"/>

<img  src="{{ site.baseurl }}/img/RQM/RQM-7.PNG" style="width: 100%" alt="Git push command"/>

Now navigate to your repository on github and you can see your files there

# Let's make it live on github pages

Go to ```Settings tab``` on your project repo page, scroll down to ```Github Pages``` section.

Select the source as your ``` master branch``` and click save.

<img  src="{{ site.baseurl }}/img/RQM/RQM-8.PNG" style="width: 100%" alt="Git push command"/>

And as you click save you will see a message saying ``` Your site is ready to be published at http://<user>.github.io/<project-name> ```.

And also click on the "Enforce https" to make your site available on https.

Now you can visit the url and see your project live.

It's live up and running. Good job.

<div style="width:100%;height:0;padding-bottom:80%;position:relative;"><iframe src="https://giphy.com/embed/26DOoDwdNGKAg6UKI" width="100%" height="100%" style="position:absolute" frameBorder="0" class="giphy-embed" allowFullScreen></iframe></div>

# Conclusion

* You successfully completed the Build a Random Quote Machine project.

* You learned about how ajax and apis work.

* You learned how to push your code to Github.

* And how to deploy your project to github pages.

If you have any doubts or questions, write in the comments. I would love to help.

Did you find this article useful? Write your comments below.

If you found this article helpful, do share with your friends. Thank you :)
