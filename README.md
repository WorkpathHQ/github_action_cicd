# github_action_cicd
This repository contains the GitHub Actions workflows and jobs for the Workpath CI/CD pipeline. 

## CI
WIP

## CD

### Build, tag, push image to AWS ECR (reusable workflow)
| Environment variable | Description |   
|---|---|
| AWS_DEFAULT_REGION  | The default region used by AWS | 
| ECR_REGISTRY | The ECR Registry this image should be pushed to | 
| ECR_REPOSITORY | The ECR Repository this image should be pushed to | 
| IMAGE_TAG | The Tag for the image: <br /> For QA Environments the tag is the commit hash <br /> For Staging the tag is 'latest' (changes when projects are versioned) | 


| Secret | Description |  
|---|---|
| AWS_ACCESS_KEY_ID | The access key id for AWS |  
| AWS_SECRET_ACCESS_KEY | The secret access key for AWS| 

#### Summary: 
1. Needs to be called by an actual workflow

2. Set all environmental variables and secrets

3. Checks out code using actions/checkout

4. Login to ECR

    - using aws-actions/configure-aws-credentials

    - using aws-actions/amazon-ecr-login

5. Build tag and push image to AWS ECR

### Deployment  (reusable workflow)

| Environment variable | Description |
|---|---|
| AWS_DEFAULT_REGION | The default region used by AWS |
| AWS_ACCESS_KEY_ID_S3 | Access Key ID for S3 |
| EKS_CLUSTER | EKS Cluster to deploy to |
| ECR_REGISTRY | ECR Registry to pull the image from |
| PROJECT_NAME | Name of the project, used to determine if this is a web or api deploy |
| GIT_SHA | GitHub long hash of the commit, used to set the namespace |
| GIT_BRANCH | GitHub branch of the commit, used to set the namespace and set label for deletion |
| WP_API_VERSION | GitHub long hash of the API commit this deployment should be matched |
| WP_WEB_VERSION | GitHub long hash of the Web commit this deployment should be matched |
| WP_ENVIRONMENT | The environment to be deployed to (qa, staging, production)  |
| WP_DOMAIN | The domain used for the deployments: <br />qa -> workpath.dev <br />staging -> wrkpth.dev <br />production -> workpath.com
| WP_K8S_VERSION | K8s Version used to deploy |

| Secrets |  |
|---|---|
| AWS_ACCESS_KEY_ID | The access key id for AWS |
| AWS_SECRET_ACCESS_KEY | The secret access key for AWS |
| AWS_SECRET_ACCESS_KEY_S3 | The secret access key for AWS S3 |
| RAILS_MASTER_KEY | The Master Key for Rails |
| SIDEKIQ_CACHE_PASSWORD_PASSWORD | The Password for Sidekiq Redis |
| FRAGMENT_CACHE_PASSWORD | The Password for Fragment Cache Redis |
| FLIPPER_CACHE_PASSWORD | The Password for Flipper Cache Redis |
| DATABASE_PASSWORD | The Password for the database |
| GIT_ACCESS_TOKEN | The Token for Github Access |

#### Summary: 
1. Needs to be called by an actual workflow

2. Set all environmental variables and secrets

3. Set EKS context

    - using aws-actions/configure-aws-credentials

4. Clone workpath_k8s repository 

5. Applying manifests with given env
