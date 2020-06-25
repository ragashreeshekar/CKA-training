# Replication in Kubernetes

Replication is essential to ensure the following:
1. Reliability: By having multiple versions of an application, you prevent problems if one or more fails.  This is particularly true if the system replaces any containers that fail.
2. Load balancing: Having multiple versions of a container enables you to easily send traffic to different instances to prevent overloading of a single instance or node. This is something that Kubernetes does out of the box, making it extremely convenient.
3. Scaling: When load does become too much for the number of existing instances, Kubernetes enables you to easily scale up your application, adding additional instances as needed.

Replication is appropriate for numerous use cases, including:
1. Microservices-based applications: In these cases, multiple small applications provide very specific functionality.
2. Cloud native applications: Because cloud-native applications are based on the theory that any component can fail at any time, replication is a perfect environment for implementing them, as multiple instances are baked into the architecture.
3. Mobile applications: Mobile applications can often be architected so that the mobile client interacts with an isolated version of the server application.

Kubernetes has following ways in which we can implement replication.

## Replication Controllers 
A ReplicationController ensures that a specified number of pod replicas are running at any one time. In other words, a ReplicationController makes sure that a pod or a homogeneous set of pods is always up and available.

Create a manifest file with Kind **ReplicationController** & use kubectl to create the object in k8s API Server.

```
kubectl create -f rc-ex1.yml                                                   # create replication Controller

kubectl get rc                                                                 # List all replication Controllers in current active namespace
kubectl get rc -n <namespace>                                                  # List the replication controllers in <namespace>
kubectl get rc --show-labels                                                   # list the labels for rc
kubectl get rc -l rc=myapprc -o wide                                           # list replication controllers with matching labels
kubectl get pods | grep tomcatrc                                               # list the pods associated with rc

kubectl get rc tomcatrc -o yaml                                                # detailed object config 
kubectl describe rc <rcname>                                                   # inspect the replication controller
kubectl scale --replicas=x rc <rcname>                                         # Scale replication controller
kubectl rolling-update tomcatrc -f rc-ex2.yml                                  # roll update rc

kubectl apply -f rc-ex1.yml                                                    # udpate the replication Controller
kubectl label rc <rcname> key=value                                            # label the replication controller

kubectl expose rc <rcname> --port=<external> --target-port=<internal>          # expose rc as service & assign port on the cluster
kubectl expose rc <rcname> --port=<external> --type=NodePort                   # expose rc as service & assign port on the Node

kubectl delete rc <rcname>                                                     # delete rc & pod under it
```

## Replica Sets

A ReplicaSet's purpose is to maintain a stable set of replica Pods running at any given time. As such, it is often used to guarantee the availability of a specified number of identical Pods

Create a manifest file with Kind ReplicaSet & use kubectl to create the object in k8s API Server.
```
kubectl create -f rs-ex1.yml                                                    # create replica set

kubectl get rs                                                                  # List all replica sets in current active namespace
kubectl get rs -n <namespace>                                                   # List the replica sets in <namespace>
kubectl get rs --show-labels                                                    # list the labels for rs
kubectl get rs -l rs=myapprs -o wide                                            # list replica sets with matching labels
kubectl get pods | grep tomcatrs                                                # list the pods associated with rs

kubectl get rs tomcatrs -o yaml                                                 # detailed object config
kubectl describe rs <rsname>                                                    # inspect the replica set

kubectl apply -f rs-ex1.yml                                                     # update replica set
kubectl label rs <rsname> key=value                                             # label the replica set
kubectl scale --replicas=x rs <rsname>                                          # Scale up/down replica set

kubectl expose rs <rsname> --port=<external> --target-port=<internal>           # expose rs as service & assign port on the cluster
kubectl expose rs <rsname> --port=<external> --type=NodePort                    # expose rs as service & assign port on the Node

kubectl delete rs <rsname>                                                      # delete rs & pod under it
```

## Note 
Replication Controller and Replica sets offer similar service however the major difference is that we use matchLabels instead of label to enable a complex selection criteria and rolling-update command works with Replication Controllers, but won’t work with a Replica Set.

## Deployments
Deployments are an update to Replica Sets and are intended to replace Replication Controllers. They provide the same replication functions (through Replica Sets) and also the ability to rollout changes and roll them back if necessary.

```
create a manifest file with Kind ReplicaSet & use kubectl to create the object in k8s API Server.

kubectl create -f deployment-ex1.yml --record                                    # create deployment

kubectl get deploy                                                               # List all deployments in current active namespace
kubectl get deploy -n <namespace>                                                # List the deployments in <namespace>
kubectl get deploy --show-labels                                                 # list the labels for deploy
kubectl get deploy -l deploy=myapprs -o wide                                     # list deployments with matching labels
kubectl get pods | grep mydeploy                                                 # list the pods associated with deployment
kubectl get deploy mydeploy -o yaml                                              # detailed object config
kubectl describe deploy <deployment>                                             # inspect the deployment

kubectl label deploy <deployment> key=value                                      # label the deployment
kubectl scale --replicas=x deploy <deployment>                                   # Scale up/down deployment
kubectl apply -f deployment-ex1.yml  --record                                    # update the deployment 

kubectl expose deploy <deploy> --port=<external> --target-port=<internal>        # expose deployment as service & assign port on the cluster
kubectl delete deploy <deployment>                                               # delete deployment & pod under it

kubectl rollout history deploy <deployname>                                      # check the revisions of a Deployment
kubectl rollout history deploy <deployname> --revision=2                         # see the details of each revision

kubectl rollout status deploy <deployname>                                       # get status of rollout 
kubectl rollout undo deploy <deployname>                                         # rollback to the previous revision
kubectl rollout undo deploy <deployname>  --revision=2                           # rollback to a specific revision

kubectl rollout pause deploy <deployname>                                        # pause a Deployment before triggering one or more updates
kubectl rollout resume deploy <deployname>                                       # resume a Deployment 
```

## Stateful Sets
StatefulSet is the workload API object used to manage stateful applications. Manages the deployment and scaling of a set of Pods, and provides guarantees about the ordering and uniqueness of these Pods.

Like a Deployment, a StatefulSet manages Pods that are based on an identical container spec. Unlike a Deployment, a StatefulSet maintains a sticky identity for each of their Pods. These pods are created from the same spec, but are not interchangeable: each has a persistent identifier that it maintains across any rescheduling.

If you want to use storage volumes to provide persistence for your workload, you can use a StatefulSet as part of the solution. Although individual Pods in a StatefulSet are susceptible to failure, the persistent Pod identifiers make it easier to match existing volumes to the new Pods that replace any that have failed.

StatefulSets are valuable for applications that require one or more of the following.
- Stable, unique network identifiers.
- Stable, persistent storage.
- Ordered, graceful deployment and scaling.
- Ordered, automated rolling updates.

## DaemonSet
A DaemonSet ensures that all (or some) Nodes run a copy of a Pod. As nodes are added to the cluster, Pods are added to them. As nodes are removed from the cluster, those Pods are garbage collected. Deleting a DaemonSet will clean up the Pods it created.

Some typical uses of a DaemonSet are:
- running a cluster storage daemon on every node
- running a logs collection daemon on every node
- running a node monitoring daemon on every node

Create a manifest file with Kind ReplicaSet & use kubectl to create the object in k8s API Server.

```
kubectl create -f ds-ex1.yml --record                                            # create daemonset

kubectl get ds                                                                   # List all daemonsets in current active namespace
kubectl get ds -n <namespace>                                                    # List the daemonsets in <namespace>
kubectl get ds --show-labels                                                     # list the labels for daemonset
kubectl get ds -l ds=myds -o wide                                                # list daemonset with matching labels
kubectl get pods | grep myds                                                     # list the pods associated with daemonset
kubectl get ds myds -o yaml                                                      # detailed object config
kubectl describe ds <myds>                                                       # inspect the daemonset

kubectl apply -f ds-ex1.yml  --record                                            # update the daemonset 
kubectl label ds <myds> key=value                                                # label the daemonset

kubectl expose ds <myds> --port=<external> --target-port=<internal>              # expose rc as service & assign port on the cluster

kubectl delete ds <myds>                                                         # delete daemonset & pod under it
```

## Jobs
A Job creates one or more Pods and ensures that a specified number of them successfully terminate. As pods successfully complete, the Job tracks the successful completions. When a specified number of successful completions is reached, the task (ie, Job) is complete. Deleting a Job will clean up the Pods it created.

A simple case is to create one Job object in order to reliably run one Pod to completion. The Job object will start a new Pod if the first Pod fails or is deleted (for example due to a node hardware failure or a node reboot). You can also use a Job to run multiple Pods in parallel

Create a manifest file with Kind ReplicaSet & use kubectl to create the object in k8s API Server.

```
kubectl create -f job-ex1.yml --record                                            # create job
kubectl apply -f job-ex1.yml  --record                                            # update the job 

kubectl get jobs                                                                  # List all jobs in current active namespace
kubectl get jobs -n <namespace>                                                   # List the jobs in <namespace>
kubectl get jobs --show-labels                                                    # list the labels for job
kubectl get jobs -o wide                                                          # list job with wider output
kubectl get pods | grep myjob                                                     # list the pods associated with jobs
kubectl get jobs myjob -o yaml                                                    # detailed object config

kubectl describe job <myjob>                                                      # inspect the job
kubectl label job <myjob> key=value                                               # label the job
kubectl delete job <myjob>                                                        # delete job & pod under it
```

## Cron Jobs
A CronJob creates Jobs on a repeating schedule. One CronJob object is like one line of a crontab (cron table) file. It runs a job periodically on a given schedule, written in Cron format.Create a manifest file with Kind ReplicaSet & use kubectl to create the object in k8s API Server.

```
kubectl create -f cronjob-ex1.yml --record                                         # create cronjob
kubectl apply -f cronjob-ex1.yml  --record                                         # update the cronjob 

kubectl get cj                                                                     # List all cronjob in current active namespace
kubectl get cj -n <namespace>                                                      # List the cronjob in <namespace>
kubectl get cj --show-labels                                                       # list the labels for cronjob
kubectl get cj -o wide                                                             # list cronjob with wider output
kubectl get cj | grep mycj                                                         # list the pods associated with cronjob
kubectl get cj mycj -o yaml                                                        # detailed object config

kubectl describe cj <mycj>                                                         # inspect the cronjob
kubectl label cj <mycj> key=value                                                  # label the cronjob
kubectl delete cj <mycj>                                                           # delete cronjob & pod under it
```
