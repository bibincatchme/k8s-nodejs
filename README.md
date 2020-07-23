printf $(kubectl get secret --namespace default my-jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo
