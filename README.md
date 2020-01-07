

# Install Kubernetes Cluster with Ansible
![Install Kubernetes Cluster with ansible](https://user-images.githubusercontent.com/3519706/70437658-27508f00-1a9d-11ea-9815-df22c78c2568.jpg)

**Requirements**
-  Ansible (≥ 2.8.5)
-  CentOS/RHEL 7
- *CoreOS **(In future release)**
- ISO Repo

We made kubernetes cluster installation automation with ansible.
**Purpose:** Provide end-to-end installation with all components 

![kubernetes cluster addons](https://user-images.githubusercontent.com/3519706/70437685-35061480-1a9d-11ea-9ae5-45d81e55e900.png)

Addons that can be installed with automation are as follows:

**Note:** Addons can be upgraded with the appopriate links or file under **vars/main.yaml**

Network
 -  [Calico   ](https://www.projectcalico.org/)(layer 3)
 -  [Flannel ](https://coreos.com/flannel/docs/latest/kubernetes.html) (layer 2)

Serverless    
 -  [Knative](https://knative.dev/docs/install/knative-with-any-k8s/)
 -  [Kubeless](https://kubeless.io/docs/quick-start/)

Storage

 - [Ceph ](https://rook.io/docs/rook/v1.1/ceph-examples.html)
 
 Other
 - [Autocompletion ](http://www.vadmin-land.com/2018/11/enabling-kubectl-autocompletion/)
 - [Kubens ](https://github.com/ahmetb/kubectx) 
 -  [Metric Server](https://github.com/kubernetes-sigs/metrics-server)
 -  [Istio](https://istio.io/docs/setup/getting-started/#install)
 -  [Helm](https://helm.sh/docs/intro/install/)
 -  [Cluster Dashboard](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)
 -  [Cluster Logging(fluentd)](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-elasticsearch-fluentd-and-kibana-efk-logging-stack-on-kubernetes)   
 -  [Cluster Monitoring](https://github.com/coreos/kube-prometheus)
 -  [Weave  ](https://www.weave.works/docs/net/latest/kubernetes/kube-addon/)
 -  [Ingress Controller](https://github.com/nginxinc/kubernetes-ingress)

**Hardware Requirements**

- [Cluster Logging(fluentd)](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-elasticsearch-fluentd-and-kibana-efk-logging-stack-on-kubernetes)   addon needs  **≥32GB** of Ram.
- To enable [Ceph](https://rook.io/docs/rook/v1.1/ceph-examples.html), **three worker nodes** should be present and worker nodes should have **≥10GB** disk space. (Extra disk should be added to use Ceph. However do not mount that disk. Otherwise Ceph will not work)

**Before Run Ansible**
```
yum install -y python-pip
pip install jmespath
```

**Run Playbook**

```
ansible-playbook playbook.yml -i inventory.yml
```

**`kubens`** helps you switch between Kubernetes namespaces smoothly

you need to define ingress for all components we install

> kiali-ingress.yaml example:

    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: kiali-ingress
      namespace: istio-system
    spec:
      rules:
      - host: kiali-10-10-10-11(worker_ip_addres).nip.io
        http:
          paths:
            - path: /
              backend:
                serviceName: kiali
                servicePort: 20001
