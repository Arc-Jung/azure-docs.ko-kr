---
title: Azure Kubernetes Service에서 Azure AD 사용
description: AKS(Azure Kubernetes Service)에서 Azure AD를 사용하는 방법 알아보기
services: container-service
ms.topic: article
ms.date: 08/26/2020
ms.author: thomasge
ms.openlocfilehash: 534c355961bb87a816f5ba50a3cc2d397e544a15
ms.sourcegitcommit: dd24c3f35e286c5b7f6c3467a256ff85343826ad
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99072311"
---
# <a name="aks-managed-azure-active-directory-integration"></a>AKS 관리 Azure Active Directory 통합

AKS로 관리 되는 Azure ad 통합은 사용자가 이전에 클라이언트 앱, 서버 앱을 만들고 디렉터리 읽기 권한을 부여 하는 데 필요한 azure ad 통합 환경을 간소화 하도록 설계 되었습니다. 새 버전에서는 AKS 리소스 공급자가 클라이언트 및 서버 앱을 자동으로 관리합니다.

## <a name="azure-ad-authentication-overview"></a>Azure AD 인증 개요

클러스터 관리자는 사용자의 id 또는 디렉터리 그룹 구성원 자격에 따라 Kubernetes Kubernetes RBAC (역할 기반 액세스 제어)를 구성할 수 있습니다. OpenID Connect와 함께 AKS 클러스터에 Azure AD 인증이 제공됩니다. OpenID Connect는 OAuth 2.0 프로토콜을 기반으로 하는 ID 계층입니다. OpenID Connect에 대한 자세한 내용은 [Open ID 연결 설명서][open-id-connect]를 참조하세요.

[Azure Active Directory 통합 개념 설명서](concepts-identity.md#azure-active-directory-integration)의 Azure AD 통합 흐름에 대해 자세히 알아보세요.

## <a name="limitations"></a>제한 사항 

* AKS에서 관리 되는 Azure AD 통합을 사용 하지 않도록 설정할 수 없음
* AKS로 관리 되는 Azure AD 통합에 대 한 Kubernetes RBAC 사용 클러스터가 지원 되지 않음
* AKS로 관리 되는 Azure AD 통합에 연결 된 Azure AD 테 넌 트 변경은 지원 되지 않음

## <a name="prerequisites"></a>전제 조건

* Azure CLI 버전 2.11.0 이상
* Kubectl [1.18.1](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.18.md#v1181) 또는 [kubelogin](https://github.com/Azure/kubelogin) 의 최소 버전
* [투구](https://github.com/helm/helm)를 사용 하는 경우 최소 버전의 투구 3.3입니다.

> [!Important]
> 최소 버전의 1.18.1 또는 kubelogin와 함께 Kubectl를 사용 해야 합니다. 올바른 버전을 사용 하지 않는 경우 인증 문제를 확인할 수 있습니다.

Kubectl 및 kubelogin을 설치 하려면 다음 명령을 사용 합니다.

```azurecli-interactive
sudo az aks install-cli
kubectl version --client
kubelogin --version
```

다른 운영 체제에 대한 [지침](https://kubernetes.io/docs/tasks/tools/install-kubectl/)을 사용합니다.

## <a name="before-you-begin"></a>시작하기 전에

클러스터의 경우 Azure AD 그룹이 필요 합니다. 이 그룹은 클러스터 관리자 권한을 부여 하기 위해 클러스터에 대 한 관리 그룹으로 필요 합니다. 기존 Azure AD 그룹을 사용 하거나 새 그룹을 만들 수 있습니다. Azure AD 그룹의 개체 ID를 기록 합니다.

```azurecli-interactive
# List existing groups in the directory
az ad group list --filter "displayname eq '<group-name>'" -o table
```

클러스터 관리자에 대 한 새 Azure AD 그룹을 만들려면 다음 명령을 사용 합니다.

```azurecli-interactive
# Create an Azure AD group
az ad group create --display-name myAKSAdminGroup --mail-nickname myAKSAdminGroup
```

## <a name="create-an-aks-cluster-with-azure-ad-enabled"></a>Azure AD를 사용하는 AKS 클러스터 만들기

다음 CLI 명령을 사용 하 여 AKS 클러스터를 만듭니다.

Azure 리소스 그룹을 만듭니다.

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location centralus
```

AKS 클러스터를 만들고 Azure AD 그룹에 대 한 관리 액세스를 사용 하도록 설정 합니다.

```azurecli-interactive
# Create an AKS-managed Azure AD cluster
az aks create -g myResourceGroup -n myManagedCluster --enable-aad --aad-admin-group-object-ids <id> [--aad-tenant-id <id>]
```

AKS로 관리 되는 Azure AD 클러스터가 성공적으로 만들어지면 응답 본문에 다음 섹션이 포함 됩니다.
```output
"AADProfile": {
    "adminGroupObjectIds": [
      "5d24****-****-****-****-****afa27aed"
    ],
    "clientAppId": null,
    "managed": true,
    "serverAppId": null,
    "serverAppSecret": null,
    "tenantId": "72f9****-****-****-****-****d011db47"
  }
```

클러스터를 만든 후에는 해당 클러스터에 대 한 액세스를 시작할 수 있습니다.

## <a name="access-an-azure-ad-enabled-cluster"></a>Azure AD를 사용하는 클러스터에 액세스

다음 단계를 수행 하려면 [Azure Kubernetes Service 클러스터 사용자](../role-based-access-control/built-in-roles.md#azure-kubernetes-service-cluster-user-role) 기본 제공 역할이 필요 합니다.

클러스터에 액세스 하기 위한 사용자 자격 증명을 가져옵니다.
 
```azurecli-interactive
 az aks get-credentials --resource-group myResourceGroup --name myManagedCluster
```
지침에 따라 로그인합니다.

Kubectl get nodes 명령을 사용 하 여 클러스터의 노드를 봅니다.

```azurecli-interactive
kubectl get nodes

NAME                       STATUS   ROLES   AGE    VERSION
aks-nodepool1-15306047-0   Ready    agent   102m   v1.15.10
aks-nodepool1-15306047-1   Ready    agent   102m   v1.15.10
aks-nodepool1-15306047-2   Ready    agent   102m   v1.15.10
```
Azure [RBAC (역할 기반 액세스 제어)](./azure-ad-rbac.md) 를 구성 하 여 클러스터에 대 한 추가 보안 그룹을 구성 합니다.

## <a name="troubleshooting-access-issues-with-azure-ad"></a>Azure AD의 액세스 문제 해결

> [!Important]
> 아래에 설명 된 단계는 일반 Azure AD 그룹 인증을 무시 합니다. 응급 상황 에서만 사용 합니다.

클러스터에 대 한 액세스 권한이 있는 유효한 Azure AD 그룹에 액세스 하지 않아 영구적으로 차단 된 경우에도 관리자 자격 증명을 가져와 클러스터에 직접 액세스할 수 있습니다.

이러한 단계를 수행 하려면 [Azure Kubernetes Service 클러스터 관리자](../role-based-access-control/built-in-roles.md#azure-kubernetes-service-cluster-admin-role) 기본 제공 역할에 대 한 액세스 권한이 있어야 합니다.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myManagedCluster --admin
```

## <a name="enable-aks-managed-azure-ad-integration-on-your-existing-cluster"></a>기존 클러스터에서 AKS로 관리 되는 Azure AD 통합 사용

기존 Kubernetes RBAC 사용 클러스터에서 AKS로 관리 되는 Azure AD 통합을 사용 하도록 설정할 수 있습니다. 클러스터에 대 한 액세스를 유지 하려면 관리자 그룹을 설정 해야 합니다.

```azurecli-interactive
az aks update -g MyResourceGroup -n MyManagedCluster --enable-aad --aad-admin-group-object-ids <id-1> [--aad-tenant-id <id>]
```

AKS로 관리 되는 Azure AD 클러스터가 성공적으로 활성화 되 면 응답 본문에 다음 섹션이 포함 됩니다.

```output
"AADProfile": {
    "adminGroupObjectIds": [
      "5d24****-****-****-****-****afa27aed"
    ],
    "clientAppId": null,
    "managed": true,
    "serverAppId": null,
    "serverAppSecret": null,
    "tenantId": "72f9****-****-****-****-****d011db47"
  }
```

[여기에 나와][access-cluster]있는 단계를 수행 하 여 사용자 자격 증명을 다시 다운로드 하 여 클러스터에 액세스 합니다.

## <a name="upgrading-to-aks-managed-azure-ad-integration"></a>AKS로 관리 되는 Azure AD 통합으로 업그레이드

클러스터에서 레거시 Azure AD 통합을 사용 하는 경우 AKS로 관리 되는 Azure AD 통합으로 업그레이드할 수 있습니다.

```azurecli-interactive
az aks update -g myResourceGroup -n myManagedCluster --enable-aad --aad-admin-group-object-ids <id> [--aad-tenant-id <id>]
```

AKS로 관리 되는 Azure AD 클러스터의 성공적인 마이그레이션에는 응답 본문에 다음과 같은 섹션이 있습니다.

```output
"AADProfile": {
    "adminGroupObjectIds": [
      "5d24****-****-****-****-****afa27aed"
    ],
    "clientAppId": null,
    "managed": true,
    "serverAppId": null,
    "serverAppSecret": null,
    "tenantId": "72f9****-****-****-****-****d011db47"
  }
```

클러스터에 액세스 하려면 [다음][access-cluster]단계를 수행 합니다.

## <a name="non-interactive-sign-in-with-kubelogin"></a>Kubelogin를 사용 하 여 비 대화형 로그인

현재 kubectl에서 사용할 수 없는 지속적인 통합 파이프라인과 같은 일부 비 대화형 시나리오가 있습니다. [`kubelogin`](https://github.com/Azure/kubelogin)를 사용 하 여 비 대화형 서비스 사용자 로그인으로 클러스터에 액세스할 수 있습니다.

## <a name="use-conditional-access-with-azure-ad-and-aks"></a>Azure AD 및 AKS를 사용 하 여 조건부 액세스 사용

AKS 클러스터와 Azure AD를 통합 하는 경우 [조건부 액세스][aad-conditional-access] 를 사용 하 여 클러스터에 대 한 액세스를 제어할 수도 있습니다.

> [!NOTE]
> Azure AD 조건부 액세스는 Azure AD Premium 기능입니다.

AKS와 함께 사용할 예제 조건부 액세스 정책을 만들려면 다음 단계를 완료 합니다.

1. Azure Portal 맨 위에서 Azure Active Directory를 검색 하 고 선택 합니다.
1. 왼쪽의 Azure Active Directory에 대 한 메뉴에서 *엔터프라이즈 응용 프로그램* 을 선택 합니다.
1. 왼쪽의 엔터프라이즈 응용 프로그램에 대 한 메뉴에서 *조건부 액세스* 를 선택 합니다.
1. 왼쪽의 조건부 액세스에 대 한 메뉴에서 *정책* , *새 정책* 을 차례로 선택 합니다.
    :::image type="content" source="./media/managed-aad/conditional-access-new-policy.png" alt-text="조건부 액세스 정책 추가":::
1. 정책 이름 (예: *aks)* 을 입력 합니다.
1. *사용자 및 그룹* 을 선택 하 고 *포함* 아래에서 *사용자 및 그룹 선택* 을 선택 합니다. 정책을 적용할 사용자 및 그룹을 선택 합니다. 이 예에서는 클러스터에 대 한 관리 액세스 권한이 있는 동일한 Azure AD 그룹을 선택 합니다.
    :::image type="content" source="./media/managed-aad/conditional-access-users-groups.png" alt-text="조건부 액세스 정책을 적용할 사용자 또는 그룹을 선택 합니다.":::
1. *클라우드 앱 또는 작업* 을 선택 하 고 *포함* 아래에서 *앱 선택* 을 선택 합니다. *Azure Kubernetes service* 를 검색 하 고 *AZURE Kubernetes service AAD 서버* 를 선택 합니다.
    :::image type="content" source="./media/managed-aad/conditional-access-apps.png" alt-text="조건부 액세스 정책을 적용 하기 위해 Azure Kubernetes Service AD 서버 선택":::
1. *액세스 제어* 에서 *권한 부여* 를 선택합니다. *액세스 허용* 을 선택 *하 고 장치를 규격으로 표시* 해야 합니다 .를 선택 합니다.
    :::image type="content" source="./media/managed-aad/conditional-access-grant-compliant.png" alt-text="조건부 액세스 정책에 대해 규격 장치만 허용 하도록 선택":::
1. *정책 사용* 아래에서 *설정* 을 선택 하 고 *만들기* 를 선택 합니다.
    :::image type="content" source="./media/managed-aad/conditional-access-enable-policy.png" alt-text="조건부 액세스 정책 사용":::

클러스터에 액세스 하기 위한 사용자 자격 증명을 가져옵니다. 예를 들면 다음과 같습니다.

```azurecli-interactive
 az aks get-credentials --resource-group myResourceGroup --name myManagedCluster
```

지침에 따라 로그인합니다.

명령을 사용 `kubectl get nodes` 하 여 클러스터의 노드를 봅니다.

```azurecli-interactive
kubectl get nodes
```

지침에 따라 다시 로그인 합니다. 로그인에 성공 했음을 나타내는 오류 메시지가 표시 되지만 관리자에 게 리소스에 액세스 하기 위해 Azure AD에서 관리 하는 액세스 권한을 요청 하는 장치가 필요 합니다.

Azure Portal에서 Azure Active Directory로 이동 하 여 *엔터프라이즈 응용 프로그램* 을 선택한 다음 *작업* 에서 *로그인* 을 선택 합니다. 위쪽의 항목에 *실패* *상태* 와 *조건부 액세스* *성공* 이 표시 됩니다. 항목을 선택 하 고 *세부 정보* 에서 *조건부 액세스* 를 선택 합니다. 조건부 액세스 정책이 나열 되어 있는지 확인 합니다.

:::image type="content" source="./media/managed-aad/conditional-access-sign-in-activity.png" alt-text="조건부 액세스 정책으로 인 한 로그인 항목 실패":::

## <a name="next-steps"></a>다음 단계

* [Kubernetes 권한 부여에 대 한 AZURE RBAC 통합][azure-rbac-integration] 에 대해 알아보기
* [KUBERNETES RBAC와 AZURE AD 통합][azure-ad-rbac]에 대해 알아봅니다.
* [Kubelogin](https://github.com/Azure/kubelogin) 를 사용 하 여 kubectl에서 사용할 수 없는 Azure 인증 기능에 액세스 합니다.
* [AKS 및 Kubernetes id 개념][aks-concepts-identity]에 대해 자세히 알아보세요.
* [ARM (Azure Resource Manager) 템플릿을][aks-arm-template] 사용 하 여 AKS로 관리 되는 Azure AD 사용 클러스터를 만듭니다.

<!-- LINKS - external -->
[kubernetes-webhook]:https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[aks-arm-template]: /azure/templates/microsoft.containerservice/managedclusters

<!-- LINKS - Internal -->
[aad-conditional-access]: ../active-directory/conditional-access/overview.md
[azure-rbac-integration]: manage-azure-rbac.md
[aks-concepts-identity]: concepts-identity.md
[azure-ad-rbac]: azure-ad-rbac.md
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-group-create]: /cli/azure/group#az-group-create
[open-id-connect]:../active-directory/develop/v2-protocols-oidc.md
[az-ad-user-show]: /cli/azure/ad/user#az-ad-user-show
[rbac-authorization]: concepts-identity.md#role-based-access-controls-rbac
[operator-best-practices-identity]: operator-best-practices-identity.md
[azure-ad-rbac]: azure-ad-rbac.md
[azure-ad-cli]: azure-ad-integration-cli.md
[access-cluster]: #access-an-azure-ad-enabled-cluster
[aad-migrate]: #upgrading-to-aks-managed-azure-ad-integration
