Get Pods IP
kubectl get pods -o wide -n apps-dev
To curl into hello-pod :
kubectl exec curlpod -n apps-dev -it -- curl http://{hello-pod IP}:80/api/

