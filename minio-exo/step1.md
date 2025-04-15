Rappels 
**Helm Chart**: Fichiers de modèles Kubernetes YAML combinés dans un seul package, les fichiers de valeurs permettent la personnalisation

**Helm Release**: Instance installée d'une **"chart"** 

**Helm Values**: Permet de personnaliser les fichiers modèles YAML dans un *chart* lors de la création d'une *release*

**Operator**: Pod qui communique avec l'API Kubernetes et peut fonctionner avec les CRD *(Custom Resource Definition - Définition de ressource personnalisée)*

**CRD**: Custom Resources sont des extensions de l'API Kubernetes 

# Etape 1: Creation de l'espace de noms (namespace)
On cree le namespace *minio* en utilisant des commandes ci-dessous : 
```
k create ns minio
kubectl create namespace minio
```
On peut verifier la creation de l'espace en faisant : 
```
k get ns
kubectl get namespace | grep -i minio
```
Une fois l'espace de nom creer, avant de passer a la l'installation de notre chart on va ajouter le repertoire de notre chart dans notre environement helm. 
```
helm repo add minio-operator https://operator.min.io
```
on peut verifier la presence de notre repo en faisant : 
```helm repo list```
# Etape 2: Installation de minio helm chart dans le ns minio
On verifie les version de minio-operator present : ```helm search repo```
On installe l'operateur minio : 
```
helm -n minio install minio-operator minio-operator/operator
``` 
Pour verifier on va faire : 
```
helm -n minio ls 

NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
minio-operator  minio           1               2025-04-15 20:48:40.501278296 +0000 UTC deployed        operator-7.0.1  v7.0.1     
```
Parce que nous avons installer minio on a de nouveau CRD disponible 
```
controlplane:~$ k -n minio get crd | grep -i min.io
policybindings.sts.min.io                             2025-04-15T20:48:41Z
tenants.minio.min.io                                  2025-04-15T20:48:41Z
controlplane:~$ 
```
L'installation de minio a rajoute des CRD. Donc nous pouvons creer des ressources *tenants* et *policybindings* comme on cree les *pods*. 

# Etape 3: Creation de Tenant 
On modifie le fichier minio-tenant.yaml et on cree le tenant. 
Pour la modification on peut utiliser notre editeur de texte *nano,vi,vim* et on rajoute ceci dans le fichier tenant: 
```
apiVersion: minio.min.io/v2
kind: Tenant
metadata:
  name: tenant
  namespace: minio
  labels:
    app: minio
spec:
  features:
    bucketDNS: false
    enableSFTP: true                     # ADD
  image: quay.io/minio/minio:latest
  pools:
    - servers: 1
      name: pool-0
      volumesPerServer: 0
      volumeClaimTemplate:
        apiVersion: v1
        kind: persistentvolumeclaims
        metadata: { }
        spec:
          accessModes:
            - ReadWriteOnce
          resources:
            requests:
              storage: 10Mi
          storageClassName: standard
        status: { }
  requestAutoCert: true
```
On cree le tenant par la suite : 
```
k -f minio-tenant.yaml apply 
```
Notes: verifier le chemin vers le fichier tenant
