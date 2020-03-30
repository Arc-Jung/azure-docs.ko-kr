---
title: 템플릿 만들기 - Visual Studio Code
description: Visual Studio Code와 Azure Resource Manager 도구 확장을 사용하여 Resource Manager 템플릿에 대해 작업합니다.
author: mumian
ms.date: 03/04/2019
ms.topic: quickstart
ms.author: jgao
ms.openlocfilehash: c9447d356cff792d9a70e33cc2a5e35898d8982b
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80131887"
---
# <a name="quickstart-create-arm-templates-by-using-visual-studio-code"></a>빠른 시작: Visual Studio Code를 사용하여 ARM 템플릿 만들기

Visual Studio Code 및 Azure Resource Manager 도구 확장을 사용하여 ARM(Azure Resource Manager) 템플릿을 만들고 편집하는 방법을 알아봅니다. 확장 없이 Visual Studio Code에서 ARM 템플릿을 만들 수 있지만, 확장에서는 템플릿 개발을 간소화하는 자동 완성 옵션을 제공합니다. Azure 솔루션 배포 및 관리와 관련된 개념을 이해하려면 [템플릿 배포 개요](overview.md)를 참조하세요.

이 빠른 시작에서는 스토리지 계정을 배포합니다.

![Resource Manager 템플릿 빠른 시작 Visual Studio Code 다이어그램](./media/quickstart-create-templates-use-visual-studio-code/resource-manager-template-quickstart-vscode-diagram.png)

Azure 구독이 아직 없는 경우 시작하기 전에 [체험](https://azure.microsoft.com/free/) 계정을 만듭니다.

## <a name="prerequisites"></a>사전 요구 사항

이 문서를 완료하려면 다음이 필요합니다.

- [Visual Studio Code](https://code.visualstudio.com/)
- Resource Manager 도구 확장. 설치하려면 다음 단계를 사용합니다.

    1. Visual Studio Code를 엽니다.
    2. **Ctrl+Shift+X**를 눌러 확장 패널을 엽니다.
    3. **Azure Resource Manager 도구**를 검색한 다음, **설치**를 선택합니다.
    4. **다시 로드**를 선택하여 확장 설치를 완료합니다.

## <a name="open-a-quickstart-template"></a>빠른 시작 템플릿 열기

템플릿을 처음부터 만드는 대신 [Azure 빠른 시작 템플릿](https://azure.microsoft.com/resources/templates/)에서 템플릿을 엽니다. Azure 빠른 시작 템플릿은 ARM 템플릿용 리포지토리입니다.

이 빠른 시작에서 사용되는 템플릿은 [표준 스토리지 계정 만들기](https://azure.microsoft.com/resources/templates/101-storage-account-create/)라고 합니다. 이 템플릿은 Azure Storage 계정 리소스를 정의합니다.

1. Visual Studio Code에서 **파일**>**파일 열기**를 차례로 선택합니다.
2. **파일 이름**에서 다음 URL을 붙여넣습니다.

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```

3. **열기**를 선택하여 파일을 엽니다.
4. **파일**>**이름으로 저장**을 차례로 선택하여 파일을 **azuredeploy.json**으로 저장합니다.

## <a name="edit-the-template"></a>템플릿 편집

Visual Studio Code를 사용하여 템플릿을 편집하는 방법을 경험하려면 `outputs` 섹션에 하나 이상의 요소를 추가하여 스토리지 URI를 표시합니다.

1. 내보낸 템플릿에 하나 이상의 출력을 추가합니다.

    ```json
    "storageUri": {
      "type": "string",
      "value": "[reference(variables('storageAccountName')).primaryEndpoints.blob]"
    }
    ```

    완료되면 출력 섹션이 다음과 같습니다.

    ```json
    "outputs": {
      "storageAccountName": {
        "type": "string",
        "value": "[variables('storageAccountName')]"
      },
      "storageUri": {
        "type": "string",
        "value": "[reference(variables('storageAccountName')).primaryEndpoints.blob]"
      }
    }
    ```

    Visual Studio Code 내에서 코드를 복사하여 붙여넣은 경우 **value** 요소를 다시 입력하여 Resource Manager 도구 확장의 IntelliSense 기능을 사용해 봅니다.

    ![Resource Manager 템플릿 - Visual Studio Code IntelliSense](./media/quickstart-create-templates-use-visual-studio-code/resource-manager-templates-visual-studio-code-intellisense.png)

2. **파일**>**저장**을 차례로 선택하여 파일을 저장합니다.

## <a name="deploy-the-template"></a>템플릿 배포

템플릿을 배포하는 방법에는 여러 가지가 있습니다. 이 빠른 시작에서는 Azure Cloud Shell을 사용합니다. Cloud Shell은 Azure CLI와 Azure PowerShell을 모두 지원합니다. 탭 선택기를 사용하여 CLI와 PowerShell 중에서 선택합니다.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

1. [Azure Cloud Shell](https://shell.azure.com)에 로그인

2. 왼쪽 위 모퉁이에서 **PowerShell** 또는 **Bash**(CLI)를 선택하여 기본 환경을 선택합니다.  전환하는 경우 셸을 다시 시작해야 합니다.

    # <a name="cli"></a>[CLI](#tab/CLI)

    ![Azure Portal - Cloud Shell에서 CLI로 전환](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-choose-cli.png)

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

    ![Azure Portal Cloud shell PowerShell](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-choose-powershell.png)

    ---

3. **파일 업로드/다운로드**를 선택한 다음, **업로드**를 선택합니다.

    # <a name="cli"></a>[CLI](#tab/CLI)

    ![Azure Portal - Cloud Shell 파일 업로드](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-upload-file.png)

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

    ![Azure Portal - Cloud Shell 파일 업로드](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-upload-file-powershell.png)

    ---

    이전 섹션에서 저장한 파일을 선택합니다. 기본 이름은 **azuredeploy.json**입니다. 템플릿 파일은 셸에서 액세스할 수 있어야 합니다.

    필요에 따라 **ls** 명령 및 **cat** 명령을 사용하여 파일이 성공적으로 업로드되었는지 확인할 수 있습니다.

    # <a name="cli"></a>[CLI](#tab/CLI)

    ![Azure Portal - Cloud Shell 파일 나열](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-list-file.png)

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

    ![Azure Portal - Cloud Shell 파일 나열](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-list-file-powershell.png)

    ---
4. Cloud Shell에서 다음 명령을 실행합니다. 탭을 선택하여 PowerShell 코드 또는 CLI 코드를 표시합니다.

    # <a name="cli"></a>[CLI](#tab/CLI)
    ```azurecli
    echo "Enter the Resource Group name:" &&
    read resourceGroupName &&
    echo "Enter the location (i.e. centralus):" &&
    read location &&
    az group create --name $resourceGroupName --location "$location" &&
    az deployment group create --resource-group $resourceGroupName --template-file "$HOME/azuredeploy.json"
    ```

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

    ```azurepowershell
    $resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"

    New-AzResourceGroup -Name $resourceGroupName -Location "$location"
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile "$HOME/azuredeploy.json"
    ```

    ---

    파일 이름을 **azuredeploy.json**이 아닌 다른 이름으로 저장한 경우 템플릿 파일 이름을 업데이트합니다.

    다음 스크린샷에서는 배포 샘플을 보여줍니다.

    # <a name="cli"></a>[CLI](#tab/CLI)

    ![Azure Portal - Cloud Shell 템플릿 배포](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-deploy-template.png)

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

    ![Azure Portal - Cloud Shell 템플릿 배포](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-deploy-template-powershell.png)

    ---

    스크린샷에는 출력 섹션의 스토리지 계정 이름과 스토리지 URL이 강조 표시되었습니다. 그 다음 단계에서 스토리지 계정 이름이 필요합니다.

5. 다음 CLI 또는 PowerShell 명령을 실행하여 새로 만든 스토리지 계정을 나열합니다.

    # <a name="cli"></a>[CLI](#tab/CLI)
    ```azurecli
    echo "Enter the Resource Group name:" &&
    read resourceGroupName &&
    echo "Enter the Storage Account name:" &&
    read storageAccountName &&
    az storage account show --resource-group $resourceGroupName --name $storageAccountName
    ```

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

    ```azurepowershell
    $resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
    $storageAccountName = Read-Host -Prompt "Enter the Storage Account name"
    Get-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName
    ```

    ---

스토리지 계정 사용에 대한 자세한 내용은 [빠른 시작: Azure Portal을 사용하여 Blob 업로드, 다운로드 및 나열](../../storage/blobs/storage-quickstart-blobs-portal.md)을 참조하세요.

## <a name="clean-up-resources"></a>리소스 정리

Azure 리소스가 더 이상 필요하지 않은 경우 리소스 그룹을 삭제하여 배포한 리소스를 정리합니다.

1. Azure Portal의 왼쪽 메뉴에서 **리소스 그룹**을 선택합니다.
2. **이름으로 필터링** 필드에서 리소스 그룹 이름을 입력합니다.
3. 해당 리소스 그룹 이름을 선택합니다.  리소스 그룹에 총 6개의 리소스가 표시됩니다.
4. 위쪽 메뉴에서 **리소스 그룹 삭제**를 선택합니다.

## <a name="next-steps"></a>다음 단계

이 빠른 시작은 Visual Studio Code를 사용하여 Azure 빠른 시작 템플릿에서 기존 템플릿을 편집하는 데 집중하고 있습니다. 또한 Azure Cloud Shell의 CLI 또는 PowerShell을 사용하여 템플릿을 배포하는 방법도 알아보았습니다. Azure 빠른 시작 템플릿의 템플릿은 필요한 모든 것을 제공하지 않을 수도 있습니다. 템플릿 개발에 대해 자세히 알아보려면 새로운 초보자용 자습서 시리즈를 참조하세요.

> [!div class="nextstepaction"]
> [초보자용 자습서](./template-tutorial-create-first-template.md)
