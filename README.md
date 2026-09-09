[DQE Unify (Legacy version)](https://elements.heroku.com/addons/dqe-unify-server) is an Heroku add-on to deduplicate, clean, and qualify your Heroku Postgres, Salesforce, and Data Cloud databases. The system operates as a microservice. Our managed package on Salesforce installs the interface, allowing you to launch and customize each process for deduplication, cleaning, and qualification.
 
This article shows how to install and manage the add-on. For instructions on how to use DQE Unify Server, [see our documentation](https://helpcenter.dqe.tech/hc/en-gb/sections/11331940017809-Salesforce).
 
## Prerequisites
 
Before provisioning the add-on, install the [Heroku CLI](heroku-cli).
 
Log in to your Heroku account and follow the prompts to [create a SSH public key](keys#generate-an-ssh-key):
 
```term
$ heroku login
```
 
## Sample App with Heroku Deploy
 
Install this sample Python app that automatically adds the buildpacks required for the add-on.
 
[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://www.heroku.com/deploy?template=https://github.com/DQE-SOFTWARE/dqe-one-button)
 
If you want to create an app on your own, follow the instructions in [Install Buildpacks](#buildpacks).
 
## Provisioning the Add-on
> callout
> Reference the [DQE Unify Elements Page](https://elements.heroku.com/addons/dqe-Unify-server) for a list of available plans and regions.
 
You can attach DQE Unify Server to a Heroku application via either the CLI:
 
```term
$ heroku addons:create dqe-unify-server:v1-1 --app your-app-name
```
 
Or by clicking **`Install DQE Unify`** in the [Elements Marketplace](https://devcenter.heroku.com/articles/dqe-unify-server).
 
## Installation
 
### Add-on
 
You must install the [Heroku Key-Value Store](https://elements.heroku.com/addons/heroku-redis) and [CloudAMQP](https://elements.heroku.com/addons/cloudamqp) add-ons before using DQE Unify:
 
```term
$ heroku addons:create heroku-redis:mini --app your-app-name
 
$ heroku addons:create cloudamqp:lemur --app your-app-name
```
 
### Buildpacks
 
After provisioning the add-ons, install these buildpacks:
 
>note
>You must install these buildpacks in the exact order.
 
- [DQE Unify buildpack](https://github.com/DQE-SOFTWARE/dqe-unify-buildpack)
- [DQE BSDDB buildpack](https://github.com/DQE-SOFTWARE/dqe-unify-bsddb3-buildpack)
- [Heroku Python buildpack](https://elements.heroku.com/buildpacks/heroku/heroku-buildpack-python)
 
Install the DQE Unify buildpack:
 
```term
$ heroku buildpacks:add --index 1 dqe-software/dqe-one-buildpack --app your-app-name
Buildpack added. Next release on dqe-one will use dqe-software/dqe-one-buildpack.
Run git push heroku main to create a new release using this buildpack.
```
 
Install the Heroku Python buildpack:
 
```term
$ heroku buildpacks:add heroku/python --app your-app-name
Buildpack added. Next release on dqe-one will use dqe-software/dqe-one-buildpack.
Run git push heroku main to create a new release using this buildpack.
```
 
## Environment Variables
 
To complete the deployment, you must put your DQE license into the `DQE_ONE_SERVER_LICENSE` variable.
 
## Deploy the Heroku App
 
Clone the repository with `git:clone` to clone your app source code to your local machine. Then create a `.python-version` file, containing "3.11.13" in it:
 
```term
$ heroku git:clone -a your-app-name
$ cd your-app-name
```
 
### Unix
 
```term
$ echo "3.11.13" > .python-version
```
 
### Windows Powershell
 
```term
$ "3.11.13" | Out-File -Encoding ascii -NoNewline ".python-version"
```
 
Make some changes to the code you just cloned and deploy them to Heroku using the `git` commands:
 
```term
$ git add .
$ git commit -am "first deploy"
$ git push heroku master
```
 
## Launch DQE Unify
 
The DQE Unify buildpack contains a [Procfile](https://devcenter.heroku.com/articles/procfile) that creates two dynos that you must start:
 
```term
$ heroku ps:scale worker=1 web=1 --app your-app-name
Scaling dynos... done, now running worker at 1:Hobby, web at 1:Hobby
```
 
## Removing the Add-on
 
You can remove DQE Unify Server via the Heroku CLI:
 
>warning
> This destroys all associated data and you can’t undo it!
 
```term
$ heroku addons:destroy dqe-one-server --app your-app-name
```
 
## Support
 
Submit all DQE Unify support and runtime issues via one of the [Heroku Support channels](support-channels). For other questions or suggestions, email us at [support@dqe-software.com](mailto:support@dqe-software.com).
