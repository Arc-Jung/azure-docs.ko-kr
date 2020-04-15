---
title: Azure Application Gateway 인증서 갱신
description: 애플리케이션 게이트웨이 수신기와 연결된 인증서를 갱신하는 방법을 알아봅니다.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 8/15/2018
ms.author: victorh
ms.openlocfilehash: 4c0c0ab84e60335f58ac075459b95cfabb9135ac
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81311958"
---
# <a name="renew-application-gateway-certificates"></a>Application Gateway 인증서 갱신

TLS/SSL 암호화를 위해 응용 프로그램 게이트웨이를 구성한 경우 인증서를 갱신해야 합니다.

Azure Portal, Azure PowerShell 또는 Azure CLI를 사용하여 수신기와 연결된 인증서를 갱신할 수 있습니다.

## <a name="azure-portal"></a>Azure portal

포털에서 수신기 인증서를 갱신하려면 애플리케이션 게이트웨이 수신기로 이동합니다. 인증서를 갱신해야 하는 수신기를 클릭한 다음, **선택한 인증서 갱신 또는 편집**을 클릭합니다.

![인증서 갱신](media/renew-certificate/ssl-cert.png)

새 PFX 인증서를 업로드하고, 이름을 지정하고, 암호를 입력한 다음, **저장**을 클릭합니다.

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure PowerShell을 사용하여 인증서를 갱신하려면 다음 스크립트를 사용합니다.

```azurepowershell-interactive
$appgw = Get-AzApplicationGateway `
  -ResourceGroupName <ResourceGroup> `
  -Name <AppGatewayName>

$password = ConvertTo-SecureString `
  -String "<password>" `
  -Force `
  -AsPlainText

set-AzApplicationGatewaySSLCertificate -Name <oldcertname> `
-ApplicationGateway $appgw -CertificateFile <newcertPath> -Password $password

Set-AzApplicationGateway -ApplicationGateway $appgw
```
## <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az network application-gateway ssl-cert update \
  -n "<CertName>" \
  --gateway-name "<AppGatewayName>" \
  -g "ResourceGroupName>" \
  --cert-file <PathToCerFile> \
  --cert-password "<password>"
```

## <a name="next-steps"></a>다음 단계

Azure 응용 프로그램 게이트웨이를 사용하여 TLS 오프로드를 구성하는 방법을 알아보려면 [TLS 오프로드 구성을](application-gateway-ssl-portal.md) 참조하세요.
