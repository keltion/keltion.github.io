---
title: "Fisrt steps"
layout: page
subtitle: sort
categories: study
tags: nest
comments: false
---

# First steps
Nest의 핵심에 대해서 알아보자. Nest 애플리케이션의 필수 구성 요소(essential building block)에 익숙해지기 위해 입문 수준에서 많이 다루는 기능을 갖춘 기본 CRUD 애플리케이션을 빌드해볼 것이다.

## Setup
Nest CLI로 프로젝트를 시작하려면 다음 명령어를 실행하면된다.
```console
npm i -g @nestjs/cli
nest new project-name
```
위 명령어를 실행하면 project-name 디렉토리가 생성되고, node modules과 다른 몇가지 boilerplate files이 설치된다. 그리고 src/ 디렉토리가 생성되어 여러 코어 파일로 채워진다.

> boilerplate files?
> node modules이란: 재사용가능한 코드들의 집합으로 라이브러리와 같다고 생각하면된다.

src 디렉토리 밑에 생성된 코어 파일들에 대해서 살펴보자

app.controller.ts : single route가 있는 기본 컨트롤러
app.controller.spec.ts : controller의 unit test
app.module.ts : 애플리케이션의 루트 모듈
app.service.ts : single method로 간단한 서비스 제공
main.ts : 핵심 함수인 NestFactory를 사용하여 Nest 애플리케이션 인스턴스를 생성하는 애플리케이션의 항목 파일

main.ts에는 애플리케이션을 bootstrap하는 비동기 함수가 포함되어 있음.
> 왜 비동기 함수로 되어있음?
> bootstrap : 한 번 시작되면 알아서 진행되는 일련의 과정

Nest 애플리케이션 인스턴스를 생성하기 위해 핵심 NestFactory클래스를 사용합니다. NestFactory는 애플리케이션 인스턴스를 생성할 수 있는 몇 가지 정적 메서드를 제공합니다. create() 메서드는 INestApplication interface를 수행하는 응용 프로그램 개체를 반환합니다. 이 개체는 다음 장에서 설명하는 method 집합을 제공합니다. 위의 main.ts 예제에서는 애플리케이션이 인바운드 HTTP요청을 기다리도록 하는 HTTP listener를 시작하기만 하면 됩니다.

>> INestApplication interface??

Nest CLI로 스캐폴딩된 프로젝트는 개발자가 각 모듈을 자체 전용 디렉토리에 보관하는 규칙을 따르도록 권장하는 초기 프로젝트 구조를 생성합니다.

> 인바운드 HTTP요청?
> 정적 메서드?

## Platform
Nest는 플랫폼에 구애받지 않는 프레임워크를 목표로 합니다. 플랫폼 독립성을 통해 개발자가 여러 다른 유형의 애플리케이션에서 활용할 수 있는 재사용 가능한 논리적 부분을 생성할 수 있습니다. 기술적으로 Nest는 어댑터가 생성되면 모든 Node HTTP 프레임워크와 함께 작동할 수 있습니다. 기본적으로 지원되는 두 가지 HTTP 플랫폼인 express 및 fastify가 있습니다. 필요에 가장 적합한 것을 선택할 수 있습니다.

>> HTTP platform?
>> nest adapter?


### 1) platform-express
Express는 노드용으로 잘 알려진 미니멀리스트 웹 프레임워크입니다. 커뮤니티에서 구현한 많은 리소스가 포함된 battle test를 거친 production-ready 라이브러리입니다. @nestjs/platform-express 패키지가 기본적으로 사용됩니다. 많은 사용자가 Express를 잘 사용하고 있으며 이를 활성화하기 위해 조치를 취할 필요가 없습니다.

### 2) platform-fastify
Fastify는 최대 효율성과 속도 제공에 중점을 둔 고성능 및 낮은 오버헤드 프레임워크입니다. 

어떤 플랫폼을 사용하든 자체 애플리케이션 인터페이스를 노출합니다. 이들은 각각 NestExpressApplication 및 NestFastifyApplication으로 표시됩니다.

아래 예제와 같이 NestFactory.create() 메서드에 유형을 전달하면 앱 개체에는 해당 특정 플랫폼에서만 사용할 수 있는 메서드를 가질거야. 그러나 기본 플랫폼 API에 실제로 액세스하려는 경우가 아니면 유형을 지정할 필요가 없습니다.

>> 플랫폼 부분 이해가 안돼.

## Running the application
설치 프로세스가 완료되면 OS 명령 프롬프트에서 다음 명령을 실행하여 인바운드 HTTP요청을 수신하는 애플리케이션을 시작할 수 있어

```console
npm run start
````

이 명령은 src/main.ts 파일에 정의된 포트에서 수신 대기하는 HTTP 서버로 앱을 시작합니다. 애플리케이션이 실행되면 브라우저를 열고 http://localhost:3000/으로 이동하여 Hello World를 봐봐.
