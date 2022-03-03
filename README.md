# Installing the JFrog Platform
This section will help you get started with the JFrog Platform installation initial steps and planning.

## Before You Begin
In this section, you will find information to get you started with installing or upgrading whether you are a new user or an existing user to the JFrog Platform. Depending on your subscription type and organization's needs, you can install the complete set of JFrog products or start with JFrog Artifactory as the base. To learn about the JFrog supported subscriptions, see the JFrog offering.

**To get started with a new installation**, you'll need to know the following:

1. Your subscription type. If you don’t have an existing subscription, [Start a Free Trial](https://www.jfrogchina.com/start-free/).
2. Your selected JFrog products to install.
3. Review the System Architecture and System Requirements
4. Plan your new installation.

### Installing JFrog Products by Subscription Type
To use JFrog products, you'll need to first install JFrog Artifactory. Once installed, you can continue to install additional products according to your subscription type and organization needs. 

>The new major versions of the JFrog Platform products are not backward compatible with previous major versions.

JFrog products should be installed in the following order:

1. [Install Artifactory](https://www.jfrog.com/confluence/display/JFROG/Installing+Artifactory)
2. Install Insight (Enterprise/Enterprise+ only)
3. Apply your JFrog License depending on your license type, you can:
    * either deploy a license manually to Artifactory (using the onboarding wizard on initial startup)
    * or use a license bucket after connecting Insight
4. Install Artifactory Edge node(s) (optional)
    * The installation process is identical to installing any other Artifactory instance.
5. [Install Xray](https://www.jfrog.com/confluence/display/JFROG/Installing+Xray)
    * Trial License: upload it to your JPD as a separate file. Read more about the JFrog Xray trial.
6. Install Distribution
    * Configure Distribution
7. Install Pipelines

Below are the products you have access to for each subscription. 
>Products in purple are mandatory and products in black are optional depending on your organization needs.

| Subscription Type | JFrog Products to Install |
| ---- | ---- |
| Artifactory OSS (Free) |JFrog Artifactory Open Source: Maven and Generic Package Manager Repository |
| Artifactory CE (Free) | JFrog Artifactory Conan Edition: Conan C/C++ and Generic Package Manager Repository |
| JFrog Container Registry (Free) | JFrog Container Registry (Powered by Artifactory): Docker, Helm and Generic Package Manager Repository |
| Pro | JFrog Artifactory: Universal Package Manager Repository |
| Pro X | JFrog Artifactory: Universal Package Manager Repository <br>JFrog Xray: Security and Compliance Scanning |
| Enterprise X | JFrog Artifactory: Universal Package Manager Repository <br>JFrog Xray: Security and Compliance Scanning |
| Enterprise+ | JFrog Artifactory: Universal Package Manager Repository <br>JFrog Insight: Manage DevOps Insights <br>JFrog Xray: Security and Compliance Scanning <br>JFrog Pipelines: CI/CD pipeline orchestration  <br>JFrog Distribution: Global software distribution |



