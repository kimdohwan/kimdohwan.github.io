---

title:  "Unix Socket으로 통신하는 이유"
tag: Etc
---
nginx 설정 시 http protocol 로 이루어진 request 를 socket 으로 바꿔주는 이유
http protocol 의 경우, 내가 보내려는 ip 로 도달하기 위해서 구성해줘야하는 meta data 부분이 굉장히 복잡한 편이라고 한다.(라우터 셋팅 등) 하지만 nginx 에서 uwsgi 로 request 를 넘겨주는 경우는 http protocol 의 형식을 사용하는 것이 지나치게 비효율적이다. 따라서 운영체제에서 효율적으로 작동시킬 수 있는 형식인 unix socket 을 이용한다.

소켓이 인터페이스와 비슷한 개념. 인터페이스는 사용자가 기능을 사용하기 위한 의사소통을 하는 통신 매개체로 볼 수 있는데, 특정 목적의 인터페이스를 만들면, 그 기능을 쓸때마다 인터페이스를 호출하여 사용하면 되고, 사용자는 인터페이스 뒤에 무엇이 있는지 신경쓰지 않고 인터페이스의 기능만 사용하여 쉽게 통신이 가능한 개념이다.

UDS(Unix Domain Socket)의 가장 큰 특징은 소켓통신 방식을 써서 만든 프로세스에 사용이 가능하기 때문에 소켓프로그래밍 구조를 유지한 채로 로컬 프로세스와의 효율적 통신을 가능케 한다는 점이다. TCP, 혹은 UDP형식 데이터를 파일 시스템을 이용해서 통신하는 구조로, 파일 시스템을 통해 파일 주소 및 inode로 각 프로세스에서 참조되며, 통신은 운영체제의 커널상에서 이뤄지기 때문에 inet소켓을 이용해서 네트워크단을 이용해 전달하는 것보다 빠르며 부하가 적게 걸린다.(기본적으로 소켓통신 방식이 TCP/IP의 4계층을 거쳐 전달되기 때문에 지연이 발생하는데 반해서 유닉스 소켓은 어플리케이션 계층에서 TCP계층으로 내려가 바로 데이터를 전달하고, 수신측도 TCP계층에서 수신해 어플리케이션 계층으로 올라간다)