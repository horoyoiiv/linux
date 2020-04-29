

## mmap의 사용  


#### 1. malloc 호출 시   
* heap의 가장 윗부분을 program break라고 한다.  
* malloc 호출 시 brk를 호출하여, 그 heap의 한계점을 증가시켜 새로운 메모리를 힙에 할당하는 것이다.  

[brk vs mmap](https://www.youtube.com/watch?v=XV5sRaSVtXQ)  
