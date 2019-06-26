---
layout: post
title:  "RDS instance 생성"
categories: AWS
---
json(base.json)을 이용해서 secret은 깃 제외 폴더,  
셋팅에는 불러오는 과정 적어둬서 로컬의 json파일이 있을 때만 시크릿 키를 획득할 수 있게 셋팅

### RDS 생성

-   postgresql 선택
-   설정
    -   DB인스턴스 식별자, 마스터 사용자 이름, 마스터 암호  
        (마스터 사용자 이름과 암호 꼭 잘 기억하기)
    -   기본 VPC 보안그룹 사용(미리 생성해놓은 보안그룹, default 지우기)  
        인바운드 설정 시, DB관련된 설정은 '내 IP'로 설정하는거 기억!
    -   퍼블릭 엑세스 가능
    -   DB 이름 설정
-   생성

### RDS 터미널로 접속

-   `psql --host=<end point 복사> --user=<사용자 이름> --port=5432 postgres`
    -   여기서 나는 에러(비밀번호가 뜨지 않는 경우)  
        보안그룹에 inbound 설정에 규칙 추가해주기
-   접속 ok sql문으로 데이터 확인하자

### json파일로 RDS 설정 보관

```

{  
  "DATABASES": {  
    "default": {  
      "ENGINE": "django.db.backends.postgresql",  
      "HOST": "엔드포인트",  
      "POST": 5432,  
      "USER": "유져",  
      "PASSWORD": "비밀번호",  
      "NAME": "ec2_deploy_rds"  
    }  
  }  
}    
```