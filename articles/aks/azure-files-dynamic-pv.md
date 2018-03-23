---
title: AKS에서 Azure File 사용
description: AKS에서 Azure 디스크 사용
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/06/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: a5126bc4c5e7c9cd9832f33fc908e6c8b9e02b91
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/16/2018
---
# <a name="persistent-volumes-with-azure-files"></a>Azure Files가 포함된 영구적 볼륨

영구적 볼륨은 Kubernetes 클러스터에서 사용하도록 프로비전된 저장소 부분을 나타냅니다. 하나 이상의 Pod에서 영구적 볼륨을 사용할 수 있으며 동적 또는 정적으로 프로비전할 수 있습니다. 이 문서에서는 Azure 파일 공유를 AKS 클러스터에 있는 Kubernetes 영구적 볼륨으로 동적으로 프로비전하는 과정을 자세히 설명합니다. 

Kubernetes 영구적 볼륨에 대한 자세한 내용은 [Kubernetes 영구적 볼륨][kubernetes-volumes]을 참조하세요.

## <a name="create-storage-account"></a>저장소 계정 만들기

Azure 파일 공유를 Kubernetes 볼륨으로 동적으로 프로비전하는 경우 AKS 클러스터와 동일한 리소스 그룹에 포함된 모든 저장소 계정을 사용할 수 있습니다. 필요한 경우 AKS 클러스터와 동일한 리소스 그룹에 저장소 계정을 만듭니다. 

적절한 리소스 그룹을 식별하려면 [az group list][az-group-list] 명령을 사용합니다.

```azurecli-interactive
az group list --output table
```

`MC_clustername_clustername_locaton`과 비슷한 이름의 리소스 그룹을 찾습니다. 여기서 clustername은 AKS 클러스터의 이름이고, location은 클러스터가 배포된 Azure 지역입니다.

```
Name                                 Location    Status
-----------------------------------  ----------  ---------
MC_myAKSCluster_myAKSCluster_eastus  eastus      Succeeded
myAKSCluster                         eastus      Succeeded
```

[az storage account create][az-storage-account-create] 명령을 사용하여 저장소 계정을 만듭니다. 

이 예제를 사용하고 `--resource-group`을 리소스 그룹의 이름으로, `--name`을 사용자가 선택한 이름으로 업데이트합니다.

```azurecli-interactive
az storage account create --resource-group MC_myAKSCluster_myAKSCluster_eastus --name mystorageaccount --location eastus --sku Standard_LRS
```

## <a name="create-storage-class"></a>저장소 클래스 만들기

저장소 클래스는 Azure 파일 공유를 만드는 방법을 정의하는 데 사용됩니다. 클래스에 특정 저장소 계정을 지정할 수 있습니다. 저장소 계정을 지정하지 않으면 `skuName` 및 `location`을 지정해야 하고, 연결된 리소스 그룹의 모든 저장소 계정이 일치하는지 평가됩니다.

Azure 파일의 Kubernetes 저장소 클래스에 대한 자세한 내용은 [Kubernetes 저장소 클래스][kubernetes-storage-classes]를 참조하세요.

파일 `azure-file-sc.yaml`을 만들고 다음 매니페스트에 복사합니다. `storageAccount`를 대상 저장소 계정의 이름으로 업데이트합니다.

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
parameters:
  storageAccount: mystorageaccount
```

[kubectl create][kubectl-create] 명령을 사용하여 저장소 클래스를 만듭니다.

```azurecli-interactive
kubectl create -f azure-file-sc.yaml
```

## <a name="create-persistent-volume-claim"></a>영구적 볼륨 클레임 만들기

PVC(영구적 볼륨 클레임)는 저장소 클래스 개체를 사용하여 Azure 파일 공유를 동적으로 프로비전합니다. 

다음 매니페스트를 사용하여 `ReadWriteOnce` 액세스 권한으로 크기가 `5GB`인 영구적 볼륨 클레임을 만들 수 있습니다.

파일 `azure-file-pvc.yaml`을 만들고 다음 매니페스트에 복사합니다. `storageClassName`이 마지막 단계에서 만든 저장소 클래스와 일치하는지 확인합니다.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azurefile
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: azurefile
  resources:
    requests:
      storage: 5Gi
```

[kubectl create][kubectl-create] 명령을 사용하여 영구 볼륨 클레임을 만듭니다.

```azurecli-interactive
kubectl create -f azure-file-pvc.yaml
```

완료되면 파일 공유가 생성됩니다. 연결 정보 및 자격 증명을 포함하는 Kubernetes 암호도 생성됩니다.

## <a name="using-the-persistent-volume"></a>영구적 볼륨 사용

다음 매니페스트는 영구적 볼륨 클레임 `azurefile`을 사용하여 `/mnt/azure` 경로에 Azure 파일 공유를 탑재하는 Pod를 만듭니다.

파일 `azure-pvc-files.yaml`을 만들고 다음 매니페스트에 복사합니다. `claimName`이 마지막 단계에서 만든 PVC와 일치하는지 확인합니다.

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/mnt/azure"
        name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azurefile
```

[kubectl create][kubectl-create] 명령을 사용하여 Pod를 만듭니다.

```azurecli-interactive
kubectl create -f azure-pvc-files.yaml
```

이제 Azure 디스크가 `/mnt/azure` 디렉터리에 탑재된 Pod가 실행되고 있습니다. `kubectl describe pod mypod`를 통해 Pod를 검사하여 볼륨 탑재를 확인할 수 있습니다.

## <a name="next-steps"></a>다음 단계

Azure Files를 사용하는 Kubernetes 영구적 볼륨에 대해 자세히 알아봅니다.

> [!div class="nextstepaction"]
> [Azure Files용 Kubernetes 플러그 인][kubernetes-files]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#create
[kubectl-describe]: https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_describe/
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-security-context]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/#azure-file
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az_group_create
[az-group-list]: /cli/azure/group#az_group_list
[az-storage-account-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-key-list]: /cli/azure/storage/account/keys#az_storage_account_keys_list
[az-storage-share-create]: /cli/azure/storage/share#az_storage_share_create
