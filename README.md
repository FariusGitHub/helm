# HELM CHARTS

![](/images/07-image01.png)
<br><br>

Helm is a package manager for Kubernetes, similar to how `sudo apt-get install` or `npm` work in other development environments.<br>

The word "chart" in Helm chart refers to a package of pre-configured Kubernetes resources that can be deployed as a unit. It is not just a name, but a term used in the context of Helm, which is a package manager for Kubernetes. The chart contains a collection of YAML files that define the desired state of the resources to be deployed in a Kubernetes cluster..<br>

In Kubernetes, applications are deployed using YAML files that describe the desired state of the system. These files can become complex and difficult to manage, especially when deploying multiple applications with different configurations. This is where Helm comes in..<br>

Helm allows you to package, deploy, and manage applications and their dependencies in Kubernetes. It provides a way to define, install, and upgrade applications using pre-configured templates called Helm charts. Helm charts are like packages that contain all the necessary Kubernetes manifests, configurations, and dependencies for an application..<br>

With Helm, you can easily install and manage applications in Kubernetes by running simple commands. For example, you can use `helm install` to deploy an application using a specific Helm chart. Helm will take care of resolving dependencies, creating the necessary Kubernetes resources, and managing the lifecycle of the application..<br>

Helm also supports versioning, allowing you to easily upgrade or rollback applications to a specific version. It provides a centralized repository called Helm Hub, where you can find and share Helm charts with the community..<br>

In summary, Helm simplifies the deployment and management of applications in Kubernetes by providing a package manager-like experience. It helps streamline the process of installing, upgrading, and managing applications, making it easier to work with complex Kubernetes environments..<br>

Helm also has its own repo similar with what Docker and GitHub have. <br>


| Criteria     | Docker Repo | GitHub Repo | Helm Repo |
|--------------|-------------|-------------|-----------|
| Purpose      | To store and distribute Docker images | To store and manage source code | To store and distribute Helm charts |
| Content      | Docker images | Source code files | Helm chart files |
| Versioning   | Uses tags to version images | Uses branches and tags to version code | Uses chart versions to version charts |
| Access       | Public or private repositories | Public or private repositories | Public or private repositories |
| Collaboration| Supports collaboration through pull requests and issues | Supports collaboration through pull requests and issues | Supports collaboration through pull requests and issues |
| Dependency Management | Can specify base images and dependencies in Dockerfile | Can specify dependencies in package.json or requirements.txt | Can specify dependencies in Chart.yaml |
| CI/CD Integration | Can be integrated with CI/CD tools for automated image building and deployment | Can be integrated with CI/CD tools for automated code testing and deployment | Can be integrated with CI/CD tools for automated chart testing and deployment |
| Searchability | Can search for images based on tags and keywords | Can search for repositories and code based on keywords | Can search for charts based on keywords |
| Community Support | Has a large community of users and contributors | Has a large community of users and contributors | Has a growing community of users and contributors |
| Ecosystem     | Integrates with other Docker tools like Docker Compose and Docker Swarm | Integrates with other Git tools like GitLab and Bitbucket | Integrates with Kubernetes for managing application deployments |
| Security      | Provides security features like image scanning and vulnerability detection | Provides security features like code scanning and vulnerability detection | Provides security features like chart scanning and vulnerability detection |
| Official Website      | hub.docker.com | gitHub.com | artifactHub.io |

While Artifact Hub is primarily known for hosting Helm charts, it also supports other types of artifacts and resources. The KCL module and Kyverno policy are examples of non-Helm artifacts that can be found on Artifact Hub. It aims to be a comprehensive repository for various types of artifacts related to cloud-native applications and infrastructure.

Let's take an example of ingress-nginx package template located in
```txt
https://artifacthub.io/packages/helm/ingress-nginx/ingress-nginx?modal=template&template=admission-webhooks/job-patch/job-createSecret.yaml
```
As you can see this template has 48 yaml files and imagine that amount of work to deploy them one by one into production without having this template.<br>

Before helm existed, we need to manually managed the yaml file deployment (foro above case there are 48 of them)
![](/images/07-image02.png)
<br><br>

| Aspect              | Kubernetes Deployment | Kubernetes Deployment with Helm |
|---------------------|-----------------------|---------------------------------|
| Ease of use         | Requires manual YAML file creation and management | Provides a higher level of abstraction and simplifies deployment management |
| Templating          | Does not provide built-in templating capabilities | Offers powerful templating engine for managing configuration files |
| Versioning          | Does not have built-in versioning support | Allows versioning of deployments and easy rollback to previous versions |
| Dependency management | Does not have built-in dependency management | Provides dependency management for managing complex application deployments |
| Community support   | Has a large and active community for support and resources | Helm has a dedicated community and a wide range of available charts for popular applications |
| Upgrades            | Requires manual updates and rolling deployments | Helm provides a streamlined process for upgrading deployments with version control |
| Scalability         | Supports scaling of deployments manually or through automation | Helm simplifies the process of scaling deployments with predefined charts |
| Release management  | Does not have built-in release management capabilities | Helm provides release management features like release history and rollbacks |
| Customization       | Allows customization through YAML files | Helm allows customization through values files and templates |
| Ecosystem           | Integrates well with other Kubernetes tools and frameworks | Helm has its own ecosystem of charts and plugins for extending functionality |

In summary, Kubernetes Deployment without Helm requires manual management of YAML files for each deployment, while Helm simplifies the deployment process by introducing charts, templating, versioning, and dependency management. Helm provides a more streamlined and efficient way to deploy and manage applications on Kubernetes.

## HELM INSTALLATION
Let's do below steps to install helm by downloading this 12.7M tar file, unzip it and move it to /usr/local/bin/helm and check if helm is now installed

```txt
curl -LO https://get.helm.sh/helm-v3.12.2-linux-amd64.tar.gzdevops@msi:~$ 
    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                    Dload  Upload   Total   Spent    Left  Speed
    100 15.2M  100 15.2M    0     0  12.7M      0  0:00:01  0:00:01 --:--:-- 12.7M

tar -zxvf helm-v3.12.2-linux-amd64.tar.gz
    linux-amd64/
    linux-amd64/helm
    linux-amd64/LICENSE
    linux-amd64/README.md

sudo mv linux-amd64/helm /usr/local/bin/helm

helm version
    version.BuildInfo{Version:"v3.12.2", GitCommit:"1e210a2c8cc5117d1055bfaa5d40f51bbc2e345e", GitTreeState:"clean", GoVersion:"go1.20.5"}
```

We can also add https://charts.helm.sh/stable in the repositories and we can see the new list 

```txt
helm repo add stable https://charts.helm.sh/stable
    "stable" has been added to your repositories

helm repo list
    NAME  	URL                          
    stable	https://charts.helm.sh/stable
```

Let's try some basic install and uninstall command like below and list them after

```txt
helm install demo-mysql stable/mysql
    WARNING: This chart is deprecated
    NAME: demo-mysql
    LAST DEPLOYED: Sat Feb  3 15:53:57 2024
    NAMESPACE: default
    STATUS: deployed
    REVISION: 1
    NOTES:
    MySQL can be accessed via port 3306 on the following DNS name from within your cluster:
    demo-mysql.default.svc.cluster.local

    To get your root password run:

        MYSQL_ROOT_PASSWORD=$(kubectl get secret --namespace default demo-mysql -o jsonpath="{.data.mysql-root-password}" | base64 --decode; echo)

    To connect to your database:

    1. Run an Ubuntu pod that you can use as a client:

        kubectl run -i --tty ubuntu --image=ubuntu:16.04 --restart=Never -- bash -il

    2. Install the mysql client:

        $ apt-get update && apt-get install mysql-client -y

    3. Connect using the mysql cli, then provide your password:
        $ mysql -h demo-mysql -p

    To connect to your database directly from outside the K8s cluster:
        MYSQL_HOST=127.0.0.1
        MYSQL_PORT=3306

        # Execute the following command to route the connection:
        kubectl port-forward svc/demo-mysql 3306

        mysql -h ${MYSQL_HOST} -P${MYSQL_PORT} -u root -p${MYSQL_ROOT_PASSWORD}

helm list
    NAME      	NAMESPACE	REVISION	UPDATED                                	STATUS  	CHART      	APP VERSION
    demo-mysql	default  	1       	2024-02-03 15:53:57.781030602 -0500 EST	deployed	mysql-1.6.9	5.7.30     

helm uninstall demo-mysql
    release "demo-mysql" uninstalled

helm list
    NAME	NAMESPACE	REVISION	UPDATED	STATUS	CHART	APP VERSION
```

Now, let's see the pods after the installation done.

```txt
helm install demo-mysql stable/mysql

kubectl get pods
    NAME                         READY   STATUS    RESTARTS   AGE
    demo-mysql-cc89d776c-pc4vt   0/1     Running   0          11s

kubectl get all | grep mysql
    pod/demo-mysql-cc89d776c-pc4vt   0/1     Running   0          22s
    service/demo-mysql   ClusterIP   10.106.224.124   <none>        3306/TCP   23s
    deployment.apps/demo-mysql   0/1     1            0           23s
    replicaset.apps/demo-mysql-cc89d776c   1         1         0       23s
```

## SECRETS
Secrets in Helm are crucial for securely managing and deploying sensitive information within a Helm chart, similar to how Windows MSI or EXE files handle sensitive data during installation or execution. They provide encryption, separation, and easy management of secrets, ensuring the security and integrity of the deployed applications.

```txt
kubectl get secrets | grep demo-mysql
    demo-mysql                         Opaque               2      4m40s
    sh.helm.release.v1.demo-mysql.v1   helm.sh/release.v1   1      4m40s

helm uninstall demo-mysql
    release "demo-mysql" uninstalled

kubectl get secrets | grep demo-mysql
    No resources found in default namespace.
```

## STRUCTURE OF HELM CHART
The common elements of a Helm chart include:

1. Chart.yaml: This file contains metadata about the chart, such as the name, version, and description.

2. values.yaml: This file contains the default values for the chart's configurable parameters. Users can override these values when installing the chart.

3. templates/: This directory contains the template files for the Kubernetes resources that will be created when the chart is installed. These templates can include YAML files for deployments, services, config maps, and other Kubernetes objects.

4. charts/: This directory can contain sub-charts, which are reusable Helm charts that can be included in the main chart.

5. README.md: This file provides documentation and instructions for using the chart.

![](/images/07-image03.png)
<br><br>

## EXCERCISE 1 : Single Helm Chart
Let's try to make a new folder called Guestbook and make a helm chart inside

```txt
mkdir Guestbook
cd Guestbook
mkdir templates

cat Chart.yaml
    apiVersion: v2
    name: Guestbook
    appVersion: "1.0"
    description: this is a guest book
    version: 0.1.0
    type: application

cd templates
cat frontend-configmap.yaml
    apiVersion: v1
    kind: ConfigMap
    metadata:
    name: frontend-config
    data:
    guestbook-name: "MyPopRock Festival 2.0"
    backend-uri: "http://backend.minikube.local/guestbook"

cat frontend-service.yaml
    apiVersion: v1
    kind: Service
    metadata:
    labels:
        name: frontend
    name: frontend
    spec:
    ports:
        - protocol: "TCP"
        port: 80
        targetPort: 4200
    selector:
        app: frontend

cat frontend.yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    name: frontend
    spec:
    replicas: 1
    selector:
        matchLabels:
        app: frontend
    template:
        metadata:
        labels:
            app: frontend
        spec:
        containers:
        - image: phico/frontend:1.0
            imagePullPolicy: Always
            name: frontend
            ports:
            - name: frontend
            containerPort: 4200

cat ingress.yaml
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
    name: guest-ingress
    spec:
    rules:
    - host: frontend.minikube.local
        http:
        paths:
        - path: /
            pathType: Prefix
            backend:
            service:
                name: frontend
                port:
                number: 80
    - host: backend.minikube.local
        http:
        paths:
        - path: /
            pathType: Prefix
            backend:
            service:
                name: backend
                port:
                number: 80

cd ..
tree
    .
    ├── Chart.yaml
    └── templates
        ├── frontend-configmap.yaml
        ├── frontend-service.yaml
        ├── frontend.yaml
        └── ingress.yaml

    2 directories, 5 files
```

Let's deploy this helm chart. Make sure that you are one level on top of Guestbook folder so this command can figure out all the files

```txt
helm install demo-guestbook Guestbook
    NAME: demo-guestbook
    LAST DEPLOYED: Sat Feb  3 17:14:30 2024
    NAMESPACE: default
    STATUS: deployed
    REVISION: 1
    TEST SUITE: None

kubectl get pod,deployment,svc,ingress,cm,secrets
    NAME                            READY   STATUS              RESTARTS   AGE
    pod/frontend-754d5d5bd5-n4xfw   0/1     ContainerCreating   0          3s

    NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/frontend   0/1     1            0           4s

    NAME                 TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
    service/frontend     ClusterIP   10.110.22.132   <none>        80/TCP    4s
    service/kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP   5d23h

    NAME                                      CLASS    HOSTS                                            ADDRESS   PORTS   AGE
    ingress.networking.k8s.io/guest-ingress   <none>   frontend.minikube.local,backend.minikube.local             80      4s

    NAME                         DATA   AGE
    configmap/frontend-config    2      4s
    configmap/kube-root-ca.crt   1      5d23h

    NAME                                          TYPE                 DATA   AGE
    secret/sh.helm.release.v1.demo-guestbook.v1   helm.sh/release.v1   1      4s
```

### upgrade
There is an installation upgrade command like helm upgrade. Let's make some changes.

```txt
nano frontend.yaml 
    # change image version
    # from phico/frontend:1.0
    # into phico/frontend:1.1

cat frontend.yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    name: frontend
    spec:
    replicas: 1
    selector:
        matchLabels:
        app: frontend
    template:
        metadata:
        labels:
            app: frontend
        spec:
        containers:
        - image: phico/frontend:1.1
            imagePullPolicy: Always
            name: frontend
            ports:
            - name: frontend
            containerPort: 4200


helm upgrade demo-guestbook Guestbook
    Release "demo-guestbook" has been upgraded. Happy Helming!
    NAME: demo-guestbook
    LAST DEPLOYED: Sat Feb  3 17:39:06 2024
    NAMESPACE: default
    STATUS: deployed
    REVISION: 2
    TEST SUITE: None
```
Notice, the new secret came out as upgrade happened.
```txt
kubectl get secrets
NAME                                   TYPE                 DATA   AGE
sh.helm.release.v1.demo-guestbook.v1   helm.sh/release.v1   1      9m4s
sh.helm.release.v1.demo-guestbook.v2   helm.sh/release.v1   1      2m4s
```
Compare the two version below
```txt
kubectl describe secret sh.helm.release.v1.demo-guestbook.v1
    Name:         sh.helm.release.v1.demo-guestbook.v1
    Namespace:    default
    Labels:       modifiedAt=1706999946
                name=demo-guestbook
                owner=helm
                status=superseded
                version=1
    Annotations:  <none>

    Type:  helm.sh/release.v1

    Data
    ====
    release:  2064 bytes

kubectl describe secret sh.helm.release.v1.demo-guestbook.v2
    Name:         sh.helm.release.v1.demo-guestbook.v2
    Namespace:    default
    Labels:       modifiedAt=1706999947
                name=demo-guestbook
                owner=helm
                status=deployed
                version=2
    Annotations:  <none>

    Type:  helm.sh/release.v1

    Data
    ====
    release:  2072 bytes
```
### rollback
So far we have 2 revision like below. Although the history does not differentiate well with version are similar the secret list could be useful to see the changes log.
```txt
helm history demo-guestbook
    REVISION	UPDATED                 	STATUS    	CHART          	APP VERSION	DESCRIPTION     
    1       	Sat Feb  3 17:32:06 2024	superseded	Guestbook-0.1.0	1.0        	Install complete
    2       	Sat Feb  3 17:39:06 2024	deployed  	Guestbook-0.1.0	1.0        	Upgrade complete

kubectl get secrets -o custom-columns="NAME:.metadata.name, REVISION:.metadata.resourceVersion,LABELS:.metadata.labels"
    NAME                                    REVISION   LABELS
    sh.helm.release.v1.demo-guestbook.v1   309290      map[modifiedAt:1706999946 name:demo-guestbook owner:helm status:superseded version:1]
    sh.helm.release.v1.demo-guestbook.v2   309294      map[modifiedAt:1706999947 name:demo-guestbook owner:helm status:deployed version:2]

helm rollback demo-guestbook 1
    Rollback was a success! Happy Helming!

kubectl get secrets -o custom-columns="NAME:.metadata.name, REVISION:.metadata.resourceVersion,LABELS:.metadata.labels"
    NAME                                    REVISION   LABELS
    sh.helm.release.v1.demo-guestbook.v1   309290      map[modifiedAt:1706999946 name:demo-guestbook owner:helm status:superseded version:1]
    sh.helm.release.v1.demo-guestbook.v2   311805      map[modifiedAt:1707001854 name:demo-guestbook owner:helm status:superseded version:2]
    sh.helm.release.v1.demo-guestbook.v3   311806      map[modifiedAt:1707001854 name:demo-guestbook owner:helm status:deployed version:3]

```

## EXCERCISE 2 : Umbrella Chart
In Helm, an umbrella chart is a way to manage multiple related charts as a single entity. It allows you to package and deploy multiple Helm charts together, making it easier to manage complex applications that consist of multiple components or microservices. The umbrella chart acts as a parent chart that includes and manages the dependencies of its child charts. This allows for easier installation, upgrade, and deletion of the entire application stack.

Let's see below simple example

```txt
Guestbook$ tree
.
├── charts
│   ├── backend
│   │   ├── Chart.yaml
│   │   └── templates
│   │       └── backend.yaml
│   ├── database
│   │   ├── Chart.yaml
│   │   └── templates
│   │       └── database.yaml
│   └── frontend
│       ├── Chart.yaml
│       └── templates
│           ├── frontend-service.yaml
│           └── frontend.yaml
└── Chart.yaml

8 directories, 8 files
```

Combining all the knowledge from exercise 1 and copy pasting a generic Chart.yaml files into each folder, we should see the umbrella chart above with all of its resources like below.

```txt
helm install demo-guestbook Guestbook
    NAME: demo-guestbook
    LAST DEPLOYED: Sun Feb  4 11:25:16 2024
    NAMESPACE: default
    STATUS: deployed
    REVISION: 1
    TEST SUITE: None
    kubectl get all
    NAME                            READY   STATUS             RESTARTS   AGE
    pod/backend-7984d5b4d8-kj29h    0/1     ImagePullBackOff   0          5m13s
    pod/frontend-754d5d5bd5-m2dpq   1/1     Running            0          5m13s
    pod/mongodb-6b94dfcb58-tjhkb    1/1     Running            0          5m13s
    NAME                 TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)           AGE
    service/backend      ClusterIP   10.110.185.112   <none>        80/TCP            5m14s
    service/frontend     ClusterIP   10.97.61.48      <none>        80/TCP            5m14s
    service/kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP           6d17h
    service/mongodb      NodePort    10.106.218.160   <none>        27017:31781/TCP   5m14s
    NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/backend    0/1     1            0           5m13s
    deployment.apps/frontend   1/1     1            1           5m13s
    deployment.apps/mongodb    1/1     1            1           5m13s
    NAME                                  DESIRED   CURRENT   READY   AGE
    replicaset.apps/backend-7984d5b4d8    1         1         0       5m13s
    replicaset.apps/frontend-754d5d5bd5   1         1         1       5m13s
    replicaset.apps/mongodb-6b94dfcb58    1         1         1       5m13s
```

# SUMMARY
Helm Chart:
- A Helm chart is a package format used to define, install, and manage Kubernetes applications.
- It contains all the necessary Kubernetes manifests, templates, and configuration files required to deploy and manage an application.
- Helm charts are versioned and can be easily shared and distributed through a chart repository.
- They provide a convenient way to package and deploy complex applications with multiple components and dependencies.

Single Chart:
- A single chart is a Helm chart that represents a single application or component.
- It contains all the necessary resources and configurations specific to that application or component.
- It is typically used to deploy and manage a standalone application or microservice.

Umbrella Chart:
- An umbrella chart is a Helm chart that acts as a parent chart and contains multiple sub-charts.
- It is used to deploy and manage a collection of related applications or components.
- Each sub-chart represents a separate application or component, and they can be deployed and managed together as a group.

Comparison:

| Feature        | Single Chart           | Umbrella Chart  |
| ------------- |:-------------:| -----:|
| Scope      | Represents a single application or component | Represents a collection of related applications or components |
| Deployment      | Deploys and manages a standalone application or microservice      | Deploys and manages a group of related applications or components |
| Structure | Contains all the necessary resources and configurations specific to the application or component      | Contains multiple sub-charts, each representing a separate application or component |
| Reusability | Can be easily shared and distributed as a standalone package      | Can be used to deploy and manage multiple related applications or components together |
| Complexity | Suitable for simple applications or components      | Suitable for complex applications with multiple components and dependencies |
| Versioning | Versioned individually      | Versioned as a whole or individually for each sub-chart |
| Dependencies | Can have dependencies on other charts      | Can have dependencies on other sub-charts within the umbrella chart |