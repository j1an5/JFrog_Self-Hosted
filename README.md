Installing the JFrog Platform
This section will help you get started with the JFrog Platform installation initial steps and planning.

Before You Begin
In this section, you will find information to get you started with installing or upgrading whether you are a new user or an existing user to the JFrog Platform. Depending on your subscription type and organization's needs, you can install the complete set of JFrog products or start with JFrog Artifactory as the base. To learn about the JFrog supported subscriptions, see the JFrog offering.

To get started with a new installation, you'll need to know the following:

Your subscription type. If you don’t have an existing subscription, Start a Free Trial.
Your selected JFrog products to install.
Review the System Architecture and System Requirements
Plan your new installation.
Installing JFrog Products by Subscription Type
To use JFrog products, you'll need to first install JFrog Artifactory. Once installed, you can continue to install additional products according to your subscription type and organization needs. 

The new major versions of the JFrog Platform products are not backward compatible with previous major versions.

JFrog products should be installed in the following order:

Install Artifactory
Install Insight (Enterprise/Enterprise+ only)
Apply your JFrog License depending on your license type, you can:
either deploy a license manually to Artifactory (using the onboarding wizard on initial startup)
or use a license bucket after connecting Insight
Install Artifactory Edge node(s) (optional)
The installation process is identical to installing any other Artifactory instance.
Install Xray
Trial License: upload it to your JPD as a separate file. Read more about the JFrog Xray trial.
Install Distribution
Configure Distribution
Install Pipelines
Below are the products you have access to for each subscription. 

Products in purple are mandatory and products in black are optional depending on your organization needs.


Artifactory OSS (Free)
JFrog Artifactory Open Source: Maven and Generic Package Manager Repository 
Artifactory CE (Free)
JFrog Artifactory Conan Edition: Conan C/C++ and Generic Package Manager Repository 
JFrog Container Registry (Free)
JFrog Container Registry (Powered by Artifactory): Docker, Helm and Generic Package Manager Repository 
Pro
JFrog Artifactory: Universal Package Manager Repository 
Pro X
JFrog Artifactory: Universal Package Manager Repository
JFrog Xray: Security and Compliance Scanning
Enterprise X
JFrog Artifactory: Universal Package Manager Repository
JFrog Xray: Security and Compliance Scanning
Enterprise+
JFrog Artifactory: Universal Package Manager Repository
JFrog Insight: Manage DevOps Insights
JFrog Xray: Security and Compliance Scanning
JFrog Pipelines: CI/CD pipeline orchestration 
JFrog Distribution: Global software distribution



