# Coworking Space Service Extension

The Coworking Space Service is a set of APIs that enables users to request one-time tokens and administrators to authorize access to a coworking space. This service follows a microservice pattern and the APIs are split into distinct services that can be deployed and managed independently of one another.

## Getting Started

### Dependencies

#### Local Environment

1. Python Environment - run Python 3.6+ applications and install Python dependencies via `pip`
2. Docker CLI - build and run Docker images locally
3. `kubectl` - run commands against a Kubernetes cluster
4. `helm` - apply Helm Charts to a Kubernetes cluster

### Setup

#### 1. Configure a Database

Set up a Postgres database using a Helm Chart.

1. Set up Bitnami Repo

```bash
helm repo add <REPO_NAME> https://charts.bitnami.com/bitnami
``` 

2. Install PostgreSQL Helm Chart

```
helm install <SERVICE_NAME> <REPO_NAME>/postgresql
```

### 2. Running the Analytics Application Locally
helm repo
In the `analytics/` directory:

1. Install dependencies

```bash
pip install -r requirements.txt
```

2. Run the application

Set the environment variables by prepending them.

```bash
DB_USERNAME=username_here DB_PASSWORD=password_here python app.py
```

#### 3. Making updates to builds

After making changes to your application, You neeed to build the docker file and push it to your ECR registry. THis is achieved through the buildspec file in the root of this directory

To do this;

1. setup a codebuild build project in AWS
2. Attach your github repository.
3. Start a new build everytime changes are made to your github repository

Note: Each time a build runs, an image will be created in ECR and tagged with the Codebuild number.

#### 4. Deploying new changes

Inside the analytics directory where your application lies. Run the command

```bash
kubectl apply -f /deployment
```

### Instance Type Recommendation

- For balanced workloads with average compute, memory, and network needs, consider AWS `t3` or `t4g` instances. This is because the api that we are running is not compute intensice so average memory CPU and networking resources can be sufficient enough for this deployment.

### Saving costs:

1. Implement Autoscaling:

Utilize Kubernetes Horizontal Pod Autoscaling (HPA) to automatically adjust the number of replicas based on resource metrics. This helps scale resources up during peak demand and down during low demand, optimizing costs.

2. Rightsize Resources:

Regularly review and adjust resource allocations for your Kubernetes pods based on actual usage. Avoid overprovisioning, and set resource limits and requests appropriately.

3. Alerts on Costly Resources:

Set up alerts based on cost thresholds to be notified when spending exceeds predefined limits. This allows for proactive cost management.

Remember to monitor your application's resource usage and adjust your instance types based on evolving requirements.
