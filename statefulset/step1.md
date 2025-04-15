There are two Pods named statedb-* in Namespace *project-state*. The Project State management asked you to scale these down to one replica to save resources.

# Etape 1 : Verification pods 
On verifie que les pods tournent dans l'espace de nom.
```
k -n project-state get pods 
``` 
On va decrire les pods pour voir comment ils sont deployer : 
```
k -n project-state describe pods nom-pods
```
On fait ensuite attention a statefulset. 
On peut aussi lister les deploiement, daemonset et statefulset du namespace pour reperer par quoi les pods sont deploye: 
```
k get deploy,ds,sts
```
# Etape 2 : Scale Down
On va scale down le statefulset 
```
k -n project-state scale sts nom-statefulset --replicas 1
```
