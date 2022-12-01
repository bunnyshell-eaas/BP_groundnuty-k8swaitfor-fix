# Fix for groundnuty permission issue
Commands to run on each deployment containing k8s nuty: 
```
...
runnerImage: 'alpine/k8s:1.22.15'
deploy:
  - 'kustomize create --autodetect --recursive '
  # role -> metadata.namespace 
  # rolebinding -> subjects[0].namespace 
  - 'sed -i "s/ENV.UNIQUE/{{ env.unique }}/g" base/kustomization.yaml'
  - 'sed -i "s/BNS_NAMESPACE_PLACEHOLDER/{{ env.k8s.namespace }}/g base/role.yaml"'
  - 'sed -i "s/BNS_NAMESPACE_PLACEHOLDER/{{ env.k8s.namespace }}/g base/role-binding.yaml"'

  - 'kubectl apply -k .'
destroy:
  - 'kustomize create --autodetect --recursive'
  - 'kubectl delete -k .'
start:
  - ""
stop:
  - ""   
```
