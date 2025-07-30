
### Activity 2: Deploy Azure Services

Deployment

 From the terminal, switch to the start branch:

   bash:
++++git checkout start++++

 Navigate to the correct folder:

   bash:
++++cd csharp/infra++++

 Log in to Azure using AZD.

   bash:
++++azd auth login++++

 Provision the Azure services and deploy the application.

   bash:
++++azd up++++

This step will take approximately 10-15 minutes.

[!IMPORTANT] 
> [!help] If you encounter any errors during the deployment, rerun azd up to continue the deployment from where it left off. This will not create duplicate resources, and tends to resolve most issues.


