---
layout: post
title:  "IAM 생성"
categories: AWS
---
### IAM 생성

#### 사용자 추가

-   프로그래밍 방식 엑세스(key를 이용해 터미널로)
-   콘솔 엑세스(인터넷 창)
    
#### 암호 설정
    
-   기존 정책 직접 연결 : s3 검색 후 AmazonS3FullAccess 선택
    
#### 비밀 엑세스 키와 암호 기록
    
-   이 때만 볼 수 있음!!
-   폴더 생성: `mkdir ~/.aws`

> credentials 파일 만들고 아이디, 키 적어주기  
> \[name\]  
> aws\_access\_key\_id = ddddd  
> aws\_secret\_access\_key = sdfdskfjhk

> config 파일  
> \[default\]  
> region = ap-northeast-2  
> output = json