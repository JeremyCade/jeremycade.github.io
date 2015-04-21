---
layout: post
title:  "Stories from Azure Migrations: The Decision to Migrate"
date:   2015-04-25
categories: azure asp.net mvc migration web app
---

Finally: The decision has been made to move away from our current "cloud" (I use the term sparingly here) hosting provider and make the move to the Azure Platform. First of all; Why Azure and not Amazon, Rackspace or any of the other world-class Cloud Hosting providers? 

In short: The Azure Web App / Platform as a Server (Paas).

The Azure Web App PaaS model uniquely fits the current development and deployment culture that exists at my current employer [AussieWeb](http://www.aussieweb.com.au). Our engineering team isn't exactly large (3 people including myself) all of whom are are multi-disciplinary, meaning each of us execute multiple engineering and business roles. The Azure Web App model provides the team with a somewhat familiar deployment model and takes away the hassle of having to deal with the underly infrastructure / virtual machines (No more late night reboots or patch Tuesday for me). 

Another major factor in making the decision towards Azure was the deployment story. Again working with a small team and limited resources, the Azure Web-App deployment story appealed to to our team. The option of either direct deployment from a git repository or web deploy made sense, and is a plug and play swap for our current deployment methods. 

The final motivating factor was the availability of multiple regions in Australia. AussieWeb has previously made use of Amazon's EC2 Infrastructure out of Singapore and Tokyo for non-critical applications, though we found the latency between Australia and the SE Asian regions to be a major issue. 