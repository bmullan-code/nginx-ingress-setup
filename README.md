# nginx-ingress-setup
nginx-ingress-setup setup on tkgs


# Install nginx-ingress
```
git clone https://github.com/nginxinc/kubernetes-ingress/
cd kubernetes-ingress/deployments/helm-chart
```

I made one change to the values.yaml file setting 
```
externalTrafficPolicy: Cluster
```
Install the chart
```
helm install ingress2 .
```

Get the nginx-ingress service and confirm an ip address has been created
```
kubectl get svc ingress2-nginx-ingress
NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
ingress2-nginx-ingress   LoadBalancer   198.51.100.230   192.168.2.6   80:31492/TCP,443:30845/TCP   3h56m
```

Edit a dns hostname to point to this ip eg.
```
nslookup ingress.vinalhaven.info
Server:		10.21.82.100
Address:	10.21.82.100#53

Non-authoritative answer:
Name:	ingress.vinalhaven.info
Address: 192.168.2.6
```


Create the example deployment and ingress
```
kubectl create -f deployment-example.yaml	
kubectl create -f ingress-example.yaml
```

Check the status of all objects
```
kubectl get svc,pods,ingress | grep ingress2
service/ingress2-nginx-ingress        LoadBalancer   198.51.100.230   192.168.2.6   80:31492/TCP,443:30845/TCP   3h40m
service/ingress2-service              ClusterIP      198.51.100.92    <none>        80/TCP                       7m2s
pod/ingress2-deployment-b47f8658c-4vvmg      1/1     Running   0          7m2s
pod/ingress2-deployment-b47f8658c-jhqrh      1/1     Running   0          7m2s
pod/ingress2-deployment-b47f8658c-wdt5g      1/1     Running   0          7m2s
pod/ingress2-nginx-ingress-6d98bfcb4-pn7rb   1/1     Running   0          3h40m
ingress.extensions/ingress2-ingress   <none>   ingress.vinalhaven.info   192.168.2.6   80      6m53s
```

Here's the ingress
```
kubectl get ingress ingress2-ingress
NAME               CLASS    HOSTS                     ADDRESS       PORTS   AGE
ingress2-ingress   <none>   ingress.vinalhaven.info   192.168.2.6   80      7m38s
```

Confirm the application can be resolved at http://ingress.vinalhaven.info

![](https://github.com/bmullan-pivotal/nginx-ingress-setup/blob/main/kuard-screenshot.png?raw=true)


