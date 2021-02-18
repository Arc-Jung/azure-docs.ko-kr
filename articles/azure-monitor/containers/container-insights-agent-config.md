---
title: 컨테이너 에이전트 데이터 컬렉션에 대 한 Azure Monitor 구성 | Microsoft Docs
description: 이 문서에서는 stdout/stderr 및 환경 변수 로그 수집을 제어 하기 위해 컨테이너 에이전트에 대 한 Azure Monitor를 구성 하는 방법을 설명 합니다.
ms.topic: conceptual
ms.date: 10/09/2020
ms.openlocfilehash: f21b841bc129012b684d2a1c59eb72989fe9e0e0
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100620199"
---
# <a name="configure-agent-data-collection-for-azure-monitor-for-containers"></a>컨테이너용 Azure Monitor에 대한 에이전트 데이터 수집 구성

컨테이너에 대 한 Azure Monitor 컨테이너 화 된 에이전트에서 관리 되는 Kubernetes 클러스터에 배포 된 컨테이너 워크 로드에서 stdout, stderr 및 환경 변수를 수집 합니다. 사용자 지정 Kubernetes ConfigMaps를 만들어이 환경을 제어 하 여 에이전트 데이터 수집 설정을 구성할 수 있습니다. 

이 문서에서는 사용자의 요구 사항에 따라 ConfigMap을 만들고 데이터 컬렉션을 구성 하는 방법을 보여 줍니다.

>[!NOTE]
>Azure Red Hat OpenShift의 경우 *openshift-Azure-로깅* 네임 스페이스에 템플릿 configmap 파일이 만들어집니다. 
>

## <a name="configmap-file-settings-overview"></a>ConfigMap 파일 설정 개요

사용자 지정 항목을 처음부터 새로 만들지 않고도 쉽게 편집할 수 있도록 하는 템플릿 ConfigMap 파일이 제공 됩니다. 시작 하기 [전에 configmaps에 대 한](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) Kubernetes 설명서를 검토 하 고 configmaps을 생성, 구성 및 배포 하는 방법을 숙지 해야 합니다. 이렇게 하면 네임 스페이스 당 또는 전체 클러스터에서 stderr 및 stdout을 필터링 할 수 있으며 클러스터의 모든 pod/노드에서 실행 중인 모든 컨테이너에 대해 환경 변수를 필터링 할 수 있습니다.

>[!IMPORTANT]
>컨테이너 작업에서 stdout, stderr 및 환경 변수를 수집 하는 데 지원 되는 최소 에이전트 버전은 ciprod06142019 이상입니다. 에이전트 버전을 확인 하려면 **노드** 탭에서 노드를 선택 하 고 속성 창에서 **에이전트 이미지 태그** 속성의 값을 확인 합니다. 에이전트 버전 및 각 릴리스에 포함 되는 내용에 대 한 자세한 내용은 [에이전트 릴리스 정보](https://github.com/microsoft/Docker-Provider/tree/ci_feature_prod)를 참조 하세요.

### <a name="data-collection-settings"></a>데이터 컬렉션 설정

다음 표에서는 데이터 수집을 제어 하기 위해 구성할 수 있는 설정을 설명 합니다.

| 키 | 데이터 형식 | 값 | 설명 |
|--|--|--|--|
| `schema-version` | 문자열 (대/소문자 구분) | v1 | 에이전트에서 사용 하는 스키마 버전입니다.<br> 이 ConfigMap을 구문 분석 하는 경우<br> 현재 지원 되는 스키마 버전은 v1입니다.<br> 이 값을 수정 하는 것은 지원 되지 않으며<br> ConfigMap을 평가할 때 거부 됩니다. |
| `config-version` | String |  | 소스 제어 시스템/리포지토리에서이 구성 파일의 버전을 추적 하는 기능을 지원 합니다.<br> 허용 되는 최대 문자 수는 10이 고 다른 모든 문자는 잘립니다. |
| `[log_collection_settings.stdout] enabled =` | 부울 | true 또는 false | Stdout 컨테이너 로그 수집이 설정 되었는지 여부를 제어 합니다. 로 설정 `true` 된 경우 stdout 로그 컬렉션에 대 한 네임 스페이스가 제외 되지 않습니다.<br> ( `log_collection_settings.stdout.exclude_namespaces` 아래 설정), stdout 로그는 클러스터에 있는 모든 pod/노드의 모든 컨테이너에서 수집 됩니다. ConfigMaps에 지정 되지 않은 경우<br> 기본값은 `enabled = true` 입니다. |
| `[log_collection_settings.stdout] exclude_namespaces =` | String | 쉼표로 구분 된 배열 | Stdout 로그가 수집 되지 않을 Kubernetes 네임 스페이스의 배열입니다. 이 설정은 다음 경우에만 적용 됩니다.<br> `log_collection_settings.stdout.enabled`<br> `true`로 설정됩니다.<br> ConfigMap에 지정 되지 않은 경우 기본값은입니다.<br> `exclude_namespaces = ["kube-system"]`. |
| `[log_collection_settings.stderr] enabled =` | 부울 | true 또는 false | Stderr 컨테이너 로그 수집이 사용 되는지 여부를 제어 합니다.<br> 로 설정 `true` 된 경우 stdout 로그 컬렉션에 대 한 네임 스페이스가 제외 되지 않습니다.<br> ( `log_collection_settings.stderr.exclude_namespaces` 설정)-클러스터에 있는 모든 pod/노드의 모든 컨테이너에서 stderr 로그가 수집 됩니다.<br> ConfigMaps에 지정 되지 않은 경우 기본값은입니다.<br> `enabled = true`. |
| `[log_collection_settings.stderr] exclude_namespaces =` | String | 쉼표로 구분 된 배열 | Stderr 로그가 수집 되지 않을 Kubernetes 네임 스페이스의 배열입니다.<br> 이 설정은 다음 경우에만 적용 됩니다.<br> `log_collection_settings.stdout.enabled`이 `true`로 설정됩니다.<br> ConfigMap에 지정 되지 않은 경우 기본값은입니다.<br> `exclude_namespaces = ["kube-system"]`. |
| `[log_collection_settings.env_var] enabled =` | 부울 | true 또는 false | 이 설정은 환경 변수 컬렉션을 제어 합니다.<br> 클러스터의 모든 pod/노드 간<br> 지정 하지 않을 경우 기본적으로로 설정 됩니다. `enabled = true`<br> ConfigMaps.<br> 환경 변수의 컬렉션을 전역적으로 사용 하는 경우 특정 컨테이너에 대해 사용 하지 않도록 설정할 수 있습니다.<br> 환경 변수 설정<br> `AZMON_COLLECT_ENV`Dockerfile 설정이 나 **env:** 섹션 아래에 있는 [Pod의 구성 파일](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/) 에서 **False** 로 설정 합니다.<br> 환경 변수의 컬렉션을 전역적으로 사용 하지 않도록 설정 하면 특정 컨테이너에 대해 컬렉션을 사용 하도록 설정할 수 없습니다. 즉, 전역에서 이미 사용 하도록 설정 된 경우 컬렉션을 사용 하지 않도록 설정 하는 것은 컨테이너 수준에서 적용할 수 있는 유일한 재정의입니다. |
| `[log_collection_settings.enrich_container_logs] enabled =` | 부울 | true 또는 false | 이 설정은 컨테이너 로그 보강를 제어 하 여 이름 및 이미지 속성 값을 채웁니다.<br> 클러스터의 모든 컨테이너 로그에 대해 ContainerLog 테이블에 기록 된 모든 로그 레코드<br> `enabled = false`ConfigMap에 지정 되지 않은 경우 기본적으로로 설정 됩니다. |
| `[log_collection_settings.collect_all_kube_events]` | 부울 | true 또는 false | 이 설정을 사용 하면 모든 유형의 Kube 이벤트를 수집할 수 있습니다.<br> 기본적으로 *Normal* 형식의 Kube 이벤트는 수집 되지 않습니다. 이 설정이로 설정 되 면 `true` *일반적인* 이벤트가 더 이상 필터링 되지 않으며 모든 이벤트가 수집 됩니다.<br> 이 매개 변수는 기본적으로 `false`로 설정됩니다. |

### <a name="metric-collection-settings"></a>메트릭 컬렉션 설정

다음 표에서는 메트릭 수집을 제어 하기 위해 구성할 수 있는 설정을 설명 합니다.

| 키 | 데이터 형식 | 값 | 설명 |
|--|--|--|--|
| `[metric_collection_settings.collect_kube_system_pv_metrics] enabled =` | 부울 | true 또는 false | 이 설정을 사용 하 여 kube 네임 스페이스에서 영구적 볼륨 (PV) 사용 메트릭을 수집할 수 있습니다. 기본적으로 kube 네임 스페이스에서 영구적 볼륨 클레임이 있는 영구적 볼륨에 대 한 사용 메트릭은 수집 되지 않습니다. 이 설정을로 설정 하면 `true` 모든 네임 스페이스에 대 한 PV 사용 메트릭이 수집 됩니다. 이 매개 변수는 기본적으로 `false`로 설정됩니다. |

ConfigMaps는 전역 목록이 며 에이전트에 하나의 Configmaps만 적용 될 수 있습니다. 컬렉션에서 다른 ConfigMaps을 과도 하 게 사용할 수 없습니다.

## <a name="configure-and-deploy-configmaps"></a>ConfigMaps 구성 및 배포

ConfigMap 구성 파일을 구성 하 고 클러스터에 배포 하려면 다음 단계를 수행 합니다.

1. [템플릿 ConfigMap yaml 파일](https://aka.ms/container-azm-ms-agentconfig) 을 다운로드 하 고 azm로 저장 합니다. 

   > [!NOTE]
   > Azure Red Hat OpenShift를 사용 하는 경우에는 ConfigMap 템플릿이 클러스터에 이미 있기 때문에이 단계가 필요 하지 않습니다.

2. 사용자 지정 항목으로 ConfigMap yaml 파일을 편집 하 여 stdout, stderr 및/또는 환경 변수를 수집 합니다. Azure Red Hat OpenShift에 대 한 ConfigMap yaml 파일을 편집 하는 경우 먼저 명령을 실행 `oc edit configmaps container-azm-ms-agentconfig -n openshift-azure-logging` 하 여 텍스트 편집기에서 파일을 엽니다.

    - Stdout 로그 컬렉션에 대 한 특정 네임 스페이스를 제외 하려면 다음 예제를 사용 하 여 키/값을 구성 `[log_collection_settings.stdout] enabled = true exclude_namespaces = ["my-namespace-1", "my-namespace-2"]` 합니다.
    
    - 특정 컨테이너에 대해 환경 변수 컬렉션을 사용 하지 않도록 설정 하려면 키/값을 설정 하 여 `[log_collection_settings.env_var] enabled = true` 전역으로 변수 컬렉션을 사용 하도록 설정 하 고 [여기](container-insights-manage-agent.md#how-to-disable-environment-variable-collection-on-a-container) 의 단계에 따라 특정 컨테이너에 대 한 구성을 완료 합니다.
    
    - Stderr 로그 수집 클러스터 전체를 사용 하지 않도록 설정 하려면 다음 예제를 사용 하 여 키/값을 구성 `[log_collection_settings.stderr] enabled = false` 합니다.

3. Azure Red Hat OpenShift 이외의 클러스터의 경우 `kubectl apply -f <configmap_yaml_file.yaml>` Azure Red Hat OpenShift 이외의 클러스터에서 다음 kubectl 명령을 실행 하 여 ConfigMap을 만듭니다. 
    
    예: `kubectl apply -f container-azm-ms-agentconfig.yaml`. 

    Azure Red Hat OpenShift의 경우 편집기에서 변경 내용을 저장 합니다.

구성 변경 내용을 적용 하기 전에 완료 하는 데 몇 분 정도 걸릴 수 있으며, 클러스터의 모든 omsagent pod가 다시 시작 됩니다. 다시 시작은 모든 omsagent pod에 대 한 롤링 다시 시작 이지만 동시에 다시 시작 되지 않습니다. 다시 시작이 완료 되 면 다음과 유사한 메시지가 표시 되 고 결과가 포함 됩니다 `configmap "container-azm-ms-agentconfig" created` .

## <a name="verify-configuration"></a>구성 확인

구성이 Azure Red Hat OpenShift 이외의 클러스터에 성공적으로 적용 되었는지 확인 하려면 다음 명령을 사용 하 여 에이전트 pod에서 로그를 검토 `kubectl logs omsagent-fdf58 -n kube-system` 합니다. Omsagent pod의 구성 오류가 있으면 출력에 다음과 유사한 오류가 표시 됩니다.

``` 
***************Start Config Processing******************** 
config::unsupported/missing config schema version - 'v21' , using defaults
```

구성 변경 내용을 적용 하는 것과 관련 된 오류도 검토할 수 있습니다. 다음 옵션은 구성 변경의 추가 문제 해결을 수행 하는 데 사용할 수 있습니다.

- 동일한 명령을 사용 하 여 에이전트 pod 로그를 사용 `kubectl logs` 합니다. 

    >[!NOTE]
    >이 명령은 Azure Red Hat OpenShift 클러스터에 적용 되지 않습니다.
    > 

- 라이브 로그를 기반으로 합니다. 라이브 로그에는 다음과 비슷한 오류가 표시 됩니다.

    ```
    config::error::Exception while parsing config map for log collection/env variable settings: \nparse error on value \"$\" ($end), using defaults, please check config map for errors
    ```

- Log Analytics 작업 영역에 있는 **KubeMonAgentEvents** 테이블의 구성 오류에 대 한 *오류* 심각도를 사용 하 여 1 시간 마다 데이터가 전송 됩니다. 오류가 없는 경우 테이블의 항목에는 오류를 보고 하지 않는 심각도 *정보* 를 포함 하는 데이터가 포함 됩니다. **Tags** 속성은 오류가 발생 한 pod 및 컨테이너 ID에 대 한 자세한 정보와 마지막으로 발생 한 시간을 포함 하 여 마지막으로 발생 한 시간을 포함 합니다.

- Azure Red Hat OpenShift를 사용 하 여 **ContainerLog** 테이블을 검색 하 여 omsagent 로그를 확인 하 여 openshift-Azure-logging의 로그 수집이 사용 되는지 확인 합니다.

Azure Red Hat OpenShift 이외의 클러스터에서 ConfigMap의 오류를 수정한 후에는 yaml 파일을 저장 하 고 명령을 실행 하 여 업데이트 된 Configmap을 적용 `kubectl apply -f <configmap_yaml_file.yaml` 합니다. Azure Red Hat OpenShift의 경우 다음 명령을 실행 하 여 업데이트 된 ConfigMaps을 편집 하 고 저장 합니다.

``` bash
oc edit configmaps container-azm-ms-agentconfig -n openshift-azure-logging
```

## <a name="applying-updated-configmap"></a>업데이트 된 ConfigMap 적용

Azure Red Hat OpenShift 이외의 클러스터에 ConfigMap을 이미 배포 했으며 최신 구성으로 업데이트 하려는 경우 이전에 사용한 ConfigMap 파일을 편집한 다음 이전과 동일한 명령을 사용 하 여 적용할 수 있습니다 `kubectl apply -f <configmap_yaml_file.yaml` . Azure Red Hat OpenShift의 경우 다음 명령을 실행 하 여 업데이트 된 ConfigMaps을 편집 하 고 저장 합니다.

``` bash
oc edit configmaps container-azm-ms-agentconfig -n openshift-azure-logging
```

구성 변경 내용을 적용 하기 전에 완료 하는 데 몇 분 정도 걸릴 수 있으며, 클러스터의 모든 omsagent pod가 다시 시작 됩니다. 다시 시작은 모든 omsagent pod에 대 한 롤링 다시 시작 이지만 동시에 다시 시작 되지 않습니다. 다시 시작이 완료 되 면 다음과 유사한 메시지가 표시 되 고 결과가 포함 됩니다 `configmap "container-azm-ms-agentconfig" updated` .

## <a name="verifying-schema-version"></a>스키마 버전 확인

지원 되는 구성 스키마 버전은 omsagent pod에서 pod 주석 (스키마 버전)으로 사용할 수 있습니다. 다음 kubectl 명령을 사용 하 여 볼 수 있습니다. `kubectl describe pod omsagent-fdf58 -n=kube-system`

출력은 주석 스키마 버전을 사용 하는 다음과 유사 하 게 표시 됩니다.

```
    Name:           omsagent-fdf58
    Namespace:      kube-system
    Node:           aks-agentpool-95673144-0/10.240.0.4
    Start Time:     Mon, 10 Jun 2019 15:01:03 -0700
    Labels:         controller-revision-hash=589cc7785d
                    dsName=omsagent-ds
                    pod-template-generation=1
    Annotations:    agentVersion=1.10.0.1
                  dockerProviderVersion=5.0.0-0
                    schema-versions=v1 
```

## <a name="next-steps"></a>다음 단계

- 컨테이너의 Azure Monitor에는 미리 정의 된 경고 집합이 포함 되지 않습니다. [컨테이너에 대 한 Azure Monitor를 사용 하 여 성능 경고 만들기](./container-insights-log-alerts.md) 를 검토 하 여 devops 또는 운영 프로세스 및 절차를 지원 하기 위해 높은 CPU 및 메모리 사용률에 대해 권장 되는 경고를 만드는 방법을 알아봅니다.

- 모니터링을 사용 하 여 AKS 또는 하이브리드 클러스터의 상태 및 리소스 사용률을 수집 하 고 해당 작업에서 실행 되는 작업을 수집 합니다. 컨테이너에 Azure Monitor [를 사용 하는 방법을](container-insights-analyze.md) 알아봅니다.

- [로그 쿼리 예](container-insights-log-search.md#search-logs-to-analyze-data) 를 확인 하 여 미리 정의 된 쿼리 및 예제를 확인 하거나 사용자 지정 하 여 클러스터에 대 한 경고, 시각화 또는 분석을 평가 하거나 사용자 지정 합니다.