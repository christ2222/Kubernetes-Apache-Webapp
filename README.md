Here’s the updated `README.md` file that starts with creating a cluster named `playground-cluster` in the `us-west-2` region and instructs how to use the LoadBalancer DNS to check the website.

---

# Apache Web Application Deployment on RHEL9 in Kubernetes

This guide provides step-by-step instructions for deploying an Apache web application on a RHEL9-based Kubernetes cluster. The setup includes:

- **Cluster Name:** `playground-cluster`
- **Deployment Name:** `apache-deployment`
- **Number of Pods:** 2
- **Service Type:** LoadBalancer
- **Exposed Port:** 80
- **Region:** `us-west-2`

## Prerequisites

- `eksctl` installed on your local machine.
- `kubectl` configured to interact with your Kubernetes cluster.
- A Docker image for the Apache web server, e.g., `christ2222/rhel9_apache_image:v1`.

## Step 1: Create the Kubernetes Cluster

First, create a Kubernetes cluster named `playground-cluster` in the `us-west-2` region.

Run the following command:

```bash
eksctl create cluster --name playground-cluster --region us-west-2 --nodes 3
```

This command will create a Kubernetes cluster with 3 nodes in the specified region.

## Step 2: Create the Deployment

The deployment manages 2 pods running the Apache web server. The deployment configuration is defined in the `deployment_manifest.yml` file.

### Apply the Deployment

To create the deployment in your Kubernetes cluster, run the following command:

```bash
kubectl apply -f deployment_manifest.yml
```

## Step 3: Create the Service

To expose your Apache web application, you need a Kubernetes Service of type `LoadBalancer`. This is defined in the `service-lb-manifest.yml` file.

### Apply the Service

To create the service in your Kubernetes cluster, run the following command:

```bash
kubectl apply -f service-lb-manifest.yml
```

## Step 4: Verify the Deployment and Service

### Check Deployment Status

Ensure that the deployment is running correctly with 2 pods:

```bash
kubectl get deployments
```

You should see something like this:

```
NAME                READY   UP-TO-DATE   AVAILABLE   AGE
apache-deployment   2/2     2            2           XXm
```

### Check Pods Status

Verify that the pods are up and running:

```bash
kubectl get pods -l app=apache
```

You should see both pods listed:

```
NAME                                 READY   STATUS    RESTARTS   AGE
apache-deployment-XXXXXX-XXXXX       1/1     Running   0          XXm
apache-deployment-XXXXXX-XXXXX       1/1     Running   0          XXm
```

### Get LoadBalancer DNS

To access the Apache web application, you need the DNS name assigned by the LoadBalancer:

```bash
kubectl get service apache-service
```

You should see output similar to this:

```
NAME             TYPE           CLUSTER-IP      EXTERNAL-IP       PORT(S)        AGE
apache-service   LoadBalancer   10.XX.XX.XX     abcdef123456.us-west-2.elb.amazonaws.com   80:XXXXX/TCP   XXm
```

### Access the Apache Web App

Open a web browser and navigate to the LoadBalancer DNS name obtained from the previous command:

```
http://abcdef123456.us-west-2.elb.amazonaws.com
```

You should see the Apache default web page or your custom web application if it's been configured.

## Step 5: Cleanup (Optional)

If you want to remove the deployment, service, and cluster, use the following commands:

```bash
kubectl delete -f deployment_manifest.yml
kubectl delete -f service-lb-manifest.yml
eksctl delete cluster --name playground-cluster --region us-west-2
```

## Conclusion

this is how you successfully deployed an Apache web application on a RHEL9-based Kubernetes cluster. The application is running in 2 pods managed by a deployment and is exposed to external traffic through a LoadBalancer service on port 80. You accessed the web application using the LoadBalancer DNS.

---

### File References:
- **Deployment Manifest:** `deployment_manifest.yml`
- **Service Manifest:** `service-lb-manifest.yml`

This `README.md` now includes steps for creating the Kubernetes cluster and using the LoadBalancer DNS to access the Apache web app.
