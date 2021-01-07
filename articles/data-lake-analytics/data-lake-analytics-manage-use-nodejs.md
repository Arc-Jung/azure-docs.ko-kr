---
title: Node.js용 Azure SDK를 사용하여 Azure 데이터 레이크 분석 관리
description: 이 문서에서는 Node.js용 Azure SDK를 사용하여 Data Lake Analytics 계정, 데이터 원본, 작업 및 사용자를 관리하는 방법을 설명합니다.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 12/05/2016
ms.custom: devx-track-js
ms.openlocfilehash: ca1c865b31739e7163b0db26cb666a8a42edbaa2
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92218462"
---
# <a name="manage-azure-data-lake-analytics-using-azure-sdk-for-nodejs"></a>Node.js용 Azure SDK를 사용하여 Azure 데이터 레이크 분석 관리
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

이 문서에서는 Node.js용 Azure SDK를 사용하여 작성된 앱을 사용하여 Azure Data Lake Analytics 계정, 데이터 원본, 사용자 및 작업을 관리하는 방법을 설명합니다. 

지원되는 버전은 다음과 같습니다.
* **Node.js 버전: 0.10.0 이상**
* **계정에 대한 REST API 버전: 2015-10-01-preview**
* **카탈로그에 대한 REST API 버전: 2015-10-01-preview**
* **작업에 대한 REST API 버전: 2016-03-20-preview**

## <a name="features"></a>기능
* 계정 관리: 만들기, 가져오기, 나열, 업데이트 및 삭제
* 작업 관리: 제출, 가져오기, 나열, 취소
* 카탈로그 관리: 가져오기 및 나열

## <a name="how-to-install"></a>설치 방법
```bash
npm install azure-arm-datalake-analytics
```

## <a name="authenticate-using-azure-active-directory"></a>Azure Active Directory를 사용하여 인증
 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-analytics-client"></a>Data Lake 분석 클라이언트 만들기
```javascript
var adlaManagement = require("azure-arm-datalake-analytics");
var accountClient = new adlaManagement.DataLakeAnalyticsAccountClient(credentials, 'your-subscription-id');
var jobClient = new adlaManagement.DataLakeAnalyticsJobClient(credentials, 'azuredatalakeanalytics.net');
var catalogClient = new adlaManagement.DataLakeAnalyticsCatalogClient(credentials, 'azuredatalakeanalytics.net');
```

## <a name="create-a-data-lake-analytics-account"></a>Data Lake 분석 계정 만들기
```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlaacct';
var location = 'eastus2';

// A Data Lake Store account must already have been created to create
// a Data Lake Analytics account. See the Data Lake Store readme for
// information on doing so. For now, we assume one exists already.
var datalakeStoreAccountName = 'existingadlsaccount';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location,
  properties: {
    defaultDataLakeStoreAccount: datalakeStoreAccountName,
    dataLakeStoreAccounts: [
      {
        name: datalakeStoreAccountName
      }
    ]
  }
};

client.account.create(resourceGroupName, accountName, accountToCreate, function (err, result, request, response) {
  if (err) {
    console.log(err);
    /*err has reference to the actual request and response, so you can see what was sent and received on the wire.
      The structure of err looks like this:
      err: {
        code: 'Error Code',
        message: 'Error Message',
        body: 'The response body if any',
        request: reference to a stripped version of http request
        response: reference to a stripped version of the response
      }
    */
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="get-a-list-of-jobs"></a>작업 목록 가져오기
```javascript
var util = require('util');
var accountName = 'testadlaacct';
jobClient.job.list(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="get-a-list-of-databases-in-the-data-lake-analytics-catalog"></a>Data Lake 분석 카탈로그에서 데이터베이스 목록 가져오기
```javascript
var util = require('util');
var accountName = 'testadlaacct';
catalogClient.catalog.listDatabases(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a>참고자료
* [Node.js용 Microsoft Azure SDK](https://github.com/azure/azure-sdk-for-node)
