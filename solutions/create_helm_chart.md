# Create Helm chart
1. Remove temporary Directory and use the Helm CLI to create a new Helm chart
```
cd devops-lab-wparish/helm-charts
 cd helm-charts/nginx-demo/
rm -rf nginx-demo/
# Note that the default `helm create` command uses the nginx community Docker
# image, what we're looking for
helm create nginx-demo
cd nginx-demo/
ls -lrt
# Note: I am making a Git commit here so I can show the changes I make later
git status
git add .
git commit -m "Initial creation of nginx-demo helm chart"
```

2. Edit the chart's values.yaml file to change the K8s svc type to NodePort
> Note: ClusterIP svc types provide internal overlay network IPs, whereas a
 NodePort will cause all K8s nodes to NAT 30,000-32,000 ports from their public
 IPs to the overlay network pod IPs
```
wes@wes-desktop-ubuntu:~/workspace/devops-lab-wparish/helm-charts/nginx-demo$ git diff
diff --git a/helm-charts/nginx-demo/values.yaml b/helm-charts/nginx-demo/values.yaml
index 1d6ea0a..777f4d7 100644
--- a/helm-charts/nginx-demo/values.yaml
+++ b/helm-charts/nginx-demo/values.yaml
@@ -37,7 +37,7 @@ securityContext: {}
   # runAsUser: 1000

 service:
-  type: ClusterIP
+  type: NodePort
   port: 80

 ingress:
wes@wes-desktop-ubuntu:~/workspace/devops-lab-wparish/helm-charts/nginx-demo$
```

3. Create a new Namespace in K8s to deploy the Helm chart to
```
kubectl create namespace nginx-demo
```

4. Deploy the Helm chart to K8s
```
helm -n nginx-demo install nginx-demo .
```

5. Wait for the nginx pod to become healthy
```
wes@wes-desktop-ubuntu:~/workspace/devops-lab-wparish/helm-charts/nginx-demo$ kubectl -n nginx-demo get pods,svc -owide
NAME                              READY   STATUS    RESTARTS   AGE   IP           NODE      NOMINATED NODE   READINESS GATES
pod/nginx-demo-84c77f7d8d-ldkhj   1/1     Running   0          13m   10.42.1.15   w-dock8   <none>           <none>

NAME                 TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE   SELECTOR
service/nginx-demo   NodePort   10.43.204.73   <none>        80:31834/TCP   13m   app.kubernetes.io/instance=nginx-demo,app.kubernetes.io/name=nginx-demo
```

6. Test using curl to ensure the default Nginx HTML page is returned
```
# Note: I'm using the last node in my cluster
k8s_node_ip=$(kubectl get nodes -ojson | jq '.items[-1].status.addresses[0].address' -r)
k8s_node_port=$(kubectl -n nginx-demo get svc nginx-demo -ojson | jq '.spec.ports[0].nodePort' -r)
curl http://${k8s_node_ip}:${k8s_node_port}
```

7. Commit and push Helm chart updates
```
git status
git diff
git add .
git commit -m "Changing Helm chart service type to NodePort"
git push origin main
```