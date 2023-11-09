---
date: 2023-11-08
authors: [treprime]
categories:
    - kubernetes
tags:
    - microk8s
---

# Kubernetes the Easy Way with MicroK8s

**MicroK8s** is a lightweight, easy-to-install Kubernetes distribution that's designed to streamline the setup of Kubernetes clusters on your local machine. Whether you're a developer seeking to experiment with containerized applications or an operator looking to test Kubernetes configurations, MicroK8s provides a simple and efficient solution.

<!-- more -->

## Key Features of MicroK8s

**1. Lightweight and Fast**: MicroK8s lives up to its name by being incredibly lightweight and efficient. It doesn't burden your system with extensive resource requirements, ensuring your local machine runs smoothly alongside your Kubernetes cluster.

**2. Quick Installation**: Installing MicroK8s is a breeze. With a single command, you can have a fully functional Kubernetes cluster up and running in no time. This eliminates the complexities of manual Kubernetes configuration.

**3. CNCF Certified**: MicroK8s is certified by the Cloud Native Computing Foundation (CNCF), guaranteeing its compatibility with the Kubernetes ecosystem and ensuring that you're working with a conformant Kubernetes cluster.

**4. Rich Ecosystem of Add-Ons**: MicroK8s offers a variety of built-in add-ons and extensions. You can easily enhance your local cluster with capabilities like a local container registry, monitoring tools, and a web-based dashboard.

**5. Multi-Node Clusters**: MicroK8s allows you to create multi-node clusters on your local machine. This feature is invaluable for replicating real-world Kubernetes environments for testing and development purposes.

<!-- ## General Steps

Getting started with MicroK8s is a straightforward process:

1. **Installation**: Use your package manager or a provided script to install MicroK8s on your system.

2. **Add-Ons**: Enable the add-ons you require for your project using the `microk8s.enable` command.

3. **Deployment**: Deploy and manage your applications using standard Kubernetes commands. MicroK8s is fully compatible with `kubectl`, the standard Kubernetes command-line tool.

4. **Cleanup**: When you're done, you can easily clean up your local cluster using the `microk8s.reset` command. -->


## Requirements
- Ubuntu

## Instructions

### Install Software Packages


1. Install docker
    ```
    sudo apt install docker.io -y
    ```

1. Install microk8s

    ```
    sudo apt install microk8s --classic
    ```

1. Enable plugins
    ```
    sudo microk8s enable dashboard
    sudo microk8s enable dns
    sudo microk8s enable ingress
    ```

### Namespace
In Kubernetes, a namespace is a logical and virtual cluster inside a physical Kubernetes cluster. It's a way to create separate and isolated environments within the same Kubernetes cluster. Namespaces help in organizing and managing resources, such as pods, services, and replication controllers, by providing a scope for these resources.

1. Create file :material-file: **juice-shop-namespace.yml**

    ```
    apiVersion: v1
    kind: Namespace
    metadata:
    name: juice-shop
    ```

1. Create the namespace using this file

    ```
    sudo microk8s kubectl apply -f juice-shop-namespace.yml
    ```


### Pod

1. Create a file :material-file: **juice-shop-pod.yml**

    ```
    apiVersion: v1
    kind: Pod
    metadata:
        labels:
            app: juice-shop
    name: juice-shop
    namespace: juice-shop
    spec:
    containers:
    - name: juice-shop
        image: bkimminich/juice-shop
        ports:
        - containerPort: 3000
    ```

1. Create the pod using this file

    ```
    sudo microk8s kubectl apply -f juice-shop-pod.yml
    ```
### Service

1. Create a file :material-file: **juice-shop-svc.yml**

    ```
    apiVersion: v1
    kind: Service
    metadata:
    name: juice-shop-service
    namespace: juice-shop
    spec:
    selector:
        app: juice-shop
    ports:
        - protocol: TCP
        targetPort: 3000

    ```
    
1. Create the service using this file

    ```
    sudo microk8s kubectl apply -f juice-shop-svc.yml
    ```

1. In a browser navigate to [https://localhost:3000](https://localhost:3000) and you should see the Juice Shop website.

:white_check_mark: Congratulations!!!

## Conclusion

MicroK8s is a valuable tool for anyone looking to simplify Kubernetes development on their local machine. Its lightweight design, ease of installation, and compatibility with the Kubernetes ecosystem make it a standout choice for developers and operations professionals. Give MicroK8s a try and experience the convenience of Kubernetes development without the complexity.

---

## References

- [MicroK8s official website](https://microk8s.io/)
