```
  kubectl run --generator=run-pod/v1 nginx-pod --image=nginx:alpine.                 # To create a pod
  kubectl get pods                                                                   # get the list of pods
  kubectl run --generator=run-pod/v1 redis --image=redis:alpine -l tier=db           # create a Pod with label
  
  kubectl expose pod redis --port=6379 --name redis-service                          # create a service 
  kubectl get svc                                                                    # get the list of services
  kubectl create deployment webapp --image=kodekloud/webapp-color                    # Create a New Deployment
  kubectl scale deployment/webapp --replicas=3                                       # Scaling the replicas
```