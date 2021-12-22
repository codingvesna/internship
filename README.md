# Triggering Multiple Jenkins Jobs

## job_deploy_and_trigger

**Goal:** build and deploy, trigger cleanup job

### build and deploy
- freestyle job
- pull the code from a GitHub repo https://github.com/codingvesna/internship/tree/multiple-jobs or https://github.com/codingvesna/devops-project/tree/java-web-app/java-web-app
- Invoke top-level Maven targets - clean test package..
- Docker build and publish - create a repo name for DockerHub registy, this is where the image will be published 
- Docker compose Build step - define the docker compose file

### trigger cleanup job

Once the deployment process is over, this job will also trigger another project `job_cleanup` to build.

- In `Post-build Actions` choose the `Trigger parameterized build on other projects` options. 
- In `Projects to build	` type `job_cleanup` as this is the name of the job that will cleanup the workspace after deployment.
- Trigger when build is `stable` 
- Add `Predefined parameters` in this case I used `Name=triggered'

## job_cleanup

**Goal:** clean up the workspace 

This project is parameterized. The purpose of this job is to cleanup the workspace after the deployment process. 

**A build parameter allows us to pass data into our Jenkins jobs. ** *reminder*

- Add parameters, Name: `Name`, default value:`triggered'
- In `Advanced` check `Use custom workspace` and input directory - ** multiple jobs are now using the same workspace so the cleanup is possible in this particular way**
- In `Post-build Actions` choose `Delete workspace when build is done`

Workflow: 
	- deploy (**job_deploy_and_trigger**) upstream job
	- trigger cleanup job 
	- clean workspace(**job_cleanup**) downstream job
	
### Notes

An upstream job is a configured project that triggers a project as part of its execution. 

A downstream job is a configured project that is triggered as part of a execution of pipeline.