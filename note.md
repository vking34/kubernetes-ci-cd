1. deploy registry
```
$ kubectl apply -f manifests/registry.yaml
```

2. build socat docker to push docker image into registry
```
$ docker build -t socat-registry -f applications/socat/Dockerfile applications/socat
```

3. Run socat container
```
$ docker run -d -e "REG_IP=`minikube ip`" -e "REG_PORT=30400" --name socat-registry -p 30400:5000 socat-registry
```

4. Build Jenkins image
```
$ docker build -t 127.0.0.1:30400/jenkins:latest -f applications/jenkins/Dockerfile applications/jenkins
```
5. Push jenkins image into registry
```
# docker push 127.0.0.1:30400/jenkins:latest
```

6. Deploy jenkins
```
$ kubectl apply -f manifests/jenkins.yaml
```

7. Get root password

```
kubectl exec -it `kubectl get pods --selector=app=jenkins --output=jsonpath={.items..metadata.name}` cat /var/jenkins_home/secrets/initialAdminPassword
```

8. 