---
title: Azure Container Registry 콘텐츠 서식
description: Azure Container Registry에서 지원되는 콘텐츠 서식에 대해 알아봅니다.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 04/18/2019
ms.author: danlep
ms.openlocfilehash: c804dd7b73ea4a51c02c8b342a4ac60d992ec7c5
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68310687"
---
# <a name="content-formats-supported-in-azure-container-registry"></a>Azure Container Registry에서 지원되는 콘텐츠 서식

Azure Container Registry에서 프라이빗 리포지토리를 사용하여 다음 콘텐츠 서식 중 하나를 관리합니다. 

## <a name="docker-compatible-container-images"></a>Docker 호환 컨테이너 이미지

다음 Docker 컨테이너 이미지 형식이 지원 됩니다.

* [Docker 이미지 매니페스트 V2, 스키마 1](https://docs.docker.com/registry/spec/manifest-v2-1/)

* [Docker 이미지 매니페스트 V2, 스키마 2](https://docs.docker.com/registry/spec/manifest-v2-2/) - 레지스트리가 단일 "image:tag" 참조에서 다중 플랫폼 이미지를 저장할 수 있는 매니페스트 목록이 포함됩니다.

## <a name="oci-images"></a>OCI 이미지

Azure Container Registry은 [OCI (Open Container 이니셔티브) 이미지 형식 사양을](https://github.com/opencontainers/image-spec/blob/master/spec.md)만족 하는 이미지도 지원 합니다. 패키지 형식에는 [SIF (특이성 Image Format)](https://www.sylabs.io/2018/03/sif-containing-your-containers/)가 포함 됩니다.

## <a name="helm-charts"></a>Helm 차트

Kubernetes에 대 한 응용 프로그램을 신속 하 게 관리 하 고 배포 하는 데 사용 되는 패키징 형식인 [투구 차트](https://helm.sh/)의 리포지토리를 호스트할 수 Azure Container Registry. [Helm 클라이언트](https://docs.helm.sh/using_helm/#installing-helm) 버전 2.11.0 이상이 지원됩니다.

## <a name="next-steps"></a>다음 단계

* Azure Container Registry를 사용하여 이미지를 [푸시하고 끌어오는](container-registry-get-started-docker-cli.md) 방법을 참조하세요.

* [ACR 작업](container-registry-tasks-overview.md)을 사용하여 컨테이너 이미지를 빌드하고 테스트합니다. 

* [ BuildKit](https://github.com/moby/buildkit)를 사용하여 OCI 형식으로 컨테이너를 빌드하고 패키징합니다.

* Azure Container Registry에서 호스트되는 [Helm 리포지토리](container-registry-helm-repos.md)를 설정합니다. 


