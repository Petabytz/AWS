### Application Deplyoment using Elastic BeanStalk
### What is Amazon Elastic Beanstalk?

With Elastic Beanstalk, you can quickly deploy and manage applications in the AWS Cloud without having to learn about the infrastructure that runs those applications. Elastic Beanstalk reduces management complexity without restricting choice or control. You simply upload your application, and Elastic Beanstalk automatically handles the details of capacity provisioning, load balancing, scaling, and application health monitoring. 
Elastic Beanstalk supports applications developed in Go, Java, .NET, Node.js, PHP, Python, and Ruby. When you deploy your application, Elastic Beanstalk builds the selected supported platform version and provisions one or more AWS resources, such as Amazon EC2 instances, to run your application. 
You can interact with Elastic Beanstalk by using the AWS Management Console, the AWS Command Line Interface (AWS CLI), or eb, a high-level CLI designed specifically for Elastic Beanstalk. 
You can also perform most deployment tasks, such as changing the size of your fleet of Amazon EC2 instances or monitoring your application, directly from the Elastic Beanstalk web interface (console). 
To use Elastic Beanstalk, you create an application, upload an application version in the form of an application source bundle (for example, a Java .war file) to Elastic Beanstalk, and then provide some information about the application. Elastic Beanstalk automatically launches an environment and creates and configures the AWS resources needed to run your code. After your environment is launched, you can then manage your environment and deploy new application versions. The following diagram illustrates the workflow of Elastic Beanstalk.
![](https://github.com/Petabytz/AWS-Projects/blob/master/Application%20Deplyoment%20using%20Elastic%20BeanStalk/Screenshot%20from%202019-07-19%2016-16-28.png)
After you create and deploy your application, information about the application—including metrics, events, and environment status—is available through the AWS Management Console, APIs, or Command Line Interfaces, including the unified AWS CLI. 
### Benefits of using Elastic Beanstalk:
AWS Elastic Beanstalk provides a cost-effective price in which you only have to pay for what you use and there are no hidden costs.
Amazon Elastic BeanStalk builds the setup and configures. Moreover, it spectators the AWS services such as EC2, Elastic Load Balancers, RDS to create Web Services.
Fast access is provided such that you can access the AWS management console in less than one hour.
EBS creates a fairly standard configuration for modern railway applications.
AWS Elastic Beanstalk supports Java .NET, PHP, Node.js, Python, Ruby, Go, and many more web applications.
SQL Server is configured through Amazon RDS but there is an EC2 server behind with a database running on it.
### AWS Elastic Beanstalk Components
Application:
An application in Elastic Beanstalk is conceptually similar to a folder
An application is a collection of components including environments, versions and environment configuration

Application Version:
An application version refers to a specific, labeled iteration of deployable code for a web application
An application version points to an Amazon S3 object that contains the deployable code such as a Java WAR file
Environment:
Environments within Elastic Beanstalk Application is where the current version of the application will be active.
Each environment runs only a single application version at a time. But it is possible to run same or different versions of an application in many environments at the same time.
Environment Tier:
Based on requirement beanstalk offers two different Environment tiers: Web Server Environment, Worker Environment
Web Server Environment: Handles HTTP requests from clients
Worker Environment: Processes background tasks which are resource consuming and time intensive
Here is an illustration to show how Application, Application version and Environments relate to each other:
![](https://github.com/Petabytz/AWS-Projects/blob/master/Application%20Deplyoment%20using%20Elastic%20BeanStalk/Screenshot%20from%202019-07-19%2016-37-37.png)
And here is how Beanstalk Environment using default container type looks like:
![](https://github.com/Petabytz/AWS-Projects/blob/master/Application%20Deplyoment%20using%20Elastic%20BeanStalk/Screenshot%20from%202019-07-19%2016-44-07.png)
### Elastic Beanstalk Architecture:
![](https://github.com/Petabytz/AWS-Projects/blob/master/Application%20Deplyoment%20using%20Elastic%20BeanStalk/Screenshot%20from%202019-07-19%2016-49-22.png)
Elastic Beanstalk environment requires an environment tier, platform, and
environment type 
Environment tier determines whether Elastic Beanstalk provisions resources to support a web application that handles HTTP(S) requests or a web application that handles background-processing tasks.
### Web Environment Tier 
An environment tier whose web application processes web requests is known as a web server tier. 
AWS resources created for a web environment tier include a Elastic Load Balancer, an Auto Scaling group, one or more EC2 instances .
Every Environment has a CNAME url pointing to the ELB, aliased in Route 53 to ELB url .
Each EC2 server instance that runs the application uses a container type, which defines the infrastructure topology and software stack .
A software component called the host manager (HM) runs on each EC2 server instance and is responsible for 
Deploying the application 
Aggregating events and metrics for retrieval via the console, the API, or the command line 
Generating instance-level events 
Monitoring the application log files for critical errors 
Monitoring the application server 
Patching instance components 
Rotating your application’s log files and publishing them to S3 .
### Worker Environment Tier  
An environment tier whose web application runs background jobs is known as a worker tier 
AWS resources created for a worker environment tier include an Auto Scaling group, one or more Amazon EC2 instances, and an IAM role. 
For the worker environment tier, Elastic Beanstalk also creates and provisions an SQS queue, if one doesn’t exist 
When a worker environment tier is launched, Elastic Beanstalk installs the necessary support files for the programming language of choice and a daemon on each EC2 instance in the Auto Scaling group reading from the same SQS queue 
Daemon is responsible for pulling requests from an SQS queue and then sending the data to the web application running in the worker environment tier that will process those messages 
Elastic Beanstalk worker environments support SQS dead letter queues which can be used to store messages that could not be successfully processed. Dead letter queue provides the ability to sideline, isolate and analyze the unsuccessfully processed messages 
One environment cannot support two different environment tiers because each requires its own set of resources; a worker environment tier and a web server environment tier each require an Auto Scaling group, but Elastic Beanstalk supports only one Auto Scaling group per environment.
### Deploy an Application on Elastic Beanstalk
Deploying an application on Elastic Beanstalk is a fairly simple process. Let’s see how to deploy an application stepwise.
### Step 1: On Elastic Beanstalk console click on Create New Application option. A dialog box appears where you can give a name and appropriate description for your application.
![](https://github.com/Petabytz/AWS-Projects/blob/master/Application%20Deplyoment%20using%20Elastic%20BeanStalk/Screenshot%20from%202019-07-19%2016-54-53.png)
### Step 2: Now that the application folder has been created, you can click on the Actions tab and select Create Environment option. Beanstalk provides you with an option where you can create multiple Environments for your application.
![](https://github.com/Petabytz/AWS-Projects/blob/master/Application%20Deplyoment%20using%20Elastic%20BeanStalk/Screenshot%20from%202019-07-19%2016-56-53.png)
### Step 3: Choose among two different Environment Tier options. Choose Web Server Environment if you want your application to handle HTTP requests or choose Worker Environment to handle background tasks.
![](https://github.com/Petabytz/AWS-Projects/blob/master/Application%20Deplyoment%20using%20Elastic%20BeanStalk/Screenshot%20from%202019-07-19%2016-58-12.png)
### Step 4: Another dialog appears, where you need to provide a domain name and description for your application.
![](https://github.com/Petabytz/AWS-Projects/blob/master/Application%20Deplyoment%20using%20Elastic%20BeanStalk/Screenshot%20from%202019-07-19%2017-00-00.png)
### Step 5: Choose a platform of your choice for your application. Elastic Beanstalk will provide you with multiple options. You can choose a sample application provided by Beanstalk, or upload a file which has code for your application.
![](https://github.com/Petabytz/AWS-Projects/blob/master/Application%20Deplyoment%20using%20Elastic%20BeanStalk/Screenshot%20from%202019-07-19%2017-04-32.png)Beanstalk will take a few minutes to launch an Environment. Once the Environment is launched, on the navigation pane you can see multiple options where you can change the configuration of your application, view log files, and events. Since you’re already on Environment page, try exploring different features that Beanstalk offers.
![](https://github.com/Petabytz/AWS-Projects/blob/master/Application%20Deplyoment%20using%20Elastic%20BeanStalk/Screenshot%20from%202019-07-19%2017-06-32.png)
![](https://github.com/Petabytz/AWS-Projects/blob/master/Application%20Deplyoment%20using%20Elastic%20BeanStalk/Screenshot%20from%202019-07-19%2017-08-56.png)
### Step 6: On the top right corner, you will find the URL of your application version. Click on that URL. You will be taken to a page which will confirm that you have successfully launched your application on Elastic Beanstalk.
You have successfully deployed an application on Elastic Beanstalk Platform.
### Conclusion:
Hence, in this AWS EBS , we studied Elastic Beanstalk helps us to easily manage the infrastructure of the AWS. EBS offers fine-grain control which helps us to choose between different technologies that are most helpful for us. Automation feature of EBS helps us to deploy applications with an ease. Containers like Docker help to enhance the deployment process. If one wants to reduce system operations focus on developing codes without worrying about other issues AWS Elastic Bean Stalk is a good choice. 
