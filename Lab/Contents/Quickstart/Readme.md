# [TODO update for newest version of app] Quick Start Guide

This is a quick start guide for getting the MotoCorpService app quickly up and running.
This app is a car service representative mobile app to help service customers. In this app, you can search for customers, add new customers, see customer details, and add new customer visits.

## Setup Environment

- [X] [Install Node](https://nodejs.org/en/) 
- [X] [Ionic cli](http://ionicframework.com/getting-started/)
```bash
npm install -g cordova ionic
```
- [X] Install the MobileFirst CLI
```bash
npm install -g mfpdev-cli
```
- [X] Make sure to either by using JVM 1.7 or 1.8. You can check your java version in the CLI with
```bash
java -version
```
- [X] Install [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [X] Optional for Secure Gateway - [Install Docker](https://docs.docker.com/engine/installation/)

- [X] Register with Bluemix: Check out the services [MobileFirst Foundation](https://console.ng.bluemix.net/catalog/services/mobile-foundation/) and [SecureGateway](https://console.ng.bluemix.net/catalog/services/secure-gateway/)

> Here is a [quick start guide](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/quick-start/cordova/
) on running MobileFirst Apps in Cordova.


## Run the App

### 1 - Add your MobileFirst foundation server with the following. 
Included are the default settings for username/passwords.
```bash
mfpdev server add 
? Enter the name of the new server profile: bluemix-server
? Enter the fully qualified URL of this server: http://<server>:80
? Enter the MobileFirst Server administrator login ID: mfpRESTUser
? Enter the MobileFirst Server administrator password: mfpadmin
? Save the administrator password for this server?: Yes
? Enter the context root of the MobileFirst administration services: mfpadmin
? Enter the MobileFirst Server connection timeout in seconds: 30
? Make this server the default?: (Y/n) Y
```
### 2 - git clone this repo
```bash
mkdir MobileFirstLab
cd MobileFirstLab
git init
git clone https://github.ibm.com/cord-americas/MotoCorpService.git
git remote add origin https://github.ibm.com/cord-americas/MotoCorpService.git
```
### 3 - Navigate to MotoCorp in the cli 
```bash
cd MotoCorpService/MotoCorp/
```
### 4 - Add the MobileFirst Plugin 
```bash
cordova plugin add cordova-plugin-mfp
```
### 5 - Add the ios plaform
```bash
ionic platform add ios
```
### 6 - Register the application
```bash
mfpdev app register
```
### 7 - Build the ios platform. You will need to do this every time you make a change in the app.
```bash
ionic build ios
```
### 8 - Build the adapters and deploy them with by uploading the `.adapter` file in `target/` in the MFP Console. 
The CustomerInfo adapter is to query, add, and view customer info.
```bash
cd ../Adapters/CustomerInfo
mfpdev adapter build
# upload CustomerInfo.adapter file
```
The SecureGatewayAdapter and SecureGatewayBinder is to allow the app to use SecureGateway.
> **note:** This step requires the secure gateway setup to be already done, to allow the right properties to be provided on the adapter, defore deploying it. 
```bash
cd ../SecureGatewayAdapter
mfpdev adapter build
# upload SecureGatewayAdapter.adapter file
```

The UserLogin adapter is used to perform the authentication of a user before allow access to resources.
```bash
cd ../UserLogin
mfpdev adapter build
# upload UserLogin.adapter file
```
### 9 - In the MobileFirst Operations Console add the scope to map your user-restricted scope to UserLogin
This will map the security check to call the UserLogin adapter to validate a user has the same username and password.
![Scope Mapping](/Lab/img/scope-mapping.png)

> Add a client secret so you can login

### 10 - Run the NodeJS app which will mock the on-prem CRM by going into
```bash
cd ../../onPremSimulator/demo_server
npm install
node server.js
```
### 11 - Set up Secure Gateway
Please see lab [2.1. quick-start-sgw](./2.1.%20quick-start-sgw.md). for further instructions on how to set up SecureGateway. Doing this will create a secure tunnel through a firewall for your app to gain access to an on-prem resource. 
This is optional to getting this app up and running, but without this service will not be able to access a resource across a firewall.

### 12 - Check in the operations console to make sure CustomerInfo adapter is pointing to `http://localhost:8080/customers` or your SecureGatewayEndpoint.
> **Attention:** This step will only work to point to localhost, if you are testing in  local mfp server, in case you are testing on bluemix Security Gateway setup on step 11 it is mandatory to allow bluemix to reach your OnPrem machine, in this case your localhost. 

![Scope Mapping](/Lab/img/on-prem-crm.png)

> **Update Customer Info Endpoint**: http:// + Secure Gateway Endpoint + /customers/ 
> e.g.: http://cap-sg-prd-4.integration.ibmcloud.com:15816/customers/

### 13 - You can now view your app by emulating it in the simulator.
```bash
#go back to project main folder, then:
cd MotoCorpService/MotoCorp/
cordova build ios
cordova emulate ios
```
### 14 - Login to the app with the same username/password (at least 4 characters, pre-populated).
You will now be able to search customers, add new customers, add new visits.
You can search for a customers like "Lime."

![Demo](/Lab/img/demo.gif)