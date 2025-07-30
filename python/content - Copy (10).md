####Activity 5: Test your Work

With the activities in this module complete, it is time to test your work.

Running the ChatAPI and Frontend App

 Start the ChatAPI

• Codespaces open a terminal and type dotnet run
• In your IDE press F5 or select Run to start the application.

2. Run the Frontend App

• Open a new terminal or use an existing one that is open to the /frontend folder.

```sh:
ng serve
```
• Navigate to, <http://localhost:4200> in your browser

3. Start a Chat Session

 Login to the frontend app.
 Start a new conversation.
 Send the message:  

   ```text:
Hello, how are you?
```
 You should see something like the output below.

    Test output in French（./media/module-01/test-output.png）

4. Stop the Application

• In the frontend terminal, press Ctrl + C to stop the application.
• In your IDE press Shift + F5 or stop the debugger.

Validation Checklist

Your implementation is successful if:

• [ ] Your app compiles with no warnings or errors.
• [ ] Your agent successfully processes user input and generates and appropriate response.

Common Issues and Troubleshooting

 Frontend issues:
   • If frontend doesn't fully start, navigate to /frontend/src/environments/environment.ts and update apiUrl: 'https://localhost:63279/'
   • Frontend will restart

