

https://cloud.google.com/architecture/jenkins-on-kubernetes-engine

# Talk
https://www.youtube.com/watch?v=IDoRWieTcMc&t=4s

# Securing jenkins
https://wiki.jenkins.io/display/JENKINS/Standard+Security+Setup


# Example jenkins pipline
https://github.com/GoogleCloudPlatform/continuous-deployment-on-kubernetes/blob/320bf7b11cc3f4f9d74fe26a173f886574e2ef2f/sample-app/Jenkinsfile#L51




- creaet container image for each build dependency
- push to registy
- user docker plugin to run jobs in containers

Keep jenkins config im source control system
Move jobs to declarative configuration using pipeline definitions
Check in pipline definitin into repo

---- to save resources
- build images for build agents
- configure agents do be deployed on demand by GKE, or kubernetes plugin

-- each team uses different set of plugins
- user repeatable mechanism for installing jenkins(ie. Helm)
- have each team define plugin list as code

# Defining agents  in jenkinsfile
- cool things
 -- define all aspects of you container
 -- may use multiple container in build
 -- dev control the environment
 Notes:
 - must be tty: true
 - default container must be jnlp, it runs agents
## Managing agent base images:
 - dockerfile in repo, once change detected,
 - new build image, rebuild image
 - push image to registry

## TO store artifacts in GCS
- intall jenkins plugin
- define step in pipline or configure in UI

When using multiple containers jenkins is able to moung workspace with each container.


## Deploying to GKE from jenkins
> kubectl apply -f manifests/

Use kubernetes CLI plugin to use kubconfig (see picture)
eg.
```
stage('deploy canary') {
    with KubeCOnfig([credentialsId:kubeconfig, contextName: canary]){
    sh 'kubectl apply -f manifests/'
}
}
```

## How to rollout code - blue - green
- blue green deployments - on k8s switch label  in service definition so different deployment will take place

## Verify deployment
```
state("build image') {
    sh 'gcloud container builds submit . -t gcr.io/a/b:$build_NUMBER
}

stage('Create new deployment'){
  sh 'kubectl apply -f deployment.yml'
}

stage('verify new deployment') {
  sh './verify-version.sh $BUID_NUMBER'
}
eg. kubectl rollout status deployment $DEPLOYMENT
kubectl get pods //all running and ready

stage(`switch service to new deployment`){
    sh 'sed -i s/build_number/$build_number/ service.yaml'
    sh 'kubectl apply -f manifests/'
}

stage('verify service'){
 sh './verify-service -f manifests/'
}
# has endpoints mapped
kubectl get endpoints $SERVICE
# ingress has hallthy backends
kubectl get ingress
# run smoke test
kubectl run --image test-image --rm test


stage("delete previous deployment'){
 sh 'kubectl delete deployment ${PREVIOUS_DEPLOYMENT}
```



## Rolling update strategy
- how many can be brought up at once
- how many pods can be down during the rollout
- supply perecentage or absolute values of pods

Readiness probe is very important - tells if app can handle traffic

Graceful termination - how many seconds,
make sure to stop accepting traffic - disable readinessProbe
