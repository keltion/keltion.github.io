---
title: "[Chromium style guide] 스마트포인터"
layout: post
subtitle: smartpointer
categories: project
tags: chromium
comments: false
---

(https://www.chromium.org/developers/smart-pointer-guidelines를 보고 번역하며 정리한 글입니다.)

# smart pointer란 무엇인가?

스마트 포인터는 'scoping object' 객체입니다. 일반 포인터와 비슷하지만 범위를 벗어날 때 자동으로 할당된 객체를 해제해줍니다. 기존에 raw pointer는 정해진 범위가 존재하지않아 메모리누수관리를 위해서 프로그래머가 직접 메모리를 해제해야 했던 것과 차이가 있습니다.

잘 와닿지 않으니 코드로써 스마트포인터가 어떠한 일을 해주는지 이해해보겠습니다.

```cpp
#include <iostream>
#include <memory>

class Cat {
public:
    Cat() {
        std::cout << "Cat constructor" << std::endl;
    }
    ~Cat() {
        std::cout << "Cat destructor" << std::endl;
    }
};
int main() {
    {
        Cat *rawCat = new Cat();
    }
    std::cout << "after scope" << std::endl;

}
```
raw pointer로 Cat의 객체를 선언해주고 컴파일 후 실행해주면 다음과 같이 출력됩니다.
```console
Cat constructor
after scope
``` 
rawCat이 정의된 scope가 벗어나도 rawCat이 가리키는 힙에 할당된 객체는 해제되지 않습니다. 이는 프로그래머가 직접 delete 키워드를 사용하여 해제해줘야합니다.

이제 스마트 포인터를 사용한 예시를 보겠습니다.

```cpp
#include <iostream>
#include <memory>

class Cat {
public:
    Cat() {
        std::cout << "Cat constructor" << std::endl;
    }
    ~Cat() {
        std::cout << "Cat destructor" << std::endl;
    }
};
int main() {
    {
        std::unique_ptr<Cat> uniCat = std::make_unique<Cat>();
    }
    std::cout << "after scope" << std::endl;
}
```
위 코드를 컴파일 후 실행해주면 다음과 같이 출력됩니다.
```console
Cat constructor
Cat destructor
after scope
``` 
uniCat이 정의된 scope가 벗어나면 자동으로 힙에 할당된 객체를 해제한다는 것을 알 수 있습니다.

'scoping object'를 사용하여 힙 할당 객체의 수명을 자동으로 관리하는 패턴을 RAII-Resource Acquisition Is Initialization이라고 합니다.


## 스마트 포인터를 왜 사용하나요?
원시 포인터를 사용하다보면 객체 생성하는 시기와 해제 시기가 멀리 떨어져 있을 때 해제하는 것을 잊기 쉽습니다. 이때 스마트 포인터를 사용하면 위와 같은 걱정을 할 필요가 없으며 함수가 많은 exit paths가 존재하더라도 함수를 더 간단하고 안전하게 만들 수 있다고 합니다. 마지막으로 한 객체를 하나의 객체에 의해서만 소유되도록 하여 메모리누수와 객체가 두번 해제되는 것을 예방합니다.

## 스마트 포인터의 유형에는 어떤 것들이 있나요?
크로미움에서 두 가지 일반적인 스마트 포인터는 std:::unique_ptr<>과 scoped_refptr<>입니다. 전자는 singly-owned objects에 사용되고, 후자는 reference-counted objects(이런 상황은 피하는게 좋습니다)에 사용됩니다.(scpoed_refptr<>은 std::shared_ptr<>와 의도가 비슷함)

WeakPtr<>은 실제로 스마트 포인터가 아니며 포인터처럼 동작하지만 객체를 자동으로 해제하는 데 사용되지 않고 다른 곳에 소유한 객체가 아직 살아 있는지 추적하는 데 사용됩니다. 개체가 제거되면 WeakPtr<>은 자동으로 null로 설정되므로 더 이상 활성 상태가 아님을 확인할 수 있습니다.(역참조 전에 null 테스트를 해야합니다) 이것은 std::weak_ptr<>과 비슷하지만 다른 API와 더 적은 제한이 있습니다.

## 언제 스마트포인터를 사용하나요?

<b>singly-owned objects</b>
std::unique_ptr을 사용합니다.

<b>non-owned objects</b>
원시포인터 또는 WeakPtr<> 사용합니다. WeakPtr<>은 생성된 동일한 스레드에서만 역참조되어야 하며, 객체가 파괴되기 직전이나 직후에 조치를 취해야 하는 경우에는 더 좋을 것입니다. 

<b>Ref-counted objects</b>
scoped_refptr<>을 사용하지만 더 나은 방법은 디자인을 재고하는 것입니다. Ref-counted objects는 특히 여러 스레드가 관련된 경우 소유권 및 소멸 순서를 이해하기 어렵게 만듭니다. refcounting을 피하기 위해 객체 계층 구조를 설계하는 다른 방법이 높은 확률로 존재합니다.


## 다른 종류의 포인터와 관련된 호출 규칙은 무엇인가요?
1. 함수의 매개변수가 std::unique_ptr<>라면 인수의 소유권을 갖는다는 의미입니다. 호출자는 전달되는 객체가 temporary가 아닌 경우 소유권을 전달하고 있음을 나타내기 위해 std::move()를 사용해야합니다. 

### a) 일반적인 상황
```cpp
void print(std::unique_ptr<Cat> mCat) {
    std::cout << mCat->Age << std::endl;
}
    

int main() {
    std::unique_ptr<Cat> uniCat = std::make_unique<Cat>(5);
    print(std::move(uniCat));
}
```
### b) 객체가 temporary인 경우 
```cpp
int main() {
    print(std::make_unique<Cat>(5));     // print(std::unique_ptr<Cat>(new Cat(5))); 와 같음
    
}
```
2. 함수가 std::unique_ptr<>을 반환하면 호출자가 반환된 객체의 소유권을 갖는다는 의미입니다. 객체를 반환하는 동안 std::move() 사용은 함수의 반환 유형이 지역 변수의 유형과 다른 경우에 필요합니다.


### a) 일반적인 상황
```cpp
class Animal {};
class Cat : public Animal {};

std::unique_ptr<Animal> Foo(std::unique_ptr<Animal> manimal) {
  if (cond) {
    return manimal;                         
  }
```

### b) 객체가 temporary인 경우
```cpp
std::unique_ptr<Animal> Foo(std::unique_ptr<Animal> manimal) {
  if (cond2) {
    return std::unique_ptr<Animal>(new Animal())); // No std::move() necessary on temporaries.
  }
```

### c) 함수의 반환 유형이 지역 변수의 유형과 다른 경우
```cpp
std::unique_ptr<Animal> Foo(std::unique_ptr<Animal> manimal) {
  std::unique_ptr<Cat> cat(new Cat());
  return std::move(cat); // Note that std::move() is necessary because
  // type of |cat| is different from the return
  // type of the function.
  }
```

<!-- > unique_ptr 내부 할당자 그거 글쓰고
> RVO 글 쓰기

지금 일단 Add()구현 안해서
Add()에 다 sourdce가 들어가면서 release()돼
무슨 소리야?

release()가 어떤 역할을 하는지 찾아보고
Add()에서 해제되는지 그 전에서 해제되는지 알아보기 -->
