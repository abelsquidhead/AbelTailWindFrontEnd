# Ignite 2018 Tour DevOps Session - Deploy your code faster yet safer (or something like that)

This is the demo script for the DevOps session. In this session, we will cover

- creating a CI Yaml based pipeline from Github to Azure Pipelines.
- adding advanced devops best practices with AB Testing
- Adding automated approval gates between stages using Azure Monitoring as a quality gate
- Fast track starting a project for any language using Azure DevOps Project
- Walk through real world build and release pipelines for Tailwind Trading.

## Setup
This session's demos are done using the browser and one instance of VSCode. Open up an instance of your favorite browser and have the following tabs

1. `Tab 1 - Git hub repo for tailwind front end` https://github.com/abelsquidhead/AbelTailWindFrontEnd 
![](readmeImages/2018-11-09-07-48-15.png)
1. `Tab 2 - Tailwind Front End App Service in Portal` https://ms.portal.azure.com/#@microsoft.onmicrosoft.com/resource/subscriptions/e97f6c4e-c830-479b-81ad-1aff1dd07470/resourceGroups/AbelIgnite2018WServers/providers/Microsoft.Web/sites/AbelTailWindFrontEnd4/appServices
![](readmeImages/2018-11-09-07-48-53.png)
1. `Tab 3 - Application Insight for TailWind Traders Staging Environment` https://ms.portal.azure.com/#@microsoft.onmicrosoft.com/resource/subscriptions/e97f6c4e-c830-479b-81ad-1aff1dd07470/resourceGroups/AbelIgnite2018WServers/providers/microsoft.insights/components/AbelTailWindFrontEnd4Staging/overview 
![](readmeImages/2018-11-09-07-49-27.png)
1. `Tab 4 - Azure Portal` https://ms.portal.azure.com/#@microsoft.onmicrosoft.com/dashboard/private/a2cca8d5-8dd2-4059-aba9-a8934116167c
![](readmeImages/2018-11-09-07-50-20.png)
1. `Tab 5 - Ignite 1 DevOps Project Dashboard` https://ms.portal.azure.com/#@microsoft.onmicrosoft.com/resource/subscriptions/e97f6c4e-c830-479b-81ad-1aff1dd07470/resourceGroups/VstsRG-AzureDevOpsDemo-a-ed75/providers/microsoft.visualstudio/account/AzureDevOpsDemo-a/project/AbelIgnite2018Tour1
![](readmeImages/2018-11-09-07-52-52.png)
1. `Tab 6 - Ignite 2 DevOps Project Dashboard` https://ms.portal.azure.com/#@microsoft.onmicrosoft.com/resource/subscriptions/e97f6c4e-c830-479b-81ad-1aff1dd07470/resourceGroups/VstsRG-AzureDevOpsDemo-a-ed75/providers/microsoft.visualstudio/account/AzureDevOpsDemo-a/project/AbelIgnite2018Tour2
![](readmeImages/2018-11-09-07-56-39.png)
1. `Tab 7 - Tailwind Build All Up` https://dev.azure.com/azuredevopsdemo-a/AbelTailwindInventoryService/_apps/hub/ms.vss-ciworkflow.build-ci-hub?_a=edit-build-definition&id=27
![](readmeImages/2018-11-09-07-59-05.png) 
1. `Tab 8 - Tailwind Release All Up` https://dev.azure.com/azuredevopsdemo-a/AbelTailwindInventoryService/_releaseDefinition?definitionId=1&_a=definition-pipeline
![](readmeImages/2018-11-09-09-09-09.png)
1. `Tab 9 - Release Gate Docusign Example`
https://msvstsdemo-a.visualstudio.com/YoCoreDemo/_releaseProgress?releaseId=10&_a=release-pipeline-progress
![](readmeImages/2018-11-09-09-10-39.png)
1. `Tab 10 - Release Gate Dynatrace Unbreakable Pipeline Pass`
https://msvstsdemo-a.visualstudio.com/AbelUnbreakablePipelineDemo/_releaseProgress?releaseId=275&_a=release-pipeline-progress
![](readmeImages/2018-11-09-09-11-41.png)
1. `Tab 11 - Release Gate Dynatrace Unbreakable Pipeline Fail`
https://msvstsdemo-a.visualstudio.com/AbelUnbreakablePipelineDemo/_releaseProgress?releaseId=276&_a=release-pipeline-progress
![](readmeImages/2018-11-09-09-12-34.png)

Open up another instance of a different browser. If you opened up the first set using chrome, open up another browser using firefox or edge and in this browser have 2 tabs

1. `Tab 1 - Tailwind Traders Staging` https://abeltailwindfrontend4staging.azurewebsites.net/
![](readmeImages/2018-11-09-09-14-45.png)
2. `Tab 2 - Tailwind Traders Production` https://abeltailwindfrontend4.azurewebsites.net/
![](readmeImages/2018-11-09-09-15-37.png)

This demo will also use VSCode to make some code changes

1. VSCode open with src/style.css open

## CLEANUP

1. Delete build and release pipelines for dev.azure.com/azuredevopsdemos-a/AbelTailWind
    - abelsquidhead.AbelTailwindFrontEnd
    - abelsquidhead.AbelTailWindFrontEnd - CD 
1. Change Testing In Production for ABTest slot to 0%
1. Delete ABTest slot
1. Delete az-pipelines.yml file
1. go to `/src/style.css` and change background color. Uncomment line 10, comment line 11
![](readmeImages/2018-11-09-09-23-10.png)

## Session script

### Slide Deck
1. I am so excited to be here today to talk to you all about DevOps. Now, I know some of you are thinking, cool DevOps! And some others are probably thinking, DevOps? Who cares? That's just CI/CD Pipelines right? Why should I care about that?
 ![](readmeImages/2018-11-09-09-25-15.png)

 1. I can give you all the reasons and I can pull out charts and graphs to back up my statements. But I wanted to show you a short film that really personifies the difference of before and after DevOps
![](readmeImages/2018-11-09-09-32-26.png)

1. ![](readmeImages/2018-11-09-09-33-37.png)

1. And THAT is why we need to do DevOps!!! NOT the way we used to. All hitting servers with hammers tryng to get our code to deploy once a year. We need to be a well oiled machine like that pit crew! Continuously delivering value!

1. Now I've mentioned DevOps a lot, but what exactly is DevOps. I bet if I ask 10 people in this room what DevOps is, we will get 10 different responses. And I'm not saying anyone elses definition is wrong. But in order to frame this conversation we are having, let me give you Microsoft's Definintion of DevOps
![](readmeImages/2018-11-09-09-36-39.png)

1. At Microsoft, DevOps is something very specific. Devops is the union of people, process and products to enable the continous delivery of value to our end users. Now notice I said that super carefully. I didn't say continously deliver code. Because what will that give us, just piples and piles of code that's no use to our end users. And notice, I didn't even say continously deliver features. Because we could be delivering feature after feature, but if we are not delivering value, we are just wasting time!
![](readmeImages/2018-11-09-09-38-08.png)

1. Now why is this important? Why do should we care about DevOps. The speed of business today is SO fast, that we must adopt DevOps best practices just to keep up. If we don't, our competitors either have or they will adopt DevOps best practices. And whey they do, they WILL out innovate us and they WILL render us obsolete. And no one wants to be rendered obsolete.
![](readmeImages/![](readmeImages/2018-11-09-09-40-38.png).png)
1. This isn't just theory anymore. We now have the cold hard imperical facts that cleary demonstrate this. Adopting DevOps best practices means you are faster to market, you have lower failure rates. Much faster lead time for changes and much faster Mean time to recover. And what does all of this translate into? INCREASED REVENUE!
![](readmeImages/2018-11-09-09-42-09.png). 

1. To implement DevOps successfully, you have to attack all three pillars when writing software. You must address the People, the Process and the Products
![](readmeImages/2018-11-09-09-45-07.png)

1. For the people portion, that's the toughest change to make. This is a cultural shift that needs to take place in the organization. Where everybody from the top down all become hyperfocused on continuously delivering value. I don't want to hear, well, that's how we always do things from anyone. Everyone needs to focus on continuously delivering vaue. ![](readmeImages/2018-11-09-09-52-52.png)

1. For the process, we need to have a process that will let us interate fast enough, yet still deliver code of high enough quality. So what does that mean? I need to be able to plan my sprints, and I need to be able to check my code in and out while tracking against the work I'm doing. And as I'm checking code in and out? Builds need to kick off. Automated Tests need to be run. Security scans need to happen. And if the build are good, an automated system needs to pick up my bits and deploy them into my Dev, QA, UAT all the way out in to production. And why does this need to be automated? Potentially, this can happen many times a day! So we need to make sure the process is consistent and repeatable. Every single time like clock work. And once the code reaches production, it doesn't end there. We still need to be able to monitor our code in production. We need to know things like, is my app up or down, is my app performing well, and what are users really doing in my app? Because answers to those questions let me know if I'm delivering vaue to my end users. And if I am, we can double down on those types of activities in the next sprint. And if we aren't, then we can quickly reprioritize our backlog and course correct.![](readmeImages/2018-11-09-09-54-28.png)

1. Now all of this requires the right products and tools to help enable all of this. So we need tools that will let us track our work throughout our sprint. We need source control systems that can corrolate our work to our checkins. We need automated build and release systems that can build on everycheckin, run all of our unit tests and automate deployment all the way to production. And we need systems in place to monitor our app in production. 
![](readmeImages/2018-11-09-09-58-25.png)

1. Out there in the world, there are all sorts of tools that do these things. And Azure is an open system, which means you can keep using all of the DevOps tools you are most familiar with.
![](readmeImages/2018-11-09-10-00-45.png)

1. However, you can replace ALL of them with just one product. Azure DevOps
![](readmeImages/2018-11-09-10-01-40.png)

1. Azure DevOps is literally everything you need to take an idea and turn that idea into a working piece of software in the hands of your end users, for ANY language targeting ANY platform. Azure DevOps is a suite of 5 sepparate products that work SEAMLESSLY together. There's a work item tracking product called Azure Boards, where you can track any unit of work in your software project with visual tools to help you manage all of your work. There is Azure Pipelines, where you can build yout your CI/CD pipelines for any language targeting any platform. There is Azure Repos where you can host your own Git repo or a centralized version control system. There is Azure Test Plans for you to create, plan and run all of your manual tests. And finally there is Azure Artifacts, where you can host your package management systems, whethere they be nuget, maven or even generic packages.
![](readmeImages/2018-11-09-10-03-00.png)

1. In today's session, we will be concentrating on Azure Pipelines, where we will deploy our code faster yet safer!
![](readmeImages/2018-11-09-10-06-46.png)

### Demo
[Bring up main browser with all the tabs and bring up tab 1]

So the code for TailWind is all in github, and we need to build a CI/CD pipeline from github to Azure Pipelines. That's super easy to do with the Azure Pipelines market place extension. Lets go into the marketplace and search for the Azure Pipeline Extension

![](readmeImages/2018-11-09-10-50-00.png)

![](readmeImages/2018-11-09-10-51-07.png)

![](readmeImages/2018-11-09-10-52-12.png)

I've already installed the extension so let's just go in and configure this

![](readmeImages/2018-11-09-10-52-57.png)

![](readmeImages/2018-11-09-10-53-32.png)

![](readmeImages/2018-11-09-10-53-55.png)

And we will configure it to create a CI pipeline for our Tailwind Front End repo
    
![](readmeImages/2018-11-09-10-55-14.png)

This takes us to the login screen of Azure DevOps

![](readmeImages/2018-11-09-10-56-02.png)

![](readmeImages/2018-11-09-10-57-27.png)

Where it will take you through a wizard to help you set up a CI pipeline quickly

![](readmeImages/2018-11-09-12-10-36.png)

![](readmeImages/2018-11-09-14-16-22.png)

![](readmeImages/2018-11-09-14-16-38.png)

Notice Azure Pipelines analyzes the repo and offers you templates that make sense for the technolgogies and languages in your repo. This repo is a node.js app so let's chose the Node.js template

![](readmeImages/2018-11-09-14-18-15.png)

This creates for us an **azure-pipelines.yml** file. So it creates for you a yaml build pipeline. The build engine in Azure pipelines is basically a task runner. It does one task after another after another. And you can describe the tasks either with a visual editor or with a yaml file. There are many benefits to using yaml builds or **Pipeline as Code**. Primarily, now your build pipeline is checked right into source control so your build pipeline is versioned right alongside your source code. If you need more info on yaml builds, just follow the docs link which will take you to our docs page which descrive everything you will need to know.
   
![](readmeImages/2018-11-09-14-23-27.png) 
   
![](readmeImages/2018-11-09-14-24-23.png)

Let's go ahead and save and run this build

![](readmeImages/2018-11-09-14-26-03.png)

![](readmeImages/2018-11-09-14-26-41.png) 

![](readmeImages/2018-11-09-14-27-39.png)

This saves the azure-pipeines.yml file into our git repo which in turn fires off our build. And what happens in the build?  First it downloads all the source code from git hub, then it executes the build steps described in the yml file. So in our case it does

 - Downloads the source code
 - Installs Node.js
 - Does an npm install and build

Now this is fine but I want to customize the build a little, and I want to show you all what the visual editor looks like. So let's hope into the visual editor

![](readmeImages/2018-11-09-14-38-59.png)

![](readmeImages/2018-11-09-14-40-24.png)

So here, you can see each individual step and now, i'm configuring my build to 

   - Install npm
   - Setup the DB connection strings
   - Build the app because we are creating a staic web app out of the node.js app
   - Zip everything up so it's ready to be deployed
   - publish the zip of the website as our build artifact back to Azure Pipelines

The build system in Azure Pipelines is 100% configurable to do ANYTHING. Any language targeting any platform! And the way you customize this build is by adding and removing tasks.

![](readmeImages/2018-11-09-14-48-23.png)

![](readmeImages/2018-11-09-14-49-20.png)

Where out of the box, there are a little over a hunred tasks that you can drag over to the left and just start using.

And what if hyou want to do something that's not out of the box?

![](readmeImages/2018-11-09-14-52-17.png)

Not a big deal, just go to our marketplace where our partners have created over 500 build and release tasks that you can just download and start using.

And if you want to do something that's not out of the box and not in the marketplace? It's still not a problem because you can write your own custom tasks. Custom tasks are nothing more than powershell or node.js. So what that means is that anything you can do from the command line, you can easily get this build and release system to do as well. Which translates into you can make this build and release system do ANYTHING! Any language targeting any platform!

Ok, so we have a visual build that describe the customized build that I want to happen, so the easiest way to add this to our YAML build is to 

![](readmeImages/2018-11-09-14-55-49.png)

view the yaml that gets generated by this visual build

Let's go ahead and copy it, and then we'll just paste this into our azure-pipelines.yml file

![](readmeImages/2018-11-09-14-56-59.png)

Let's go back into VSCode and we'll pull down the latest changes with a git pull

![](readmeImages/2018-11-09-14-58-24.png)

and now let's paste the yaml and replace everything in azure-pipelines.yml 

![](readmeImages/2018-11-09-15-00-32.png)

And now lets push that yaml file back into GitHub. And once the code hits github, it will kick off a new build

![](readmeImages/2018-11-09-15-06-54.png)

![](readmeImages/2018-11-09-15-07-14.png)
   
And what's this build doing? It's downloading the latest source from github, including the azure-pipelines.yml file. It then kicks off a build and executes the build steps described in the yml file! Pipeline as code!!!. And then it will

   - Do an npm install
   - Setup my db connections
   - Build app
   - zip up the website so it's ready to be deployed
   - publish the zip file as the build artifact for this build back to Azure Pipelines

Oh and if you notice, right now, I'm just hardcoding my database end points. If we want to become more secure, we can even store secrets in the pipeline and then use the secrets in the yaml file. 

To set up secrets let's edit our build

![](readmeImages/2018-11-09-15-11-11.png)

![](readmeImages/2018-11-09-15-13-28.png)

Where we can now add variables, lock them to encrypt it and now, if we go back to our azure-pipelines.yml file, we can reference the secret build variables by adding line 32 and 33

![](readmeImages/2018-11-09-15-15-56.png)

Ok, looks like our build has completed

![](readmeImages/2018-11-09-15-17-08.png)

And we get a nice build report that shows everything that happened during the build includeing tests. 

![](readmeImages/2018-11-09-15-17-42.png)

We can even examine the build artifact by clicking on the drop

![](readmeImages/2018-11-09-15-19-02.png)

Where you can see we created a zip file of the website

![](readmeImages/2018-11-09-15-19-29.png)

So just like that, we can create a build pipeline. But we still need to create a release pipeline to release this app. To do that, we'll just click on the Release button

![](readmeImages/2018-11-09-15-20-41.png)

Which brings up the visual editor for the release pipeline. Tailwind Traders website will be hosted in Azure App Service, so we can just select the Azure App Service Deployment Template

![](readmeImages/2018-11-09-15-22-21.png)

And now we just need to finish configuring the release. To configure a release, first we create the stage or environment. The first environment I want to deploy to is my staging environment so I'll replace the name with `Staging`

![](readmeImages/2018-11-09-15-24-17.png)

After defining your stage, next, you get to define the steps that will happen to deploy your app to that stage. So clicking on the steps link will take us to the task runner for this stage.

![](readmeImages/2018-11-09-15-25-22.png)

And since we've already selected the App Service Deployment template, there's not a whole lot of configuring left to do. We just need to chose our azure subscription and chose the app service we want to deploy to. In this case, I want to deploy to the Tailwind Front End staging app service.

![](readmeImages/2018-11-09-15-27-16.png)

Now that we have defined a stage, and defined the steps needed to deploy my app, we can choose manual approvers before and after each stage. For the Staging environment, let's just create a post depoyment approver. That way, if a new build kicks off, it will automatically deploy into my staging environment with no manual intervention

![](readmeImages/2018-11-09-15-29-23.png)

![](readmeImages/2018-11-09-15-33-43.png)

I'll just add myself as a manual approver for this demo. You can add a list of people where everyone on the list has to approve before it will pass through the manual gate. or you can create a group of people and if one person in the group approves, it will pass through the gate. Or you can use a combintion of lists and groups. So you can tighten down security as much as you need to.

Now, let's add another stage to deploy to our production environment. Hover over the environment and select clone to clone the environment.

![](readmeImages/2018-11-09-15-35-50.png)

And we will name the new stage Prod

![](readmeImages/2018-11-09-15-46-22.png)

And now we need to tweak the release steps a little bit so it deploys to the production environment.

Click on the steps in the prod environment 

![](readmeImages/2018-11-09-15-48-12.png)

And change the app service to the production app service.

![](readmeImages/2018-11-09-15-48-41.png)

Click save and voila! We just created a release pipeline that releases Tailwind Traders front end into the staging environment, and then after approvers into the production environment.

Let's see the release in action so we'll click release and click Create Release

![](readmeImages/2018-11-09-15-50-51.png)

And then we'll creat a release using the latest build by clicking Create

![](readmeImages/2018-11-09-15-51-44.png)

![](readmeImages/2018-11-09-15-52-21.png)

 Where we can now watch the release happen live. 
 ![](readmeImages/2018-11-09-15-52-52.png)

And what is happening? The release is going to the build's drop location. It's going to pick up the deployment bits from the drop location and it will deploy those bits into the staging environment based off of the steps that we configured for the staging stage.

![](readmeImages/2018-11-09-15-55-57.png)

Just like with the build the release is fully customizable where you can make it do anything. It's just a task runner, so like the build pipeline, you customize the release steps by adding and removing task runner. Out of the box, you get hundreds of tasks with many hundreds more coming from the marketplace. And you can write your own custom tasks that can make this release system do anything.

![](readmeImages/2018-11-09-15-57-01.png)

Ok, looks like the release finished release in the staging environment and it is now waiting for a post deployment approval

![](readmeImages/2018-11-09-15-58-58.png)

![](readmeImages/2018-11-09-15-59-50.png)

Before we approve it, let's check out our staging environment to see if the new code actually go deployed.

- Bring up browser with the front end in the two tabs, bring up tab 1

  ![](readmeImages/2018-11-09-16-00-54.png)

- Click refresh
     
  ![](readmeImages/2018-11-09-16-02-23.png)

And Voila! Code deployed into the staging environment!

We can now go back to Azure Pipelines, and click the post deployment approval

![](readmeImages/2018-11-10-07-07-47.png)

![](readmeImages/2018-11-10-07-08-07.png)

![](readmeImages/2018-11-10-07-08-33.png)

And now the code flows into the Prod environent

![](readmeImages/2018-11-10-07-09-12.png)

And what is it doing? Release management is going to the drop location for this specific build. The very same drop location where it picked up the bits for staging. And it will deploy the EXACT same bits it deployed into staging will now be deployed into production. There is no new build that's kicked off. There's no way stray code can slip in. Its the exact same bits.

Ok, looks like the bits have been deployed and we are now waiting for a post deployment approval.

Let's check out production

![](readmeImages/2018-11-10-07-11-24.png)

And refresh

![](readmeImages/2018-11-10-07-11-35.png)

And Bam!  New code deployed all the way to prod. So now I'll approve the post deployment approval

![](readmeImages/2018-11-10-07-12-21.png)

![](readmeImages/2018-11-10-07-12-27.png)

![](readmeImages/2018-11-10-07-12-33.png)

And now, we have built our first build and release pipeline where any checkin from github will kick off our build and release through staging all the way into production.

Cool huh?

But you know what? we can do even better. Remember Azure Pipelines is fully customizable where we can make the release system do anything. Including Advanced DevOps best practices. Things like Blue Green deployments, where we first deploy into an environment that's an exact replica of what's in production. Do our testing in it, and when we r ready, we swap production with the BlueGreen environment. So now, what was in production is in my BlueGreen spot and what was in my Blue Green spot is now in production. Something like that is easily doable using Azure Pipelines. Even something like AB Testing, where we deploy new code and route just some of the traffic, like 10% to the new code and route the rest of the traffic to the old code. This way, we can slowly and carefully gather telemetry, make sure we are delivering value. And if things look good, we can slowly bump up the traffic to 20, 30 eventually 100 percent.

In fact, using the power of Azure Pipelines and Azure App service, this is really pretty trivial to do. Check this out.

[Go back to main browser,  `Tab 2 - Tailwind Front End App Service in Portal`]

Here is the azure portal page for my app service which is hosting the Tailwind Traders front end.

![](readmeImages/2018-11-10-16-31-18.png)

To implement AB testing, scroll down to **Development Tools** and select **Testing In Production**

![](readmeImages/2018-11-10-16-38-42.png)

Here we get to add a deployment slot. Deployment slots are... think of them like a virtual directory for your web app. 

![](readmeImages/2018-11-10-16-39-37.png)

We'll create a slot and then we can direct what percentage of the traffic we want routed to what slot.

I'll name this new slot **ABTesting**

![](readmeImages/2018-11-10-16-41-09.png)

And this creates me my slot. Now, I just need to add in the percentage of traffic that I want to route to the slot. For this demo, I'll pick 50%.

![](readmeImages/2018-11-10-16-42-35.png)

And that's all I need to do on the infrastructure side. To implement AB testing in my pipeline, all I'll need to do is make some simple changes.

[Bring up browser tab with Azure Pipeline]

![](readmeImages/2018-11-12-08-09-39.png)

First, I'm going to clone my prod environment and we'll call it Prod B

![](readmeImages/2018-11-12-08-11-45.png)

![](readmeImages/2018-11-12-08-12-08.png)

Next, I'll rename my Prod stage to Prod A

![](readmeImages/2018-11-12-08-12-48.png)

And we do need to tweak the deployment step, because instead of deploying the new code to the production slot, we will deploy the new code to the ABTesting slot we just created 

![](readmeImages/2018-11-12-08-16-41.png)

And save and Bam. That's all we have to do. Now, to see this in action let's go make some code changes.

Let's turn the background of our app purple

![](readmeImages/2018-11-12-08-29-50.png)

Save it, and commit

