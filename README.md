# Despliega un cluster kubernetes en 5 minutos con k3s

## Objetivos

* Repasar los conceptos sobre contenedores
* Repasar los conceptos sobre kubernetes
* Conoce las características del instalador k3s
* Desplegar un cluster kubernetes con k3s
* Probar el funcionamiento del cluster kubernetes

## Nivel

Intermedio

## ¿Qué aprenderás en el taller?

* Repasar conceptos sobre contenedores y kubernetes
* Desplegar un cluster multinodo de kubernetes con k3s
* Configurar el acceso al cluster desde un cliente
* Probar la funcionalidad del cluster que hemos creado

## Conocimientos previos

* Conocer los conceptos básicos de alguna tecnología de contenedores (por ejemplo docker)
* Conocer los recursos que nos ofrece un cluster e kubernetes
* Conocimientos básicos sobre linux, que será el sistema operativo usado en este taller

## Desarrollo

    su
    cd /usr/local/bin
    wget https://github.com/rancher/k3s/releases/download/v0.2.0/k3s
    chmod +x k3s


    k3s server &

    k3s kubectl get nodes
    NAME      STATUS   ROLES    AGE   VERSION
    stretch   Ready    <none>   47s   v1.13.4-k3s.1

    cat /var/lib/rancher/k3s/server/node-token

    En los otros dos nodos como su:

    k3s agent --server https://192.168.33.10:6443 --token ${NODE_TOKEN}

Instalo kubectl

    apt-get update && apt-get install -y apt-transport-https curl git
    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
    echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | tee -a /etc/apt/sources.list.d/kubernetes.list
    apt-get update
    apt-get install -y kubectl

Configuración de un cliente externo. En el master:

    scp jose@xxxxx:/etc/rancher/k3s/k3s.yaml ~/.kube/config

Y la copio en el cliente y le cambio la ip

    ...
    server: https://192.168.33.10:6443
    ...

    export KUBECONFIG=~/.kube/config 
    vagrant@cliente:~$ kubectl get nodes
    NAME       STATUS   ROLES    AGE   VERSION
    maquina1   Ready    <none>   37m   v1.13.4-k3s.1
    maquina2   Ready    <none>   36m   v1.13.4-k3s.1
    maquina3   Ready    <none>   34m   v1.13.4-k3s.1

    kubectl create deploy nginx --image=nginx
    kubectl expose deploy nginx --port=80 --type=NodePort
    kubectl scale --replicas=3 deploy/nginx
    kubectl delete deploy/nginx
    kubectl delete service/nginx
    
    cd /vagrant/letschat
    

