

## Before  
[Ref](http://jake.dothome.co.kr/user-virtual-maps-brk/)  

#### 1. ARENA  

#### 2. program_break  
heap memory의 가장 위쪽 주소  

#### 2. brk()  

#### 3. mmap()  


## malloc  
* 힙에 메모리를 할당받는다.  

#### 1. how  
* malloc은 regular function(as opposed to a system call)  

* 내부적으로 brk() 와 mmap() 두 가지를 사용한다.  

일정수치 이상이면 mmap을 호출하여, 할당.  
![스크린샷, 2020-04-30 08-43-32](https://user-images.githubusercontent.com/62331555/80657513-c0b92580-8abe-11ea-8d8c-ed01ba3cc034.png)  

