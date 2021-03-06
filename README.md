#Description and use of this module.

###Moving of threads
	Description:
	    The function moveThread is used to move a thread to be the child thread of a specified thread.
	    All of the moving thread's children will move with the thread.
		moveThread first checks if the parameter for the new parent thread is not null (if it is null the function returns false). It then removes the moving thread from its current parent's list of children threads.
		moveThread then adds the moving thread to its new parent's list of child threads. 
		The new parent thread is then assigned as the moving thread's parent thread.
		The moving thread then sets its status to match its new parent's status, after which it similarly sets all of its own children threads' statuses.
		Lastly a helper function setLevels() is called to set the moving thread (and its children's) new levels as they are after having moved.
	How to use:
	    The thread to be moved must call moveThread().
		It receives one parameter: a reference to the desired new parent node to which the moving thread must move (i.e. become one of its children).
		It returns one result: "true" if the move was succesful or "false" if it was unsuccessful.
###Querying of threads
	Description:
	    The function queryThread() is used to query threads according to certain parameters.
		queryThread() checks if values have been assigned to the maxLevel and minLevel parametersand if not assigns default values. 
		It then calls the recursive function queryThreadRecursive() to further analyze the request.
		queryThreadRecursive() first checks if must assign default values to the parameters startDateTime, endDateTime, userGroup and phraseSet.
		It then uses a series of if statements to finally check if the current thread falls within the range of all the relevant parameters. 
		If it does then the function addToQueryAnswer() is called to add this thread's info to the answer which will eventually be returned.
		queryThreadRecursive() traverses the current thread's children and repeats the above process.
		addToQueryAnswer() does one final check to see if the current thread falls within the range set by the phraseSet parameter and then (if it does) 
		adds the current thread to an answer array to be returned once queryThreadRecursive() has finished traversing all relevant threads.
	How to use:
	    The thread to be queried must call queryThread().
		It receives six parameters: 
			startDateTime -  Restrict returned posts to be after this time stamp. Default is the time stamp of the root post in the Buzz space.
	    		endDateTime -  Restrict returned posts to be before this time stamp. If unspecified all posts are returned.
	    		maxLevel - Restrict returned posts to be at most at the specified depth relative to the post. If this value is 0, minLevel will also be 0 only the specified post is returned.
	    		minLevel - Restrict returned posts to be at least at the specified depth relative to the post. Obviously it has to be less or equal to maxLevel. If both minLevel and maxLevel is 1, only the immediate children are retirieved.
	    		userGroup - Restricts returned posts to be limited to a specific user group.
				/**
	                 	*Example of how a userGroup data set looks
	                 	*
	                 	*  userGroup = [
	                 	*     'John',
	                 	*     'Susan'
	                 	*  ];
	                 	**/
	   		phraseSet - Restrict returned posts to be only posts that contains all the strings specified in the phrase set. The default is an empty set. If the set is empty all posts are returned.		
				/**
	          		*Example of how a phraseSet data set looks
	          		*
	          		*  phraseSet = [
	         		*     'example phrase',
	      		  	*     'second example phrase'
	       		  	*  ];
	      		 	**/
		It returns one result: An array of queryInfo objects which conforms to the following format:
		/**
		*	var queryInfo =
	        *		{ParentID:  The ID of the current thread's parent,
		*		Author:  The user who posted the current thread,
		*	        TimeStamp:  The date and time the current threads was made,
	        *		Content:  The content of the post of the current thread,
		*       	Status:  The status of the current thread,
		*	        Level:  The depth level of the current thread in the main tree};
		**/

###Closing of thread:

The function closeThread is used to close thread and its children. First, it closes the children making them inaccessible which prevents a user to add to the discussion or create a new post on the thread 
children. It then closes their parent. After closing the parent, it changes the status to closed and create a summary that states the contents, mime type, the date and time the thread was created. Only  
administrator is authorized to close a thread.

###How createSummary() works:

It creates a summary object and append it on the closed parent thread as a child thread.

###Reopening of thread:

The function reopenThread is used to open thread and allowing modification to that thread. It calls unfreeze() to accomplish the task. But first it test if the thread is closed and calls unfreeze(). Unfreeze() function basically change the status of the current thread and its children to open. Only administrator is authorized to reopen a thread.

###Mark post as read:

The function markPostAsRead is used to mark a post to indicate that it has been viewed before by the current logged in user. It checks if a post has been read,then it returns. Otherwise it calls a readPost() function to read the post.

###How readPost() works:

It passes the userid and postid as parameters and use them to check if the current logged in user and the specified post exist  in the database. If they do, it changes the status of the specified post to read.


### hideThread()
	Description:
	    The function hideThread() is used to hide threads,it doesn't take any parameters.
		hideThread() ,basically the selected thread nodes and all its descendant nodes will be marked as hidden
		it changes the status to hidden,then based on the interface,the top level team will decide if hidden threads
		should show as inactive or not show at all
	How to use:
	    To hideThreads you must call hideThread().


### unhideThread()
	Description:
	    The function unhideThread() is used to unhide threads,it doesn't take any parameters.
		unhideThread() ,basically the selected thread nodes and all its descendant nodes will be marked as unhidden
		it changes the status to unhidden,then based on the interface,once they are unhidden they will show as active in the page
	How to use:
	    To unhideThreads you must call unhideThread().

		



# Threads
Functionality around threads and posts.

This Git Repository will be used to collaborate on the Buzz Space mini project.

Our buzz space name is: D3  -  discuss , debate & deliberate ....

## Project time-line
- **22 March 11:59PM**: __Core functionality of module complete so integration for demo can begin__ 
- **27 March**: Mid-Implementation Demo
- **TBA probably in middle of the holiday**: Module complete & final integration
- **17 April**: Post-Implementation Demo
- **17 April**: Testing phase teams allocated
- **24 April**: Final Demo

## What happens now ? 
1. Read this document thoroughly
2. In your groups identify core functionality for your module 
2. Implement & TEST said core functionality before 22 March 11:59PM (23:59)
3. Implement & TEST the rest of the functionality 
4. Be done with the mini project and let integration team worry about integrating your excellent work :) 

## General remarks
- ###Independence of node packages/modules:  
Please remember that the module you as a team need to deliver needs to be able to run on its own regardless of the user interface in front and/or behind it. 
e.g. Threads should not explicitly output to a html file everything should for example send and receive JSON which could be inserted into any page or file. 
The interface needs to be completely separate from the business logic. 

- ### Testing & web:
Every module needs to have multiple unit tests to ensure that it works correctly. The unit tests will provide the module you are writing with "dummy data".
Your module should/will not run a nodeJS web server(except if you want to test it this way), that is what the integration team will do.

- ###Use of external libraries: 
You may and should use as many libraries as possible to increase code re-use and minimise errors / problems.

### Coding Standard
Please follow Google coding standards throughout the implementation of this project.
- JavaScript Standards: https://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml
- HTML and CSS Standards: http://google-styleguide.googlecode.com/svn/trunk/htmlcssguide.xml

We will be using http://usejsdoc.org/about-getting-started.html to generate documentation for our project. 
So remember to document your code.


### **Version Control** - git and npm
Both GitHub and npm will be used to collaborate and perform version control of this project. 
For more details see the Master Specification document on the CS website. 
You do not have to add your module to NPM or any repository other than GitHub, as NPM call pull directly from GitHub. 

We ask that you periodically or after reaching some milestone publish a release (with incrementing version number e.g the first release will be v0.0.1 the second v0.0.2 and so on) 
see https://help.github.com/articles/creating-releases/ for more information.

We would like everyone to use the flowing template for the package.json file (modify as needed & enter relevant details)
```
{
  "private": true,
  "main": "mainFileName.js",
  "name": "ModuleName",
  "description": "Module description",
  "version": "0.0.1",
  "repository": {
    "type": "git",
    "url": "https://github.com/BuzzSpaceB/Module"
  },
  "bugs": {
    "url": "https://github.com/BuzzSpaceB/Module/issues"
  },
  "homepage": "https://github.com/BuzzSpaceB/Module",
  "contributors": [
    {
      "name": "Group Member One",
      "email": "group@member.com"
    },
    {
      "name": "Group Member Two",
      "email": "email@group.com"
    },
    {
      "name": "Group member n",
      "email": "n@email.com"
    }
  ],
  "dependencies": {
      "body-parser": "~1.12.0",
      "broadway": "^0.3.6",
      "cookie-parser": "~1.3.4",
      "debug": "~2.1.1",
      "ejs": "~2.3.1",
      "electrolyte": "0.0.6",
      "express": "~4.12.2",
      "handlebars": "^3.0.0",
      "jsreport": "^0.2.10",
      "ldapjs": "^0.7.1",
      "mongoose": "^3.8.25",
      "morgan": "~1.5.1",
      "node-aop": "^0.1.0",
      "node-cache": "^1.1.0",
      "nodemailer": "^1.3.2",
      "react": "^0.13.1",
      "react-bootstrap": "^0.17.0",
      "scribe-js": "^2.0.4",
      "serve-favicon": "~2.2.0"
    }
}

```

## Technologies we will use
### **IDE** - WebStorm (version 9)

You have complete freedom over which IDE you want to use. 
We do however strongly recommend that everyone uses WebStorm 9. 
We recommend this because it integrates very well with all the technologies we will be using. 
You can get WebStorm here: https://www.jetbrains.com/webstorm/download . 
To get a license key register with your Tuks email account here  https://www.jetbrains.com/estore/students/


### **Execution Environment** - Node.js (version 0.12.0)
NodeJS will be used as an execution environment. 
To get a better idea of what NodeJS is and how it works, watch this video: https://www.youtube.com/watch?v=pU9Q6oiQNd0

### **HTTP Server** - Express
Your module will not run a web server, that is what the integration team will manage and run. 
You need to make sure your code can be "plugged" into the greater web server package. 
But for the sake of completeness our website will run on an HTTP server, and we will be using the Express package for NodeJS to achieve this.
The above mentioned video also contains information on this.
But please remember your code needs to be independent from the web server.

### **Database** - MongoDB
We will be using MongoDB as a database. 
MongoDB is a document database and doesn't make use of relational tables such as SQL. 
Documents are inserted into the database (like a file system) and each of these documents (like a Thread object, or an image) are indexed so they can be retrieved later.

### **Database Schema** - Mongoose
Mongoose is a package for NodeJS that wraps around the normal NodeJS MongoDB driver. 
Mongoose allows us to create a schema for our database, which will validate the documents being inserted into the database. 
This includes simple things like making sure a new thread being inserted contains all of the right fields, 
and that these fields are of the right data type. More can be read here: http://mongoosejs.com/

### **Dependency Injection** - ElectroLyte __NB NB NB NB __
Firstly you might ask, what is dependency injection, and why should it be used? 
Please have a look here: http://www.jamesshore.com/Blog/Dependency-Injection-Demystified.html. 
It is an old article, but it explains it well. 

To make our lives easier, we will use ElectroLyte to handle dependency injection. Read about it here:
https://github.com/jaredhanson/electrolyte

Everyone should be using this, in every module. 

### **Aspect Oriented Programming** - node-aop
Within the Buzz System many requests will have to be intercepted. 
For example, if a user clicks on a button to submit a post, 
that request has to be intercepted to first check if the user is authorized to make this post. 
In this case an AuthorizationInterceptor will be used. 
But how will we achieve this? The answer is Aspect Oriented Programming. 
It allows you to intercept function calls. 

We will be using node-aop as a NodeJS package to achieve this: https://www.npmjs.com/package/node-aop

The integration teams will mostly deal with this. 

### **Modular design** - Broadway
Each team will be working on a module of the system, and each of these modules will have to integrate with each other. 
For this we will be using the Broadway plug-in framework. 
This should be of interest to the middle and top level integration teams, 
but lower level teams need to make sure Broadway is compatible with what they did. 

More can be read about it here: https://github.com/flatiron/broadway

### **Templates** - HandleBars
Some modules require a or part of a web front end. 
All of these front end parts will have to integrate in to one web GUI (by the integration team). 
This is a difficult task, and  we'll be using templates so that styling and layout is kept consistent and pluggable. 
We will be using handlebars for this. Read more here: http://handlebarsjs.com/

Remember that the module that you are writing should not for example explicitly output to a web page. 
The module you are coding needs to be able to function without the web front end if need be. 
The web front end is a way of making use of the module. 
For example a "getThread(x111)" function call should instead of sending HTML send JSON which can be inserted into any 
access channel. 

More info on how this will be communicated to you, in due time.

### **UI performance** - reactJs
ReactJS automatically updates the user interface when certain data changes, without reloading the whole page. 
So this can for example be used while a page is open that is showing threads. 
As threads are being posted, the user won't have to refresh, the threads will appear on the page as they come in. 
Have a look here: http://facebook.github.io/react/docs/why-react.html

### **Internationalization** -  i18n-2 framework
The system should support all of the official languages of the University of Pretoria. 
This framework will basically translate the website depending on which language the user's browser is set to. 
More can be read about it here (including examples): https://github.com/jeresig/i18n-node-2

Make sure your modules can integrate with this. But focus on this later, 
getting a working English version is more important at this stage.

### **Caching** - node-cache
Many documents used throughout the system will no change very often. 
These documents can be cached to improve performance. 
For this we will be using a nodeJS package called node-cache. 
More can be read about it here: https://github.com/ptarjan/node-cache

### **CS LDAP Integration** - ldapJS
The Buzz System will have to get user information from the CS LDAP server, like user roles and email addresses. 
This will be done using the ldapJS framework, which is used in conjunction with NodeJS. More info here: http://ldapjs.org/

### **Email Notifications** - NodeMailer
Users can get notified of certain things like threads they subscribe to. 
To do this we will use the NodeMailer JavaScript email client in NodeJS. 
More information here: https://github.com/andris9/Nodemailer

### **Logging** - Scribe.js
Certain activities happening within the system will have to be logged. 
For this we will use Scribe.js, in conjunction with node-aop. 
Node-aop intercepts whatever needs to be logged, and scribe does the logging. 
More info here: https://github.com/bluejamesbond/Scribe.js/tree/master

### **Reporting** - jsreport
Various reports will have to be created in the Buzz System, like reports for user activity etc. 
This CAN be done with the assistance of the jsreport reporting framework. 
This, as most of the other technologies we're using, runs in NodeJS. Read more here: http://jsreport.net/
