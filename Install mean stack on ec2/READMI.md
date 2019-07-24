# MEAN stack
Mean stack refers to the collection of technologies which are the collection of JavaScript used to develop web applications. 
Mean Stack has given a modern approach of web development which runs on every tier of your application. Mean is a combination 
of a group of technologies synergizing well together and Mean stands for “MongoDB Express.js AngularJS Node.js”. 
The Node.js saves us from learning a separate language as it allows us to use JavaScript on the frontend as well as the backend. 
In a mean stack with NoSQL, nature of MongoDB you can build a product with clear attributes, and without worrying about migrations
you can change and alter the data layers.

Mean is a simple rearrangement of initial letters of a group of technology. Upgrading the technology and switching the base 
platform from an OS (Linux) to JavaScript runtime has helped achieve OS independence.

AngularJS runs on smart phones, tablets, laptops, desktop and even smart TVs, so it is not only centered towards mobiles and does
not treat mobile devices as a second-class citizen. The stack is basically a set of languages and technologies used to develop 
sites or applications by the developers.

You can easily write comprehensive test suites for your Mean app with world-class texting frameworks such as JasmineJS, KarmaJS,
and MochaJS. Mean is an application completely written on JavaScript technologies.

![](https://github.com/Petabytz/AWS-Projects/blob/master/Install%20mean%20stack%20on%20ec2/mean%20stack.jpeg)

Introduction to Means (MongoDB Express.js AngularJS Node.js):

* MongoDB

It makes it easier to pass data between client and server by saving data in binary JSON format. The concept on which it works is collection and documentation. MongoDB provides high performance, high availability, and easy scalability because it is a cross-platform, document-oriented database. To adopt JSON-like documents which have BSON (dynamic schemes) MongoDB shuns the relational database’s table-based structure, which makes data integration for certain types of applications faster and easier.

* EXPRESS.JS

It provides features for single and multipage web applications and is inspired by the popular Ruby framework, Sinatra. It provides a robust set of features for web and mobile applications; it is a minimal and flexible Node.js web application framework. Express.js allowed developers to build software with JavaScript on the server side for the first time by giving JavaScript newfound back-end functionality.

* ANGULARJS

AngularJS’s helps in eliminating much of the unwanted code which otherwise we would have written with its feature of data binding and dependency injection. To express your application’s components clearly can use HTML as your template language and extend HTML’s syntax. It is also a structural framework for dynamic web apps. It provides features like two-way data binding.

* NODE.JS

Node.js is lightweight and efficient because it uses an event-driven, on-blocking I/O model.  Chrome’sV8 JavaScript engine is used to build JavaScript runtime. It has the largest ecosystem of open source libraries in the world known as npm. It can also run JavaScript applications outside the browser. It is a cross-platform runtime environment. It is free to use is open source.


Working of Mean Stack Architecture:

   * Angularjs being a client-side language in JavaScript so it is the first one to process the request made by a client.
   * The request then enters the Nodejs which is the phase 2 being it the server side language in JavaScript
   * Then the request enters the phase 3 i.e. Expressjs which makes a request to the database.
   * After these three phases, the data is retrieved and the response is returned to the Expressjs and this work is done by MongoDB.
   * Then in the final step, the data is to be returned back to the AngularJS to display the result and this is done by NodeJS which in return takes the data back from the ExpressJS.
    
    

APPLICATIONS OF MEAN STACK DEVELOPMENT:

   * Mean stack gets updated often because it has open source components.
   * It is easy to develop web applications using Node.js because it provides a good variety of JavaScript modules from its humongous library.
   * Mean stack allows the developers to write code in only one language which is JavaScript for both client and server side which makes it a simple and fast language.
   * The data flows precisely among the layers of JSON and does not require rewriting or reformatting because Mean uses common JSON format of data everywhere.
   * Mean stack has become a leading end technology because transferring of code from a particular framework to another framework is easy from Mean.
   * Mean is highly flexible. You can easily test your application on the cloud platform after the successful completion of its development process, and even add extra information by just adding an extra field to your form.
   * Development of apps using mean stack is less costly because it just requires developers who are proficient at javascript.
   * Mean helps in the current period to develop real-time demonstration applications which are trending. There is no need to constantly update web pages for each server request because it utilizes single-page applications (SPAs).
   * Developing applications using Mean saves your lot of time and this it does because it has an infinite set of module libraries for Node.js that are ready for use and you don’t have to waste your time in creating modules from scratch. It not only saves your time but also creates quality world-class applications.
   * MVC (Model View Controller) architecture is also supported by Mean.
   * Mean Stack application is very flexible to understand which helps the developers to customize it as per their needs.
   * Front-end development: in front-end developing the developers makes the website interactive by working on its appearances. Mean stack provides us with this feature.
   * Back-end development: in the back end development the developers make the website more functional by adding new features, making it more faster etc. this feature is also provided to us by the mean stack.
   
   
 Create a new MEAN project

If you would like to create a new MEAN project, follow the steps below:

  * Log in to your server console.

  * Create a new folder for your project.
  
  Replace the installdir placeholder with the full installation directory for your Bitnami stack.

~~~bash
mkdir installdir/projects
~~~

  



