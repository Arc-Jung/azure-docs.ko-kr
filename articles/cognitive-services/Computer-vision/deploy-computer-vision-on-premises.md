---
title: Kubernetes 및 투구와 Computer Vision 컨테이너 사용
titleSuffix: Azure Cognitive Services
description: Kubernetes 및 투구를 사용 하 여 Computer Vision 컨테이너를 배포 하는 방법을 알아봅니다.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 11/23/2020
ms.author: aahi
ms.openlocfilehash: dce8893cac156ce2941652e32409357cb8ec3b1a
ms.sourcegitcommit: 6a770fc07237f02bea8cc463f3d8cc5c246d7c65
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/24/2020
ms.locfileid: "96015318"
---
# <a name="use-computer-vision-container-with-kubernetes-and-helm"></a>Kubernetes 및 투구와 Computer Vision 컨테이너 사용

온-프레미스 Computer Vision 컨테이너를 관리 하는 한 가지 옵션은 Kubernetes 및 투구를 사용 하는 것입니다. Kubernetes 및 투구를 사용 하 여 Computer Vision 컨테이너 이미지를 정의 합니다. Kubernetes 패키지를 만듭니다. 이 패키지는 온-프레미스 Kubernetes 클러스터에 배포 됩니다. 마지막으로 배포 된 서비스를 테스트 하는 방법을 살펴보겠습니다. Kubernetes 오케스트레이션을 사용 하지 않고 Docker 컨테이너를 실행 하는 방법에 대 한 자세한 내용은 [Computer Vision 컨테이너 설치 및 실행](computer-vision-how-to-install-containers.md)을 참조 하세요.

## <a name="prerequisites"></a>필수 조건

온-프레미스 Computer Vision 컨테이너를 사용 하기 전에 다음 필수 구성 요소가 필요 합니다.

| 필수 | 목적 |
|----------|---------|
| Azure 계정 | Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정][free-azure-account]을 만듭니다. |
| Kubernetes CLI | [KUBERNETES CLI][kubernetes-cli] 는 컨테이너 레지스트리에서 공유 자격 증명을 관리 하는 데 필요 합니다. Kubernetes는 Kubernetes 패키지 관리자 인 투구 이전에도 필요 합니다. |
| Helm CLI | 투구 차트 (컨테이너 패키지 정의)를 설치 하는 데 사용 되는 [투구 CLI][helm-install]를 설치 합니다. |
| Computer Vision 리소스 |컨테이너를 사용하려면 다음이 있어야 합니다.<br><br>Azure **Computer Vision** 리소스 및 연결 된 API 키 끝점 URI입니다. 두 값은 모두 리소스의 개요 및 키 페이지에서 사용할 수 있으며 컨테이너를 시작 하는 데 필요 합니다.<br><br>**{API_KEY}**: **키** 페이지에서 사용 가능한 두 리소스 키 중 하나<br><br>**{ENDPOINT_URI}**: **개요** 페이지에 제공 된 끝점입니다.|

[!INCLUDE [Gathering required parameters](../containers/includes/container-gathering-required-parameters.md)]

### <a name="the-host-computer"></a>호스트 컴퓨터

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>컨테이너 요구 사항 및 추천

[!INCLUDE [Container requirements and recommendations](includes/container-requirements-and-recommendations.md)]

## <a name="connect-to-the-kubernetes-cluster"></a>Kubernetes 클러스터에 연결

호스트 컴퓨터에 사용 가능한 Kubernetes 클러스터가 있어야 합니다. Kubernetes 클러스터를 호스트 컴퓨터에 배포 하는 방법에 대 한 개념을 이해 하려면 [Kubernetes 클러스터 배포](../../aks/tutorial-kubernetes-deploy-cluster.md) 에 대 한이 자습서를 참조 하세요. 배포에 대 한 자세한 내용은 [Kubernetes 설명서](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)에서 확인할 수 있습니다.

## <a name="configure-helm-chart-values-for-deployment"></a>배포에 대 한 투구 차트 값 구성

먼저 *read* 라는 폴더를 만듭니다. 그런 다음, 다음 YAML 콘텐츠를 라는 새 파일에 붙여넣습니다 `chart.yaml` .

```yaml
apiVersion: v2
name: read
version: 1.0.0
description: A Helm chart to deploy the Read OCR container to a Kubernetes cluster
dependencies:
- name: rabbitmq
  condition: read.image.args.rabbitmq.enabled
  version: ^6.12.0
  repository: https://kubernetes-charts.storage.googleapis.com/
- name: redis
  condition: read.image.args.redis.enabled
  version: ^6.0.0
  repository: https://kubernetes-charts.storage.googleapis.com/
```

투구 차트 기본값을 구성 하려면 다음 YAML를 복사 하 여 라는 파일에 붙여 넣습니다 `values.yaml` . `# {ENDPOINT_URI}`및 `# {API_KEY}` 주석을 사용자 고유의 값으로 바꿉니다. 필요한 경우 resultExpirationPeriod, Redis 및 RabbitMQ를 구성 합니다.

```yaml
# These settings are deployment specific and users can provide customizations
read:
  enabled: true
  image:
    name: cognitive-services-read
    registry:  mcr.microsoft.com/
    repository: azure-cognitive-services/vision/read
    tag: 3.2-preview.1
    args:
      eula: accept
      billing: # {ENDPOINT_URI}
      apikey: # {API_KEY}
      
      # Result expiration period setting. Specify when the system should clean up recognition results.
      # For example, resultExpirationPeriod=1, the system will clear the recognition result 1hr after the process.
      # resultExpirationPeriod=0, the system will clear the recognition result after result retrieval.
      resultExpirationPeriod: 1
      
      # Redis storage, if configured, will be used by read container to store result records.
      # A cache is required if multiple read containers are placed behind load balancer.
      redis:
        enabled: false # {true/false}
        password: password

      # RabbitMQ is used for dispatching tasks. This can be useful when multiple read containers are
      # placed behind load balancer.
      rabbitmq:
        enabled: false # {true/false}
        rabbitmq:
          username: user
          password: password
```

> [!IMPORTANT]
> - `billing`및 값이 `apikey` 제공 되지 않은 경우 서비스는 15 분 후에 만료 됩니다. 마찬가지로, 서비스를 사용할 수 없기 때문에 확인에 실패 합니다.
> 
> - 예를 들어 Docker Compose 또는 Kubernetes에서 부하 분산 장치 뒤에 여러 읽기 컨테이너를 배포 하는 경우 외부 캐시가 있어야 합니다. 처리 컨테이너와 GET 요청 컨테이너는 다를 수 있기 때문에 외부 캐시는 결과를 저장 하 고 컨테이너 간에 공유 합니다. 캐시 설정에 대 한 자세한 내용은 [Computer Vision Docker 컨테이너 구성](./computer-vision-resource-container-config.md)을 참조 하세요.
>

*읽기* 디렉터리 아래에 *템플릿* 폴더를 만듭니다. 다음 YAML을 복사 하 여 라는 파일에 붙여 넣습니다 `deployment.yaml` . `deployment.yaml`이 파일은 투구 템플릿 역할을 합니다.

> 템플릿은 Kubernetes에서 이해할 수 있는 YAML 형식의 리소스 설명 인 매니페스트 파일을 생성 합니다. [-투구 차트 템플릿 가이드][chart-template-guide]

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: read
  labels:
    app: read-deployment
spec:
  selector:
    matchLabels:
      app: read-app
  template:
    metadata:
      labels:
        app: read-app       
    spec:
      containers:
      - name: {{.Values.read.image.name}}
        image: {{.Values.read.image.registry}}{{.Values.read.image.repository}}
        ports:
        - containerPort: 5000
        env:
        - name: EULA
          value: {{.Values.read.image.args.eula}}
        - name: billing
          value: {{.Values.read.image.args.billing}}
        - name: apikey
          value: {{.Values.read.image.args.apikey}}
        args:        
        - ReadEngineConfig:ResultExpirationPeriod={{ .Values.read.image.args.resultExpirationPeriod }}
        {{- if .Values.read.image.args.rabbitmq.enabled }}
        - Queue:RabbitMQ:HostName={{ include "rabbitmq.hostname" . }}
        - Queue:RabbitMQ:Username={{ .Values.read.image.args.rabbitmq.rabbitmq.username }}
        - Queue:RabbitMQ:Password={{ .Values.read.image.args.rabbitmq.rabbitmq.password }}
        {{- end }}      
        {{- if .Values.read.image.args.redis.enabled }}
        - Cache:Redis:Configuration={{ include "redis.connStr" . }}
        {{- end }}
      imagePullSecrets:
      - name: {{.Values.read.image.pullSecret}}      
--- 
apiVersion: v1
kind: Service
metadata:
  name: read-service
spec:
  type: LoadBalancer
  ports:
  - port: 5000
  selector:
    app: read-app
```

동일한 *템플릿* 폴더에서 다음 도우미 함수를 복사 하 여에 붙여넣습니다 `helpers.tpl` . `helpers.tpl` 투구 템플릿을 생성 하는 데 도움이 되는 유용한 함수를 정의 합니다.

```yaml
{{- define "rabbitmq.hostname" -}}
{{- printf "%s-rabbitmq" .Release.Name -}}
{{- end -}}

{{- define "redis.connStr" -}}
{{- $hostMaster := printf "%s-redis-master:6379" .Release.Name }}
{{- $hostSlave := printf "%s-redis-slave:6379" .Release.Name -}}
{{- $passWord := printf "password=%s" .Values.read.image.args.redis.password -}}
{{- $connTail := "ssl=False,abortConnect=False" -}}
{{- printf "%s,%s,%s,%s" $hostMaster $hostSlave $passWord $connTail -}}
{{- end -}}
```
템플릿은 부하 분산 장치 서비스와 읽기를 위해 컨테이너/이미지 배포를 지정 합니다.

### <a name="the-kubernetes-package-helm-chart"></a>Kubernetes 패키지 (투구 차트)

*투구 차트* 에는 컨테이너 레지스트리에서 끌어올 docker 이미지의 구성이 포함 되어 있습니다 `mcr.microsoft.com` .

> [투구 차트][helm-charts] 는 관련 된 Kubernetes 리소스 집합을 설명 하는 파일의 컬렉션입니다. 단일 차트는 memcached pod 또는 HTTP 서버, 데이터베이스, 캐시 등의 전체 웹 앱 스택과 같이 복잡 한 항목을 배포 하는 데 사용할 수 있습니다.

제공 된 *투구 차트* 는 Computer Vision 서비스의 docker 이미지와 컨테이너 레지스트리에서 해당 하는 서비스를 가져옵니다 `mcr.microsoft.com` .

## <a name="install-the-helm-chart-on-the-kubernetes-cluster"></a>Kubernetes 클러스터에 투구 차트 설치

*투구 차트* 를 설치 하려면 명령을 실행 해야 [`helm install`][helm-install-cmd] 합니다. 폴더 위의 디렉터리에서 install 명령을 실행 해야 `read` 합니다.

```console
helm install read ./read
```

성공적인 설치 실행에서 볼 수 있는 출력의 예는 다음과 같습니다.

```console
NAME: read
LAST DEPLOYED: Thu Sep 04 13:24:06 2019
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/Pod(related)
NAME                    READY  STATUS             RESTARTS  AGE
read-57cb76bcf7-45sdh   0/1    ContainerCreating  0         0s

==> v1/Service
NAME     TYPE          CLUSTER-IP    EXTERNAL-IP  PORT(S)         AGE
read     LoadBalancer  10.110.44.86  localhost    5000:31301/TCP  0s

==> v1beta1/Deployment
NAME    READY  UP-TO-DATE  AVAILABLE  AGE
read    0/1    1           0          0s
```

Kubernetes 배포를 완료 하는 데 몇 분 정도 걸릴 수 있습니다. Pod와 서비스가 올바르게 배포 되 고 사용 가능한 지 확인 하려면 다음 명령을 실행 합니다.

```console
kubectl get all
```

다음 출력과 유사한 내용이 표시 되어야 합니다.

```console
kubectl get all
NAME                        READY   STATUS    RESTARTS   AGE
pod/read-57cb76bcf7-45sdh   1/1     Running   0          17s

NAME                   TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/kubernetes     ClusterIP      10.96.0.1      <none>        443/TCP          45h
service/read           LoadBalancer   10.110.44.86   localhost     5000:31301/TCP   17s

NAME                   READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/read   1/1     1            1           17s

NAME                              DESIRED   CURRENT   READY   AGE
replicaset.apps/read-57cb76bcf7   1         1         1       17s
```

## <a name="deploy-multiple-v3-containers-on-the-kubernetes-cluster"></a>Kubernetes 클러스터에 여러 v3 컨테이너 배포

컨테이너의 v3부터 작업 및 페이지 수준 모두에서 컨테이너를 병렬로 사용할 수 있습니다.

기본적으로 각 v3 컨테이너에는 디스패처 및 인식 worker가 있습니다. 디스패처는 다중 페이지 작업을 여러 단일 페이지 하위 작업으로 분할 하는 작업을 담당 합니다. 인식 작업자는 단일 페이지 문서를 인식 하는 데 최적화 되어 있습니다. 페이지 수준 병렬 처리를 구현 하려면 부하 분산 장치 뒤에 여러 v3 컨테이너를 배포 하 고 컨테이너에서 범용 저장소 및 큐를 공유 하도록 합니다. 

> [!NOTE] 
> 현재 Azure Storage 및 Azure 큐만 지원 됩니다. 

요청을 받는 컨테이너는 작업을 단일 페이지 하위 작업으로 분할 하 여 유니버설 큐에 추가할 수 있습니다. 사용량이 적은 컨테이너의 모든 인식 작업자는 큐에서 단일 페이지 하위 작업을 사용 하 고, 인식을 수행 하 고, 결과를 저장소에 업로드할 수 있습니다. `n`배포 된 컨테이너의 수에 따라 처리량이 최대 시간까지 향상 될 수 있습니다.

다음 YAML을 복사 하 여 라는 파일에 붙여 넣습니다 `deployment.yaml` . `# {ENDPOINT_URI}`및 `# {API_KEY}` 주석을 사용자 고유의 값으로 바꿉니다. `# {AZURE_STORAGE_CONNECTION_STRING}`주석을 Azure Storage 연결 문자열로 바꿉니다. `replicas`다음 예에서로 설정 된 원하는 수로 구성 `3` 합니다.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: read
  labels:
    app: read-deployment
spec:
  selector:
    matchLabels:
      app: read-app
  replicas: # {NUMBER_OF_READ_CONTAINERS}
  template:
    metadata:
      labels:
        app: read-app
    spec:
      containers:
      - name: cognitive-services-read
        image: mcr.microsoft.com/azure-cognitive-services/vision/read
        ports:
        - containerPort: 5000
        env:
        - name: EULA
          value: accept
        - name: billing
          value: # {ENDPOINT_URI}
        - name: apikey
          value: # {API_KEY}
        - name: Storage__ObjectStore__AzureBlob__ConnectionString
          value: # {AZURE_STORAGE_CONNECTION_STRING}
        - name: Queue__Azure__ConnectionString
          value: # {AZURE_STORAGE_CONNECTION_STRING}
--- 
apiVersion: v1
kind: Service
metadata:
  name: azure-cognitive-service-read
spec:
  type: LoadBalancer
  ports:
  - port: 5000
    targetPort: 5000
  selector:
    app: read-app
```

다음 명령을 실행합니다. 

```console
kubectl apply -f deployment.yaml
```

다음은 성공적인 배포 실행에서 볼 수 있는 출력의 예입니다.

```console
deployment.apps/read created
service/azure-cognitive-service-read created
```

Kubernetes 배포를 완료 하는 데 몇 분 정도 걸릴 수 있습니다. Pod와 서비스가 올바르게 배포 되 고 사용 가능한 지 확인 하려면 다음 명령을 실행 합니다.

```console
kubectl get all
```

다음과 유사한 콘솔 출력이 표시 됩니다.

```console
kubectl get all
NAME                       READY   STATUS    RESTARTS   AGE
pod/read-6cbbb6678-58s9t   1/1     Running   0          3s
pod/read-6cbbb6678-kz7v4   1/1     Running   0          3s
pod/read-6cbbb6678-s2pct   1/1     Running   0          3s

NAME                                   TYPE           CLUSTER-IP   EXTERNAL-IP    PORT(S)          AGE
service/azure-cognitive-service-read   LoadBalancer   10.0.134.0   <none>         5000:30846/TCP   17h
service/kubernetes                     ClusterIP      10.0.0.1     <none>         443/TCP          78d

NAME                   READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/read   3/3     3            3           3s

NAME                             DESIRED   CURRENT   READY   AGE
replicaset.apps/read-6cbbb6678   3         3         3       3s
```

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="next-steps"></a>다음 단계

AKS (Azure Kubernetes Service)에서 투구를 사용 하 여 응용 프로그램을 설치 하는 방법에 대 한 자세한 내용은 [여기를 참조][installing-helm-apps-in-aks]하세요.

> [!div class="nextstepaction"]
> [Cognitive Services 컨테이너][cog-svcs-containers]

<!-- LINKS - external -->
[free-azure-account]: https://azure.microsoft.com/free
[git-download]: https://git-scm.com/downloads
[azure-cli]: /cli/azure/install-azure-cli?view=azure-cli-latest
[docker-engine]: https://www.docker.com/products/docker-engine
[kubernetes-cli]: https://kubernetes.io/docs/tasks/tools/install-kubectl
[helm-install]: https://helm.sh/docs/using_helm/#installing-helm
[helm-install-cmd]: https://helm.sh/docs/intro/using_helm/#helm-install-installing-a-package
[helm-charts]: https://helm.sh/docs/topics/charts/
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[helm-test]: https://helm.sh/docs/helm/#helm-test
[chart-template-guide]: https://helm.sh/docs/chart_template_guide

<!-- LINKS - internal -->
[vision-container-host-computer]: computer-vision-how-to-install-containers.md#the-host-computer
[installing-helm-apps-in-aks]: ../../aks/kubernetes-helm.md
[cog-svcs-containers]: ../cognitive-services-container-support.md