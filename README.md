# ai-recipe-generator
Overview :

The Serverless AI Recipe Generator is a cloud-based application that uses artificial intelligence to generate personalized recipes from user-provided ingredients. It utilizes AWS Amplify for the frontend, Amazon Bedrock for AI-powered recipe generation, and AWS Lambda for backend processing. Users simply input ingredients, and the AI creates a recipe, which is then displayed on the web interface. This serverless architecture ensures high scalability, and cost-efficiency and eliminates the need for server management.

Step 1 : Host static website<br>
Create a GitHub Repository.<br>
Sign in to GitHub at https://github.com/.<br>
In the Start a New Repository section, make the following selections:<br>
For the Repository name, enter ai-recipe-generator, and choose the Public radio button.<br>
Then select, Create a new repository.<br>

Open a new terminal window, navigate to your projects root folder (ai-recipe-generator), and run the following commands to initialize a git and push of the application to the new GitHub repo:<br>
git init <br>
git add README.md <br>
git commit -m "first commit" <br>
git remote add origin git@github.com:<your-username>/ai-recipe-generator.git git branch -M main <br>
git push -u origin main<br>
Create a New React Application<br>
Clone the repository on your laptop through the GitHub app before you apply the above given code. Then, save the project file on your computer.<br>
In a new terminal or command line window, run the following command after calling the file on your computer to use Vite to create a React application:<br>
npm create vite@latest ai-recipe-generator -- --template react-ts -y<br>
cd ai-recipe-generator<br>
npm install<br>
npm run dev<br>

Install Amplify Packages<br>
Open a new terminal window, navigate to your app’s root folder (ai-recipe-generator), and run the following command:<br>
npm create amplify@latest -y<br>
Running the previous command will scaffold a lightweight Amplify project in the app’s directory. <br>
In your terminal window, run the following command to push the changes to GitHub: <br>
git add .<br>
git commit -m 'installing amplify'<br>
git push origin main<br>

Deploy App on Amazon Amplify<br>
Sign in to the AWS Management console in a new browser window, and open the AWS Amplify console.<br>
Choose Create new app. <br>
On the Start building with Amplify page, for Deploy your app, select GitHub, and select Next.<br>
When prompted, authenticate with GitHub. You will be automatically redirected back to the Amplify console. Choose the repository and main branch you created earlier. Then select Next.<br>
Leave the default build settings, and select Next.<br>
Review the inputs selected, and choose Save and Deploy.<br>
AWS Amplify will now build your source code and deploy your app at https://…amplifyapp.com, and on every git push your deployment instance will update. It may take up to 5 minutes to deploy your app.<br>
Once the build completes, select the Visit deployed URL button to see your web app up and running live. <br>

Step 2 : Manage Users<br>
Set-up Amplify Auth<br>
On your local machine, navigate to the ai-generated-recipes/amplify/auth/resource.ts file and update it with the following code. Then, save the file.<br>
Set-up Amazon Bedrock Model Access<br>
Sign in to the AWS Management console in a new browser window, and open the AWS Amazon Bedrock console.<br>
Verify that you are in the N. Virginia us-east-1 region, and choose Get started.<br>
In the Foundation models section, choose the Claude model.<br>
Scroll down to the Claude models section, and choose the Claude 3 Sonnet tab, and select Request model access.<br>
In the Base models section, for Claude 3 Sonnet, choose Available to request, and select Request model access.<br>
On the Edit model access page, choose Next.<br>
On the Review and Submit page, choose Submit.<br>

Step 3 : Build Serverless Back-end<br>
Create a Lambda Function for Handling Request<br>
On your local machine, navigate to the ai-recipe-generator/amplify/data folder, and create a file named bedrock.js. <br>
Then, update the file with the following code:<br>
The following code defines a request function that constructs the HTTP request to invoke the Claude 3 Sonnet foundation model in Amazon Bedrock. The response function parses the response and returns the generated recipe.<br>
Update the amplify/backend.ts file with the following code. Then, save the file.<br>
The code adds an HTTP data source for Amazon Bedrock to your API and grant it permissions to invoke the Claude model.<br>

Step 4 : Deploy the back-end API<br>
Set-up Amplify Data<br>
On your local machine, navigate to the ai-recipe-generator/amplify/data/resource.ts file, and update it with the following code. Then, save the file.<br>
The following code defines the askBedrock query that takes an array of strings called ingredients and returns a BedrockResponse. The .handler(a.handler.custom({ entry: “./bedrock.js”, dataSource: “bedrockDS” })) line sets up a custom handler for this query, defined in bedrock.js, using bedrockDS as its data source.<br>
Open a new terminal window, navigate to your apps project folder (ai-recipe-generator), and run the following command to deploy cloud resources into an isolated development space so you can iterate fast.<br>
Once the cloud sandbox has been fully deployed, your terminal will display a confirmation message and the amplify_outputs.json file will be generated and added to your project. <br>
npx ampx sandbox<br>

Step 5 : Build the Front-end<br>
Install Amplify Libraries<br>
You will need two Amplify libraries for your project. The main aws-amplify library contains all of the client-side APIs for connecting your app’s frontend to your backend, and the @aws-amplify/ui-react library contains framework-specific UI components.<br>
npm install aws-amplify @aws-amplify/ui-react<br>
On your local machine, navigate to the ai-recipe-generator/src/index.css file, and update it with the following code to center the App UI. Then, save the file.<br>
Update the src/App.css file with the following code to style the ingredients form. Then, save the file.<br>
On your local machine, navigate to the ai-recipe-generator/src/main.tsx file, and update it with the following code. Then, save the file.<br>
The code will use the Amplify Authenticator component to scaffold out an entire user authentication flow allowing users to sign up, sign in, reset their password, and confirm sign-in for multifactor authentication (MFA).<br>
Open the ai-recipe-generator/src/App.tsx file, and update it with the following code. Then, save the file.<br>
The code starts by configuring the Amplify library with the client configuration file (amplify_outputs.json). It then generates a data client using the generateClient() function. The app presents a form to users for submitting a list of ingredients. Once submitted, it will use the data client to pass the list to the askBedrock query and retrieve the generated recipe then display it to the user.<br>

Step 6 : Final Project<br>
Open a new terminal window, navigate to your projects root directory (ai-recipe-generator), and run the following command to launch the app:<br>
npm run dev<br>
Select the Local host link to open the Vite + React application.<br>
Choose the Create Account tab, and use the authentication flow to create a new user by entering your email address and a password. Then, choose Create Account.<br>
You will get a verification code sent to your email. Enter the verification code to log in to the app.<br>
When signed in, you can start inputting ingredients and generating recipes.<br>
