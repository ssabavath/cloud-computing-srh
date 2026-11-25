# Session 4: the generator on GKE

## Pods at 3 replicas (kubectl get pods -o wide)

```
NAME                         READY   STATUS    RESTARTS   AGE     IP           NODE
generator-85dcb4bffc-lwlgr   1/1     Running   0          3s      10.16.0.19   gke-freight-main-0c84bba9-3kdw
generator-85dcb4bffc-lxp9t   1/1     Running   0          3s      10.16.0.18   gke-freight-main-0c84bba9-3kdw
generator-85dcb4bffc-pmhtv   1/1     Running   0          2m56s   10.16.0.15   gke-freight-main-0c84bba9-3kdw
```

## Rollout history (kubectl rollout history deployment/generator)

```
REVISION  CHANGE-CAUSE
1         <none>
2         <none>
```

## What happened when I deleted the pod

I deleted the pod generator-85dcb4bffc-f67z7 with kubectl delete pod. Within about 1 second a new pod, generator-85dcb4bffc-pmhtv, was Running. I never told >
