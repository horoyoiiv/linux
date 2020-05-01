

# daemon  

1. 부팅과 동시에 **systemd에 의해 실행**되며, **메모리에 계속해서 상주**한다.  
2. 계속 실행되면서 요청이 발생하면 그 요청을 처리하는 역할  
3. apached, mysqld 등과 같이 마지막이 d (daemon)으로 끝남.  
4. root권한으로 실행된다.  


### 1. daemon 과 background program (& 으로 실행)   
* 터미널에서 &으로 실행되기에 그 터미널 프로세스에 종속되면 안된다.  
[diff](https://unix.stackexchange.com/questions/56495/whats-the-difference-between-running-a-program-as-a-daemon-and-forking-it-into)  


