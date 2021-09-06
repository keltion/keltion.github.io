---
title: "Controllers"
layout: page
subtitle: sort
categories: study
tags: nest
comments: false
---

# Controllers
컨트롤러는 들어오는 요청을 처리하고 클라이언트에 응답을 반환하는 역할을 합니다.

라우팅 메커니즘은 어떤 컨트롤러가 어떤 요청을 수신하는지를 제어합니다.

기본 컨트롤러를 만들기 위해 클래스와 데코레이터를 사용합니다. 데코레이터는 클래스를 필수 메타데이터와 연결하고 Nest가 라우팅 맵을 생성할 수 있도록 합니다.(요청을 해당 컨트롤러에 연결)

## Routing
다음 예제에서는 기본 컨트롤러를 저의하는 데 필요한 @Controller()데코레이터를 사용합니다. Controller() 데코레이터에서 경로 접두사를 사용하면 관련 경로 세트를 쉽게 그룹화하고 반복적인 코드를 최소화할 수 있습니다.

```console
nest g controller cats
```
로 컨트롤러 생성.

findAll() 메서드 앞의 @Get() HTTP request decorator는 Nest에 HTTP요청에 대한 특정 endpoint에 대한 핸들러를 생성하도록 지시. 앤드포인트는 HTTP 요청방법(GET) 및 route path에 해당합니다. 
What route path?
핸들러의 route path는 Controller에 대해 선언된 접두사와 매서드의 데코레이터에 지정된 경로를 연결하여 결정됩니다. 

위의 예에서 이 엔드포인트에 GET 요청이 발생하면 Nest는 요청을 사용자 정의 findAll() 매서드로 라우팅합니다.
이 메서드는 200 상태코드와 관련 응답을 반환합니다. 이 경우에는 문자열일 뿐입니다. 왜 그런일이 발생>? 설명을 위해 먼저 Nest가 응답을 조작하기 위해 두 가지 다른 옵션을 사용한다는 개념을 소개

### 1) Standard
이 built-in 메소드를 사용하면 request handler가 Javascript객체 나 배열을 반환할 때 자동으로 JSON으로.그러나 Javascript 기본 유형(문자열, 숫자, 부울)을 반환하면 Nest는 직렬화를 시도하지 않고 값만 보냅니다. 이렇게 하면 응답 처리가 간단해집니다. 값을 반환하기만 하면 Nest가 나머지를 처리합니다.

또한 응답의 상태 코드는 201을 사용하는 POST 요청을 제외하고 기본적으로 항상 200입니다. 핸들러 수준에서 @HttpCode(...) 데코레이터를 추가하여 이 동작을 쉽게 변경할 수 있습니다.
### 2) Library-specific
method handler signature(findAll(@Res() response)에서 @Res()데코레이터를 사용하여 주입할 수 있는 라이브러리별(ex. Express) 응답 개체를 사용할 수 있습니다. 이 접근 방식을 사용하면 해당 개체에 의해 노출된 기본 응답 처리 메서드를 사용할 수 있습니다. 예를 들어 Express를 사용하면 response.status(200).send()와 같은 코드를 사용하여 응답을 구성할 수 있습니다.

## Request object
핸들러는 종종 클라이언트 요청 세부 정보에 엑세스해야 합니다. Nest는 기본 플랫폼의 요청 개체에 대한 엑세스를 제공합니다(express). handler's signature에 @Req() 데코레이터를 추가하여 요청 개체를 삽입하도록 Nest에 지시하여 요청 개체에 액세스할 수 있습니다.

요청 객체는 HTTP 요청을 나타내며 요청 쿼리 문자열, 매개변수, HTTP 헤더 및 본문에 대한 속성을 가지고 있습니다. 대부분의 경우 이러한 속성을 수동으로 가져올 필요가 없습니다. 대신 바로 사용할 수 있는 @Body()또는 @Query()와 같은 전용 데코레이터를 사용할 수 있습니다.

기본 HTTP 플랫폼(예: Express 및 Fastify)에서 입력과의 호환성을 위해 Nest는 @Res() 및 @Response() 데코레이터를 제공합니다. @Res()는 단순히 @Response()의 별칭입니다. 둘 다 underlying native platform 응답 개체 인터페이스를 직접 노출합니다. 그것들을 사용할 때, 최대한 활용하려면 기본 라이브러리에 대한 타이핑도 가져와야합니다. method handler에 @Res()또는 @Response()를 삽입할 때 Nest를 해당 핸들러에 대한 라이브러리 특정모드로 설정하고 응답 관리를 담당하게 됩니다. 그렇게 할 때 응답 객체(res.json(...) or res.send(...))를 호출하여 일종의 응답을 발행해야 합니다. 그렇지 않으면 HTTP 서버가 중단됩니다.





