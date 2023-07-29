---
title: "스프링 MVC 2-1: 메시지, 국제화"

categories:
  - Spring MVC
tags:
  - Spring MVC
---

## 스프링 MVC 2-1: 메시지, 국제화

#### 0. 메시지, 국제화

---

**메시지**

기획자가 화면에 보이는 문구가 마음에 들지 않는다고, 상품명이라는 단어를 모두 상품이름으로 고쳐달라고 하면 어떻게 해야할까?

여러 화면에 보이는 상품명, 가격, 수량 등, label 에 있는 단어를 변경하려면 다음 화면들을 다 찾아가면서 모두 변경해야 한다.
지금처럼 화면 수가 적으면 문제가 되지 않지만 화면이 수십개 이상이라면 수십개의
파일을 모두 고쳐야 한다.
왜냐하면 해당 HTML 파일에 메시지가 하드코딩 되어 있기 때문이다.

이런 다양한 메시지를 한 곳에서 관리하도록 하는 기능을 **메시지 기능**이라 한다.

예를 들어서 messages.properties 라는 메시지 관리용 파일을 만들고,

```
item=상품
item.id=상품 ID
item.itemName=상품명
item.price=가격
item.quantity=수량
```

각 HTML들은 다음과 같이 해당 데이터를 key 값으로 불러서 사용하는 것이다.
addForm.html

```html
<label for="itemName" th:text="#{item.itemName}"></label>
```

---

**국제화**

메시지에서 한 발 더 나가보자.
메시지에서 설명한 메시지 파일( messages.properties )을 각 나라별로 별도로 관리하면 서비스를 국제화 할 수 있다.
예를 들어서 다음과 같이 2개의 파일을 만들어서 분류한다.

**messages_en.properties**

```
item=Item
item.id=Item ID
item.itemName=Item Name
item.price=price
item.quantity=quantity
```

**messages_ko.properties**

```
item=상품
item.id=상품 ID
item.itemName=상품명
item.price=가격
item.quantity=수량
```

한국에서 접근한 것인지 영어에서 접근한 것인지는 인식하는 방법은 HTTP accept-language 해더 값을 사용하거나 사용자가 직접 언어를 선택하도록 하고, 쿠키 등을 사용해서 처리하면 된다.

#### 1. 스프링 메시지 소스 설정: MessageSource

---

메시지 관리 기능을 사용하려면 스프링이 제공하는 MessageSource 를 스프링 빈으로 등록하면 되는데, MessageSource 는 인터페이스이다. 따라서 구현체인 ResourceBundleMessageSource 를 스프링 빈으로 등록하면 된다.

```java
@Bean
public MessageSource messageSource() {
 ResourceBundleMessageSource messageSource = new ResourceBundleMessageSource();
 messageSource.setBasenames("messages", "errors");
 messageSource.setDefaultEncoding("utf-8");
 return messageSource;
}
```

setBasenames : 설정 파일의 이름을 지정한다.
messages 로 지정하면 messages.properties 파일을 읽어서 사용한다.
추가로 국제화 기능을 적용하려면 messages_en.properties , messages_ko.properties 와 같이 파일명 마지막에 언어 정보를 주면된다. 만약 찾을 수 있는 국제화 파일이 없으면 messages.properties (언어정보가 없는 파일명)를 기본으로 사용한다.
여러 파일을 한번에 지정할 수 있다. 여기서는 messages , errors 둘을 지정했다.

**스프링 부트**
스프링 부트를 사용하면 스프링 부트가 MessageSource 를 자동으로 스프링 빈으로 등록한다.
스프링 부트를 사용하면 application.properties에서 메시지 소스를 설정할 수 있다.

**스프링 부트 메시지 소스 기본 값**

```
spring.messages.basename=messages
```
