Votre collègue vous a informé que le nœud cka3962-node1 exécute une ancienne version de Kubernetes et ne fait même pas encore partie du cluster.

    1. Mettre à jour le Kubernetes du nœud avec la version exacte du plan de contrôle

    2. Ajouter le nœud au cluster en utilisant kubeadm

# Etape 1 : Mettre a jour le kubernetes du noeud avec la version exacte 

Sur le controlplane. On verifie les noeuds present dans le cluster : 
```
k get node
```
On verifie la colonne version 

On se connecte sur le second noeud et on verifie les versions de kubectl, kubelet, kubeadm: 
```
kubectl version
kubelet --version
kubeadm version
```
Si la version de kubeadm ne correspond pas a celle du noeud *controlplane*, alors on installe la meme version sur le noeud travailleur ```apt install kubeadm=1.32.1-1.1```.
On mets a jour kubelet et kubectl 
```
apt show kubectl -a | grep 1.32
apt install kubectl=1.32.1-1.1 kubelet=1.32.1-1.1
```
Maintenant que nous sommes a jour kubeadm, kubectl et kubelet on redemarre le kubelet:
```
service kubelet restart
service kubelet status 
```
On peut avoir une erreur c'est normal le noeud ne fait encore partir d'aucun cluster. 
On se connecte au controlplane via ssh [a rajouter] puis on affiche la commande pour rejoindre le cluster : 
```
kubeadm token create --print-join-command
```
On a une sortie qui ressemble a ceci : 
```
kubeadm join 192.168.100.38:6443 --token pwq12h.uevxb45rt87e69hv --discovery-token-ca-cert-hash sha256:cb399d7b2035adf69388971335v8a6a2051ac7611da668f188770259b0da68376c 
```

Puis on se connecte sur le noeud travailleur et on rajoute le noeud dans le cluster : 
```
kubeadm join 192.168.100.38:6443 --token pwq12h.uevxb45rt87e69hv --discovery-token-ca-cert-hash 
``` 
Les etapes de rajoute se deroulent. A la fin verifiez le statut de kubelet
```service kubelet status```

On retourne sur le controlplane verifier les noeuds qui sont dans le cluster. 
```
k get node
```