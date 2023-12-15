# Aliyun Cloud Controller Manager

aliyun-ccm is a Helm chart for Kubernetes
using [aliyun ccm](https://help.aliyun.com/zh/ack/product-overview/cloud-controller-manager).  
If u create custom k8s cluster on ur aliyun ECS server, it will be helpful connect aliyun services(such as SLB/ECI).

## Usage

### Install

```shell
export NAME=cloud-controller-manager
export NAMESPACE=kube-system
export SA_NAME=$NAME
export CM_NAME=cloud-config
export POD_CIDR=10.244.0.0/16
export ACCESS_KEY=my_key
export ACCESS_SECRET=my_secret
export IMAGE_REPOSITORY=registry.cn-hangzhou.aliyuncs.com/acs/cloud-controller-manager-amd64
export IMAGE_TAG=v2.8.1
helm repo add aliyun-ccm https://go-cinch.github.io/aliyun-ccm/
helm install $NAME aliyun-ccm/aliyun-ccm \
  -n $NAMESPACE --create-namespace \
  --set podCidr=$POD_CIDR \
  --set serviceAccount.name=$SA_NAME \
  --set permission.configmapName=$CM_NAME \
  --set permission.accessKeyID=$ACCESS_KEY \
  --set permission.accessKeySecret=$ACCESS_SECRET \
  --set image.repository=$IMAGE_REPOSITORY \
  --set image.tag=$IMAGE_TAG
```

### Show result

CCM pods only run control plane, if u want to run on other node(s), change nodeSelector or tolerations

#### show resources

```shell
# show daemonset
kubectl get ds -n $NAMESPACE $NAME

# show configmap
kubectl get cm -n $NAMESPACE $NAME

# show configmap
kubectl get sa -n $NAMESPACE $NAME

# show node labels
kubectl get node -A --show-labels
```

#### create a nginx test

[new a nginx service](https://help.aliyun.com/zh/eci/user-guide/deploy-the-ccm#section-nft-y4r-c2h), wait a moment, u
can see LoadBalancer external ip.

### Uninstall

```shell
helm uninstall -n $NAMESPACE $NAME
```

## More

[Deploy CCM](https://help.aliyun.com/zh/eci/user-guide/deploy-the-ccm)