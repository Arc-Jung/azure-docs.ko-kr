﻿<properties
   pageTitle="HDInsight에서 Hadoop Pig 사용 | Azure"
   description="원격 데스크톱을 통해 HDInsight에서 Hadoop와 함께 Pig 사용 방법에 대해 알아봅니다."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang=""
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="02/18/2015"
   ms.author="larryfr"/>

#Pig 명령을 사용하여 Pig 작업 실행(원격 데스크톱)

[AZURE.INCLUDE [pig-selector](../includes/hdinsight-selector-use-pig.md)]

이 문서는 HDInsight 클러스터의 Linux 기반 Hadoop에 Pig Latin 문을 대화형으로 실행되도록 하거나 일괄 작업으로 실행되도록 Pig 명령을 사용하는 연습을 제공합니다. Pig Latin을 사용하면 매핑하고 함수를 줄이는 대신 데이터 변환을 설명하여 MapReduce 응용 프로그램을 만들 수 있습니다.

##<a id="prereq"></a>필수 조건

이 문서의 단계를 완료하려면 다음이 필요합니다.

* Windows 기반 HDInsight(HDInsight의 Hadoop) 클러스터

* Windows 7, 8 또는 10 클라이언트

##<a id="connect"></a>원격 데스크톱을 사용하여 연결

HDInsight 클러스터에 대해 원격 데스크톱을 사용하도록 설정한 다음 <a href="http://azure.microsoft.com/documentation/articles/hdinsight-administer-use-management-portal/#rdp" target="_blank">RDP를 사용하여 HDInsight 클러스터에 연결</a>의 지침에 따라 연결합니다.

##<a id="pig"></a>Pig 명령 사용

2. 연결되면 데스크톱의 아이콘을 사용하여 **Hadoop 명령줄**을 시작합니다.

2. Pig 명령줄을 시작하려면 다음을 참조하세요.

		%pig_home%\bin\pig

	 `grunt>` 프롬프트가 나타납니다. 

3. 다음 문을 입력합니다.

		LOGS = LOAD 'wasb:///example/data/sample.log';

	이 명령은 sample.log 파일의 내용을 로그에 로드합니다. 다음을 사용하여 파일의 내용을 볼 수 있습니다.

		DUMP LOGS;

4. 이어서 다음을 사용하여 각 레코드에서 로깅 수준만 추출하는 정규식을 적용하여 데이터를 변환합니다.

		LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

	**DUMP**를 사용하여 변환 후 데이터를 볼 수 있습니다. 이 경우 `DUMP LEVELS;`입니다.

5. 다음 문을 사용하여 변환 적용을 계속합니다.  `DUMP`를 사용하여 각 단계 후의 변환 결과를 확인합니다.

	<table>
	<tr>
	<th>문</th><th>기능</th>
	</tr>
	<tr>
	<td>FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;</td><td>로그 수준에 대한 null 값을 포함하는 행을 제거하고 FILTEREDLEVELS에 결과를 저장합니다.</td>
	</tr>
	<tr>
	<td>GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;</td><td>로그 수준에 따라 행을 그룹화하고 GROUPEDLEVELS에 결과를 저장합니다.</td>
	</tr>
	<tr>
	<td>FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;</td><td>고유한 각 로그 수준 값 및 발생 횟수를 포함하는 데이터의 새 집합을 만듭니다. FREQUENCIES에 저장됩니다.</td>
	</tr>
	<tr>
	<td>RESULT = order FREQUENCIES by COUNT desc;</td><td>로그 수준을 개수(내림차순)를 기준으로 정렬하고 RESULT에 저장합니다.</td>
	</tr>
	</table>

6.  `STORE` 문을 사용하여 변환 결과를 저장할 수도 있습니다. 예를 들어 다음은 클러스터의 기본 저장소 컨테이너에 있는 **/example/data/pigout** 디렉터리에 `RESULT`를 저장합니다.

		STORE RESULT into 'wasb:///example/data/pigout'

	> [AZURE.NOTE] 데이터는 이름이 **part-nnnnn**인 파일에서 지정된 디렉터리에 저장됩니다. 해당 디렉터리가 이미 존재하는 경우 오류가 발생합니다.

7. 성가신 프롬프트를 종료하려면 다음 문을 입력합니다.

		QUIT;

###Pig Latin 배치 파일

Pig 명령을 사용하여 파일에 포함된 Pig Latin을 실행할 수도 있습니다.

3. 성가신 프롬프트를 종료한 후 **메모장**을 열고 **% PIG_HOME %** 디렉터리에 **pigbatch.pig**라는 새로운 파일을 만듭니다.

4. 다음 줄을 **pigbatch.pig** 파일에 입력하거나 붙여 넣은 다음 완료되면 저장합니다.

		LOGS = LOAD 'wasb:///example/data/sample.log';
		LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
		FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
		GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
		FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
		RESULT = order FREQUENCIES by COUNT desc;
		DUMP RESULT;

5. pig 명령을 사용하여 **pigbatch.pig** 파일을 실행하려면 다음을 사용합니다.

		pig %PIG_HOME%\pigbatch.pig

	일괄 작업이 완료되면 다음과 같은 출력이 표시되며, 이 출력은 이전 단계에서 `DUMP RESULT;`를 사용했을 때와 같습니다.

		(TRACE,816)
		(DEBUG,434)
		(INFO,96)
		(WARN,11)
		(ERROR,6)
		(FATAL,2)

##<a id="summary"></a>요약

이처럼 Pig 명령을 사용하면 배치 파일에 저장된 문을 실행할 뿐 아니라 Pig Latin을 사용하여 MapReduce 작업을 대화형으로 실행할 수 있습니다.

##<a id="nextsteps"></a>다음 단계

HDInsight에서 Pig에 대한 일반 정보.

* [HDInsight에서 Hadoop과 Pig 사용](../hdinsight-use-pig/)

HDInsight에서 Hadoop으로 작업하는 다른 방법에 관한 내용입니다.

* [HDInsight에서 Hadoop과 Hive 사용](../hdinsight-use-hive/)

* [HDInsight에서 Hadoop과 MapReduce 사용](../hdinsight-use-mapreduce/)
<!--HONumber=47-->
