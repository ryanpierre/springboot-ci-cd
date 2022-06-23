# CI/CD Pipelines

## Learning Objectives

- [ ] I can integrate a CI/CD Pipeline to my projects
- [ ] I can build and deploy a simple web app to a hosted cloud solution
- [ ] I can see changes I push to my codebase reflected in production automatically

## Intro

How does an app go from running on localhost to the internet ? Well, there are numerous ways you could do this but lets start at talking about how this was done once upon a time. From there, we can understand the need for pipelines and infrastructure-as-code, such as terraform.

Once upon a time, if I was using a language like Java with the Spring Boot Framework, I would compile my application to a production build. From there, I would buy a server (a dedicated, physical machine somewhere in the world that I could access via SSH). I would then install a bunch of dependencies on that server, namely a Web Server like Nginx or Apache, a database driver like Postgres, and the JVM runtime so that I could run my application. 

My physical machine somewhere in the world would be represented by an IP address on the internet. I'd likely configure a firewall so that that IP address is only accessible a certain way, namely via a browser using HTTP, or by SSH with my admin priveleges. Finally, I would run my application on localhost:3000 like normal, *on the server* and expose that port to the outside world with Nginx or Apache. The end result of this is that when I access my physical server's IP address from my browser, it would safely share the result of whatever is running on localhost:3000 to the rest of the internet, and I'd see my app in the browser. 

You'd probably do a few extra things too, like update a DNS record so that www.mywebsite.com points to the machines IP address. 

Let's look at a few problems with this approach:

## Updating code

With this process, I have to manually copy my codebase to my server, and repeat this process every time there is updates. Typically in the past this would mean hours of downtime for a website while we upgrade all the servers to the newest code. This could be slightly accelerated by giving the machine access to github so it could pull the code directly, but it would then need to be compiled on the server before it could be run. 

It also may be the case that I deployed some buggy code. I forgot to lint the code, or maybe I forgot to check that all the tests were passing. Well now I've deployed that code to production after a big downtime orchestration, and I need to go down again to roll back !

Enter: *Continuous Integration and Continuous Delivery Pipelines, more commonly known as CI/CD Pipelines.

## CI/CD Pipelines

When we are speaking about Continuous Integration, we are speaking about automating code deployment so we aren't manually pulling code, installing dependencies and running applications. Typically we want to integrate with an SCM system (such as GitHub) so that when changes are made to our codebase, we can run our continuous integration system to get the new code live seamlessly.

The aim of Continous Delivery is to ensure that our code is ready for deployment and has passed a sufficient amount of checks, verifications and other scripts in order to decide the update is 'done'. Typically we also integrate with an SCM system except this time we will typically block access to the CI pipeline if a sufficient amount of checks are not passed. These checks will be run when new updates to the codebase are proposed.

So what does this look like in the wild ? Let's follow the path of an app deploying:

- I finish my feature on my local machine
- I check it into git, on my local branch
- I create a pull request for these changes, requesting to merge my local branch onto the main branch
- When my pull request is created my CD pipeline triggers. It checks whether all tests are passing and that the code passes a lint check.
- Once that's verified, I can merge my pull request to the main branch
- Once the code reaches the main branch, my CI pipeline triggers. It compiles the code, updates the code on all my servers, and starts running the new code so that users can now see it

## Whats next ? 

Head on over to [today's challenge](./CHALLENGES.md) to get started

