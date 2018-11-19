---
layout: post
title:  "Setup a local Dynamics 365 for Finance and Operations development VM as build machine"
---

When you set up a build machine in your Dynamics 365 for Finance and Operations project, as part of your subscription or Cloud Hosted Environment, the setup of the build configuration is taken care of. But sometimes, for whatever reason, you wish you could execute builds locally as well. As <a href="https://community.dynamics.com/365/financeandoperations/b/365foroperationstechnical/archive/2018/02/14/setup-a-local-build-vm">this post</a> describes: You can.

Just for my own personal acrhive, I'm reposting the steps here.

## Manual steps to create a local Dynamics 365 for Finance and Operations build VM

1. Download the VHD from LCS and deploy it in your local infrastructure (or run it on your laptop ;) )
2. Login to the VM using RDP
3. Rename the machine, change the Reporting Services database connection using "Reporting Services Configuration Manager" and reboot 
4. Logon to your Azure DevOps/VSTS project and create an Access token (similar that you need when linking an LCS project to your Azure DevOps/VSTS project) and copy the token for later use
5. On the VM, navigate to the `C:\DynamicsSDK` folder
6. Edit *__SetupBuildAgent.ps1:__*
   - Add your Azure DevOps/VSTS project URL to `$VSO_ProjectCollection` variable. Change attribute Mandatory to false (otherwise the script will prompt you for input when it's executed).
   - Add the Access token to `$VSOAccessToken` variable. 
7. Edit *__BuildEnvironmentReadiness.ps1:__* 
   - Add your Azure DevOps/VSTS project URL to `$VSO_ProjectCollection` variable. Change attribute Mandatory to false.
   - Add your Azure DevOps/VSTS project name to `$ProjectName` variable. Change attribute Mandatory to false.
   - Add the access token to `$VSOAccessTokenVariable`.
8. Execute *__SetupBuildAgent.ps1__*
9. Execute *__BuildEnvironmentReadiness.ps1__*

Now you should see the build definition in VSTS, as well as the build agent queue and you can test the build process by "Queue new build...".