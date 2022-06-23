# CI/CD Challenges

### Github Actions

Github actions are a form of CI/CD pipeline that allow us to create sequences of events. Those sequences of events live on their own, they aren't explicity triggered by anything in their own code, but the merely exist as tools to be executed by other events.

Events like:
- When you push code to main branch
- When you create a pull request that is attempting to push to main branch
- When code has been reverted on the main branch

Actions, like other things we've encountered so far can be public as well. If they are public, they'll be available on [Github Marketplace](https://github.com/marketplace?type=actions). We can use actions from github marketplace (or create our own) to accomplish commonly requested tasks. 

Github actions are also like small computer programs on there own in the sense that an action will be executed by a real, one time use computer somewhere in the world called a **Lamda function**. 

So, to get started, we will need to specify what type of machine will be needed to run our code in a github action. A github action might look something like this:

```
name: Java CI/CD Pipeline

on:
  push:
    branches:
      - deploy
  pull_request:
    types: [opened, synchronize, reopened, closed]
    branches:
      - deploy

jobs:
  build_and_deploy_job:
    if: github.event_name == 'push' || (github.event_name == 'pull_request' && github.event.action != 'closed')
   runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - name: Set up JDK 11
      uses: actions/setup-java@v3
      with:
        java-version: '11'
        distribution: 'temurin'
    - name: Build with Gradle
      uses: gradle/gradle-build-action@67421db6bd0bf253fb4bd25b31ebb98943c375e1
      with:
        arguments: build
```

First, we're specifying that we'd like this action to trigger on 

Then, we are saying that we need a computer using the latest version of ubuntu to perform this task. Ubuntu is a slim version of linux that is widely used on servers globally because of how lightweight it is.

We're also saying, only run this job if the event name that triggered it is a push, or a pull request action that isn't closing the pull request. 

Finally, we're saying "use the actions/checkout@v2" action from github actions marketplace as one of our steps to complete our action and installing dependencies. Each `-` in our steps section specifies another step in our action. It's not uncommon for actions to have many steps associated with them. You can also pass arguments to steps using the `with` keyword. For example:

```
 - name: Set up JDK 11
      uses: actions/setup-java@v3
      with:
        java-version: '11'
        distribution: 'temurin'
```

This action uses the `actions/setup-java` action from the marketplace, and then passes the argument `java-version: '11'` and `distribution: 'temurin'` to say: go to the marketplace and run the setup java action to install java on my linux machine using the temurin distribution from adopt open jdk.

You'll also likely need to login to a service or two while you're doing this. Since this code might (and likely will) be public, we don't want to be posting our username and password to github (ever !)

Suppose we wanted to log into AWS in our action for example

Instead, we'll use **secrets**. Secrets are referenced in our github actions like so:

```
- name: Configure AWS Credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: us-east-2
        role-to-assume: ${{ secrets.AWS_ROLE_TO_ASSUME }}
        role-external-id: ${{ secrets.AWS_ROLE_EXTERNAL_ID }}
        role-duration-seconds: 1200
        role-session-name: MySessionName
```

We would then add those secrets (AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_ROLE_TO_ASSUME, AWS_ROLE_EXTERNAL_ID) to our github account so that it could automatically read those values and fill them in at run time. 

The final thing to remember with all this: in github actions, **indentation matters**. 

This

```
- name: Ryan
  uses: something
```

is not the same thing as

```
- name: Ryan
    uses: something
```

### Your task

Your task today is to connect your source code repository to Github Actions and create a CI pipeline. 

We want to accomplish 3 things:

- We want to create a **NEW SPRINGBOOT PROJECT FROM SCRATCH**. Add a Controller called `SmileController`, an Entity called `Smile` and a repository called `SmileRepository`. The smile controller should return a `Smile` which has the fields 

  id: Long
  isCrying: Boolean
  isLaughing: Boolean

Make sure to write a few simple tests for your new simple application.
- We want to push this new codebase to github
- We want to check that all our tests are passing before we are able to merge a pull request into the main branch by creating a github action
- We want to check that there are no code style errors using [super linter](https://github.com/github/super-linter)


### Resources 

- [Github Actions](https://resources.github.com/devops/tools/automation/actions)
- [Building and testing with Github Actions](https://docs.github.com/en/github-ae@latest/actions/automating-builds-and-tests/building-and-testing-java-with-gradle)
- [Github Secrets](https://github.com/Azure/actions-workflow-samples/blob/master/assets/create-secrets-for-GitHub-workflows.md)
- [Spring Initializr for starting new projets](https://start.spring.io/)