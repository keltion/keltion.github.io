---
title: "Chromium scoped_ptr"
layout: post
subtitle: scoped_ptr
categories: project
tags: chromium
comments: true
---

# chromium scoped_ptr

## 주의!

해당 포스팅에는 아직 부족한 점이 많습니다. 

1. 크로미움에서는 멀티쓰레드 환경에서 refcount를 관리해주는 객체(RefCountedThreadSafe, RefCountedThreadSafeBase)와 싱글쓰레드 환경에서 refcount를 관리해주는 객체(RefCounted, RefCountedBase)가 따로 있습니다. 아직 멀티쓰레드에 대한 지식이 부족하여 여기서는 간단하게 RefCountedBase만을 이용하여 참조카운팅을 관리 하도록 하였습니다.
2. std::shared_ptr을 사용하는 대신 scoped_refptr을 사용하면 malloc 호출이 한 번 줄게 되는데 이것이 얼마나 큰 장점이 있는지 조사가 필요합니다.


## std::shared_ptr vs scoped_refptr

modern c++에는 소유권을 공유하기 위해서 사용하는 스마트 포인터로 std::shared_ptr이 있습니다. 크로미움에서는 refcounted 객체를 관리하기 위해서 std::shared_ptr을 사용하는 대신 scoped_refptr을 사용합니다. std::shared_ptr과 scoped_refptr에는 어떠한 차이점이 있을까요?

## std::shared_ptr

std::shared_ptr은 다른 객체와 소유권을 공유하는 스마트 포인터입니다. shared_ptr은 참조 카운팅을 통해서 메모리가 해제되는 시점을 결정합니다. 따라서 내부적으로 자원의 주소를 저장할 포인터와 참조 카운팅을 수행할 control block에 대한 포인터로 총 2개의 포인터를 가집니다. shared_ptr 자체적으로 참조 카운팅을 수행할 수 없습니다. (구현해보시면 자연스럽게 알게 됩니다!)

위의 설명으로 짐작했을 때 shared_ptr이 태생적으로 가질 수 밖에 없는 특징은 무엇일까요? 바로 shared_ptr을 생성하게 되면 메모리 할당을 위해서 2번의 malloc이 호출된다는 것입니다.

## scoped_refptr

크로미움에서 사용하는 scoped_refptr은 malloc의 호출을 1번으로 줄입니다. 어떻게 그것이 가능할까요? 바로 control block 대신 상속을 이용하여 참조 카운팅을 관리하기 때문입니다. 다음은 제가 크로미움 오픈소스를 보고 간략히 만든 scoped_refptr입니다. (편의를 위해 header, cpp 파일을 제대로 분리하지 않았습니다.)

```cpp
// scoped_refptr.h
#include <iostream>
namespace keltion{
template<class T>
class scoped_refptr {
public:
    scoped_refptr(T* p) : ptr_{p} {
        if(ptr_) {
            ptr_ -> AddRef();
        }
    }
    scoped_refptr& operator=(const scoped_refptr& other) {
        ptr_ -> Release();
        ptr_ = other.ptr_;
        ptr_ -> AddRef();
        return *this;
    }
    int use_count() {
        return ptr_ -> Count();
    }
    ~scoped_refptr() {
        if (ptr_) {
            ptr_ -> Release();
        }
    }
private:
    T* ptr_ = nullptr;
};
}// namespace keltion
```
```cpp
// main.cpp
#include <iostream>
#include "scoped_refptr.h"

class refCountedBase {
public:
    refCountedBase() : ref_count_{0} { }
    virtual ~refCountedBase() {}
    void AddRef() {
        (ref_count_)++;
    }
    void Release() {
        (ref_count_)--;
        if(ref_count_ == 0) {
            delete this;
        }
    }
    int Count() {
        return ref_count_;
    }
private:
    int ref_count_;
};

class TigerImpl : public refCountedBase {
public:
    ~TigerImpl() override {}
};

int main() {
	scoped_refptr<Tiger> bum = scoped_refptr<Tiger>(new Tiger());
}
```

scoped_refptr<T>은 자원의 주소를 저장할 포인터(ptr_) 하나만 가지고 있고, refcounted 객체에게 refCountedBase를 상속하여 참조카운팅을 관리하는 것을 확인 할 수 있습니다. 즉 메모리 할당을 위해 malloc이 한 번 호출됩니다.