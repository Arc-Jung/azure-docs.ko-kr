---
title: 컨테이너 라이브 데이터 (미리 보기)에 대 한 Azure Monitor 설정 | Microsoft Docs
description: 이 문서에서는 컨테이너에 대해 Azure Monitor와 함께 kubectl를 사용 하지 않고 컨테이너 로그 (stdout/stderr) 및 이벤트에 대 한 실시간 보기를 설정 하는 방법을 설명 합니다.
ms.topic: conceptual
ms.date: 01/08/2020
ms.custom: references_regions
ms.openlocfilehash: 3c176b2db659577d585ac077eebe0484203eb9cf
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100614319"
---
# <a name="how-to-set-up-the-live-data-preview-feature"></a>라이브 데이터 (미리 보기) 기능을 설정 하는 방법

AKS (Azure Kubernetes Service) 클러스터의 컨테이너에 대 한 Azure Monitor를 사용 하 여 라이브 데이터 (미리 보기)를 보려면 Kubernetes 데이터에 대 한 액세스 권한을 부여 하도록 인증을 구성 해야 합니다. 이 보안 구성을 통해 Azure Portal에서 직접 Kubernetes API를 통해 데이터에 실시간으로 액세스할 수 있습니다.

이 기능은 로그, 이벤트 및 메트릭에 대 한 액세스를 제어 하는 다음 메서드를 지원 합니다.

- Kubernetes RBAC 권한 부여를 사용하도록 설정되지 않은 AKS
- Kubernetes RBAC 권한 부여를 사용하도록 설정된 AKS
    - 클러스터 역할 바인딩 ClusterMonitoringUser를 사용 하 여 구성 된 AKS **[](/rest/api/aks/managedclusters/listclustermonitoringusercredentials)**
- AD (Azure Active Directory) SAML 기반 single sign-on을 사용 하는 AKS

이러한 지침을 사용 하려면 Kubernetes 클러스터에 대 한 관리 권한이 필요 하 고, 사용자 인증에 AD (Azure Active Directory)를 사용 하도록 구성 하는 경우 Azure AD에 대 한 관리 액세스 권한이 필요 합니다.

이 문서에서는 클러스터에서 라이브 데이터 (미리 보기) 기능에 대 한 액세스를 제어 하도록 인증을 구성 하는 방법을 설명 합니다.

- Kubernetes Kubernetes RBAC (역할 기반 액세스 제어) 사용 AKS 클러스터
- Azure Active Directory integrated AKS cluster.


## <a name="authentication-model"></a>인증 모델

라이브 데이터 (미리 보기) 기능은 명령줄 도구와 동일한 Kubernetes API를 활용 합니다 `kubectl` . Kubernetes API 끝점은 브라우저에서 유효성을 검사할 수 없는 자체 서명 된 인증서를 활용 합니다. 이 기능은 내부 프록시를 사용 하 여 AKS 서비스에 인증서의 유효성을 검사 하 고 트래픽을 신뢰할 수 있도록 합니다.

Azure Portal은 Azure Active Directory 클러스터에 대 한 로그인 자격 증명의 유효성을 검사 하 라는 메시지를 표시 하 고 클러스터를 만드는 동안 클라이언트 등록 설정으로 리디렉션하고이 문서에서 다시 구성 됩니다. 이 동작은에서 요구 하는 인증 프로세스와 유사 `kubectl` 합니다.

>[!NOTE]
>클러스터에 대 한 권한 부여는 Kubernetes 및 구성 된 보안 모델을 통해 관리 됩니다. 이 기능에 액세스 하는 사용자는 실행과 비슷하게 Kubernetes 구성 (*kubeconfig*)을 다운로드할 수 있는 권한이 필요 `az aks get-credentials -n {your cluster name} -g {your resource group}` 합니다. 이 구성 파일에는 azure **Kubernetes Service 클러스터 사용자 역할** 에 대 한 권한 부여 및 인증 토큰이 포함 되어 있으며, azure rbac 사용 및 AKS 클러스터의 경우 Kubernetes rbac 권한 부여를 사용 하도록 설정 되어 있지 않습니다. AKS가 AD (Azure Active Directory) SAML 기반 single sign-on을 사용 하도록 설정 된 경우 Azure AD 및 클라이언트 등록 세부 정보에 대 한 정보가 포함 되어 있습니다.

>[!IMPORTANT]
>이 기능의 사용자는를 다운로드 하 고이 기능을 사용 하기 위해 클러스터에 대 한 [Azure Kubernetes 클러스터 사용자 역할이](../../role-based-access-control/built-in-roles.md) 필요 합니다 `kubeconfig` . 사용자는이 기능을 활용 하기 위해 클러스터에 대 한 참가자 액세스 권한이 필요 **하지 않습니다** .

## <a name="using-clustermonitoringuser-with-kubernetes-rbac-enabled-clusters"></a>Kubernetes RBAC 사용 클러스터와 함께 clusterMonitoringUser 사용

[KUBERNETES RBAC](#configure-kubernetes-rbac-authorization) 권한 부여를 사용 하도록 설정한 후 Kubernetes 사용자 역할을 사용 하 여 라이브 데이터 (미리 보기) 기능에 대 한 **clusteruser** 액세스를 허용 하는 추가 구성 변경을 적용할 필요가 없도록 하려면 AKS가 **clusterMonitoringUser** 라는 새 Kubernetes 클러스터 역할 바인딩을 추가 했습니다. 이 클러스터 역할 바인딩에는 Kubernetes API 및 라이브 데이터 (미리 보기) 기능을 활용 하기 위한 끝점에 액세스 하는 데 필요한 모든 권한이 있습니다.

이 새 사용자와 함께 라이브 데이터 (미리 보기) 기능을 사용 하려면 AKS 클러스터 리소스에 대 한 [Azure Kubernetes Service 클러스터 사용자](../../role-based-access-control/built-in-roles.md#azure-kubernetes-service-cluster-user-role) 또는 [참가자](../../role-based-access-control/built-in-roles.md#contributor) 역할의 구성원 이어야 합니다. 컨테이너의 Azure Monitor 사용 하도록 설정 하면 기본적으로 clusterMonitoringUser를 사용 하 여 인증 하도록 구성 됩니다. ClusterMonitoringUser 역할 바인딩이 클러스터에 존재 하지 않는 경우 대신 **Clusteruser** 가 인증에 사용 됩니다. 참가자는 clusterMonitoringUser (있는 경우)에 대 한 액세스를 제공 하 고 Azure Kuberenetes Service 클러스터 사용자는 clusterUser에 대 한 액세스를 제공 합니다. 이러한 두 역할은이 기능을 사용할 수 있는 충분 한 액세스 권한을 제공 합니다.

AKS는 1 월 2020 일에이 새 역할 바인딩을 릴리스 했으므로 1 월 2020 일 이전에 만든 클러스터에는 포함 되지 않습니다. 2020 년 1 월 이전에 만든 클러스터가 있는 경우 클러스터에서 PUT 작업을 수행 하거나 클러스터에서 클러스터에 대 한 PUT 작업을 수행 하는 클러스터에서 다른 작업을 수행 하 여 새 **clusterMonitoringUser** 를 기존 클러스터에 추가할 수 있습니다. 예를 들어 클러스터 버전을 업데이트 하는 등의 작업을 수행할 수 있습니다.

## <a name="kubernetes-cluster-without-kubernetes-rbac-enabled"></a>Kubernetes RBAC를 사용 하지 않는 Kubernetes 클러스터

Kubernetes RBAC 인증으로 구성되지 않았거나 Azure AD Single Sign-On과 통합된 Kubernetes 클러스터가 있는 경우 다음 단계를 수행할 필요가 없습니다. 이는 기본적으로 RBAC가 아닌 구성에서 관리 권한을 갖고 있기 때문입니다.

## <a name="configure-kubernetes-rbac-authorization"></a>Kubernetes RBAC 권한 부여 구성

Kubernetes RBAC 권한 부여를 사용 하도록 설정 하면 두 명의 사용자 ( **Clusteruser** 및 **Clusteruser** )가 Kubernetes API에 액세스 하는 데 사용 됩니다. 이는 관리 옵션 없이 실행 하는 것과 유사 `az aks get-credentials -n {cluster_name} -g {rg_name}` 합니다. 즉, **Clusteruser** 에 Kubernetes API의 끝점에 대 한 액세스 권한을 부여 해야 합니다.

다음 예제 단계에서는 이 yaml 구성 템플릿에서 클러스터 역할 바인딩을 구성하는 방법을 보여 줍니다.

1. yaml 파일을 복사하여 붙여넣고, LogReaderRBAC.yaml로 저장합니다.

    ```
    apiVersion: rbac.authorization.k8s.io/v1
    kind: ClusterRole
    metadata:
       name: containerHealth-log-reader
    rules:
        - apiGroups: ["", "metrics.k8s.io", "extensions", "apps"]
          resources:
             - "pods/log"
             - "events"
             - "nodes"
             - "pods"
             - "deployments"
             - "replicasets"
          verbs: ["get", "list"]
    ---
    apiVersion: rbac.authorization.k8s.io/v1
    kind: ClusterRoleBinding
    metadata:
       name: containerHealth-read-logs-global
    roleRef:
       kind: ClusterRole
       name: containerHealth-log-reader
       apiGroup: rbac.authorization.k8s.io
    subjects:
    - kind: User
      name: clusterUser
      apiGroup: rbac.authorization.k8s.io
    ```

2. 구성을 업데이트 하려면 명령을 실행 `kubectl apply -f LogReaderRBAC.yaml` 합니다.

>[!NOTE]
> 이전 버전의 파일을 클러스터에 적용 한 경우 `LogReaderRBAC.yaml` 위의 1 단계에 표시 된 새 코드를 복사 하 여 붙여넣어 업데이트 한 다음 2 단계에 나와 있는 명령을 실행 하 여 클러스터에 적용 합니다.

## <a name="configure-ad-integrated-authentication"></a>AD 통합 인증 구성

사용자 인증에 AD (Azure Active Directory)를 사용 하도록 구성 된 AKS 클러스터는이 기능에 액세스 하는 사용자의 로그인 자격 증명을 활용 합니다. 이 구성에서는 Azure AD 인증 토큰을 사용 하 여 AKS 클러스터에 로그인 할 수 있습니다.

Azure Portal에서 권한 부여 페이지를 신뢰할 수 있는 리디렉션 URL로 리디렉션할 수 있도록 Azure AD 클라이언트 등록을 다시 구성 해야 합니다. 그런 다음 Azure AD의 사용자는 **Clusterroles** 및 **clusterrolebindings** 를 통해 동일한 Kubernetes API 끝점에 대 한 액세스 권한을 직접 부여 합니다.

Kubernetes의 고급 보안 설정에 대 한 자세한 내용은 [Kubernetes 설명서](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)를 참조 하세요.

>[!NOTE]
>새 Kubernetes RBAC 사용 클러스터를 만드는 경우 [Azure Kubernetes Service와 Azure Active Directory 통합](../../aks/azure-ad-integration-cli.md) 을 참조 하 고 azure AD 인증을 구성 하는 단계를 따르세요. 클라이언트 응용 프로그램을 만드는 단계에서 해당 섹션의 참고 사항에는 아래 3 단계에서 지정 된 것과 일치 하는 컨테이너에 대 한 Azure Monitor 하기 위해 만들어야 하는 두 개의 리디렉션 Url이 강조 표시 되어 있습니다.

### <a name="client-registration-reconfiguration"></a>클라이언트 등록 재구성

1. Azure Portal의 **Azure Active Directory > 앱 등록** 에서 Azure AD의 Kubernetes 클러스터에 대 한 클라이언트 등록을 찾습니다.

2. 왼쪽 창에서 **인증** 을 선택 합니다.

3. **웹** 응용 프로그램 유형으로이 목록에 두 개의 리디렉션 url을 추가 합니다. 첫 번째 기준 URL 값은 이어야 `https://afd.hosting.portal.azure.net/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html` 하 고 두 번째 기준 url 값은 이어야 합니다 `https://monitoring.hosting.portal.azure.net/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html` .

    >[!NOTE]
    >Azure 중국에서이 기능을 사용 하는 경우 첫 번째 기준 URL 값은 이어야 `https://afd.hosting.azureportal.chinaloudapi.cn/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html` 하며 두 번째 기준 url 값은 이어야 합니다 `https://monitoring.hosting.azureportal.chinaloudapi.cn/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html` .

4. 리디렉션 Url을 등록 한 후 **암시적 권한 부여** 에서 **액세스 토큰** 및 **ID 토큰** 에 대 한 옵션을 선택 하 고 변경 내용을 저장 합니다.

>[!NOTE]
>Single sign-on에 대 한 Azure Active Directory를 사용 하 여 인증을 구성 하는 것은 새 AKS 클러스터의 초기 배포 중에만 수행할 수 있습니다. 이미 배포된 AKS 클러스터에는 Single Sign-On을 구성할 수 없습니다.

>[!IMPORTANT]
>업데이트 된 URI를 사용 하 여 사용자 인증을 위해 Azure AD를 다시 구성한 경우 브라우저의 캐시를 지워 업데이트 된 인증 토큰이 다운로드 되어 적용 되는지 확인 합니다.

## <a name="grant-permission"></a>사용 권한 부여

라이브 데이터 (미리 보기) 기능에 액세스 하려면 각 Azure AD 계정에 Kubernetes의 적절 한 Api에 대 한 권한을 부여 해야 합니다. Azure Active Directory 계정을 부여 하는 단계는 [KUBERNETES RBAC 인증](#configure-kubernetes-rbac-authorization) 섹션에 설명 된 단계와 유사 합니다. Yaml 구성 템플릿을 클러스터에 적용 하기 전에 **Clusterrolebinding** 의 **clusteruser** 를 원하는 사용자로 바꿉니다.

>[!IMPORTANT]
>Kubernetes RBAC 바인딩을 부여한 사용자가 동일한 Azure AD 테 넌 트에 있는 경우 userPrincipalName에 따라 사용 권한을 할당 합니다. 사용자가 다른 Azure AD 테 넌 트에 있는 경우를 쿼리하고 objectId 속성을 사용 합니다.

AKS cluster **Clusterrolebinding** 을 구성 하는 방법에 대 한 추가 도움말은 [Create Kubernetes RBAC binding](../../aks/azure-ad-integration-cli.md#create-kubernetes-rbac-binding)를 참조 하세요.

## <a name="next-steps"></a>다음 단계

이제 인증을 설정 했으므로 클러스터에서 실시간으로 [메트릭](container-insights-livedata-metrics.md), [배포](container-insights-livedata-deployments.md)및 [이벤트와 로그](container-insights-livedata-overview.md) 를 볼 수 있습니다.
