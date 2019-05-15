Ce projet kubernetes a pour but d'installer deux pods :
- Un avec un serveur NGINX et son reverse proxy. 
- Le second avec un curl qui permet de tester le bon fonctionnement. 

Dans ce fichier je vais vous décrire comment installer ce projet sur votre machine. 
Il faut que les logiciel lié a kubernetes soit présent, je vous renvoie sur la documentation officiel : 
- https://kubernetes.io/docs/home/

Pour utilisé ce projet, il y'a quelques modifications a effectuer : 

Dans le dossier Key, modifier le fichier : kustomization.yaml

<<
  - docker-server=https://index.docker.io/v1/
  - docker-username=user
  - docker-password=password
  - docker-email=email
>>

En se placant dans dossier key faire : 

kubectl apply -k . ou kubectl apply -k ./key

Il devrait générer une clé secret avec un identifiant

<<
➜  key kubectl apply -k .
secret/tp3secretkey-7t7fgdfhc2 created
>>

Copié la clé : tp3secretkey-7t7fgdfhc2 

Ou vérifié vos clé via la commande suivante : 


<<
kubectl get secret
NAME                      TYPE                                  DATA   AGE
default-token-4ghr8       kubernetes.io/service-account-token   3      44h
mysecret                  kubernetes.io/dockerconfigjson        1      19h
tp3secretkey-7t7fgdfhc2   docker-registry                       4      40s
tp3secretkey-gmtc572dt8   docker-registry                       4      25m
>>


Copié votre clé et retourné a la racine du projet : cd .. si vous étiez dans le dossier key

Modifier le fichier install.yaml pour remplacer le nom de la clé actuel avec celui généré. 

<<
imagePullSecrets:
 - name: tp3secretkey-gmtc572dt8 

 imagePullSecrets:
 - name: tp3secretkey-7t7fgdfhc2 
>>

Créer votre propre namespace ou réutiliser le suivant : 
kubectl create namespace tp3

En cas de création d'un nouveau namespace ne pas oublier de modifier les namespaces des fichiers yamel du projet. 

<<
metadata:
  name: curl-pod
  namespace: tp3   
>>

Si vous utilisez votre propre dockerhub, pensez a changer le nom des images par les votres :

<<
  - name: nginx
    image: somafairey/tp3.5:nginx (votre image nginx)
    ports: 
     - containerPort: 80 
  - name: go
    image: somafairey/tp3.5:hellogo (votre image hellogo)
    ports:
     - containerPort: 8080 
>>

Maintenant vous êtes prêt a installer votre projet.
<<
kubectl apply -f install.yaml
pod/install-pod created
kubectl apply -f curl.yaml
pod/curl-pod created
>>

Vérifiez que vos deux pods sont au status running
<<
kubectl get pod -n tp3 (remplacer tp3 par votre namespace) -o wide
NAME        READY   STATUS    RESTARTS   AGE   IP            NODE       NOMINATED NODE   READINESS GATES
curl-pod    1/1     Running   0          27m   172.17.0.11   minikube   <none>           <none>
hello-pod   2/2     Running   0          28m   172.17.0.10   minikube   <none>           <none>
>>

L'adresse ip et le spacename sont a adapté

<<
kubectl exec curl-pod -n tp3 -it curl 172.17.0.10/api/
>>

Et vous devriez voir que sa fonctionne.

Merci d'avoir lu ce guide.