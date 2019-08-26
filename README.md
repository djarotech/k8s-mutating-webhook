# k8s-mutating-webhook

## Status

Current version of the code is at :
    https://github.com/cloud-ark/kubeplus/tree/master/mutating-webhook
    
I started the design from scratch, taking some boilerplate from https://github.com/morvencao/kube-mutating-webhook-tutorial

## Description
Works kind of like helm, except at runtime it modifies and replaces Fn::ImportValue stuff from annotations of other Custom resource objects. There are two DFS functions that were necessary for generalizing this for any custom resource definition. Go only uses structured json unmarshalling, so i used jsonparser library I found on github, which is space/time efficient . 

This is similar to AWS CloudFormation, it is aiming to give operators better interopability by defining some useful functions

It intercepts the kubernetes api request, modifies the json object with values that combine the operators together, and then kubernetes receives the request and saves to its etcd store. 

## Steps
1. Generate certs
    `make gen-certs`
    
2. Deploy Mutating Webhook Using Docker
    `dep ensure`
    Set environment variables
    ${PROJECT_ID} -> gcr project id
    ${IMAGE_NAME} -> crd-hook
    `make docker`
    `make deploy`
3. Install Operators
    `make install-operators`
4. Deploy application
    `make cluster`
    `make moodle`
5. Clean up
delete webhook: `make delete`
delete instances: `make delall`
