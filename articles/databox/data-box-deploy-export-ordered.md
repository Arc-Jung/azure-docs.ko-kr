---
title: Azure Data Box에서 데이터를 내보내는 자습서 | Microsoft Docs
description: 배포 필수 구성 요소 및 Azure Data Box에서 데이터를 내보내는 방법에 대해 알아봅니다.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: how-to
ms.date: 12/18/2020
ms.author: alkohli
ms.openlocfilehash: ac86dfba02b54730773a1d243981e85d563517e7
ms.sourcegitcommit: 0aec60c088f1dcb0f89eaad5faf5f2c815e53bf8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98183833"
---
# <a name="tutorial-create-export-order-for-azure-data-box"></a>자습서: Azure Data Box 내보내기 순서 만들기

Azure Data Box은 Azure에서 사용자의 위치로 데이터를 이동할 수 있도록 하는 하이브리드 솔루션입니다. 이 자습서에서는 Azure Data Box에 대 한 내보내기 주문을 만드는 방법을 설명 합니다. 내보내기 순서를 만드는 주된 이유는 재해 복구를 위한 것입니다. 온-프레미스 저장소가 손상 되 고 백업을 복원 해야 하는 경우가 여기에 해당 합니다.

이 자습서에서는 다음에 대해 알아봅니다.

> [!div class="checklist"]
>
> * 내보내기에 대 한 필수 구성 요소
> * 내보내기에 대 한 Data Box 주문
> * 내보내기 순서 추적
> * 내보내기 순서를 취소 합니다.

## <a name="prerequisites"></a>필수 구성 요소

장치를 주문 하기 전에 Data Box 서비스 및 장치에 대 한 다음 구성 필수 구성 요소를 완료 합니다.

### <a name="for-service"></a>서비스의 경우

[!INCLUDE [Data Box service prerequisites](../../includes/data-box-supported-subscriptions.md)]

* Azure Data Box 사용할 수 있는 기존 리소스 그룹이 있는지 확인 합니다.

* 데이터를 내보내려는 Azure Storage 계정이 [Data Box에 대해 지원 되는 저장소 계정](data-box-system-requirements.md#supported-storage-accounts)에 설명 된 대로 지원 되는 저장소 계정 유형 중 하나 인지 확인 합니다.

### <a name="for-device"></a>디바이스의 경우

시작하기 전에 다음 사항을 확인합니다.

* 호스트 컴퓨터를 데이터 센터 네트워크에 연결해야 합니다. Azure Data Box의 데이터를이 컴퓨터로 복사 합니다. [Azure Data Box 시스템 요구 사항](data-box-system-requirements.md)에 설명된 대로 호스트 컴퓨터는 지원되는 운영 체제를 실행해야 합니다.

* 데이터 센터에는 고속 네트워크가 있어야 합니다. 10GbE 연결이 하나 이상 있는 것이 좋습니다. 10GbE 연결을 사용할 수 없으면 1GbE 데이터 링크를 사용할 수 있지만 복사 속도에 영향을 미칩니다.

## <a name="order-data-box-for-export"></a>내보내기에 대 한 Data Box 주문

디바이스를 주문하려면 Azure Portal에서 다음 단계를 수행합니다.

1. Microsoft Azure 자격 증명을 사용하여 다음 URL에서 로그인합니다. [https://portal.azure.com](https://portal.azure.com)

2. **+ 리소스 만들기** 를 선택하고 *Azure Data Box* 를 검색합니다. **Azure Data Box** 를 선택합니다.

   ![리소스 만들기](media/data-box-deploy-export-ordered/azure-data-box-export-order-create-resource.png)

3. **만들기** 를 선택합니다.

   ![Azure Data Box 리소스 만들기](media/data-box-deploy-export-ordered/azure-data-box-export-order-create-data-box-resource.png)

4. 사용자의 지역에서 Azure Data Box 서비스를 사용할 수 있는지 확인 합니다. 다음 정보를 입력하거나 선택하고 **적용** 을 클릭합니다.

    |설정  |값  |
    |---------|---------|
    |전송 형식     | **Azure로 내보내기를** 선택 합니다.        |
    |Subscription     | Data Box 서비스에 대한 EA, CSP 또는 Azure 스폰서쉽 구독을 선택합니다. <br> 구독은 대금 청구 계정에 연결됩니다.       |
    |Resource group     |    기존 리소스 그룹을 선택합니다. <br> 리소스 그룹은 함께 관리하거나 배포할 수 있는 리소스에 대한 논리 컨테이너입니다.         |
    |원본 Azure 지역    |    현재 데이터가 있는 Azure 지역을 선택 합니다.         |
    |대상 국가     |     장치를 배송 하려는 국가를 선택 합니다.        |

   ![Data Box 설정 선택](media/data-box-deploy-export-ordered/azure-data-box-export-order-data-box-settings.png)

5. **Data Box** 를 선택합니다. 단일 주문의 최대 사용 가능한 용량은 80TB입니다. 더 큰 데이터 크기에 대해 여러 개의 주문을 만들 수 있습니다.

   ![Data Box 용량 선택](media/data-box-deploy-export-ordered/azure-data-box-export-order-capacity.png)

6. **순서** 대로 **기본** 주문 세부 정보를 지정 합니다. 다음 정보를 입력하거나 선택합니다.

    |설정  |값  |
    |---------|---------|
    |Subscription     | 이전에 선택한 내용에 따라 구독이 자동으로 채워집니다.|
    |Resource group | 이전에 선택한 리소스 그룹입니다. |
    |내보내기 주문 이름     |  주문을 추적하는 데 친숙한 이름을 입력합니다. <br> 이 이름은 2~24자 사이의 문자, 숫자 및 하이픈일 수 있습니다. <br> 이름은 문자 또는 숫자로 시작하고 끝나야 합니다.      |

    ![내보내기 주문 기본 사항](media/data-box-deploy-export-ordered/azure-data-box-export-order-basics-order-name.png)

    **다음: 데이터 선택** 을 선택 하 여 계속 진행 합니다.

7. **데이터 선택** 에서 **저장소 계정 추가 및 형식 내보내기** 를 선택 합니다.

    ![저장소 계정 및 내보내기 유형 추가](media/data-box-deploy-export-ordered/azure-data-box-export-order-basics-add-storage.png)

8. **내보내기 옵션 선택** 에서 내보내기 옵션 세부 정보를 지정 합니다. 다음 정보를 입력 하거나 선택 하 고 **추가** 를 선택 합니다.

    |설정  |값  |
    |---------|---------|
    |스토리지 계정     | 데이터를 내보낼 Azure Storage 계정입니다. |
    |내보내기 유형     | **모든 개체** 에서 내보내고 **XML 파일을 사용** 하는 데이터 형식을 지정 합니다.<ul><li> **모든 개체** - **전송 옵션** 에 대 한 선택 항목에 따라 작업에서 모든 데이터를 내보내도록 지정 합니다.</li><li> **Xml 파일 사용** – 저장소 계정에서 내보낼 blob 및/또는 파일의 경로 및 접두사 집합을 포함 하는 xml 파일을 지정 합니다. XML 파일은 선택한 저장소 계정의 컨테이너에 있어야 하 고, 파일 공유에서 선택 하는 기능은 현재 지원 되지 않습니다. 파일은 비어 있지 않은 .xml 파일 이어야 합니다.</li></ul>        |
    |전송 옵션     |  **모두 선택**, **모든 blob** 및 **모든 파일** 에서 데이터 전송 옵션을 지정 합니다. <ul><li> **모두 선택** -모든 blob 및 Azure Files을 내보내도록 지정 합니다. Blob만 지 원하는 저장소 계정 (Blob Storage 계정)을 사용 하는 경우 **모든 파일** 옵션은 선택할 수 없습니다.</li><li> **모든 blob** -블록 및 페이지 blob만 내보내도록 지정 합니다.</li><li> **모든 파일** -blob을 제외 하 고 모든 파일을 내보내도록 지정 합니다. 저장소 계정 유형 (GPv1 및 GPv2, premium storage 또는 blob storage)은 내보낼 수 있는 데이터의 유형을 결정 합니다. 자세한 내용은 [지원 되는 저장소 계정 내보내기](../storage/common/storage-import-export-requirements.md#supported-storage-types)를 참조 하세요.</li></ul>         |
    |자세한 로그 포함     | 성공적으로 내보낸 모든 파일의 목록을 포함 하는 자세한 로그 파일이 필요한 지 여부를 나타냅니다.        |

    > [!NOTE]
    >
    > **내보내기 유형** 설정에 **xml 파일 사용** 을 선택 하는 경우 xml에 유효한 경로 및/또는 접두사가 포함 되어 있는지 확인 해야 합니다. XML 파일을 생성 하 고 제공 해야 합니다.  파일이 잘못 되었거나 지정 된 경로와 일치 하는 데이터가 없는 경우에는 주문이 부분 데이터로 종료 되거나 데이터를 내보내지 않습니다.

    컨테이너에 XML 파일을 추가 하는 방법에 대 한 자세한 내용은 [xml 파일을 사용 하 여 순서 내보내기](data-box-deploy-export-ordered.md#export-order-using-xml-file)를 참조 하세요.

   ![내보내기 옵션 선택](media/data-box-deploy-export-ordered/azure-data-box-export-order-export-option.png)

   Xml 입력의 예를 보려면 [샘플 xml 입력](data-box-deploy-export-ordered.md#sample-xml-file) 을 참조 하세요.

9. **데이터 선택** 에서 설정을 검토 하 고 **다음: 보안>** 를 선택 하 여 계속 합니다.

   ![내보내기 순서, 데이터 선택](media/data-box-deploy-export-ordered/azure-data-box-export-order-data-selection.png)

    **보안** 화면에서는 사용자의 암호화 키를 사용 하 여 이중 암호화를 사용 하도록 선택할 수 있습니다.

    **보안** 화면의 모든 설정은 선택 사항입니다. 설정을 변경하지 않으면 기본 설정이 적용됩니다.

    ![Data Box 가져오기 주문 마법사의 보안 화면](media/data-box-deploy-export-ordered/data-box-export-security-01.png)

10. 사용자 고유의 고객 관리형 키를 사용하여 새 리소스에 대한 잠금 해제 암호를 보호하려면 **암호화 유형** 을 펼칩니다.

    Azure Data Box에 대한 고객 관리형 키 구성은 선택 사항입니다. 기본적으로 Data Box는 Microsoft 관리형 키를 사용하여 잠금 해제 암호를 보호합니다.

    고객 관리형 키는 디바이스의 데이터가 암호화되는 방식에 영향을 주지 않습니다. 키는 디바이스 잠금 해제 암호를 암호화하는 데만 사용됩니다.

    고객 관리 키를 사용 하지 않으려면 16 단계로 건너뜁니다.

    ![암호화 유형 설정을 보여 주는 보안 화면](./media/data-box-deploy-export-ordered/customer-managed-key-01.png)

11. 키 유형으로 **고객 관리형 키** 를 선택합니다. 그런 다음, **Key Vault 및 키 선택** 을 선택합니다.
   
    ![보안 화면, 고객 관리형 키 설정](./media/data-box-deploy-export-ordered/customer-managed-key-02.png)

12. **Azure Key Vault에서 키 선택** 화면에 구독이 자동으로 채워집니다.

    - **키 자격 증명 모음** 의 드롭다운 목록에서 기존 키 자격 증명 모음을 선택할 수 있습니다.

      ![Azure Key Vault에서 키 선택 화면](./media/data-box-deploy-export-ordered/customer-managed-key-03.png)

    - **새로 만들기** 를 선택하여 새로운 키 자격 증명 모음을 만들 수도 있습니다. **키 자격 증명 모음 만들기** 화면에서 리소스 그룹 및 키 자격 증명 모음 이름을 입력합니다. **일시 삭제** 및 **보호 제거** 를 사용하도록 설정되어 있는지 확인합니다. 다른 모든 기본값을 적용하고, **검토 + 만들기** 를 선택합니다.

      ![새 Azure Key Vault 설정 만들기](./media/data-box-deploy-export-ordered/customer-managed-key-04.png)

      키 자격 증명 모음에 대한 정보를 검토하고, **만들기** 를 선택합니다. 키 자격 증명 모음 만들기가 완료될 때까지 몇 분 정도 기다립니다.

      ![새 Azure Key Vault 검토 화면](./media/data-box-deploy-export-ordered/customer-managed-key-05.png)

13. **Azure Key Vault에서 키 선택** 화면의 키 자격 증명 모음에서 기존 키를 선택할 수 있습니다.

    ![Azure Key Vault에서 기존 키 선택](./media/data-box-deploy-export-ordered/customer-managed-key-06.png)

    새 키를 만들려면 **새로 만들기** 를 선택합니다. RSA 키를 사용해야 합니다. 크기는 2048 이상일 수 있습니다. 새 키에 대한 이름을 입력하고, 다른 기본값을 적용한 다음, **만들기** 를 선택합니다.

      ![새 키 만들기 옵션](./media/data-box-deploy-export-ordered/customer-managed-key-07.png)

      키가 키 자격 증명 모음에 만들어지면 알림이 표시됩니다.

14. 사용할 키의 **버전** 을 선택한 다음, **선택** 을 선택합니다.

      ![키 자격 증명 모음에 새로 생성된 키](./media/data-box-deploy-export-ordered/customer-managed-key-08.png)

    새 키 버전을 만들려면 **새로 만들기** 를 선택합니다.

    ![새 키 버전을 만들기 위한 대화 상자 열기](./media/data-box-deploy-export-ordered/customer-managed-key-08-a.png)

    **새 키 만들기** 화면에서 새 키 버전에 대 한 설정을 선택 하 고 **만들기** 를 선택 합니다.

    ![새로운 키 버전 만들기](./media/data-box-deploy-export-ordered/customer-managed-key-08-b.png)

    **보안** 화면의 **암호화 유형** 설정에 키 자격 증명 모음 및 키가 표시됩니다.

    ![고객 관리형 키에 대한 키 및 키 자격 증명 모음](./media/data-box-deploy-export-ordered/customer-managed-key-09.png)

15. 이 리소스에 대한 액세스를 관리하는 데 사용할 사용자 ID를 선택합니다. **사용자 ID 선택** 을 선택합니다. 오른쪽 패널에서 사용할 구독 및 관리 ID를 선택합니다. 그런 다음, **선택** 을 선택합니다.

    사용자가 할당한 관리 ID는 여러 리소스를 관리하는 데 사용할 수 있는 독립 실행형 Azure 리소스입니다. 자세한 내용은 [관리 ID 유형](/azure/active-directory/managed-identities-azure-resources/overview)을 참조하세요.  

    관리 id를 새로 만들어야 하는 경우에는 [Azure Portal를 사용 하 여 사용자 할당 관리 id에 대 한 역할 만들기, 나열, 삭제 또는 할당](../../articles/active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)의 지침을 따르세요.
    
    ![사용자 ID 선택](./media/data-box-deploy-export-ordered/customer-managed-key-10.png)

    사용자 ID는 **암호화 유형** 설정에 표시됩니다.

    **암호화 유형** 설정을 지금 축소할 수 있습니다.

    ![암호화 유형 설정에 표시된 선택한 사용자 ID](./media/data-box-deploy-export-ordered/customer-managed-key-11.png)

16. 소프트웨어 기반 이중 암호화를 사용 하도록 설정 하려면 **이중 암호화 (보안 우선 환경)** 를 확장 하 고 **순서에 대해 이중 암호화 사용** 을 선택 합니다. 

    소프트웨어 기반 암호화는 Data Box 데이터의 AES-256비트 암호화와 함께 수행됩니다.

    > [!NOTE]
    > 이 옵션을 사용하도록 설정하면 주문 처리 및 데이터 복사 시간이 더 길어질 수 있습니다. 주문을 만든 후에는 이 옵션을 변경할 수 없습니다.

    ![데이터 상자 가져오기, 이중 암호화를 위한 보안 화면](media/data-box-deploy-export-ordered/azure-data-box-export-order-security-double-encryption.png)

    완료되면 **다음: 연락처 정보** 를 선택하여 계속 진행합니다.

11. **연락처 세부 정보** 에서 **+ 배송 주소 추가** 를 선택 하 여 배송 정보를 입력 합니다.

    ![배송 주소 추가](media/data-box-deploy-export-ordered/azure-data-box-export-order-add-shipping-address.png)

12. **배송 주소 추가** 에서 회사의 성과 이름, 이름 및 우편 주소를 입력 하 고 유효한 전화 번호를 입력 합니다. **유효성 검사** 를 선택합니다. 서비스에서 서비스 가용성을 위해 배송 주소의 유효성을 검사합니다. 지정한 배송 주소에 대해 서비스를 사용할 수 있으면 해당 알림을 받게 됩니다.

    ![배송 주소 유효성 검사](media/data-box-deploy-export-ordered/azure-data-box-export-order-validate-shipping-address.png)

    자체 관리 전달 기능을 사용할 수 있는 지역에서 순서를 지정 하는 경우이 옵션을 선택할 수 있습니다. 자체 관리형 배송에 대한 자세한 내용은 [자체 관리형 배송 사용](data-box-portal-customer-managed-shipping.md)을 참조하세요.

13. 배송 세부 정보의 유효성을 검사 하 고 나면 **배송 주소 추가** 를 선택 합니다.

14. **연락처 세부 정보** 에서 배송 주소와 전자 메일 주소를 검토 합니다. 서비스에서는 주문 상태에 대한 모든 업데이트와 관련된 이메일 알림을 지정한 이메일 주소로 보냅니다.

    그룹의 관리자가 떠나는 경우에도 계속 알림을 받으려면 그룹 이메일을 사용하는 것이 좋습니다.

    ![연락처 세부 정보](media/data-box-deploy-export-ordered/azure-data-box-export-order-contact-details.png)

15. **다음: 검토 + 순서>** 를 선택 합니다. 주문 만들기를 계속 하려면 사용 약관에 동의 해야 합니다.

16. **주문** 을 선택합니다. 주문을 만드는 데 몇 분 정도 걸립니다.

    ![커밋 순서](media/data-box-deploy-export-ordered/azure-data-box-select-export-order-commit-order.png)

## <a name="export-order-using-xml-file"></a>XML 파일을 사용 하 여 주문 내보내기

**XML 파일 사용** 을 선택 하는 경우 내보내려는 특정 컨테이너와 blob (페이지 및 블록)를 지정할 수 있습니다. XML 서식 지정에 대 한 [예제 xml 파일 테이블](#sample-xml-file) 사양을 따라야 합니다. 아래 단계에서는 XML 파일을 사용 하 여 데이터를 내보내는 방법을 보여 줍니다.

1. **내보내기 유형** 에서 **XML 파일 사용** 을 선택 합니다. 내보내려는 특정 blob 및 Azure 파일을 지정 하는 XML 파일입니다. XML 파일을 추가 하려면 **xml 파일을 선택 하려면 여기를 클릭** 하십시오 .를 선택 합니다.

     ![내보내기 옵션, XML 선택](media/data-box-deploy-export-ordered/azure-data-box-export-sms-use-xml-file-select-xml-option.png)

2. **+ 컨테이너** 를 선택 하 여 컨테이너를 만듭니다.

    ![내보내기 옵션, 컨테이너를 선택 합니다.](media/data-box-deploy-export-ordered/azure-data-box-export-sms-use-xml-file-containers-option.png)

3. Azure Portal 오른쪽에서 팝 되는 **새 컨테이너** 탭에서 컨테이너의 이름을 추가 합니다. 이름은 소문자 여야 하 고 숫자 및 대시 '-'를 포함할 수 있습니다. 그런 다음 드롭다운 목록 상자에서 **공용 액세스 수준을** 선택 합니다. 다른 사용자가 데이터에 액세스 하지 못하도록 하려면 **개인 (익명이 아닌 액세스)** 을 선택 하는 것이 좋습니다. 컨테이너 액세스 수준에 대 한 자세한 내용은 [컨테이너 액세스 권한](../storage/blobs/anonymous-read-access-configure.md#set-the-public-access-level-for-a-container)을 참조 하세요.

   ![내보내기 옵션, 새 컨테이너 설정을 선택 합니다.](media/data-box-deploy-export-ordered/azure-data-box-export-sms-use-xml-file-container-settings.png)

4. **만들기** 를 선택합니다.

   ![내보내기 옵션을 선택 하 고 새 컨테이너를 만듭니다.](media/data-box-deploy-export-ordered/azure-data-box-export-sms-use-xml-create-container.png)

   컨테이너가 성공적으로 생성 되 면 다음과 같은 메시지가 표시 됩니다.

   ![컨테이너를 만들었습니다.](media/data-box-deploy-export-ordered/azure-data-box-export-sms-use-xml-container-success.png)

5. 만든 컨테이너를 선택 하 고 두 번 클릭 합니다.

   ![컨테이너 세부 정보 보기](media/data-box-deploy-export-ordered/azure-data-box-export-sms-use-xml-view-container-details.png)

6. 컨테이너를 두 번 클릭 하면 컨테이너 속성 보기가 표시 됩니다. 이제 내보낼 blob 및/또는 Azure Files 목록이 포함 된 XML 파일을 첨부 (또는 탐색) 하려고 합니다. **업로드** 를 선택합니다.

   ![컨테이너에 blob 업로드](media/data-box-deploy-export-ordered/azure-data-box-export-sms-use-xml-blob-to-container.png)

7. 컨테이너에 XML 파일을 추가 했습니다. 이 XML에서 지정한 blob 및 Azure Files만 내보내집니다.

   ![컨테이너에 추가 된 XML 파일](media/data-box-deploy-export-ordered/azure-data-box-export-sms-use-xml-file-added-to-container.png)

## <a name="track-the-order"></a>주문 추적

주문이 완료되면 Azure Portal에서 주문 상태를 추적할 수 있습니다. Data Box 주문, **개요** 로 차례로 이동하여 상태를 확인합니다. 포털에서는 해당 주문을 **주문됨** 상태로 표시합니다.

장치 준비가 완료 되 면 선택한 저장소 계정에서 데이터 복사가 시작 됩니다. 포털에서는 **데이터 복사 진행** 중 상태의 순서를 표시 합니다.

![Data Box 내보내기 순서, 데이터 복사 진행 중](media/data-box-deploy-export-ordered/azure-data-box-export-order-data-copy-in-progress.png)

Data Box은 원본 저장소 계정에서 데이터를 복사 합니다. 데이터 복사가 완료 되 면 Data Box 잠기고 포털에 **복사 완료 됨** 상태가 표시 됩니다.

![Data Box 내보내기 순서, 데이터 복사 완료](media/data-box-deploy-export-ordered/azure-data-box-export-order-data-copy-complete.png)

장치를 사용할 수 없는 경우 알림을 받게 됩니다. 디바이스를 사용할 수 있으면 Microsoft에서는 배송할 디바이스를 확인하고 배송을 준비합니다. 장치를 준비 하는 동안 다음 작업이 수행 됩니다.

* 디바이스와 연결된 각 스토리지 계정에 대해 SMB 공유가 생성됩니다.
* 각 공유에 대한 사용자 이름 및 암호와 같은 액세스 자격 증명이 생성됩니다.
* 장치가 잠겨 있으며 장치 잠금 해제 암호를 사용 하 여 액세스할 수 있습니다. 암호를 검색 하려면 Azure Portal 계정에 로그인 하 고 **장치 세부 정보** 를 선택 해야 합니다.

그러면 Microsoft에서 지역 캐리어를 통해 장치를 준비 하 고 디스패치합니다. 디바이스가 배송되면 추적 번호를 받게 됩니다. 포털에서 해당 작업을 **발송됨** 상태로 표시합니다.

![Data Box 내보내기 주문 디스패치](media/data-box-deploy-export-ordered/azure-data-box-export-order-dispatched.png)

자체 관리 배송을 선택한 경우 데이터 센터에서 장치를 선택할 준비가 되 면 다음 단계와 함께 전자 메일 알림을 받게 됩니다. 자체 관리 배송에 대 한 자세한 내용은 [자체 관리 전달](data-box-portal-customer-managed-shipping.md)을 참조 하세요.

![픽업에 대해 자체 관리 되는 배송 준비](media/data-box-deploy-export-ordered/azure-data-box-export-order-ready-for-pickup.png)

## <a name="cancel-the-order"></a>주문 취소

이 주문을 취소하려면 Azure Portal에서 **개요** 로 이동하고, 명령 모음에서 **취소** 를 선택합니다.

주문을 처리 하기 전에 언제 든 지 취소할 수 있습니다.

취소된 주문을 삭제하려면 **개요** 로 이동하고, 명령 모음에서 **삭제** 를 선택합니다.

## <a name="sample-xml-file"></a>샘플 XML 파일

다음 xml은 **xml 파일 사용** 옵션을 선택할 때 내보내기 순서에서 사용 하는 xml 형식에 포함 된 blob 이름, blob 접두사 및 Azure Files의 예를 보여 줍니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
   <!-- BlobList/prefix/Container list for Blob storage for export  -->
   <BlobList>
      <BlobPath>/8tbpageblob/8tbpageblob/8tbpageblob</BlobPath>
      <BlobPathPrefix>/blockblob4dot75tbdata/</BlobPathPrefix>
      <BlobPathPrefix>/1tbfilepageblob</BlobPathPrefix>
      <BlobPathPrefix>/1tbfile/</BlobPathPrefix>
      <BlobPathPrefix>/8mbfiles/</BlobPathPrefix>
      <BlobPathPrefix>/64mbfiles/</BlobPathPrefix>
   </BlobList>
   <!-- FileList/prefix/Share list for Azure File storage for export  -->
   <AzureFileList>
      <FilePathPrefix>/64mbfiles/</FilePathPrefix>
      <FilePathPrefix>/4mbfiles/prefix2/subprefix</FilePathPrefix>
      <FilePathPrefix>/1tbfile/prefix</FilePathPrefix>
   </AzureFileList>
```

Xml 파일에 대 한 몇 가지 중요 한 사항은 다음과 같습니다.

* Xml 태그는 대/소문자를 구분 하므로 위의 샘플에 지정 된 것과 정확 하 게 일치 해야 합니다.
* 여는 태그와 닫는 태그는 일치 해야 합니다.
* Xml 태그나 형식이 잘못 되 면 데이터 내보내기 오류가 발생할 수 있습니다.
* Blob 및/또는 파일 접두사가 잘못 된 경우 데이터를 내보낼 수 없습니다.

### <a name="examples-of-valid-blob-paths"></a>유효한 Blob 경로의 예

다음 표에는 유효한 Blob 경로의 예가 있습니다.

   | 선택기 | Blob 경로 | Description |
   | --- | --- | --- |
   | 다음으로 시작 |/ |스토리지 계정의 모든 Blob을 내보냄 |
   | 다음으로 시작 |/$root/ |루트 컨테이너의 모든 Blob을 내보냄 |
   | 다음으로 시작 |/컨테이너 |접두사 **컨테이너로** 시작 하는 모든 컨테이너의 모든 blob을 내보냅니다. |
   | 다음으로 시작 |/container-name/ |컨테이너 **컨테이너 이름** 에 있는 모든 blob을 내보냅니다. |
   | 다음으로 시작 |/container-name/prefix |접두사 **접두사로** 시작 하는 컨테이너 **컨테이너 이름** 에 있는 모든 blob을 내보냅니다. |
   | 같음 |$root/logo.bmp |루트 컨테이너의 Blob **logo.bmp** 를 내보냄 |
   | 같음 |8tbpageblob/mydata.txt |컨테이너 **8tbpageblob** blob **mydata.txt** 를 내보냅니다. |

## <a name="sample-log-files"></a>샘플 로그 파일

이 섹션에서는 내보내기 중 생성 되는 샘플 로그 파일을 제공 합니다. 오류 로그는 자동으로 생성 됩니다. 자세한 정보 표시 로그 파일을 생성 하려면 내보내기 순서를 구성할 때 Azure Portal에 **자세한 정보 표시 로그 포함** 을 선택 해야 합니다.
복사 및 자세한 정보 로그에 대 한 자세한 내용은 [로그 복사](data-box-deploy-export-copy-data.md#copy-data-from-data-box)를 참조 하세요.

<!-- ### Verbose log

The following log files show examples of verbose logging when you select **Include verbose log**:

```xml
<File CloudFormat="BlockBlob" Path="validblobdata/test1.2.3.4" Size="1024" crc64="7573843669953104266"></File>
<File CloudFormat="BlockBlob" Path="validblobdata/helloEndWithDot..txt" Size="11" crc64="7320094093915972193"></File>
<File CloudFormat="BlockBlob" Path="validblobdata/test..txt" Size="12" crc64="17906086011702236012"></File>
<File CloudFormat="BlockBlob" Path="validblobdata/test1" Size="1024" crc64="7573843669953104266"></File>
<File CloudFormat="BlockBlob" Path="validblobdata/test1.2.3" Size="1024" crc64="7573843669953104266"></File>
<File CloudFormat="BlockBlob" Path="validblobdata/.......txt" Size="11" crc64="7320094093915972193"></File>
<File CloudFormat="BlockBlob" Path="validblobdata/copylogb08fa3095564421bb550d775fff143ed====..txt" Size="53638" crc64="1147139997367113454"></File>
<File CloudFormat="BlockBlob" Path="validblobdata/testmaxChars-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-12345679" Size="1024" crc64="7573843669953104266"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/file0" Size="0" crc64="0"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/file1" Size="0" crc64="0"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/file4096_000001" Size="4096" crc64="16969371397892565512"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/file4096_000000" Size="4096" crc64="16969371397892565512"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/64KB-Seed10.dat" Size="65536" crc64="10746682179555216785"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/LiveSiteReport_Oct.xlsx" Size="7028" crc64="6103506546789189963"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/NE_Oct_GeoReport.xlsx" Size="103197" crc64="13305485882546035852"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/64KB-Seed1.dat" Size="65536" crc64="3140622834011462581"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/1mbfiles-0-0" Size="1048576" crc64="16086591317856295272"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/file524288_000001" Size="524288" crc64="8908547729214703832"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/4mbfiles-0-0" Size="4194304" crc64="1339017920798612765"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/file524288_000000" Size="524288" crc64="8908547729214703832"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/8mbfiles-0-1" Size="8388608" crc64="3963298606737216548"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/1mbfiles-0-1" Size="1048576" crc64="11061759121415905887"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/XLS-10MB.xls" Size="1199104" crc64="2218419493992437463"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/8mbfiles-0-0" Size="8388608" crc64="1072783424245035917"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/4mbfiles-0-1" Size="4194304" crc64="9991307204216370812"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/VL_Piracy_Negtive10_TPNameAndGCS.xlsx" Size="12398699" crc64="13526033021067702820"></File>
```

### Copy logs

For more information regarding copy logs, see [Copy logs](data-box-deploy-export-copy-data.md#copy-data-from-data-box). -->

<!-- The following xml shows an example of the copy log when the export is successful:

```xml
<CopyLog Summary="Summary">
  <Status>Succeeded</Status>
    <TotalFiles_Blobs>27</TotalFiles_Blobs>
    <FilesErrored>0</FilesErrored>
</CopyLog>
```

For more information regarding copy logs, see [Copy logs](data-box-deploy-export-copy-data.md#copy-data-from-data-box).

The following xml shows an example of the copy log when the export has errors:

```xml
<ErroredEntity CloudFormat="AppendBlob" Path="export-ut-appendblob/wastorage.v140.3.0.2.nupkg">
  <Category>UploadErrorCloudHttp</Category>
  <ErrorCode>400</ErrorCode>
  <ErrorMessage>UnsupportBlobType</ErrorMessage>
  <Type>File</Type>
</ErroredEntity><ErroredEntity CloudFormat="AppendBlob" Path="export-ut-appendblob/xunit.console.Primary_2020-05-07_03-54-42-PM_27444.hcsml">
  <Category>UploadErrorCloudHttp</Category>
  <ErrorCode>400</ErrorCode>
  <ErrorMessage>UnsupportBlobType</ErrorMessage>
  <Type>File</Type>
</ErroredEntity><ErroredEntity CloudFormat="AppendBlob" Path="export-ut-appendblob/xunit.console.Primary_2020-05-07_03-54-42-PM_27444 (1).hcsml">
  <Category>UploadErrorCloudHttp</Category>
  <ErrorCode>400</ErrorCode>
  <ErrorMessage>UnsupportBlobType</ErrorMessage>
  <Type>File</Type>
</ErroredEntity><CopyLog Summary="Summary">
  <Status>Failed</Status>
  <TotalFiles_Blobs>4</TotalFiles_Blobs>
  <FilesErrored>3</FilesErrored>
</CopyLog>
``` -->

## <a name="next-steps"></a>다음 단계

이 자습서에서는 Azure Data Box 항목에 대해 다음과 같은 내용을 알아보았습니다.

> [!div class="checklist"]
>
> * 내보내기에 대 한 필수 구성 요소
> * 내보내기에 대 한 Data Box 주문
> * 내보내기 순서 추적
> * 내보내기 순서를 취소 합니다.

Data Box를 설정하는 방법을 알아보려면 다음 자습서로 계속 진행하세요.

> [!div class="nextstepaction"]
> [Azure Data Box 설정](./data-box-deploy-set-up.md)
