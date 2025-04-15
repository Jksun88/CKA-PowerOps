Lorsque les ressources de mémoire ou de processeur disponibles sur les nœuds atteignent leur limite, Kubernetes recherche les pods qui utilisent plus de ressources qu'ils n'en ont demandé. Ce seront les premiers candidats à l'arrêt. Si certains conteneurs Pods n'ont pas de demandes/limites de ressources définies, ils sont alors considérés par défaut comme utilisant plus que ce qui est demandé. Kubernetes attribue des classes de qualité de service aux Pods en fonction des ressources et des limites définies.

Nous devons donc rechercher les Pods qui n'ont pas de demandes de ressources définies, ce que nous pouvons faire avec une approche manuelle :
```
k -n project-n1 describe pod | less -p Requests
```
ou nous pouvons faire ceci : 
```
k -n project-n1 describe pod | grep -A 3 -E 'Requests|^Name:'
```

On pourra aussi regarder les pods dont la qualite de service (QoS) est *BestEffort* ces pods n'ont pas de ressources et limites definie. On peut aussi utiliser les outils comme prometheus pour voir combien de ressources sont consommees par le containers. On peut aussi se connecter dans le container et faire la commande top pour voir les ressources utiliser. `kubectl top pod` ou `kubectl exec`