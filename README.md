# KGRobot
KGRobot is KG's automated Telegram assisant.

The KGRobot project was started with the goal of providing KG with utility, fun, and an opportunity for non-developers to get their hands dirty with a simple coding project.

## Features
### Lunch
KGRobot is here to be helpful!  You can ask the bot what we will be having for Friday lunch this week.  If he knows, he will tell you!  Simply ask in natural English:

>Hey KGBot!  What's for lunch this week?

### Quotes
KGRobot has a great memory.  Unfortunately, he often remembers things that people have said and now regret.  Fortunately, this can be very amusing for us!  You can ask KGRobot to give you a KGer quote, and if he has one he will display it for you to proudly read out loud! He will also give you another quote if one simply isn't enough!  Simply ask in natural English:

>KGBot, tell us a Ryan Egan quote!

>KGBot, give me a Zammit Zinger!

>KGRobot, give us a Juice-Gem!

>KGRobot, tell us another!

## Developer Guide
The KGRobot project was built from the ground up with extensibility in mind.  It is extremely easy to set up the project and build your own modules. Get creative!

### Project Setup
1. Download MS Visual Studio Code: https://code.visualstudio.com/
2. Clone the repository: `git clone git@konradgroup.git.beanstalkapp.com:/konradgroup/kgbot.git` NOTE: If you are not familiar with Git or don't have access, ask an SA for help!
3. Install the project dependencies by running `npm install` in a terminal in the project root folder
4. Open the project folder in Studio Code
5. Switch to the debug tab (CMD + SHIFT D) and click the green arrow

Congratulations! You are now running your own local instance of KGRobot.

### Creating your own module
1. Create a new file for your module in the /modules directory.  The name should be {your_module}.js.
2. Declare a new object that will provide your functionality and keep a reference to the core module:
```javascript
var myModule = {
	core: null,
	
	init: function () {
		//YOUR INITIALIZATION CODE HERE
	},
	
	//YOUR MODULE CODE HERE
	
	}
```
3. Declare that your module can be required by the primary module, and save a reference to the core module:
```javascript
module.exports = function (core) {
	myModule.core = core;
	myModule.init();
	return myModule;
}
```
4. In the primary module, require your new module:
```javascript
// Load modules
require('./modules/lunch')(core);
require('./modules/quotes')(core);
require('./modules/myModule')(core);
```

### Subscribing to messages
1. Write a function that will be invoked when the bot receives a message.
```javascript
var myModule = {
	core: null,
	
	init: function () {
		//YOUR INITIALIZATION CODE HERE
	},
	
	//YOUR MODULE CODE HERE
	
	myMessageHandler: function (message) {
		var chatId = message.chat.id;
		var sender = message.from;
		var senderFirstName = from.first_name;
		var senderLastName = from.last_name;
		var messageText = message.text;
	}
}
```
2. You can get any information you need about the incoming message from the 'message' object, documented here: https://core.telegram.org/bots/api#message
3. Register your new function to be called when the bot receives your keywords:
```javascript
init: function () {
	this.core.registerKeywords(this.myMessageHandler.bind(this), [["KGRobot", "kgbot"], "my", "keywords");
}
```
You can provide one or multiple keywords.  All keywords in the list parameters are treated as required.  If the list contains a sublist, one of the keywords in the sublist will be required.

If the bot receives a message that includes all of your keywords, the function passed in the first parameter of the register call will be invoked and passed the message data.

### Replying to messages
The core module provides a very easy way to respond to messages. You must provide the chatId and the text for the response.
```javascript
this.core.telegram.sendMessage(message.chat.id, 'Your response text here');
```javascript

### Saving data
The core module provides a very easy way to persist data for your module. You can access the storedData object from the core module as follows:
```javascript
var storedData = this.core.storedData;
```
By convention, you should store your module's data in subobjects prefixed with your module name:
```javascript
var myData = 'data here!';
this.core.storedData.myModule_myData = myData;
```
That's it! You can access your data later and it will persist after server restarts.
```javascript
var myData = this.core.storedData.myModule_myData;
```





