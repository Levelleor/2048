A testing project for purposes of learning GitOps workflow. Any commit to this repository/every day triggers a pipeline of building a docker container off the code provided in the ./app folder and publishing it to a AWS ASG stack via AWS CDK.

The project consists of several parts:
1) AWS CDK stack
2) GitHub Actions ci
3) 2048 app that is being compressed and deployed via a Docker container

1. AWS CDK Stack is in its own separate repository. 
https://github.com/Levelleor/awscdk-test

This particular project only uses one stack provided in the repository "GitOps-ecs" or under file "awscdk-stack-2.ts".

CDK deploys resources to AWS ending up with a fully working load balanced fargate service. The app is loaded via Docker container hosted at DockerHub.

2. GitHub actions triggers every day at midnight or at every push to the repo and redeploys the stack. 
https://github.com/Levelleor/2048/blob/master/.github/workflows/github-actions.yml

First javascript of the app is compressed and obfuscated, then deployed to a container on DockerHub. AWS/CDK cli installed on the GitHub Actions runner and the CDK repo is cloned, then the deploy starts, deploying the new container image to the AWS stack. 

3. The app is a simple static website. It is hosted via nginx container. Any change to the app will trigger GitHub Actions build and redeploy it immediately to the AWS stack. 
