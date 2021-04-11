---
title: OPC 자격 증명 모음이란? - Azure | Microsoft Docs
description: 이 문서는 OPC 자격 증명 모음에 대한 개요를 제공합니다. 클라우드 내 OPC UA 애플리케이션에 대한 인증서 수명 주기를 구성, 등록 및 관리할 수 있습니다.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 715ed204e28d6260c28fa099b40fc78aa12de44d
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/30/2021
ms.locfileid: "105646529"
---
# <a name="what-is-opc-vault"></a>OPC 자격 증명 모음이란?

> [!IMPORTANT]
> 이 문서를 업데이트하는 동안 최신 콘텐츠는 [Azure Industrial IoT](https://azure.github.io/Industrial-IoT/)를 참조하세요.

OPC 자격 증명 모음은 클라우드 내 OPC UA 서버 및 클라이언트 애플리케이션에 대한 인증서 수명 주기를 구성, 등록 및 관리할 수 있는 마이크로서비스입니다. 이 문서에서는 OPC 자격 증명 모음의 간단한 사용 사례를 설명합니다.

## <a name="certificate-management"></a>인증서 관리

예를 들어 제조 회사에서 새로 빌드한 클라이언트 애플리케이션에 자신의 OPC UA 서버 머신을 연결해야 합니다. 제조 업체가 처음 서버 머신에 액세스할 때, OPC UA 서버 애플리케이션에 클라이언트 애플리케이션이 안전하지 않음을 알리는 오류 메시지가 즉시 표시됩니다. 이 메커니즘은 권한이 없는 애플리케이션 액세스를 방지하기 위해 OPC UA 서버 머신에 내장되어 있으며, 이는 작업 현장의 악성 해킹을 방지합니다.

## <a name="application-security-management"></a>애플리케이션 보안 관리
OPC 자격 증명 모음에는 인증서 레지스트리, 스토리지 및 수명 주기 관리를 위한 모든 기능이 있으므로 보안 전문가는 OPC 자격 증명 모음 마이크로서비스를 사용하여 OPC UA 서버가 모든 클라이언트 애플리케이션과 쉽게 통신할 수 있도록 합니다. 이제 OPC UA 서버가 안전하게 연결되어 새로 빌드된 클라이언트 애플리케이션과 통신할 수 있습니다.

## <a name="the-complete-opc-vault-architecture"></a>전체 OPC 자격 증명 모음 아키텍처
다음 다이어그램은 전체 OPC 자격 증명 모음 아키텍처를 보여줍니다.

![OPC 자격 증명 모음 아키텍처](media/overview-opc-vault-architecture/opc-vault.png)

## <a name="next-steps"></a>다음 단계

이제 OPC Vault와 그 용도에 대해 파악했으므로, 권장되는 단계는 다음과 같습니다.

[OPC 자격 증명 모음 아키텍처](overview-opc-vault-architecture.md)