# GitOps and Serverless CI/CD with OpenShift

This is **not** a generic demo.  It requires quay and github credentials (as sealed secrets) to work.

If you want to try this demo on your own clusters, you should fork this repositiroy, find/replace any instances of `pittar` and make sure you add your own quay and GitHub credentials as sealed secretes to you repos :)


# Steps.

In non-prod cluster:

1. Create non-prod envs.

```
oc apply -k gitops/01-cluster-admin/argocd/non-prod/01-namespaces
```

2. Create the "apps" Argo CD instance.

```
oc apply -k gitops/01-cluster-admin/argocd/non-prod/02-operators
```

3. Create the shared CI/CD tooling:

```
oc apply -k gitops/02-developers/argocd/non-prod/01-cicd-tools
```

4. Create the Pet Clinic developer environments.

```
oc apply -k gitops/02-developers/argocd/non-prod/02-petclinic
```

5. Kick off a build.

```
oc create -f gitops/pipeline-run/build-and-rollout-pipeline-run.yaml -n petclinic-cicd
```

In prod cluster:

6. Create prod envs.

```
oc apply -k gitops/01-cluster-admin/argocd/prod/01-namespaces
```

7. Create the "apps" Argo CD instance.

```
oc apply -k gitops/01-cluster-admin/argocd/prod/02-operators
```

8. Create the Pet Clinic prod environment.

```
oc apply -k gitops/02-developers/argocd/prod/02-petclinic
```

