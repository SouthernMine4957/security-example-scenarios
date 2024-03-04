# Example / Demo / Testing Scenarios for Sysdig Secure

These are the Kubernetes manifests and scripts from the Sysdig EKS Workshop - https://github.com/jasonumiker-sysdig/sysdig-aws-workshop-instructions

I provide them here so that you can run through that workshop on your own cluster/environment (independent of Sysdig).

## Prerequisites

You need to have:
* An AWS EKS Cluster
* A machine that is able to run kubectl commands against it (i.e. you've already run the `aws eks update-kubeconfig` with the right AWS IAM available to do so etc.)
* A Sysdig Secure subscription

## Configuring Sysdig Secure's Runtime Policies

The Workshop assumes that you have your Runtime Policies configured in the following way:

### AWS CloudTrail
You will create two custom policies (click the Add Policy button in the upper right and choose AWS CloudTrail):
* SSM Session Manager Connect
    * Scope it to Entire Infrastructure
    * Include the rule SSM Start Session (Import it from Library)
* Make Bucket Public
    * Scope it to Entire Infrastructure
    * Include the rules (Import them from Library):
        * Delete Bucket Public Access Block
        * Put Bucket ACL for AllUsers
        * Put Bucket Policy

### Workload
Enable the following Managed Policies:
* Sysdig Runtime Threat Detection
* Sysdig Runtime Threat Intelligence
* Sysdig Runtime Notable Events

### Kubernetes Audit
Enable the Managed Policy Sysdig K8s Notable Events

### Container Drift 
You will create four custom policies (click the Add Policy button in the upper right and choose Container Drift):
* Detect Container Drift (security-playground)
    * Scope it to kubernetes.namespace.name = security-playground
    * Ensure Actions -> Prevent is off
* Detect Container Drift (security-playground-restricted)
    * Scope it to kubernetes.namespace.name = security-playground-restricted
    * Ensure Actions -> Prevent is off
* Detect Container Drift (security-playground-restricted-nomalware)
    * Scope it to kubernetes.namespace.name = security-playground-nomalware
    * Ensure Actions -> Prevent is off
* Prevent Container Drift (security-playground-restricted-nodrift)
    * Scope it to kubernetes.namespace.name = security-playground-restricted-nodrift
    * Ensure Actions -> Prevent is **on**

### Malware
You will create two custom policies (click the Add Policy button in the upper right and choose Malware):
* Detect Malware
    * Scope it to Entire Infrastructure
    * Ensure Actions -> Prevent is off
* Prevent Malware (security-playground-restricted-nomalware)
    * Scope it to kubernetes.namespace.name = security-playground-restricted-nomalware
    * Ensure Actions -> Prevent is **on**

## Deploy the Kubernetes Manifests
Ensure your kubectl is working for the intended cluster and you are in the example-scenarios folder then run the following command `kubectl apply -k k8s-manifests`

## Do the workshop!
The scripts that you would expect to be on the Jumpbox are all in the scripts folder.

Everything should now work the same as if it was a lab environment provided by Sysdig.