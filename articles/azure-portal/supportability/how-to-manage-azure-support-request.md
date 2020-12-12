---
title: Azure 지원 요청 관리
description: 지원 요청을 보고, 메시지를 보내고, 요청 심각도 수준을 변경 하 고, 진단 정보를 Azure 지원과 공유 하 고, 닫힌 지원 요청을 다시 열고, 파일을 업로드 하는 방법을 설명 합니다.
tags: billing
ms.assetid: 86697fdf-3499-4cab-ab3f-10d40d3c1f70
ms.topic: how-to
ms.date: 06/30/2020
ms.openlocfilehash: 882dfaa802638efd98eaf6f12a33a77a9727adc2
ms.sourcegitcommit: dfc4e6b57b2cb87dbcce5562945678e76d3ac7b6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/12/2020
ms.locfileid: "97359067"
---
# <a name="manage-an-azure-support-request"></a>Azure 지원 요청 관리

[Azure 지원 요청을 만든](how-to-create-azure-support-request.md)후에는이 문서에서 설명 하는 [Azure Portal](https://portal.azure.com)에서 관리할 수 있습니다. [Azure 지원 티켓 REST API](/rest/api/support)를 사용하여 프로그래밍 방식으로 요청을 만들고 관리할 수도 있습니다.

## <a name="view-support-requests"></a>지원 요청 보기

**도움말 +**  >   **모든 지원 요청** 지원으로 이동 하 여 지원 요청의 세부 정보 및 상태를 확인 합니다.

:::image type="content" source="media/how-to-manage-azure-support-request/all-requests-lower.png" alt-text="모든 지원 요청":::

이 페이지에서 지원 요청을 검색, 필터링 및 정렬할 수 있습니다. 요청에 연결 된 심각도 및 모든 메시지를 포함 하 여 세부 정보를 볼 수 있는 지원 요청을 선택 합니다.

## <a name="send-a-message"></a>메시지 보내기

1. **모든 지원 요청** 페이지에서 지원 요청을 선택 합니다.

1. **지원 요청** 페이지에서 **새 메시지** 를 선택 합니다.

1. 메시지를 입력 하 고 **제출** 을 선택 합니다.

## <a name="change-the-severity-level"></a>심각도 수준 변경

> [!NOTE]
> 최대 심각도 수준은 [지원 계획](https://azure.microsoft.com/support/plans)에 따라 달라 집니다.
>

1. **모든 지원 요청** 페이지에서 지원 요청을 선택 합니다.

1. **지원 요청** 페이지에서 **변경** 을 선택 합니다.

    :::image type="content" source="media/how-to-manage-azure-support-request/change-severity.png" alt-text="지원 요청 심각도 변경":::

1. Azure Portal는 요청이 지원 엔지니어에 게 이미 할당 되었는지 여부에 따라 다음 두 화면 중 하나를 표시 합니다.

    - 요청이 할당 되지 않은 경우 다음과 같은 화면이 표시 됩니다. 새 심각도 수준을 선택한 다음 **변경** 을 선택 합니다.

        :::image type="content" source="media/how-to-manage-azure-support-request/unassigned-can-change-severity.png" alt-text="새 심각도 수준을 선택 합니다.":::

    - 요청이 할당 된 경우 다음과 같은 화면이 표시 됩니다. **확인** 을 선택한 다음 [새 메시지](#send-a-message) 를 만들어 심각도 수준의 변경을 요청 합니다.

        :::image type="content" source="media/how-to-manage-azure-support-request/assigned-cant-change-severity.png" alt-text="새 심각도 수준을 선택할 수 없습니다.":::

## <a name="share-diagnostic-information-with-azure-support"></a>Azure 지원에 진단 정보 공유

지원 요청을 만들 때 기본적으로 **진단 정보 공유** 옵션이 선택 되어 있습니다. 이렇게 하면 azure 지원에서 Azure 리소스의 [진단 정보](https://azure.microsoft.com/support/legal/support-diagnostic-information-collection/) 를 수집할 수 있습니다.

* 요청을 만든 후에는이 옵션을 지울 수 없습니다.

* 요청을 만들 때이 옵션을 선택 취소 한 경우 요청을 만든 후에이 옵션을 선택할 수 있습니다.

    1. **모든 지원 요청** 페이지에서 지원 요청을 선택 합니다.
    
    1. **지원 요청** 페이지에서 **권한 부여** 를 선택 하 고 **예** , **확인** 을 차례로 선택 합니다.
    
        :::image type="content" source="media/how-to-manage-azure-support-request/grant-permission-manage.png" alt-text="진단 정보에 대 한 사용 권한 부여":::

## <a name="upload-files"></a>파일 업로드

파일 업로드 옵션을 사용 하 여 지원 요청에 관련 된 것으로 생각 되는 진단 파일 또는 다른 파일을 업로드할 수 있습니다.

1. **모든 지원 요청** 페이지에서 지원 요청을 선택 합니다.

1. **지원 요청** 페이지에서 파일을 찾은 다음 **업로드** 를 선택 합니다. 파일이 여러 개인 경우 프로세스를 반복 합니다.

    :::image type="content" source="media/how-to-manage-azure-support-request/file-upload.png" alt-text="파일 업로드":::.

### <a name="file-upload-guidelines"></a>파일 업로드 지침

파일 업로드 옵션을 사용 하는 경우 다음 지침을 따르세요.

* 개인 정보를 보호하려면 업로드 시 개인 정보를 포함시키기 마세요.
* 파일 이름은 110자 이하여야 합니다.
* 둘 이상의 파일을 업로드할 수 없습니다.
* 파일은 4mb 보다 클 수 없습니다.
* 모든 파일에는 *.docx* 또는 *.xlsx* 와 같은 파일 이름 확장명이 있어야 합니다. 다음 표에서는 업로드할 수 있는 파일 이름 확장명을 보여 줍니다.

| 0-9, A-C     | D-G   | H-M         | N-P   | R-T      | U-W        | X-Z     |
|-------------|-------|-------------|-------|----------|------------|---------|
| .7z         | .dat  | . har        | .odx  | .rar     | .tdb       | .xlam   |
| .a          | .db   | .hwl        | .oft  | .rdl     | .tdf       | .xlr    |
| .abc        | .DMP  | .ics        | .old  | .rdlc    | .text      | .xls    |
| .adm        | .do_  | .ini        | .one  | .re_     | .thmx      | .xlsb   |
| .aspx       | .doc  | .java       | .osd  | .reg     | .tif       | .xlsm   |
| .ATF        | .docm | .jpg        | .OUT  | .remove  | .trc       | .xlsx   |
| .b          | .docx | .LDF        | .p1   | .ren     | .TTD       | .xlt    |
| .ba_        | .dotm | .letterhead | .pcap | .rename  | .tx_       | .xltx   |
| .bak        | .dotx | .lnk        | .pdb  | .rft     | .txt       | .xml    |
| .bat        | .dtsx | .lo_        | .pdf  | .rpt     | .uccapilog | .xmla   |
| .blg        | .eds  | .log        | .piz  | .rte     | .uccplog   | .xps    |
| .CA_        | .emf  | .lpk        | .pmls | .rtf     | .udcx      | .xsd    |
| .CAB        | .eml  | .manifest   | .png  | .run     | .vb_       | .xsn    |
| .cap        | .emz  | .master     | .potx | .saz     | .vbs_      | .xxx    |
| .catx       | .err  | .mdmp       | .ppt  | .sql     | .vcf       | .z_     |
| .CFG        | .etl  | .mof        | .pptm | .sqlplan | .vsd       | .z01    |
| .compressed | .evt  | .mp3        | .pptx | .stp     | .wdb       | .z02    |
| .Config     | .evtx | .mpg        | .prn  | .svclog  | .wks       | .zi     |
| .cpk        | .EX   | .ms_        | .psf  | -        | .wma       | .zi_    |
| .cpp        | .ex_  | .msg        | .pst  | -        | .wmv       | .zip    |
| .cs         | .ex0  | .msi        | .pub  | -        | .wmz       | .zip_   |
| .CSV        | .FRD  | .mso        | -     | -        | .wps       | .zipp   |
| .cvr        | .gif  | .msu        | -     | -        | .wpt       | .zipped |
| -           | .guid | .nfo        | -     | -        | .wsdl      | .zippy  |
| -           | .gz   | -           | -     | -        | .wsp       | .zipx   |
| -           | -     | -           | -     | -        | .wtl       | .zit    |
| -           | -     | -           | -     | -        | -          | .zix    |
| -           | -     | -           | -     | -        | -          | .zzz    |

## <a name="reopen-a-closed-request"></a>닫힌 요청 다시 열기

종결 된 지원 요청을 다시 열어야 하는 경우 [새 메시지](#send-a-message)를 만들면 요청이 자동으로 다시 열립니다.

## <a name="next-steps"></a>다음 단계

[Azure 지원 요청을 만드는 방법](how-to-create-azure-support-request.md)

[Azure 지원 티켓 REST API](/rest/api/support)
