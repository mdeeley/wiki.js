---
title: ADO Service principles
description: 
published: true
date: 2025-05-05T11:58:56.068Z
tags: 
editor: markdown
dateCreated: 2025-05-05T11:58:56.068Z
---

# Header
Your content here

All code and configuration for PLM will be stored in ADO source control.
We will use ADO to automate the build and deployment of the PLM code and configuration across environments.
To enable this, we need to configure ADO to have access to the PP environments.

There are a number of steps to do this. Happy to have a call to work through this.

1) Install Power admin CLI
2) Connect to PP through CLI
3) Register service principle for PROD environment

1) The PP command line interface (CLI) can be installed from the link below.
https://learn.microsoft.com/en-us/power-platform/developer/howto/install-cli-msi
Direct link to download -> https://aka.ms/PowerAppsCLI

After install on local PC, 
open a powershell window.
run "pac"
which should display the version number for Power Platform CLI.

2) Now we connect to the environment using the cli

In powershell, run
pac auth create --environment "<PROD>"
This will create a connection for you to the PP environment. You will need to enter your credentials.

3) Now we create a service principal for the environment which we can use to connect from DevOps to PROD.
pac admin create-service-principal  --environment <PROD>

This will register an application ID in Azure AD,
configure an application account in the PP environment using the application ID,
Give the application account a PP security role.

Please note down and send me the output of this command, it will look like this.

Application Name         <My Environment name>
Tenant Id                xxx
Application Id           xxx
Service Principal Id     xxx
Client Secret            xxx
Client Secret Expiration xxx
System User Id           xxx

