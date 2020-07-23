To get your admin password of jenkins run
kubectl get secret --namespace default jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode; echo


To get your admin user of jenkins run
kubectl get secret --namespace default jenkins -o jsonpath="{.data.jenkins-admin-user}" | base64 --decode; echo
