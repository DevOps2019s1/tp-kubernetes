Get Pods IP
kubectl get pods -o wide -n app-dev
To curl into hello-pod :
kubectl exec curlpod -n app-dev -it -- curl http://{hello-pod IP}:80/api/

