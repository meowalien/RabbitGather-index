# RabbitGetter

Welcome to RabbitGetter! This is a side project created by me, and I will update it frequently with interesting content.

This project serves as an index for all my projects, blogs, and other related stuff.

You can find all the projects below:

- [HomePage](https://github.com/meowalien/homepage) - My resume and the entry point for all projects.
- [HomapageI18n](https://github.com/meowalien/homapage-i18n) - Provides all the multi-language resources needed for all
  projects.
- [HomePageClusterConfig](https://github.com/meowalien/homepage-cluster-config) - The ArgoCD configuration for the
  cluster that runs everything.
- [PriceReporter](https://github.com/meowalien/price-reporter) - A price reporter that sends the price of a stock to a
  RabbitMQ queue.
- [AccountOperator](https://github.com/meowalien/account-operator) - The account operator that manages all the accounts.
- [HomepageAuthorization](https://github.com/meowalien/homepage-authorization) - The authorization service for all
  projects.

## How to run this project locally (MacOS)

1. Install needed tools

   Fallow the instructions on the official website:

   orbstack(Docker): https://docs.orbstack.dev/quick-start

   minikube: https://minikube.sigs.k8s.io/docs/start/?arch=/macos/arm64/stable/homebrew

   kubectl: https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/
   
   kubevpn: https://github.com/kubenetworks/kubevpn
    ```shell
    # you can skip this step if you have already installed docker
    brew install orbstack
    brew install kubectl
    brew install minikube
    brew install kubevpn
    brew install argocd
    ```
2. Start minikube
    ```shell
    minikube start
    ```
3. Make a directory to store all the projects
    ```shell
    mkdir -p ./RabbitGather
    cd ./RabbitGather
    git clone -b minikube https://github.com/meowalien/homepage-cluster-config.git
    git clone -b minikube https://github.com/meowalien/homapage-i18n.git
    git clone -b minikube https://github.com/meowalien/homepage.git
    git clone -b main https://github.com/meowalien/homepage-authorization.git
    ```
4. Start a cluster and make a vpn tunnel into the cluster
    >This step may require you to input your password, you may see a prompt asking for your password like this:
    ![img.png](img.png)
    ```shell
    cd ./homepage-cluster-config
   ./start.sh
    ```
5. Deploy needed dependencies
    ```shell
    ./deploy_dependencies.sh
    ```
6. Build and push the images to the local registry in minikube
    ```shell
    cd ../homapage-i18n
    ./ci.sh
    cd ../homepage
   ./ci.sh
    ```
7. Deploy the projects
    ```shell
    cd ../homepage-cluster-config
    ./deploy_applications.sh
    ```
8. Visit the homepage
    Go to `https://homepage-service.homepage.svc.cluster.local/` in your browser, you should see the homepage.
