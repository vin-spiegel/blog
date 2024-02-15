---
title: "OpenAI을 활용한 주간보고 자동화 프로그램 개발"
description: "OpenAI를 활용하여 주간보고 작성을 자동화하는 개발 과정에 대해 소개합니다."
date: 2024-02-16
update: 2024-02-16
tags:
  - .NET
  - OpenAI
  - 자동화
series: "개발자를 위한 주간보고 자동화 프로그램"
---

## .NET을 활용한 주간보고 자동화 프로그램 개발기

다니고 있는 회사는 매주 주간 보고를 합니다. 이때 그 동안의 깃 커밋 내역을 확인하고 요약하게 되는데, 이 과정에서 중요한 정보를 놓칠때도 있고, 여러 작업을 동시에 하는 경우, 요약이 제대로 안될때가 많았습니다. 매주 프로젝트의 깃(Git) 커밋 내역을 확인하고 이를 기반으로 주간보고를 작성하는 것은 시간과 노력이 많이 필요한 작업이었습니다. 따라서 자동화된 프로그램이 필요하다고 판단했습니다.
이번 포스트에서는 그 프로그램을 개발하는 과정과 사용된 기술에 대해 소개하겠습니다.

## 기술 선택

C#을 주로 사용하기 때문에 .NET 프레임워크를 선택했습니다. 사용한 라이브러리는 간단한 Cli 프로그램으로 만들생각이었기 때문에
[CommandLineParser](https://github.com/commandlineparser/commandline), OpenAI의 API를 좀 더 쉽게 사용할 수 있게 만들어주는
래퍼 라이브러리인 [OpenAI-DotNet](https://github.com/RageAgainstThePixel/OpenAI-DotNet), Git 커맨드를 C#코드로 사용 할 수 있게 만들어주는 [LibGit2Sharp](https://github.com/libgit2/libgit2sharp/)를 이용했습니다.

## 개발 과정

프로그램 개발은 크게 세 가지 단계로 나눌 수 있습니다.

가. 깃 커밋 내역 수집: LibGit2Sharp를 활용하여 깃 저장소의 커밋 내역을 수집합니다.
나. 자연어 처리: 퓨샷 러닝
다. 보고서 생성: 요약된 커밋 내역을 바탕으로 주간 보고서를 자동으로 생성합니다.

### 자연어 처리
자연어 처리 기술로는 퓨샷 러닝을 활용했습니다. 퓨샷 러닝은 적은 양의 데이터로도 높은 성능을 내는 학습 기술입니다.
```cs
// 깃 로그
var request = new Message(Role.User, @"
240301 - Spiegel - 로그인 이벤트 핸들링 부분 예외처리 추가                                   
240301 - Spiegel - 개발 환경에서 로그인 프로세스 패스")

// Open ai가 응답할 답변
var response = new Message(Role.System, @"
Spiegel - 로그인 버그 수정 및 테스트 환경 작업")
```

위의 코드 예시에서 `request` 변수는 사용자로부터 입력된 깃 커밋 메시지를 한 주 동안의 작업 내역을 담고있습니다. 시스템의 응답은 `response`에 제시되어 있습니다. 이런 질의응답 방식의 사전정보를 ai에게 같이 제공함으로써 응답 형식을 제어할 수 있습니다.

## 결론

포스트에 사용된 프로그램의 GitHub 레포지토리 링크는 [여기](https://github.com/vin-spiegel/AICommitSummarizer)에서 확인할 수 있습니다.