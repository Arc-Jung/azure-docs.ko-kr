---
title: '여러 종속 서비스 실행: .NET Core 및 Visual Studio'
services: azure-dev-spaces
ms.custom: vs-azure
ms.workload: azure-vs
ms.date: 07/09/2018
ms.topic: tutorial
description: 이 자습서에서는 Azure Dev Spaces 및 Visual Studio를 사용하여 Azure Kubernetes Service에서 다중 서비스 .NET Core 애플리케이션을 디버깅하는 방법을 보여줍니다.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, 컨테이너, Helm, 서비스 메시, 서비스 메시 라우팅, kubectl, k8s
ms.openlocfilehash: 7f95c21c2cf5b7adcdb34d7bbe2b1f8314c20333
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75438396"
---
# <a name="running-multiple-dependent-services-net-core-and-visual-studio-with-azure-dev-spaces"></a>여러 종속 서비스 실행: Azure Dev Spaces가 포함된 .NET Core 및 Visual Studio

이 자습서에서는 Azure Dev Spaces를 사용하여 다중 서비스 애플리케이션을 개발하는 방법과 Dev Spaces가 제공하는 추가적인 이점 몇 가지를 알아봅니다.

## <a name="call-another-container"></a>다른 컨테이너 호출
이 섹션에서는 두 번째 서비스인 `mywebapi`를 만들고 `webfrontend`에서 이 서비스를 호출하도록 합니다. 각 서비스는 별도의 컨테이너에서 실행됩니다. 그런 다음, 두 컨테이너 모두에서 디버그합니다.

![](media/common/multi-container.png)

### <a name="download-sample-code-for-mywebapi"></a>*mywebapi* 샘플 코드 다운로드
이제 GitHub 리포지토리에서 샘플 코드를 다운로드해 보겠습니다. [https://github.com/Azure/dev-spaces](https://github.com/Azure/dev-spaces ) 로 이동하고 **복제 또는 다운로드**를 선택하여 GitHub 리포지토리를 다운로드합니다. 이 섹션에서 사용할 코드는 `samples/dotnetcore/getting-started/mywebapi`에 있습니다.

### <a name="run-mywebapi"></a>*mywebapi* 실행
1. `mywebapi`별도의 Visual Studio 창*에서*  프로젝트를 엽니다.
1. 이전에 **프로젝트에 대해 수행한 대로 시작 설정 드롭다운에서**Azure Dev Spaces`webfrontend`를 선택합니다. 이번에는 새 AKS 클러스터를 만드는 대신, 이미 만든 것과 동일한 클러스터를 선택합니다. 이전과 마찬가지로, [공간]을 기본값인 `default`로 그대로 두고 **확인**을 클릭합니다. 출력 창에서는 Visual Studio에서 디버깅을 시작할 때 속도를 높이기 위해 개발 환경의 이 새로운 서비스를 "준비"하기 시작한다는 것을 알 수 있습니다.
1. F5 키를 누르고, 서비스가 빌드되고 배포될 때까지 기다립니다. Visual Studio 상태 표시줄이 주황색으로 바뀌면 준비가 되었음을 알 수 있습니다.
1. **출력** 창의 **AKS용 Azure Dev Spaces** 창에 표시된 엔드포인트 URL을 적어둡니다. 이는 `http://localhost:<portnumber>`과 비슷합니다. 컨테이너가 로컬에서 실행되는 것처럼 보일 수도 있지만, 실제로는 Azure의 개발 환경에서 실행됩니다.
2. `mywebapi`가 준비되면 브라우저를 localhost 주소로 열고, `/api/values`에 대한 기본 GET API를 호출하기 위해 URL에 `ValuesController`를 추가합니다. 
3. 모든 단계가 성공적으로 완료되면 다음과 같은 `mywebapi` 서비스의 응답이 표시될 수 있습니다.

    ![](media/get-started-netcore-visualstudio/WebAPIResponse.png)

### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>*webfrontend*에서 *mywebapi*로 요청
이제 `webfrontend`에 요청하는 코드를 `mywebapi`에 작성해 보겠습니다. `webfrontend` 프로젝트가 있는 Visual Studio 창으로 전환합니다. `HomeController.cs` 파일에서 About 메서드에 대한 코드를 다음 코드로 *바꿉니다*.

   ```csharp
   public async Task<IActionResult> About()
   {
      ViewData["Message"] = "Hello from webfrontend";

      using (var client = new System.Net.Http.HttpClient())
            {
                // Call *mywebapi*, and display its response in the page
                var request = new System.Net.Http.HttpRequestMessage();
                request.RequestUri = new Uri("http://mywebapi/api/values/1");
                if (this.Request.Headers.ContainsKey("azds-route-as"))
                {
                    // Propagate the dev space routing header
                    request.Headers.Add("azds-route-as", this.Request.Headers["azds-route-as"] as IEnumerable<string>);
                }
                var response = await client.SendAsync(request);
                ViewData["Message"] += " and " + await response.Content.ReadAsStringAsync();
            }

      return View();
   }
   ```

위의 코드 예제는 `azds-route-as` 헤더를 수신 요청에서 발신 요청으로 전달합니다. 나중에 [팀 시나리오](team-development-netcore-visualstudio.md)에서 이를 통해 더 생산적인 개발 환경을 용이하게 하는 방법을 살펴보겠습니다.

### <a name="debug-across-multiple-services"></a>여러 서비스에서 디버깅
1. 이 시점에서 `mywebapi`는 디버거가 연결된 상태로 계속 실행되고 있습니다. 그렇지 않으면 `mywebapi` 프로젝트에서 F5 키를 누릅니다.
1. `Get(int id)` GET 요청을 처리하는 `Controllers/ValuesController.cs` 파일의 `api/values/{id}` 메서드에 중단점을 설정합니다.
1. 위 코드를 붙여넣은 `webfrontend` 프로젝트에서 GET 요청을 `mywebapi/api/values`로 보내기 바로 전에 중단점을 설정합니다.
1. `webfrontend` 프로젝트에서 F5 키를 누릅니다. Visual Studio에서 브라우저를 해당 localhost 포트로 다시 열고, 웹앱이 표시됩니다.
1. **프로젝트에서 중단점을 트리거하려면 페이지 위쪽에 있는**정보`webfrontend` 링크를 클릭합니다. 
1. F10 키를 눌러 계속 진행합니다. 이제 `mywebapi` 프로젝트의 중단점이 트리거됩니다.
1. F5 키를 눌러 계속 진행하고 `webfrontend` 프로젝트의 코드로 돌아갑니다.
1. 한 번 더 F5 키를 누르면 요청이 완료되고 브라우저에서 페이지가 반환됩니다. 웹앱의 [정보] 페이지에는 두 서비스로 연결된 "Hello from webfrontend and Hello from mywebapi.(webfrontend에서 보낸 Hello 및 mywebapi에서 보낸 Hello입니다.)"라는 메시지가 표시됩니다.

### <a name="well-done"></a>모두 완료되었습니다!
이제 각 컨테이너를 개별적으로 개발하고 배포할 수 있는 다중 컨테이너 애플리케이션이 있습니다.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [Dev Spaces의 팀 개발에 대해 알아보기](team-development-netcore-visualstudio.md)
