# tekton-triggers

- Prepare tekton trigger

```
# Temas de autorización
kubectl apply -f secret_github-ibm.yml -n tekton-trigger
kubectl apply -f serviceaccount.yaml -n tekton-trigger
kubectl apply -f triggerbinding-roles -n tekton-trigger

# Trigger setup
kubectl apply -f triggertemplate.yaml -n tekton-trigger
kubectl apply -f triggerbinding.yaml -n tekton-trigger
kubectl apply -f triggerbinding-message.yaml -n tekton-trigger
kubectl apply -f eventlistener.yaml -n tekton-trigger

# Tasks and pipeline setup
kubectl apply -f example-tasks.yaml -n tekton-trigger
kubectl apply -f example-pipeline.yaml -n tekton-trigger

```

- Port forward

```
kubectl --namespace tekton-trigger port-forward svc/el-listener 8080:8080
```

- Hit the eventlistener

```
curl -X POST http://localhost:8080 -H 'Content-Type: application/json' -H 'X-Hub-Signature:sha1=2da37dcb9404ff17b714ee7a505c384758ddeb7b' \
-d '{"head_commit": {"id": "master"	},"repository":{"url": "https://github.com/tektoncd/triggers.git"}}'
```

- Expose the listener por el ingressroute

```
kubectl apply -f ingressroute.yml -n tekton-trigger
```