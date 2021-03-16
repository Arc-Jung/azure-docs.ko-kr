---
title: 'PowerShell을 사용 하 여 데이터 관리: Azure Data Lake Storage Gen2'
description: PowerShell cmdlet을 사용 하 여 계층적 네임 스페이스를 사용 하는 저장소 계정의 디렉터리와 파일을 관리 합니다.
services: storage
author: normesta
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.topic: how-to
ms.date: 02/17/2021
ms.author: normesta
ms.reviewer: prishet
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 552d53ff0257105ff61397e281504c5270512319
ms.sourcegitcommit: 87a6587e1a0e242c2cfbbc51103e19ec47b49910
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/16/2021
ms.locfileid: "103573866"
---
# <a name="use-powershell-to-manage-directories-and-files-in-azure-data-lake-storage-gen2"></a>PowerShell을 사용 하 여 Azure Data Lake Storage Gen2에서 디렉터리 및 파일 관리

이 문서에서는 PowerShell을 사용 하 여 계층적 네임 스페이스를 포함 하는 저장소 계정의 디렉터리와 파일을 만들고 관리 하는 방법을 보여 줍니다.

디렉터리 및 파일의 ACL (액세스 제어 목록)을 가져오고, 설정 하 고, 업데이트 하는 방법에 대 한 자세한 내용은 [PowerShell을 사용 하 여 Azure Data Lake Storage Gen2에서 acl 관리](data-lake-storage-acl-powershell.md)를 참조 하세요.

[참조](/powershell/module/Az.Storage/)  |  [Gen1 To Gen2 mapping](#gen1-gen2-map)  |  [사용자 의견 제공](https://github.com/Azure/azure-powershell/issues)

## <a name="prerequisites"></a>필수 조건

- Azure 구독 [Azure 평가판](https://azure.microsoft.com/pricing/free-trial/)을 참조하세요.

- 계층적 네임 스페이스를 사용 하는 저장소 계정입니다. 만들려면 [다음 지침](create-data-lake-storage-account.md)을 수행합니다.

- .NET Framework 4.7.2 이상 설치 되어 있습니다. [다운로드 .NET Framework](https://dotnet.microsoft.com/download/dotnet-framework)를 참조 하세요.

- PowerShell 버전 `5.1` 이상.

## <a name="install-the-powershell-module"></a>PowerShell 모듈 설치

1. 다음 명령을 사용 하 여 설치한 PowerShell 버전이 이상 인지 확인 `5.1` 합니다.    

   ```powershell
   echo $PSVersionTable.PSVersion.ToString() 
   ```

   PowerShell 버전을 업그레이드 하려면 [기존 Windows Powershell 업그레이드](/powershell/scripting/install/installing-windows-powershell#upgrading-existing-windows-powershell) 를 참조 하세요.

2. 설치 **Az. Storage** module.

   ```powershell
   Install-Module Az.Storage -Repository PSGallery -Force  
   ```

   PowerShell 모듈을 설치 하는 방법에 대 한 자세한 내용은 [Azure PowerShell 모듈 설치](/powershell/azure/install-az-ps) 를 참조 하세요.

## <a name="connect-to-the-account"></a>계정에 연결

명령을 통해 저장소 계정에 대 한 권한 부여를 가져오는 방법을 선택 합니다. 

### <a name="option-1-obtain-authorization-by-using-azure-active-directory-azure-ad"></a>옵션 1: Azure Active Directory (Azure AD)를 사용 하 여 권한 부여 가져오기

이 접근 방식을 사용 하는 경우 시스템은 사용자 계정에 적절 한 Azure RBAC (역할 기반 액세스 제어) 할당 및 ACL 권한이 있는지 확인 합니다.

1. Windows PowerShell 명령 창을 열고 명령을 사용 하 여 Azure 구독에 로그인 하 `Connect-AzAccount` 고 화면의 지시를 따릅니다.

   ```powershell
   Connect-AzAccount
   ```

2. Id가 둘 이상의 구독과 연결 된 경우 활성 구독을 디렉터리를 만들고 관리 하려는 저장소 계정의 구독으로 설정 합니다. 이 예에서는 `<subscription-id>` 자리 표시자 값을 구독의 ID로 바꿉니다.

   ```powershell
   Select-AzSubscription -SubscriptionId <subscription-id>
   ``` 

3. 저장소 계정 컨텍스트를 가져옵니다.

   ```powershell
   $ctx = New-AzStorageContext -StorageAccountName '<storage-account-name>' -UseConnectedAccount
   ```

### <a name="option-2-obtain-authorization-by-using-the-storage-account-key"></a>옵션 2: 저장소 계정 키를 사용 하 여 권한 부여 가져오기

이 방법을 사용 하면 시스템에서 Azure RBAC 또는 ACL 사용 권한을 확인 하지 않습니다. 계정 키를 사용 하 여 저장소 계정 컨텍스트를 가져옵니다.

```powershell
$ctx = New-AzStorageContext -StorageAccountName '<storage-account-name>' -StorageAccountKey '<storage-account-key>'
```

## <a name="create-a-container"></a>컨테이너 만들기

컨테이너는 파일에 대 한 파일 시스템 역할을 합니다. Cmdlet을 사용 하 여 만들 수 있습니다 `New-AzStorageContainer` . 

이 예제에서는 라는 컨테이너를 만듭니다 `my-file-system` .

```powershell
$filesystemName = "my-file-system"
New-AzStorageContainer -Context $ctx -Name $filesystemName
```

## <a name="create-a-directory"></a>디렉터리 만들기

Cmdlet을 사용 하 여 디렉터리 참조를 만듭니다 `New-AzDataLakeGen2Item` . 

이 예제에서는 라는 디렉터리를 `my-directory` 컨테이너에 추가 합니다.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
New-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -Directory
```

이 예제에서는 동일한 디렉터리를 추가 하지만 사용 권한, umask, 속성 값 및 메타 데이터 값도 설정 합니다. 

```powershell
$dir = New-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -Directory -Permission rwxrwxrwx -Umask ---rwx---  -Property @{"ContentEncoding" = "UDF8"; "CacheControl" = "READ"} -Metadata  @{"tag1" = "value1"; "tag2" = "value2" }
```

## <a name="show-directory-properties"></a>디렉터리 속성 표시

이 예제에서는 cmdlet을 사용 하 여 디렉터리를 가져온 `Get-AzDataLakeGen2Item` 다음 속성 값을 콘솔에 출력 합니다.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$dir =  Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname
$dir.ACL
$dir.Permissions
$dir.Group
$dir.Owner
$dir.Properties
$dir.Properties.Metadata
```

> [!NOTE]
> 컨테이너의 루트 디렉터리를 가져오려면 `-Path` 매개 변수를 생략 합니다.

## <a name="rename-or-move-a-directory"></a>디렉터리 이름 바꾸기 또는 이동

Cmdlet을 사용 하 여 디렉터리 이름을 바꾸거나 이동 `Move-AzDataLakeGen2Item` 합니다.

이 예에서는 이름에서 이름으로 디렉터리의 이름을 바꿉니다 `my-directory` `my-new-directory` .

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$dirname2 = "my-new-directory/"
Move-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -DestFileSystem $filesystemName -DestPath $dirname2
```

> [!NOTE]
> 메시지를 `-Force` 표시 하지 않고 덮어쓰려면 매개 변수를 사용 합니다.

이 예에서는 라는 디렉터리를 `my-directory` 라는의 하위 디렉터리로 `my-directory-2` 이동 `my-subdirectory` 합니다. 

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$dirname2 = "my-directory-2/my-subdirectory/"
Move-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -DestFileSystem $filesystemName -DestPath $dirname2
```

## <a name="delete-a-directory"></a>디렉터리 삭제

Cmdlet을 사용 하 여 디렉터리를 삭제 `Remove-AzDataLakeGen2Item` 합니다.

다음 예제에서는 `my-directory`라는 디렉터리를 삭제합니다.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
Remove-AzDataLakeGen2Item  -Context $ctx -FileSystem $filesystemName -Path $dirname 
```

매개 변수를 사용 `-Force` 하 여 프롬프트 없이 파일을 제거할 수 있습니다.

## <a name="download-from-a-directory"></a>디렉터리에서 다운로드

Cmdlet을 사용 하 여 디렉터리에서 파일을 다운로드 `Get-AzDataLakeGen2ItemContent` 합니다.

다음 예제에서는 `my-directory`라는 디렉터리에서 `upload.txt`라는 파일을 다운로드합니다.

```powershell
$filesystemName = "my-file-system"
$filePath = "my-directory/upload.txt"
$downloadFilePath = "download.txt"
Get-AzDataLakeGen2ItemContent -Context $ctx -FileSystem $filesystemName -Path $filePath -Destination $downloadFilePath
```

## <a name="list-directory-contents"></a>디렉터리 콘텐츠 나열

Cmdlet을 사용 하 여 디렉터리의 내용을 나열 합니다 `Get-AzDataLakeGen2ChildItem` . 선택적 매개 변수를 사용 `-OutputUserPrincipalName` 하 여 사용자의 개체 ID 대신 이름을 가져올 수 있습니다.

이 예제에서는 이름이 인 디렉터리의 내용을 나열 `my-directory` 합니다.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
Get-AzDataLakeGen2ChildItem -Context $ctx -FileSystem $filesystemName -Path $dirname -OutputUserPrincipalName
```

다음 예에서는 `ACL` `Permissions` `Group` `Owner` 디렉터리에 있는 각 항목의,, 및 속성을 나열 합니다. `-FetchProperty`매개 변수는 속성에 대 한 값을 가져오는 데 필요 `ACL` 합니다. 

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$properties = Get-AzDataLakeGen2ChildItem -Context $ctx -FileSystem $filesystemName -Path $dirname -Recurse -FetchProperty
$properties.ACL
$properties.Permissions
$properties.Group
$properties.Owner
```

> [!NOTE]
> 컨테이너의 루트 디렉터리 내용을 나열 하려면 매개 변수를 생략 합니다 `-Path` .

## <a name="upload-a-file-to-a-directory"></a>디렉터리에 파일 업로드

Cmdlet을 사용 하 여 디렉터리에 파일을 업로드 `New-AzDataLakeGen2Item` 합니다.

다음 예제에서는 `upload.txt`라는 파일을 `my-directory`라는 디렉터리에 업로드합니다. 

```powershell
$localSrcFile =  "upload.txt"
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$destPath = $dirname + (Get-Item $localSrcFile).Name
New-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $destPath -Source $localSrcFile -Force 
```

이 예에서는 동일한 파일을 업로드 하지만 대상 파일의 사용 권한, umask, 속성 값 및 메타 데이터 값을 설정 합니다. 또한이 예제에서는 이러한 값을 콘솔에 출력 합니다.

```powershell
$file = New-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $destPath -Source $localSrcFile -Permission rwxrwxrwx -Umask ---rwx--- -Property @{"ContentEncoding" = "UDF8"; "CacheControl" = "READ"} -Metadata  @{"tag1" = "value1"; "tag2" = "value2" }
$file1
$file1.Properties
$file1.Properties.Metadata

```

> [!NOTE]
> 컨테이너의 루트 디렉터리에 파일을 업로드 하려면 `-Path` 매개 변수를 생략 합니다.

## <a name="show-file-properties"></a>파일 속성 표시

이 예제에서는 cmdlet을 사용 하 여 파일을 가져온 `Get-AzDataLakeGen2Item` 다음 속성 값을 콘솔에 출력 합니다.

```powershell
$filepath =  "my-directory/upload.txt"
$filesystemName = "my-file-system"
$file = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $filepath
$file
$file.ACL
$file.Permissions
$file.Group
$file.Owner
$file.Properties
$file.Properties.Metadata
```

## <a name="delete-a-file"></a>파일 삭제

Cmdlet을 사용 하 여 파일을 삭제 `Remove-AzDataLakeGen2Item` 합니다.

이 예에서는 라는 파일을 삭제 `upload.txt` 합니다. 

```powershell
$filesystemName = "my-file-system"
$filepath = "upload.txt"
Remove-AzDataLakeGen2Item  -Context $ctx -FileSystem $filesystemName -Path $filepath 
```

매개 변수를 사용 `-Force` 하 여 프롬프트 없이 파일을 제거할 수 있습니다.

<a id="gen1-gen2-map"></a>

## <a name="gen1-to-gen2-mapping"></a>Gen1 to Gen2 Mapping

다음 표에서는 Data Lake Storage Gen1에 사용 되는 cmdlet이 Data Lake Storage Gen2 cmdlet에 매핑되는 방법을 보여 줍니다.

|Data Lake Storage Gen1 cmdlet| Data Lake Storage Gen2 cmdlet| 참고 |
|--------|---------|-----|
|Get-AzDataLakeStoreChildItem|Get-AzDataLakeGen2ChildItem|기본적으로 Get-AzDataLakeGen2ChildItem cmdlet은 첫 번째 수준의 자식 항목만 나열 합니다. -재귀 매개 변수는 자식 항목을 재귀적으로 나열 합니다. |
|Get-AzDataLakeStoreItem<br>Get-AzDataLakeStoreItemAclEntry<br>Get-AzDataLakeStoreItemOwner<br>Get-AzDataLakeStoreItemPermission|Get-AzDataLakeGen2Item|Get-AzDataLakeGen2Item cmdlet의 출력 항목에는 Acl, Owner, Group, Permission 속성이 있습니다.|
|Get-AzDataLakeStoreItemContent|Get-AzDataLakeGen2FileContent|Get-AzDataLakeGen2FileContent cmdlet은 파일 콘텐츠를 로컬 파일에 다운로드 합니다.|
|Move-AzDataLakeStoreItem|Move-AzDataLakeGen2Item||
|New-AzDataLakeStoreItem|New-AzDataLakeGen2Item|이 cmdlet은 로컬 파일에서 새 파일 콘텐츠를 업로드 합니다.|
|Remove-AzDataLakeStoreItem|Remove-AzDataLakeGen2Item||
|Set-AzDataLakeStoreItemOwner<br>Set-AzDataLakeStoreItemPermission<br>Set-AzDataLakeStoreItemAcl|Update-AzDataLakeGen2Item|Update-AzDataLakeGen2Item cmdlet은 단일 항목만 업데이트 하 고 재귀적은 업데이트 하지 않습니다. 재귀적으로 업데이트 하려면 Get-AzDataLakeStoreChildItem cmdlet을 사용 하 여 항목을 나열 한 다음 Update-AzDataLakeGen2Item cmdlet으로 파이프라인을 사용 합니다.|
|Test-AzDataLakeStoreItem|Get-AzDataLakeGen2Item|항목이 존재 하지 않는 경우 Get-AzDataLakeGen2Item cmdlet에서 오류를 보고 합니다.|

## <a name="see-also"></a>추가 정보

- [알려진 문제](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
- [스토리지 PowerShell cmdlet](/powershell/module/az.storage)
