All that's asked for here could be extracted by manually reading the kubeconfig file. But we're going to use kubectl for it.

# Step 1

First we get all context names:
```
k --kubeconfig /opt/course/1/kubeconfig config get-contexts
k --kubeconfig /opt/course/1/kubeconfig config get-contexts -oname
k --kubeconfig /opt/course/1/kubeconfig config get-contexts -oname > /opt/course/1/contexts
```
We can do the extraction using jsonpath
```
k --kubeconfig /opt/course/1/kubeconfig config view -o yaml


k --kubeconfig /opt/course/1/kubeconfig config view -o jsonpath="{.contexts[*].name}"
```
# Step 2 : Interroge le contexte courant
Pour avoir le contexte courant a partir du fichier de config donner, on va renseigner la commande ci dessous : 
```
k --kubeconfig /opt/course/1/kubeconfig config current-context 
```
ou encore la commande : 
```
k config current-context
```
Lorsque le fichier de configuration utiliser se trouve dans le repertoire `~/.kube/`. 
### Enregistrer le contexte courant dans un fichier
Enregistre le contexte courant dans ce fichier /opt/course/1/current-context
```
k --kubeconfig /opt/course/1/kubeconfig config current-context > /opt/course/1/current-context
```
# Etape 3 : Extraire le certificat et le decode en base64
Pour extraire le certificat on va afficher la configuration ouverte du fichier de configuration au format yaml
```
k --kubeconfig /opt/course/1/kubeconfig config view -o yaml --raw
```
L'option *--raw* permet d'afficher en clair les informations sensibles du certificat. Cet information est situe apres la variable `client-certificate-data: valeur sensible`
Une autre methode consiste a ouvrir le fichier de configuration avec un editeur et de copier la valeur de cette variable et la decoder en base64. En faisant la commande : 
`echo LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUN2RE... | base64 -d > /opt/course/1/cert`

