

# systemd  

1. **init 프로세스를 대체**하게 된 pid=1을 가지는 프로세스  
[Switch to the systemd](https://wiki.cdot.senecacollege.ca/wiki/Init_vs_systemd)  

* top으로 확인해보면 system의 pid는 1이다.  
![image](https://user-images.githubusercontent.com/62331555/80826008-777ee800-8c1c-11ea-8d57-75378e74793e.png)  


2. 모든 프로세스의 **부모**  

3. 컴퓨터 부팅이 완료된 후 systemd가 제어권을 넘겨받으면 **/etc/init.d**에 등록된 **start-up daemon**들을 실행시킨다.  
* etc/init.d 아래에 apache2가 있다.  
이는 shell script로서, systemd가 부팅 후 자동으로 실행하여, apache2가 부팅되면 자동으로 실행되는 것이다.  
![image](https://user-images.githubusercontent.com/62331555/80826932-09d3bb80-8c1e-11ea-8085-379c8b5168ec.png)  

