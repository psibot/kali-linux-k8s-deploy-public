# kali-linux-in-k8s

Kali Deploy for Kubernetes using Rancher + Longhorn 

Easy To Deploy

### !!! YOU NEED LONGHORN FOR THIS TO WORK !!!

This will create a namespace called cybersec with PVC's so you can not loose the install 

Change the resources as needed in kali-small.yaml based on your cluster sizes ect.

CPU's :1 
Memory : 4 Gi 

SHARED STORAGE 250 Gi

FOLLOW THE STEPS!!!: 

To Deploy run the kali-small.yaml 

```
kubectl create  -f kali-small.yaml
```

When Deployed login to pod and run , !!! THIS WILL TAKE A WHILE BUT YOU ONLY HAVE TO DO IT ONCE !!! 

I am working  on A HELM CHART , Watch this space !!!

1. StatefulSet:
Purpose: StatefulSet is used to manage stateful applications, where each pod needs to maintain a unique identity and stable storage. It's useful for applications that require persistent storage, like databases (e.g., MySQL, Cassandra).

Pod Identity: Pods in a StatefulSet are created with a stable, unique identity (e.g., pod-0, pod-1, etc.). Even after scaling down and restarting, the pod retains its unique identity.
Stable Storage: Each pod can be associated with a persistent volume, and this association is maintained even when pods are restarted or rescheduled.

Pod Ordering: Pods are created and deleted in a sequential order (e.g., pod-0 is created first, then pod-1). This is important for applications that rely on specific ordering, like distributed databases or clustered services.
Use Cases: Databases (MySQL, PostgreSQL, Cassandra), Zookeeper, Kafka, or any other service that requires persistence, stable network identity, or ordered startup/shutdown.

## To Kill 

```
kubectl delete -f kali-small.yaml
```

## Check Deployment

```
 kubectl describe deploy kali-deployment -n cybersec
```

## Check Logs 

```
kubectl logs -f deployment/kali-deployment -n cybersec
```

## Issues  Terminate All Pods and KILL NAMESPACE 

```
kubectl delete pods <pod> --grace-period=0 --force -n cybersec
```

Kill Namespace 

```
kubectl get namespace "cybersec" -o json \
  | tr -d "\n" | sed "s/\"finalizers\": \[[^]]\+\]/\"finalizers\": []/" \
  | kubectl replace --raw /api/v1/namespaces/cybersec/finalize -f -
  
  ```


