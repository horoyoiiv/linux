# mmap의 사용  

* file을 (mmap을 호출한) process의 가상 메모리에 매핑시킨다.  
* 혹은 메모리를 할당한다.(anonymous)  

## 1. 용어 정리  
#### 1.1 Page cache  
* Kernel 메모리 공간에서 file로부터 읽어들인 데이터를 저장하는 공간  
#### 1.2 Buffer cache  
* kernel 메모리 공간에서 file들의 메타데이터를 저장하는 공간  


## 2. 동작방식  
[Ref](https://medium.com/@sasha_f/why-mmap-is-faster-than-system-calls-24718e75ab37)  
[slide](http://nyx.skku.ac.kr/wp-content/uploads/2019/04/OS07-PageReplacement-1.pdf)  

* 일단 **디스크에서 데이터를 읽어오면 커널의 페이지 케시에 저장**된다.  
* 일반적인 **read()** 를 사용하면 [디스크] -> [커널 공간의 페이지 캐시] -> [유저 공간의 할당된 버퍼(또 다른 물리 공간에 할당이 필요)]로 복사된다.  
* mmap을 사용하면 [디스크] -> [커널 공간의 페이지 캐시]   
이 상태에서 프로세스의 페이지 테이블의 PTE는 이 **커널 공간의 페이지 캐시**를 가르킨다.  


![스크린샷, 2020-04-30 09-59-36](https://user-images.githubusercontent.com/62331555/80661337-5954a300-8ac9-11ea-8f33-049429751652.png)  

* mmap을 사용하지 않으면, 파일로부터 데이터를 읽고 쓸 때마다 read/write 등의 system call을 써야하는 오버헤드  
* mmap을 사용하면, 한번의 systemcall인 mmap 이후, 메모리에 접근하듯이 읽고 쓸 수 있다.  

* 또한 file의 특정 block은 실제로 참조될 때, 메모리로 로딩하는 **Lazy loading** 전략 사용.  


## 3. Function  

#### 1.void* mmap(int *start, size_t len, int prot, int flags, int fd, off_t offset)  
[Usage](https://www.clear.rice.edu/comp321/html/laboratories/lab10/)  

1. **start** : (kernel에 알려주는 힌트) start 주소부터 매핑하고자 한다.  
null이면 kernel이 결정  

2. **len** : 할당받을 크기(byte)  

3. **prot** : protection - 접근제어 등의 설정  
PROT_READ : readable by the process   
PROT_WRITE : writable by the process  
PROT_EXEC : executable by the process  


4. **flags** :   
MAP_ANONYMOUS (or MAP_ANON) - Allocate anonymous memory; the pages are not backed by any file.  
MAP_FILE - The default setting; it need not be specified. The mapped region is backed by a regular file.  
MAP_PRIVATE - Modifications to the mapped memory region are not visible to other processes mapping the same file.  
MAP_SHARED - Modifications to the mapped memory region are visible to other processes mapping the same file and are eventually reflected in the file.  
5. **fd** : memory에 매핑시킬 file descriptor - anonymous라면 -1을 넘김.  
6. **offset** : file에서의 offset부터 매핑시킴.  

## How it works and Why it is faster  

[Here](https://medium.com/@sasha_f/why-mmap-is-faster-than-system-calls-24718e75ab37)  

* Lazy Loading : file을 메모리에 매핑시켰을 때, 실제로 참조해야 디스크에서 메모리로 읽는다.  



If you use fread(), there will always be overhead from the system call API (int 80, trapping into kernel space) while the mmap() let you set up once of the page table mapping for the file content and all the subsequent memory operation is done without trapping into kernel space.
Of course, when the disk block is accessed for the first time, it has to be populated into page cache, and this is the price that both fread() and mmap() need to pay.
Thus, if you need to repeatedly access the same file content, then mmap() will work better under the condition that your page cache is large enough to hold the content that you are accessing.



## Conclusion  

* **파일**을 **Kernel의 Page cache**로 읽어들인 후**user space의 버퍼**에 복사하는 것이 아닌, 그 **커널 공간을 Page table의 PTE가 가르키게** 하는 것.  
* 이를 통해 **read/write 시의 system call 호출 오버헤드를 피하고**, 메모리에 접근하듯이 파일을 읽고 쓸 수 있는 기법.  






  


