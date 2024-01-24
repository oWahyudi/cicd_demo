# CI/CD Pipeline with SonarCloud and Snyk

This repository demonstrates the experimental setup of a Continuous Integration/Continuous Deployment (CI/CD) pipeline using SonarCloud for code quality analysis and Snyk for security vulnerability scanning.

## Overview

The CI/CD pipeline in this project includes the following steps:

1. **Build**: Compile and build the project.
2. **Code Quality Analysis**: Analyze the code using SonarCloud for detecting code quality issues.
3. **Security Scanning**: Scan for security vulnerabilities using Snyk.
4. **Deployment**: Deploy the application or perform any other deployment-related tasks.

## Prerequisites

Before setting up the pipeline, make sure you have the following:

- A GitHub repository with your code.
- Accounts on [SonarCloud](https://sonarcloud.io/) and [Snyk](https://snyk.io/) with projects set up.

## Setting Up the CI/CD Pipeline

1. **Create a CI/CD Configuration File** 

2. **Configure SonarCloud Token**: Generate a SonarCloud token and add it as a secret in your GitHub repository. Replace `SONAR_TOKEN` in the CI/CD configuration with your actual SonarCloud token.

3. **Configure Snyk Token**: Generate a Snyk token and add it as a secret in your GitHub repository. Replace `SNYK_TOKEN` in the CI/CD configuration with your actual Snyk token.

4. **Configure AWS Token**: Generate a AWS access token and add it as a secret in your GitHub repository. Replace `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` in the CI/CD configuration with your actual AWS token.

5. **Commit and Push**: Commit the changes to your repository and push them to trigger the CI/CD pipeline.

6. **Monitor Results**: Visit your CI/CD platform (e.g., GitHub Actions) to monitor the progress and results of the pipeline.

## Additional Notes

- Customize the CI/CD configuration file based on your project's requirements.
- Explore additional features and options provided by SonarCloud and Snyk for more advanced analysis.
- Explore auto deployment to AWS via beanstalk-deploy scripts.
  ```yaml
      - name: Deploy to AWS Elastic Beanstalk
        uses: einaregilsson/beanstalk-deploy@v15
        with:
          aws_access_key  : <AWS_ACCESS_KEY_ID>
          aws_secret_key  : <AWS_SECRET_ACCESS_KEY>
          application_name: <Application-Name>
          environment_name: <Environment-Name>
          region          : <AWS-Region>
          version_label   : <Unique-Version-No>
          source_bundle   : <JAR-File-Path>
  
- Explore auto deployment AWS via CLI elastic Beanstalk.
  ```yaml
      - name: Deploy to AWS Elastic Beanstalk
        run: |
          aws elasticbeanstalk create-application-version \
          --application-name <Application-Name> \
          --source-bundle S3Bucket=<Bucket-Name>,S3Key=<Package-Name> \
          --version-label <Unique-Version-No> \
          --description <Description>

          aws elasticbeanstalk update-environment \
          --application-name <Application-Name> \
          --environment-name <Environment-Name> \
          --version-label <Unique-Version-No>

- Explore alternative using IAM Role for automated deployments instead of IAM User
  ```yaml
      - name: Assume IAM Role for Granting Permission
        id: assume-role
        uses: aws-actions/configure-aws-credentials@v1
        with:
          role-to-assume: arn:aws:iam::<ACCOUNT_ID>:role/<YourRoleName>
          aws-region: <Aws-Region>

      - name: Copy JAR file to S3 bucket
        run: aws s3 cp <filepath/filename> s3://<S3-Bucket-Name/filename>
        env:
          AWS_ACCESS_KEY_ID: ${{ steps.assume-role.outputs.access_key }}
          AWS_SECRET_ACCESS_KEY: ${{ steps.assume-role.outputs.secret_access_key }}
          AWS_SESSION_TOKEN: ${{ steps.assume-role.outputs.session_token }}
          
## Resources

- [SonarCloud Documentation](https://sonarcloud.io/documentation)
- [Snyk Documentation](https://support.snyk.io/)
- [Beanstalk Deploy Documentation](https://github.com/marketplace/actions/beanstalk-deploy#)
- [AWS CLI Create Appliction package version](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/create-application-version.html)
- [AWS CLI Update Environment](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/update-environment.html)
- [AWS Credentials for Github Actions](https://github.com/marketplace/actions/configure-aws-credentials-action-for-github-actions)


