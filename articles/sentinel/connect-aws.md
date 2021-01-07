---
title: AWS CloudTrail를 Azure 센티널에 연결 | Microsoft Docs
description: AWS 커넥터를 사용 하 여 Azure 센티널 액세스를 AWS 리소스 로그에 위임 하 고 AWS CloudTrail와 센티널 간의 트러스트 관계를 만듭니다.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/27/2020
ms.author: yelevin
ms.openlocfilehash: a7405824d2477d2d39c45a56ae545e58a090c321
ms.sourcegitcommit: 5e5a0abe60803704cf8afd407784a1c9469e545f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96436609"
---
# <a name="connect-azure-sentinel-to-aws-cloudtrail"></a>AWS CloudTrail에 Azure 센티널 연결

AWS 커넥터를 사용 하 여 AWS CloudTrail 관리 이벤트를 Azure 센티널로 스트리밍합니다. 이 연결 프로세스는 AWS 리소스 로그에 Azure 센티널에 대 한 액세스를 위임 하 여 AWS CloudTrail와 Azure 센티널 간의 트러스트 관계를 만듭니다. 이는 AWS 로그에 액세스할 수 있도록 Azure 센티널에 권한을 부여 하는 역할을 만들어 AWS에서 수행 됩니다.

> [!NOTE]
> AWS CloudTrail의 LookupEvents API에는 [기본 제공 되는 제한 사항이](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/WhatIsCloudTrail-Limits.html) 있습니다. 계정 당 초당 2 개 이상의 트랜잭션을 허용 하며 각 쿼리는 최대 50 개의 레코드를 반환할 수 있습니다. 따라서 한 지역에서 단일 테 넌 트가 초당 100 개 보다 많은 레코드를 생성 하는 경우에는 데이터 수집의 백로그 및 지연이 발생 합니다.

## <a name="prerequisites"></a>전제 조건

Azure 센티널 작업 영역에 대 한 쓰기 권한이 있어야 합니다.

> [!NOTE]
> Azure 센티널은 모든 지역에서 CloudTrail 관리 이벤트를 수집 합니다. 한 지역에서 다른 지역으로 이벤트를 스트리밍하는 것이 좋습니다.

## <a name="connect-aws"></a>AWS 연결 


1. Azure 센티널에서 **데이터 커넥터** 를 선택한 다음 테이블에서 **Amazon Web Services** 줄을 선택 하 고 오른쪽에 있는 AWS 창에서 **커넥터 페이지 열기** 를 클릭 합니다.

1. 다음 단계를 사용 하 여 **구성** 의 지침을 따릅니다.
 
1.  Amazon Web Services 콘솔의 **보안, id & 준수** 에서 **IAM** 을 선택 합니다.

    ![AWS1](./media/connect-aws/aws-1.png)

1.  **역할** 을 선택 하 고 **역할 만들기** 를 선택 합니다.

    ![AWS2](./media/connect-aws/aws-2.png)

1.  **다른 AWS 계정을 선택 합니다.** **계정 id** 필드에 Azure 센티널 포털의 AWS 커넥터 페이지에서 찾을 수 있는 **Microsoft 계정 id** (**123412341234**)를 입력 합니다.

    ![AWS3](./media/connect-aws/aws-3.png)

1.  **외부 Id 필요** 가 선택 되어 있는지 확인 하 고 Azure 센티널 포털의 AWS 커넥터 페이지에서 찾을 수 있는 외부 Id (작업 영역 id)를 입력 합니다.

    ![AWS4](./media/connect-aws/aws-4.png)

1.  **권한 연결 정책** 에서 **AWSCloudTrailReadOnlyAccess** 를 선택 합니다.

    ![AWS5](./media/connect-aws/aws-5.png)

1.  태그를 입력 합니다 (선택 사항).

    ![AWS6](./media/connect-aws/aws-6.png)

1.  그런 다음 **역할 이름을** 입력 하 고 **역할 만들기** 단추를 선택 합니다.

    ![AWS7](./media/connect-aws/aws-7.png)

1.  역할 목록에서 만든 역할을 선택 합니다.

    ![AWS8](./media/connect-aws/aws-8.png)

1.  **역할 ARN** 을 복사 합니다. Azure 센티널 포털의 Amazon Web Services 커넥터 화면에서 **추가할 역할** 에 해당 필드를 붙여넣고 **추가** 를 클릭 합니다.

    ![AWS9](./media/connect-aws/aws-9.png)

1. AWS 이벤트에 대 한 Log Analytics에서 관련 스키마를 사용 하려면 **Awscloudtrail** 를 검색 합니다.

    > [!IMPORTANT]
    > 2020 년 12 월 1 일부 터 **Awsrequestid** 필드는 **AwsRequestId_** 필드로 대체 되었습니다 (추가 된 밑줄을 참조). 이전 **Awsrequestid** 필드의 데이터는 고객의 지정 된 데이터 보존 기간이 끝날 때까지 유지 됩니다.

## <a name="next-steps"></a>다음 단계
이 문서에서는 AWS CloudTrail를 Azure 센티널에 연결 하는 방법을 알아보았습니다. Azure Sentinel에 대한 자세한 내용은 다음 문서를 참조하세요.
- [데이터에 대한 가시성을 얻고 재적 위협을 확인](quickstart-get-visibility.md)하는 방법을 알아봅니다.
- [Azure Sentinel을 사용하여 위협 검색](tutorial-detect-threats-built-in.md)을 시작합니다.
- [통합 문서를 사용](tutorial-monitor-your-data.md)하여 데이터를 모니터링합니다.
