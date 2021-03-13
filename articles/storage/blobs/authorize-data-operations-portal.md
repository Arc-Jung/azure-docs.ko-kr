---
title: Azure Portal에서 blob 데이터에 대 한 액세스 권한을 부여 하는 방법을 선택 합니다.
titleSuffix: Azure Storage
description: Azure Portal를 사용 하 여 blob 데이터에 액세스 하는 경우 포털은 내부적으로 Azure Storage에 대 한 요청을 수행 합니다. 이러한 Azure Storage에 대 한 요청은 Azure AD 계정 또는 저장소 계정 액세스 키를 사용 하 여 인증 하 고 권한을 부여할 수 있습니다.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 02/10/2021
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: blobs
ms.custom: contperf-fy21q1
ms.openlocfilehash: a12936f8f9f84dacfab4850253df665ae7758be1
ms.sourcegitcommit: d135e9a267fe26fbb5be98d2b5fd4327d355fe97
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2021
ms.locfileid: "102613252"
---
# <a name="choose-how-to-authorize-access-to-blob-data-in-the-azure-portal"></a>Azure Portal에서 blob 데이터에 대 한 액세스 권한을 부여 하는 방법을 선택 합니다.

[Azure Portal](https://portal.azure.com)를 사용 하 여 blob 데이터에 액세스 하는 경우 포털은 내부적으로 Azure Storage에 대 한 요청을 수행 합니다. Azure AD 계정 또는 저장소 계정 액세스 키 중 하나를 사용 하 여 Azure Storage에 대 한 요청을 인증할 수 있습니다. 포털은 사용 중인 방법을 나타내며, 적절 한 사용 권한이 있는 경우 둘 사이를 전환할 수 있습니다.  

Azure Portal에서 개별 blob 업로드 작업에 권한을 부여 하는 방법을 지정할 수도 있습니다. 기본적으로 포털은 blob 업로드 작업에 권한을 부여 하기 위해 이미 사용 하 고 있는 방법을 사용 하지만 blob을 업로드할 때이 설정을 변경할 수 있는 옵션이 있습니다.

## <a name="permissions-needed-to-access-blob-data"></a>Blob 데이터에 액세스 하는 데 필요한 권한

Azure Portal에서 blob 데이터에 대 한 액세스 권한을 부여 하려는 방법에 따라 특정 권한이 필요 합니다. 대부분의 경우 이러한 권한은 Azure 역할 기반 access control (Azure RBAC)을 통해 제공 됩니다. Azure RBAC에 대 한 자세한 내용은 azure [역할 기반 액세스 제어 란?](../../role-based-access-control/overview.md)을 참조 하세요.

### <a name="use-the-account-access-key"></a>계정 액세스 키 사용

계정 액세스 키를 사용 하 여 blob 데이터에 액세스 하려면 Azure RBAC 동작 **Microsoft. Storage/storageAccounts/listkeys/action** 을 포함 하는 azure 역할을 할당 받아야 합니다. 이 Azure 역할은 기본 제공 또는 사용자 지정 역할 일 수 있습니다. **Microsoft. Storage/storageAccounts/listkeys/action** 을 지 원하는 기본 제공 역할은 다음과 같습니다.

- Azure Resource Manager [소유자](../../role-based-access-control/built-in-roles.md#owner) 역할
- Azure Resource Manager [참가자](../../role-based-access-control/built-in-roles.md#contributor) 역할
- [저장소 계정 기여자](../../role-based-access-control/built-in-roles.md#storage-account-contributor) 역할

Azure Portal에서 blob 데이터에 액세스 하려고 하면 포털은 먼저 **Microsoft. Storage/storageAccounts/listkeys/action** 을 사용 하 여 역할이 할당 되었는지 여부를 확인 합니다. 이 작업을 사용 하 여 역할을 할당 한 경우 포털에서 blob 데이터에 액세스 하기 위해 계정 키를 사용 합니다. 이 작업을 사용 하 여 역할을 할당 하지 않은 경우 포털은 Azure AD 계정을 사용 하 여 데이터에 액세스 하려고 시도 합니다.

> [!IMPORTANT]
> Azure Resource Manager **ReadOnly** 잠금을 사용 하 여 저장소 계정이 잠긴 경우 해당 저장소 계정에 대 한 [키 나열](/rest/api/storagerp/storageaccounts/listkeys) 작업이 허용 되지 않습니다. **목록 키** 는 게시 작업 이며, 계정에 대해 **ReadOnly** 잠금이 구성 된 경우 모든 post 작업은 차단 됩니다. 이러한 이유로 계정이 **읽기 전용** 잠금으로 잠겨 있는 경우 사용자는 Azure AD 자격 증명을 사용 하 여 포털에서 blob 데이터에 액세스 해야 합니다. Azure AD를 사용 하 여 포털에서 blob 데이터에 액세스 하는 방법에 대 한 자세한 내용은 [AZURE ad 계정 사용](#use-your-azure-ad-account)을 참조 하세요.

> [!NOTE]
> 클래식 구독 관리자 역할 서비스 관리자 및 Co-Administrator에는 Azure Resource Manager [소유자](../../role-based-access-control/built-in-roles.md#owner) 역할에 해당 하는 항목이 포함 됩니다. **Owner** 역할은 **Microsoft. Storage/storageaccounts/listkeys/action** 을 비롯 한 모든 동작을 포함 하므로 이러한 관리 역할 중 하나가 있는 사용자는 계정 키를 사용 하 여 blob 데이터에 액세스할 수도 있습니다. 자세한 내용은 [클래식 구독 관리자 역할, Azure 역할 및 Azure AD 관리자 역할](../../role-based-access-control/rbac-and-directory-admin-roles.md#classic-subscription-administrator-roles)을 참조하세요.

### <a name="use-your-azure-ad-account"></a>Azure AD 계정 사용

Azure AD 계정을 사용 하 여 Azure Portal에서 blob 데이터에 액세스 하려면 다음 두 문이 모두 true 여야 합니다.

- 최소한 저장소 계정 수준으로 범위가 지정 된 Azure Resource Manager [읽기 권한자](../../role-based-access-control/built-in-roles.md#reader) 역할이 할당 되었습니다. **읽기 권한자** 역할은 가장 제한 된 권한을 부여 하지만 저장소 계정 관리 리소스에 대 한 액세스 권한을 부여 하는 다른 Azure Resource Manager 역할도 허용 됩니다.
- Blob 데이터에 대 한 액세스를 제공 하는 기본 제공 또는 사용자 지정 역할이 할당 되었습니다.

사용자가 Azure Portal에서 저장소 계정 관리 리소스를 보고 탐색할 수 있도록 **읽기** 역할 할당 또는 다른 Azure Resource Manager 역할 할당이 필요 합니다. Blob 데이터에 대 한 액세스 권한을 부여 하는 Azure 역할은 저장소 계정 관리 리소스에 대 한 액세스 권한을 부여 하지 않습니다. 포털에서 blob 데이터에 액세스 하려면 저장소 계정 리소스를 탐색할 수 있는 권한이 사용자에 게 필요 합니다. 이 요구 사항에 대 한 자세한 내용은 [포털 액세스를 위한 읽기 권한자 역할 할당](../common/storage-auth-aad-rbac-portal.md#assign-the-reader-role-for-portal-access)을 참조 하세요.

Blob 데이터에 대 한 액세스를 지 원하는 기본 제공 역할은 다음과 같습니다.

- [저장소 Blob 데이터 소유자](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner): Azure Data Lake Storage Gen2에 대 한 POSIX 액세스 제어입니다.
- [저장소 Blob 데이터 참가자](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor): blob에 대 한 읽기/쓰기/삭제 권한입니다.
- [저장소 Blob 데이터 판독기](../../role-based-access-control/built-in-roles.md#storage-blob-data-reader): blob에 대 한 읽기 전용 권한입니다.

사용자 지정 역할은 기본 제공 역할에서 제공 하는 것과 동일한 권한의 여러 조합을 지원할 수 있습니다. Azure 사용자 지정 역할을 만드는 방법에 대 한 자세한 내용은 azure [사용자 지정 역할](../../role-based-access-control/custom-roles.md) 및 [azure 리소스에 대 한 역할 정의 이해](../../role-based-access-control/role-definitions.md)를 참조 하세요.

> [!IMPORTANT]
> Azure Portal Storage 탐색기의 미리 보기 버전은 Azure AD 자격 증명을 사용 하 여 blob 데이터를 확인 하 고 수정 하는 기능을 지원 하지 않습니다. Azure Portal Storage 탐색기는 항상 계정 키를 사용 하 여 데이터에 액세스 합니다. Azure Portal에서 Storage 탐색기를 사용 하려면 **Microsoft. Storage/storageAccounts/listkeys/action** 이 포함 된 역할이 할당 되어야 합니다.

## <a name="navigate-to-blobs-in-the-azure-portal"></a>Azure Portal blob로 이동 합니다.

포털에서 blob 데이터를 보려면 저장소 계정에 대 한 **개요** 로 이동 하 고 **blob** 에 대 한 링크를 클릭 합니다. 또는 메뉴의 **컨테이너** 섹션으로 이동할 수 있습니다.

:::image type="content" source="media/authorize-data-operations-portal/blob-access-portal.png" alt-text="Azure Portal에서 blob 데이터를 탐색 하는 방법을 보여 주는 스크린샷":::

## <a name="determine-the-current-authentication-method"></a>현재 인증 방법 결정

컨테이너로 이동할 때 Azure Portal는 현재 계정 액세스 키를 사용 하 고 있는지 아니면 Azure AD 계정을 사용 하 고 있는지를 나타냅니다.

### <a name="authenticate-with-the-account-access-key"></a>계정 액세스 키를 사용 하 여 인증

계정 액세스 키를 사용 하 여 인증 하는 경우 포털에서 인증 방법으로 지정 된 **액세스 키** 가 표시 됩니다.

:::image type="content" source="media/authorize-data-operations-portal/auth-method-access-key.png" alt-text="계정 키를 사용 하 여 현재 컨테이너에 액세스 하는 사용자를 보여 주는 스크린샷":::

Azure AD 계정을 사용 하도록 전환 하려면 이미지에 강조 표시 된 링크를 클릭 합니다. 사용자에 게 할당 된 Azure 역할을 통해 적절 한 권한이 있는 경우 계속 진행할 수 있습니다. 그러나 적절 한 권한이 없으면 다음과 같은 오류 메시지가 표시 됩니다.

:::image type="content" source="media/authorize-data-operations-portal/auth-error-azure-ad.png" alt-text="Azure AD 계정이 액세스를 지원 하지 않는 경우 표시 되는 오류":::

Azure AD 계정에이를 볼 수 있는 권한이 없는 경우 목록에 blob이 표시 되지 않습니다. 액세스 키를 사용 하 여 인증 **키로 전환** 링크를 클릭 하 여 인증에 대 한 액세스 키를 다시 사용 합니다.

### <a name="authenticate-with-your-azure-ad-account"></a>Azure AD 계정을 사용하여 인증

Azure AD 계정을 사용 하 여 인증 하는 경우 포털에서 인증 방법으로 지정 된 **AZURE Ad 사용자 계정이** 표시 됩니다.

:::image type="content" source="media/authorize-data-operations-portal/auth-method-azure-ad.png" alt-text="현재 Azure AD 계정을 사용 하 여 컨테이너에 액세스 하는 사용자를 보여 주는 스크린샷":::

계정 액세스 키를 사용 하도록 전환 하려면 이미지에 강조 표시 된 링크를 클릭 합니다. 계정 키에 대 한 액세스 권한이 있는 경우 계속 진행할 수 있습니다. 그러나 계정 키에 대 한 액세스 권한이 없는 경우 다음과 같은 오류 메시지가 표시 됩니다.

:::image type="content" source="media/authorize-data-operations-portal/auth-error-access-key.png" alt-text="계정 키에 대 한 액세스 권한이 없는 경우 표시 되는 오류":::

계정 키에 대 한 액세스 권한이 없는 경우 목록에 blob이 표시 되지 않습니다. Azure ad **사용자 계정으로 전환** 링크를 클릭 하 여 인증에 azure ad 계정을 다시 사용 합니다.

## <a name="specify-how-to-authorize-a-blob-upload-operation"></a>Blob 업로드 작업에 권한을 부여 하는 방법 지정

Azure Portal에서 blob을 업로드 하는 경우 계정 액세스 키 또는 Azure AD 자격 증명을 사용 하 여 해당 작업을 인증 하 고 권한을 부여할 것인지 여부를 지정할 수 있습니다. 기본적으로 포털은 현재 인증 방법을 [결정](#determine-the-current-authentication-method)하는 것 처럼 현재 인증 방법을 사용 합니다.

Blob 업로드 작업에 권한을 부여 하는 방법을 지정 하려면 다음 단계를 수행 합니다.

1. Azure Portal에서 blob을 업로드 하려는 컨테이너로 이동 합니다.
1. **업로드** 단추를 선택합니다.
1. **고급** 섹션을 확장 하 여 blob에 대 한 고급 속성을 표시 합니다.
1. 다음 그림에 표시 된 것 처럼 **인증 유형** 필드에서 Azure AD 계정 또는 계정 액세스 키를 사용 하 여 업로드 작업에 권한을 부여할 것인지 여부를 지정 합니다.

    :::image type="content" source="media/authorize-data-operations-portal/auth-blob-upload.png" alt-text="Blob 업로드에 대 한 권한 부여 방법을 변경 하는 방법을 보여 주는 스크린샷":::

## <a name="next-steps"></a>다음 단계

- [Azure Active Directory를 사용 하 여 Azure blob 및 큐에 대 한 액세스 인증](../common/storage-auth-aad.md)
- [Azure Portal을 사용하여 Blob 및 큐 데이터에 액세스하기 위한 Azure 역할 할당](../common/storage-auth-aad-rbac-portal.md)
- [Azure CLI를 사용 하 여 blob 및 큐 데이터에 액세스 하기 위한 Azure 역할을 할당 합니다.](../common/storage-auth-aad-rbac-cli.md)
- [Azure PowerShell 모듈을 사용 하 여 blob 및 큐 데이터에 액세스 하기 위한 Azure 역할을 할당 합니다.](../common/storage-auth-aad-rbac-powershell.md)
