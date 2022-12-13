# Basic-WebApp-AWS

I built a simple webapp on AWS, connected the WebApp to a serverless backend and then finally I added some interactivity with an API and Database.
- Fill in your name (or whatever you prefer) and choose the Call API button.
- You should see a message that starts with Hello from Lambda followed by the text you filled in.

The AWS service I used to complete the project include:
 - AWS Amplify
 - Amazon API Gateway
 - AWS Lambda
 - AWS IAM
 - Amazon DynamoDB
The cost implication of carrying out this project was `$0` as all the resources used are Free Tier eligible.

The project is divided into 5 parts:
- Create Web App
- Build Serverless Function
- Link Serverless Function to Web App
- Build Data Table
- Add interactivity to WebApp.
At the end this is what the application architecture should look [like](/architecture.PNG)


# 1. Create WebApp
This part involves using `Amazon Amplify` to deploy static resources of the webapp, including HTML, CSS.
Other files like JavaScript, images, can also be uploaded and hosted by AWS Amplify. I selected the Amplify service because it makes it straightforward to host and deploy static websites. End users will access the site using the URL exposed by Amplify, for most real applications, a "custom domain would be preferrable and if this is the case, `Amplify` also readily provides support for custom domains.

# 2. Build Serverless Function
Here I wrote a small piece of python [code](/lambda_fuction.py) which is responsible for adding interactivity to the webapp later on.
I empolyed the use of `AWS Lambda`, a compute service that let's you create serverless functions, eliminating the need to manage software and hardware.
These functions are triggered by specific events defined in the code. I wrote some [JSON](/test.json) events to test and ensure that everything worked properly.
`AWS Lambda` is a very affordable service as you are only charged for the number of events you process, not the idle time. Best of all, you do not have to worry about managing any servers.

# 3. Link Serverless Function to WebApp
I used `Amazon API Gateway` to create a RESTful API that would enable making calls to the Lambda function created earlier from the web client (end users web browser) The API Gateway would act as the middleman between the HTML client created in part one and the serverless backend created in part two.
- Http methods were defined on the API and configured such that the lambda function can be triggered by the API
- Cross-Origin resource share (CORS) is also enabled on the API so it can consume resources from a different origin.


# 4. Create a Data Table
For this part, for storage, I created a Table to persist data using `Amazon DynamoDB`.
`DynamoDB` is a key-value database service, therefore there's no need to create a schema for my data.
It is highly scalable and also serverless.
This is also where the `AWS Identity and Access Management` comes in. 
It is neccessary to securely give the services used, the neccessary [permissions](/policy.json) to interact with each other, specifically I added a policy to the role created for the "Lambda function" which will allow it to write to our newly created DynamoDB table, using `AWS SDK`, the Python [code](/lambda_fuction.py) used to create the lambda function in part 2 already reflects this modification.

# 5. Add Interactivity to WebApp
Back on `Amplify` The HTML [file](/index (2).zip) is updated to invoke the REST API created in part 3. This is what enables the WebApp to display text based on user input.

