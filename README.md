# Integrations

1. You can use any hypervisor to run these containers in the cluster. In my case, I will use Minikube.

    ```bash
        # to start the minukube
        minikube start

        # verify if the minikube is runnin
        kubectl get nodes
        # NAME       STATUS   ROLES           AGE     VERSION
        # minikube   Ready    control-plane   7d12h   v1.30.0
    ```

2. Create namespaces to group your containers within the cluste

    ```bash
        # create namespace microservice-app, we will use this to group our resources
        kubectl create namespace microservice-app

        # verify if the namespace is created
        kubectl get namespaces
        # NAME               STATUS   AGE
        # default            Active   7d12h
        # microservice-app   Active   2d10h
    ```

3. After creating the namespace, go to the secrets directory to deploy the YAML file that will be used to access the containers from the container registry

    ```bash
        # create the secret yaml file
        kubectl create -f secret-cred.yaml
        # secret/docker-creds created

        # verify if the secret yaml is successfully created, declare also the namespace where we deploy it
        kubectl get secrets -n microservice-app
        # NAME           TYPE                             DATA   AGE
        # docker-creds   kubernetes.io/dockerconfigjson   1      70s
    ```

4. Go to the ConfigMaps directory to deploy the environment variables that the web application will use.

    ```bash
        # create the configmap yaml file
        kubectl create -f configs-js-python.yaml
        # configmap/js-config created

        # verify if the configmap yaml is successfully created, declare also the namespace where we deploy it
        kubectl get configmaps -n microservice-app
        # NAME               DATA   AGE
        # js-config          4      11s
    ```

5. Proceed to the deployment directory to deploy the containers

  ```bash
        # create the 3 deployment yaml file
        kubectl create -f js-api-deployment.yaml
        kubectl create -f python-api-deployment.yaml
        kubectl create -f js-web-deployment.yaml

        # verify if the deployment yaml files is successfully created, declare also the namespace where we deploy it
        kubect get deployments -n microservice-app
        # NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
        # js-api-deployment       1/1     1            1           113s
        # js-web-deployment       1/1     1            1           97s
        # python-api-deployment   1/1     1            1           107s

        # you can also check the running pods 
        kubectl get pods -n microservice-app
        # NAME                                    READY   STATUS    RESTARTS   AGE
        # js-api-deployment-557c66f8df-sm7xd      1/1     Running   0          3m10s
        # js-web-deployment-756f4bc776-48p8v      1/1     Running   0          2m53s
        # python-api-deployment-d9b864c9f-pmrcg   1/1     Running   0          3m4s
    ```
