---
title: 'Python 스크립트 실행: 모듈 참조'
titleSuffix: Azure Machine Learning
description: Azure Machine Learning 디자이너에서 Python 스크립트 실행 모듈을 사용 하 여 Python 코드를 실행 하는 방법을 알아봅니다.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.custom: devx-track-python
author: likebupt
ms.author: keli19
ms.date: 01/02/2021
ms.openlocfilehash: 6003ca9156d8553604d7ebbf94c5c3373d077f0f
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2021
ms.locfileid: "102500702"
---
# <a name="execute-python-script-module"></a>Python 스크립트 실행 모듈

이 문서에서는 Azure Machine Learning 디자이너의 Python 스크립트 실행 모듈을 설명 합니다.

이 모듈을 사용 하 여 Python 코드를 실행 합니다. Python의 아키텍처 및 디자인 원칙에 대 한 자세한 내용은 [Azure Machine Learning designer에서 python 코드 실행 방법](../how-to-designer-python.md)을 참조 하세요.

Python을 사용 하면 다음과 같이 기존 모듈이 지원 하지 않는 작업을 수행할 수 있습니다.

+ 를 사용 하 여 데이터 시각화 `matplotlib`
+ Python 라이브러리를 사용 하 여 작업 영역에서 데이터 집합 및 모델을 열거 합니다.
+ [데이터 가져오기](./import-data.md) 모듈이 지원 하지 않는 원본의 데이터를 읽고 로드 하 고 조작 합니다.
+ 심층 학습 코드를 직접 실행 합니다. 

## <a name="supported-python-packages"></a>지원 되는 Python 패키지

Azure Machine Learning는 데이터 처리를 위한 여러 일반적인 유틸리티를 포함 하는 Python의 Anaconda 배포를 사용 합니다. Anaconda 버전이 자동으로 업데이트 됩니다. 현재 버전:
 -  Python 3.6에 대 한 Anaconda 4.5 + 배포 

전체 목록은 [사전 설치 된 Python 패키지](#preinstalled-python-packages)섹션을 참조 하세요.

사전 설치 된 목록에 없는 패키지 (예: *scikit*)를 설치 하려면 스크립트에 다음 코드를 추가 합니다. 

```python
import os
os.system(f"pip install scikit-misc")
```

특히 유추를 위해 다음 코드를 사용 하 여 더 나은 성능을 위해 패키지를 설치 합니다.
```python
import importlib.util
package_name = 'scikit-misc'
spec = importlib.util.find_spec(package_name)
if spec is None:
    import os
    os.system(f"pip install scikit-misc")
```

> [!NOTE]
> 파이프라인에 사전 설치 된 목록에 없는 패키지를 필요로 하는 여러 개의 Python 스크립트 실행 모듈이 포함 되어 있는 경우 각 모듈에 패키지를 설치 합니다.

> [!WARNING]
> Excute Python 스크립트 모듈은 Java, PyODBC 등의 명령을 사용 하 여 추가 네이티브 라이브러리에 종속 된 패키지를 설치 하는 것을 지원 하지 않습니다. 이 모듈은 Python이 사전 설치 되어 있고 비관리자 권한이 있는 간단한 환경에서 실행 되기 때문입니다.  

## <a name="access-to-current-workspace-and-registered-datasets"></a>현재 작업 영역 및 등록 된 데이터 집합에 대 한 액세스

다음 샘플 코드를 참조 하 여 작업 영역에서 등록 된 [데이터 집합](../how-to-create-register-datasets.md) 에 액세스할 수 있습니다.

```Python
def azureml_main(dataframe1 = None, dataframe2 = None):

    # Execution logic goes here
    print(f'Input pandas.DataFrame #1: {dataframe1}')
    from azureml.core import Run
    run = Run.get_context(allow_offline=True)
    #access to current workspace
    ws = run.experiment.workspace

    #access to registered dataset of current workspace
    from azureml.core import Dataset
    dataset = Dataset.get_by_name(ws, name='test-register-tabular-in-designer')
    dataframe1 = dataset.to_pandas_dataframe()
     
    # If a zip file is connected to the third input port,
    # it is unzipped under "./Script Bundle". This directory is added
    # to sys.path. Therefore, if your zip file contains a Python file
    # mymodule.py you can import it using:
    # import mymodule

    # Return value must be of a sequence of pandas.DataFrame
    # E.g.
    #   -  Single return value: return dataframe1,
    #   -  Two return values: return dataframe1, dataframe2
    return dataframe1,
```

## <a name="upload-files"></a>파일 업로드
Python 스크립트 실행 모듈은 [Azure Machine Learning PYTHON SDK](/python/api/azureml-core/azureml.core.run%28class%29#upload-file-name--path-or-stream-)를 사용 하 여 파일 업로드를 지원 합니다.

다음 예제에서는 Python 스크립트 실행 모듈에서 이미지 파일을 업로드 하는 방법을 보여 줍니다.

```Python

# The script MUST contain a function named azureml_main,
# which is the entry point for this module.

# Imports up here can be used to
import pandas as pd

# The entry point function must have two input arguments:
#   Param<dataframe1>: a pandas.DataFrame
#   Param<dataframe2>: a pandas.DataFrame
def azureml_main(dataframe1 = None, dataframe2 = None):

    # Execution logic goes here
    print(f'Input pandas.DataFrame #1: {dataframe1}')

    from matplotlib import pyplot as plt
    plt.plot([1, 2, 3, 4])
    plt.ylabel('some numbers')
    img_file = "line.png"
    plt.savefig(img_file)

    from azureml.core import Run
    run = Run.get_context(allow_offline=True)
    run.upload_file(f"graphics/{img_file}", img_file)

    # Return value must be of a sequence of pandas.DataFrame
    # For example:
    #   -  Single return value: return dataframe1,
    #   -  Two return values: return dataframe1, dataframe2
    return dataframe1,
}
```

파이프라인 실행이 완료 되 면 모듈의 오른쪽 패널에서 이미지를 미리 볼 수 있습니다.

> [!div class="mx-imgBorder"]
> ![업로드 된 이미지 미리 보기](media/module/upload-image-in-python-script.png)

## <a name="how-to-configure-execute-python-script"></a>Python 스크립트 실행을 구성 하는 방법

Python 스크립트 실행 모듈에는 시작 지점으로 사용할 수 있는 샘플 Python 코드가 포함 되어 있습니다. Python 스크립트 실행 모듈을 구성 하려면 **python 스크립트** 텍스트 상자에서 실행할 입력 및 python 코드 집합을 제공 합니다.

1. 파이프라인에 **Python 스크립트 실행** 모듈을 추가 합니다.

2. **Dataset1** 에서 입력에 사용할 디자이너의 모든 데이터 집합을 추가 하 고 연결 합니다. Python 스크립트에서이 데이터 집합을 **DataFrame1** 로 참조 합니다.

    데이터 집합 사용은 선택 사항입니다. Python을 사용 하 여 데이터를 생성 하려는 경우 또는 Python 코드를 사용 하 여 데이터를 모듈로 직접 가져오는 경우이를 사용 합니다.

    이 모듈은 **Dataset2** 에서 두 번째 데이터 집합 추가를 지원 합니다. Python 스크립트의 두 번째 데이터 집합을 **DataFrame2** 로 참조 합니다.

    이 모듈을 사용 하 여 로드 하면 Azure Machine Learning에 저장 된 데이터 집합이 자동으로 pandas 데이터 프레임으로 변환 됩니다.

    ![Python 입력 맵 실행](media/module/python-module.png)

4. 새 Python 패키지 또는 코드를 포함 하려면 이러한 사용자 지정 리소스가 포함 된 압축 파일을 **스크립트 번들** 포트에 연결 합니다. 또는 스크립트가 16kb 보다 큰 경우에는 **스크립트 번들** 포트를 사용 하 여 *명령줄에서 16597 문자 제한을 초과* 하는 오류를 방지 합니다. 

    
    1. 스크립트 및 기타 사용자 지정 리소스를 zip 파일에 번들로 묶습니다.
    1. **파일 데이터 집합** 으로 zip 파일을 스튜디오에 업로드 합니다. 
    1. 디자이너 제작 페이지의 왼쪽 모듈 창에 *있는 데이터 집합 목록에서* 데이터 집합 모듈을 끌어 옵니다. 
    1. 데이터 집합 모듈을 **Python 스크립트 실행** 모듈의 **스크립트 번들** 포트에 연결 합니다.
    
    업로드 된 압축 된 보관 파일에 포함 된 모든 파일은 파이프라인 실행 중에 사용할 수 있습니다. 보관 파일에 디렉터리 구조가 포함 되어 있으면 구조가 유지 됩니다.
 
    > [!WARNING]
    > **앱** 은 기본 제공 서비스에 대해 예약 된 단어 이므로 폴더 또는 스크립트의 이름으로 **앱** 을 사용 **하지 마세요** . 그러나와 같은 다른 네임 스페이스를 사용할 수 있습니다 `app123` .
   
    다음은 python 스크립트 파일 및 txt 파일이 포함 된 스크립트 번들 예제입니다.
      
    > [!div class="mx-imgBorder"]
    > ![스크립트 번들 예제](media/module/python-script-bundle.png)  

    다음은의 내용입니다 `my_script.py` .

    ```python
    def my_func(dataframe1):
    return dataframe1
    ```
    다음은 스크립트 번들의 파일을 사용 하는 방법을 보여 주는 샘플 코드입니다.    

    ```python
    import pandas as pd
    from my_script import my_func
 
    def azureml_main(dataframe1 = None, dataframe2 = None):
 
        # Execution logic goes here
        print(f'Input pandas.DataFrame #1: {dataframe1}')
 
        # Test the custom defined python function
        dataframe1 = my_func(dataframe1)
 
        # Test to read custom uploaded files by relative path
        with open('./Script Bundle/my_sample.txt', 'r') as text_file:
            sample = text_file.read()
    
        return dataframe1, pd.DataFrame(columns=["Sample"], data=[[sample]])
    ```

5. **Python 스크립트** 텍스트 상자에 올바른 Python 스크립트를 입력 하거나 붙여 넣습니다.

    > [!NOTE]
    >  스크립트를 작성할 때 주의 해야 합니다. 선언 되지 않은 변수 또는 가져온 모듈이 나 함수를 사용 하는 것과 같은 구문 오류가 없는지 확인 합니다. 사전 설치 된 모듈 목록에 추가 주의를 기울여야 합니다. 나열 되지 않은 모듈을 가져오려면 다음과 같이 스크립트에 해당 패키지를 설치 합니다.
    >  ``` Python
    > import os
    > os.system(f"pip install scikit-misc")
    > ```
    
    **Python 스크립트** 텍스트 상자는 주석의 몇 가지 지침과 데이터 액세스 및 출력에 대 한 샘플 코드를 사용 하 여 미리 채워져 있습니다. 이 코드를 편집 하거나 바꾸어야 합니다. 들여쓰기 및 대/소문자 구분에 대 한 Python 규칙을 따릅니다.

    + 스크립트는 `azureml_main` 이 모듈에 대 한 진입점으로 이라는 함수를 포함 해야 합니다.
    + 진입점 함수에는 `Param<dataframe1>` `Param<dataframe2>` 스크립트에서 이러한 인수가 사용 되지 않는 경우에도 두 개의 입력 인수 및가 있어야 합니다.
    + 세 번째 입력 포트에 연결 된 압축 파일의 압축을 푼 다음 디렉터리에 저장 합니다 `.\Script Bundle` .이 파일은 Python에도 추가 됩니다 `sys.path` . 

    .Zip 파일에이 포함 되어 있으면 `mymodule.py` 를 사용 하 여 가져옵니다 `import mymodule` .

    두 데이터 집합은 형식의 시퀀스 여야 하는 디자이너로 반환 될 수 있습니다 `pandas.DataFrame` . Python 코드에서 다른 출력을 만들고 Azure storage에 직접 쓸 수 있습니다.

    > [!WARNING]
    > **Python 스크립트 실행 모듈** 에서 데이터베이스나 다른 외부 저장소에 연결 하지 **않는** 것이 좋습니다. [데이터 가져오기 모듈](./import-data.md) 및 [데이터 내보내기 모듈](./export-data.md) 을 사용할 수 있습니다.     

6. 파이프라인을 제출합니다.

    모듈이 완료 되 면 예상 대로 출력을 확인 합니다.

    모듈이 실패 한 경우 몇 가지 문제를 해결 해야 합니다. 모듈을 선택 하 고 오른쪽 창에서 **출력 + 로그** 를 엽니다. **70_driver_log.txt** 를 열고 **azureml_main** 를 검색 하면 오류가 발생 한 줄을 찾을 수 있습니다. 예를 들어 azureml_main "파일"/tmp/tmp01_ID/user_script. py "는 python 스크립트의 17 줄에서 오류가 발생 했음을 나타냅니다.

## <a name="results"></a>결과

포함 된 Python 코드에의 한 계산 결과는 `pandas.DataFrame` 자동으로 Azure Machine Learning 데이터 집합 형식으로 변환 되는로 제공 되어야 합니다. 그런 다음 파이프라인의 다른 모듈과 함께 결과를 사용할 수 있습니다.

이 모듈은 두 개의 데이터 집합을 반환 합니다.  
  
+ **결과 데이터 집합 1** 은 Python 스크립트에서 첫 번째 반환 된 pandas 데이터 프레임에 의해 정의 됩니다.

+ **결과 데이터 집합 2** 는 Python 스크립트에서 반환 된 두 번째 pandas 데이터 프레임으로 정의 됩니다.

## <a name="preinstalled-python-packages"></a>사전 설치 된 Python 패키지
사전 설치 된 패키지는 다음과 같습니다.
-    adal==1.2.2
-    applicationinsights==0.11.9
-    attrs = = 19.3.0
-    azure-common = = 1.1.25
-    azure-core = = 1.3.0
-    azure-graphrbac==0.61.1
-    azure-identity = = 1.3.0
-    azure-mgmt-authorization==0.60.0
-    azure-mgmt-containerregistry==2.8.0
-    azure-mgmt-keyvault = = 2.2.0
-    azure-mgmt-resource = = 8.0.1
-    azure-관리-저장소 = = 8.0.0
-    azure-storage-blob = = 1.5.0
-    azure-저장소-공통 = = 1.4.2
-    azureml-코어 = = 1.1.5.5
-    azureml-dataprep-native = = 14.1.0
-    azureml-dataprep = = 1.3.5
-    azureml-기본값 = = 1.1.5.1
-    azureml-디자이너-클래식-모듈 = = 0.0.118
-    azureml 디자이너-코어 = = 0.0.31
-    azureml-디자이너-내부 = = 0.0.18
-    azureml-모델-관리-sdk = = 1.0.1 b6. post1
-    azureml-파이프라인-코어 = = 1.1.5
-    azureml-원격 분석 = = 1.1.5.3
-    backports.tempfile==1.0
-    backports.weakref==1.0.post1
-    boto3 = = 1.12.29
-    botocore = = 1.15.29
-    cachetools = = 4.0.0
-    certifi = = 2019.11.28
-    cffi==1.12.3
-    chardet==3.0.4
-    = = 7.1.1를 클릭 합니다.
-    cloudpickle = = 1.3.0
-    configparser = = 3.7.4
-    contextlib2 = = 0.6.0 post1
-    암호화 = = 2.8
-    cycler==0.10.0
-    dill==0.3.1.1
-    distro==1.4.0
-    docker = = 4.2.0
-    docutils==0.15.2
-    dotnetcore2 = = 2.1.13
-    flask = = 1.0.3
-    fusepy==3.0.1
-    gensim==3.8.1
-    google-api-core = = 1.16.0
-    google-auth = = 1.12.0
-    google-cloud-core = = 1.3.0
-    google-cloud-storage = = 1.26.0
-    google-다시 시작 가능-미디어 = = 0.5.0
-    googleapis-protos = = 1.51.0
-    gunicorn = = 19.9.0
-    idna = = 2.9
-    불균형-배우기 = = 0.4.3
-    isodate==0.6.0
-    itsdangerous = = 1.1.0
-    jeepney = = 0.4.3
-    jinja2 = = 2.11.1
-    jmespath = = 0.9.5
-    joblib = = 0.14.0
-    py = = 0.2
-    jsonpickle = = 1.3
-    jsonschema = = 3.0.1
-    kiwisolver==1.1.0
-    liac-arff = = 2.4.0
-    lightgbm==2.2.3
-    markupsafe = = 1.1.1
-    matplotlib = = 3.1.3
-    itertools = = 6.0.0
-    msal-extensions = = 0.1.3
-    msal = = 1.1.0
-    msrest = = 0.6.11
-    msrestazure = = 0.6.3
-    ndg-httpsclient==0.5.1
-    nimbusml = = 1.6.1
-    numpy = = 1.18.2
-    oauthlib==3.1.0
-    pandas = = 0.25.3
-    pathspec = = 0.7.0
-    pip = = 20.0.2
-    portalocker = = 1.6.0
-    protobuf = = 3.11.3
-    pyarrow = = 0.16.0
-    pyasn1 = = 0.2.8
-    pyasn1 = = 0.4.8
-    pycparser = = 2.20
-    pycryptodomex = = 3.7.3
-    pyjwt = = 1.7.1 for
-    pyopenssl = = 19.1.0
-    pyparsing = = 2.4.6
-    pyrsistent = = 0.16.0
-    python-dateutil = = 2.8.1
-    pytz==2019.3
-    요청-oauthlib = = 1.3.0
-    요청 = = 2.23.0
-    rsa = = 4.0
-    ruamel.yaml==0.15.89
-    s3transfer = = 0.3.3
-    scikit-배우기 = = 0.22.2
-    scipy = = 1.4.1
-    secretstorage = = 3.1.2
-    setuptools = = 46.1.1. post20200323
-    6 = = 1.14.0
-    스마트 열림 = = 1.10.0
-    urllib3 = = 1.25.8
-    websocket-client = = 0.57.0
-    werkzeug = = 0.16.1
-    wheel = = 0.34.2

## <a name="next-steps"></a>다음 단계

Azure Machine Learning에서 [사용 가능한 모듈 세트](module-reference.md)를 참조하세요.