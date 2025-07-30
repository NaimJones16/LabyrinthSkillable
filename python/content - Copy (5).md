### Activity 5: Compile and Run

Running the ChatAPI and Frontend App

 Start the ChatAPI

If running on Codespaces

 Navigate to src\ChatAPI.
2. Run the following command to trust the development certificate:

   sh:
++++dotnet dev-certs https --trust++++

3. Start the application:

   sh:
++++dotnet run++++

4. In the Ports tab, right-click and select the Port Visibility option to set port 63280 as Public.
5. Copy the URL for 63280 port.

If running locally on Visual Studio or VS Code

 Navigate to src\ChatAPI.
2. Press F5 or select Run to start the application.
3. Copy the URL from the browser window that opens.

2. Run the Frontend App

 Open a new terminal. Navigate to the frontend folder.
 Copy and run the following:

   sh:
++++npm i++++
  ++++ng serve --host 0.0.0.0++++

If running locally

 Open your browser and navigate to <http://localhost:4200/>.

If running on Codespaces

 From the PORTS tab, search for the port with the label Frontend app. Hover over the address and choose Open in Browser (second icon) to access the frontend application.

3. Start a Conversation

 Open the frontend app.
 Start a new conversation.
 Send the message:

   text:
Hello, how are you?

 You should see something like the output below.

   Test output（./media/module-00/test-output.png）

4. Stop the Application

• In the frontend terminal, press Ctrl + C to stop the application.
• In your IDE press Shift + F5 or stop the debugger.

Deployment Validation

Use the steps below to validate that the solution was deployed successfully.

• [ ] All Azure resources are deployed successfully
• [ ] You can compile the solution in CodeSpaces or locally
• [ ] You can start the project and it runs without errors
• [ ] You are able to launch the Chat Frontend app , create a new chat session, and get a reply when you send a message.

Common Issues and Troubleshooting

 Errors during azd deployment:
   • Service principal "not found" error.
   • Rerun azd up
 Azure OpenAI deployment issues:
   • Ensure your subscription has access to Azure OpenAI
   • Check regional availability
 Frontend issues:
   • If frontend doesn't fully start, navigate to /frontend/src/environments/environment.ts and update apiUrl: 'https://localhost:63279/'
   • Frontend will restart
 Connecting to backend running CodeSpaces

   • If you cannot get the front end to connect to the backend service when running in Codespaces try the following

     • Navigate to the /src/ChatAPI folder in the Terminal
     • Run the following command to trust the development certificate:

       sh:
++++dotnet dev-certs https --trust++++

     • Then start the application:

       sh:
++++dotnet run++++

   • Copy the URL from the Ports tab and use this for the environments.ts file

Success Criteria

To complete this Module successfully, you should be able to:

• Verify that all services have been deployed successfully.
• Have an open IDE or CodeSpaces session with the source code and environment variables loaded.
• Be able to compile and run the application with no warnings or errors.

Next Steps
