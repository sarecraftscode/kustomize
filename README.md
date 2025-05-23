### Installation

```bash
 Kustomize build k8s/ | kubectl apply -f -
```

### Syntax

or

```bash
kubectl apply -k k8s/
```

### Print all environnments variables used by a pod

```bash
kubectl exec nginx-deployment-tyu88U -- printenv | grep -i db
```

## Transformers in Kustomization file

commonLabel
namePrefix/Suffix
Namespace
commonAnnotations

## Image Transformer in Kustomization

## Patches

Another method to modify k8s configs
Taget one or more specific sections in k8s resource

3 parameters : Operation, Target, Value

###

- Json 6902 Patch
- Strategic merge patch

### Differents types of patch

- inline
- separate file

### overlays

- base config
- environment specific configurations that add or modify base configs

## Components

Provide ability to define reusable pieces of config (resources + patches) that can be included in multiple overlays

Components are useful in situations where applications support multiple optional features

## Generators

When we change value in configMap, pods declare in deployment does not pick up the new value.

We need to rollout

```bash
kubectl rollout restart deployment <deploy-name>
```

- configMap
- secret

To address the previous issue, we neeed a generator.

- Generator add new set of character with radom suffix to a name, and this resulting in a new deployment , because deployment config file is updated.

- For this, use configMapGenerator and secretGenerator

### garbage collection

Anytime we made a change in configMapGenerator, we create a new configMap.
Add labels to the configMapGenerator, and use this label to garbage collect the unsed configMap

```bash
kubectl apply -k k8s/ --prune -l app-config=my-config
```

### Imperative commands

```bash
kustomize edit --help
```

#### change image

Those commands change the Kustomisation file

```bash
kustomize edit set image nginx=nginx:1.2.2 vs for new tag only kustomize edit set image nginx:1.2.2

kustomize edit set namespace staging
kustomize edit set label org:kodeKloud env:staging
kustomize edit set replicas nginx-deployment=5
kustomize edit add configmap db-creds --from-literal=password=password1 --from-literal=username=root

kustomize edit add resource db/db-deply.yaml

kustomize edit add secret my-secret --from-literal=username=root --from-literal=password=rootpassword123
```

### Edit commands in CI/CD Use-Case

After new image is push on registry, we can use edit command like that:

```bash
kustomize edit set image api=api:$GITHUB_SA
```

### Components

```yaml
components:
  - ../../auth/
```

### Snippets

```yaml
- name: POSTGRES_PASSWORD
  valueFrom:
    secretKeyRef:
      name: db-secret
      key: password
```
