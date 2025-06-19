🚀 AWS CI/CD Pipeline for Python Application
This project demonstrates how to set up a Continuous Integration pipeline using AWS CodePipeline, CodeBuild, and GitHub for a Python application.

🧰 Prerequisites
AWS Account

GitHub Account

A simple Python application

📁 Step 1: Set Up GitHub Repository
Go to GitHub and log in.

Click the "+" icon in the top-right corner → New repository.

Name your repository and (optionally) add a description.

Choose public or private visibility.

Initialize with a README.md.

Click Create repository.

⚙️ Step 2: Create an AWS CodePipeline
Open AWS Management Console → Navigate to AWS CodePipeline.

Click Create pipeline.

Enter a pipeline name → Click Next.

Source stage:

Select GitHub as the provider.

Connect your GitHub account and select the repo and branch.

Build stage:

Choose AWS CodeBuild.

Click Create project.

Configure build settings (runtime, build commands, artifacts).

Optionally configure Deploy stage using:

AWS Elastic Beanstalk

Amazon ECS

S3 or other services

Review and click Create pipeline.

🏗️ Step 3: Configure AWS CodeBuild
Open AWS CodeBuild in the console.

Click Create build project.

Name the build project.

Select AWS CodePipeline as the source provider.

Configure environment settings:

OS, runtime, and compute requirements.

Define buildspec.yml commands:

yaml
Copy
Edit
version: 0.2
phases:
  install:
    runtime-versions:
      python: 3.11
    commands:
      - pip install -r requirements.txt
  build:
    commands:
      - python -m unittest discover tests
artifacts:
  files:
    - '**/*'
Save the project.

🔁 Step 4: Trigger the CI Pipeline
Push a new commit to your GitHub repo.

Go to AWS CodePipeline and open your pipeline.

You’ll see the pipeline automatically detect the change and start:

Source → Build → Deploy

Watch the logs to verify that everything works as expected!

✅ Outcome
With this setup, any code changes pushed to GitHub automatically trigger the pipeline, build and test your Python application using AWS CodeBuild, and optionally deploy the updated application—all without manual intervention.
🛠️ AWS CodePipeline + CodeDeploy Debugging Guide
This document summarizes the key issues encountered and how they were resolved while integrating AWS CodePipeline with AWS CodeDeploy for a Docker-based EC2 deployment.

✅ Common Errors & Fixes
❌ 1. Not Authorized to Perform: codedeploy:CreateDeployment
Error:

vbnet
User is not authorized to perform: codedeploy:CreateDeployment on resource...
Fix:

Attach the following policy to the CodePipeline service role:

json
{
  "Effect": "Allow",
  "Action": [
    "codedeploy:CreateDeployment"
  ],
  "Resource": "arn:aws:codedeploy:us-east-1:<account-id>:deploymentgroup:sample-python-app/sample-python-app"
}
❌ 2. Not Authorized: codedeploy:GetDeploymentConfig
Error:

vbnet
User is not authorized to perform: codedeploy:GetDeploymentConfig on resource...
Fix:
Add this to the pipeline IAM role:

json
{
  "Effect": "Allow",
  "Action": [
    "codedeploy:GetDeploymentConfig"
  ],
  "Resource": "arn:aws:codedeploy:us-east-1:<account-id>:deploymentconfig:CodeDeployDefault.AllAtOnce"
}
❌ 3. Not Authorized: codedeploy:RegisterApplicationRevision
Error:

vbnet
User is not authorized to perform: codedeploy:RegisterApplicationRevision...
Fix:

json
{
  "Effect": "Allow",
  "Action": [
    "codedeploy:RegisterApplicationRevision"
  ],
  "Resource": "arn:aws:codedeploy:us-east-1:<account-id>:application:sample-python-app"
}
❌ 4. Not Authorized: codedeploy:GetApplicationRevision
Error:

vbnet
User is not authorized to perform: codedeploy:GetApplicationRevision...
Fix:

json
{
  "Effect": "Allow",
  "Action": [
    "codedeploy:GetApplicationRevision"
  ],
  "Resource": "arn:aws:codedeploy:us-east-1:<account-id>:application:sample-python-app"
}
✅ Combined Policy for CodePipeline IAM Role
Replace <account-id> with your actual AWS account ID.

json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "codedeploy:CreateDeployment",
        "codedeploy:GetDeployment",
        "codedeploy:GetDeploymentConfig",
        "codedeploy:RegisterApplicationRevision",
        "codedeploy:GetApplicationRevision"
      ],
      "Resource": [
        "arn:aws:codedeploy:us-east-1:<account-id>:deploymentgroup:sample-python-app/sample-python-app",
        "arn:aws:codedeploy:us-east-1:<account-id>:deploymentconfig:CodeDeployDefault.AllAtOnce",
        "arn:aws:codedeploy:us-east-1:<account-id>:application:sample-python-app"
      ]
    }
  ]
}
🧠 EC2 Instance Issues & Fixes
❌ Deployment fails with:
nginx
Too many individual instances failed deployment...
Fix:

Ensure EC2 instances are running.

Check that CodeDeploy agent is installed and running:

bash
sudo service codedeploy-agent status
Restart agent if needed:

bash
sudo service codedeploy-agent restart
Make sure scripts (like start_container.sh) do not fail (e.g., Docker installed, correct permissions):

bash
chmod +x scripts/*.sh
Check script logs in:

bash
/opt/codedeploy-agent/deployment-root/<deployment-id>/d-*/logs/scripts.log
🛡 IAM Role vs Attached Policy Note
If using inline policies, they appear under the role’s Permissions tab, not under “Attached entities”.

You may use either inline or managed policies, but verify that they’re applied to the exact role used by your pipeline.

💬 Additional Tips
Wait a few minutes after updating IAM policies for changes to propagate.

Use CloudTrail to trace exact permission denials.

Use CodeDeployDefault.OneAtATime or HalfAtATime during testing to reduce blast radius.

🔗 GitHub Repository
https://github.com/DeepakrajRavi/aws-cicd

🔖 Suggested LinkedIn Caption
🚀 Just wrapped up building a CI pipeline on AWS using CodePipeline + CodeBuild integrated with GitHub!
Automated the build and test cycle for a Python application—no more manual deployments!

🧠 Learned a lot about AWS DevOps tools
💡 Real-world CI/CD in action

#AWS #DevOps #CodePipeline #CI #Python #CloudComputing #ContinuousIntegration #CodeBuild #GitHub #ProjectShowcase
