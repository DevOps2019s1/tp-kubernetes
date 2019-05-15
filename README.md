Project : 
1. Créer les images Docker de l’application et du reverse proxy.
2. Pousser les images sur la registry docker de votre application.
3. Définissez un secret (login mdp crypté) pour télécharger les images depuis la registry
Docker ATTENTION: secret dockerregistry
4. Créer le manifest du pod YAML qui hébergera les conteneurs du reverse proxy et de
l’application GO
5. Créez le pod dans le namespace par défaut apps-dev
6. créer un pod curl pour tester le micro-service en dehors du pod.

Use this command first for launch Pods
k apply -f ./helloapp-pod.yml
k apply -f ./curl-pod.yml

Check ip hello app pod
k get po -n=apps-dev -o wide

Finally use curl command
k exec curl-pod -n=apps-dev -i -t -- curl http://<hello_app_pod_here>:80/api/

