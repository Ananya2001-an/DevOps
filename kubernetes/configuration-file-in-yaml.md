# Configuration file in YAML

It’s used to create K8s components like deployments, services, etc., with ease.

It has 3 main parts;

1. **metadata**
2. **Specification (specs)**
3. **Status** → automatically added by K8s; it’s used to see if the desired state of component is matching with the actual state about which we gain info from “etcd”. If not then we make changes to achieve the desired state.

### Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
 name: nginx-deployment
 labels:
  app: nginx
spec:
 replicas: 2
 selector:
  matchLabels:
   app: nginx
 template:
  metadata:
   labels:
    app: nginx
  spec:
   containers:
    - name: nginx
      image: nginx:1.16
      ports:
      - containerPort: 8080
```

Using labels and selectors we connect components like deployments to pods and services to deployments. Like in pod spec we can see label as a key-value pair(app: nginx) and in deployment spec we have selector with the same key-value pair. This is done so that deployments know that which pods are under the same deployment. Also below given service config Yaml file has a spec with selector which defines that it should connect to which deployment and its pods.

### Service (internal, default type: ClusterIP )

```yaml
apiVersion: v1
kind: Service
metadata:
 name: nginx-service
spec: 
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
```

If another service tries to connect with this service it will connect through port 80 and then the current service will direct this request to the target port 8080 where our container is listening as defined by the deployment config file.

### Secret

```yaml
apiVersion: v1
kind: Secret
metadata:
    name: mongodb-secret
type: Opaque
data:
    mongo-root-username: ... //must be base64 encoded
    mongo-root-password: ... 
```

For getting the base64 output for your secrets you can simply run below command in terminal:

```bash
echo -n "username" | base64
```

After the secrets file is implemented only then you can use it in your deployment file for running the pod container otherwise it will give errors. Below example shows how to use secrets in your config file for deployment.

```yaml
...
spec:
   containers:
    - name: mongodb
      image: mongo
      ports:
      - containerPort: 27017
      env:
      - name: MONGO_INITDB_ROOT_USERNAME
        valueFrom:
          secretKeyRef:
           name: mongodb-secret
           key: mongo-root-username
      - name: MONGO_INITDB_ROOT_PASSWORD
        valueFrom:
          secretKeyRef:
           name: mongodb-secret
           key: mongo-root-password  
```

> We can write configuration for more than 1 components in a single file by separating them by "---" like shown below.

```yaml
apiVersion: v1
kind: Deployment
...

---

apiVersion: v1
kind: Service
...
```

### ConfigMap

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
    name: mongodb-configmap
data:
    database_url: mongodb-service //plain text format 
```

For referencing ConfigMap data we do something similar like we did for secrets:

```yaml
 valueFrom:
       configMapKeyRef:
        name: mongodb-configmap
        key: database_url
```

### Service (external, type: LoadBalancer)

```yaml
apiVersion: v1
kind: Service
metadata:
 name: mongo-express-service
spec: 
  selector:
    app: mongo-express
  type: LoadBalancer
  ports:
  - protocol: TCP
    port: 8081
    targetPort: 8081
    nodePort: 30000 //should be between 30000 to 32767 for external access
```

For this service we will get both internal and external service IP address but in minikube this external IP is not automatically implemented. To get it we have to run one more command:

```bash
minikube service "service name"
```

### Ingress

```yaml
apiVersion: …
kind: Ingress
metadata:
    name: myapp-ingress
spec:
    rules:
    - host: myapp.com
      http:
       paths:
       - backend:
          serviceName: myapp-internal-service
          servicePort: 8080
```

When the user types the above host url into the search bar, ingress will then route the request to the internal service given by serviceName.

“http” in this yaml file refers to the protocol taken when making request to the internal service not the one that’s made from browser to ingress.

### Ingress controller

To implement ingress we need an ingress controller. It can’t be implemented simply using the above yaml configuration given.

Roles:

1. Evaluates all the ingress rules
2. manages redirections&#x20;
3. entrypoint to the cluster
4. many third party implementations

By default K8s provides you with the “**K8s Nginx ingress controller**”

Now depending on the environment on which your cluster runs we can have 2 ways to set up external request service:

1. If you are using a cloud provider like AWS, Google cloud or Linode then we don’t have to implement load balancer by ourselves. There would be a cloud load balancer that will be readily available to listen to the public requests and then forward them to the ingress controller which will then evaluate the rules and handle forwarding from there.
2. Or else you can set up a proxy server inside or outside your cluster that will listen to your requests and then forward them to ingress controller. Acts as the only entry point to the cluster as it will have a public IP address.&#x20;

Below command will enable the K8s Nginx ingress controller In minikube.

```bash
minikube addons enable ingress
```

Now once you apply the above ingress yaml file you can get the IP address of ingress controller by writing command:

```bash
kubectl get ingress
```

Now save this information In the /etc/hosts file by writing the IP and host name in one line like this:&#x20;

```
192.0.9.78(example IP)     myapp.com 
```

Now you can access the internal service and therefore the running pod at this host.

#### Multiple paths for same host

```yaml
host: myapp.com
…
paths:
 - path: /analytics
   …
 - path: /shopping
   …
```

#### Multiple sub-domains or domains

```yaml
- host: ananlytics.myapp.com
  …
- host: shopping.myapp.com
  …
```

#### Configure TLS certificate to make external requests through “https” protocol

```yaml
spec:
  tls:
  - hosts:
    - myapp.com
    secretName: myapp-secret-tls
```

```yaml
apiVersion:…
kind: Secret
metadata:
    name: myapp-secret-tls
data:
    tls.crt: base64 encoded cert
    tls.key: base64 encoded key
type: kubernetes.io/tls
```
