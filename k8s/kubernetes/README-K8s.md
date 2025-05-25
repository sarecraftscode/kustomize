We use kubectl run command with --dry-run=client -o yaml option to create a manifest file :

```bash
kubectl run redis --image=redis123 --dry-run=client -o yaml > redis-definition.yaml
```

Use the kubectl edit command to update the image of the pod to redis.

```bash
kubectl edit pod redis
```

Explain the fields and structure of various resources

```bash
Kubectl explain pods

kubectl explain deployment | head -n3
```

### Rollout and revision

To go back to previous revision.
For each depployment, new rollout is trigged and version is created

```bash
Kubectl rollout status deployment/<my_deployment>
# to view revision
kubectl rollout history deployment/<my_deployment>
```

There are two types of reployment strategy

- Recreate : destroy all pods and create new one => Application down
- RollingUpdate: destroy only part of old pods and create new pods and replace one by one to avoid application to be unaivailble

- To rollback to previous revision

```bash
Kubectl rollout undo deployment/<deployment_name>
```

```bash
Kubectl set image deployment myapp-deployment nginx=nginx:<nginx_version>
```

# Services

Get the node ip address on minikube

```bash
minikube service spring-boot-service --url
```
