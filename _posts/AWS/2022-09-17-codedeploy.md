---
layout: single
title: "[AWS] CodeDeploy 사용기"
categories: AWS
tag: [CodeDeploy]
toc: true
toc_label: "Contents" # toc 제목
toc_icon: "cog" # toc 아이콘(톱니바퀴)
author_profile: false
classes: wide
sidebar:
    nav: "docs"

---



# CodeDeploy

※참고 교재 : AWS 인프라 구축 가이드(위키북스)

## EC2정책 및 역할 생성

---

138 page
◼︎ "**EC2**"에 대한 1.**정책생성**. 그리고 그 정책을 적용한 2.**역할생성**

1. [정책생성] EC2 안의 Agent가 필요로 하는 "**A정책**" 생성('Github'나 'S3' 읽기와 관련된 여러 권한들을 JSON기반으로 적용)
    - 정책 JSON
      
        ```json
        {
        	"Version" : "2012-10-17",
        	"Statement" : [
        		{
        			"Action" : [
        				"s3:Get*",
        				"s3:List*"
        			],
        			"Effect" : "Allow",
        			"Resource" : "*"
        		}
        	]
        }
        ```
    
2. [역할생성] 정책이 적용될 역할 생성, 역할을 맡을 대상(서비스)은 "EC2" 
3. 역할 만들다보면 '연결할 정책'을 위의 "**A정책**" 선택 → 역할 생성 완료
4. 생성된 역할의 상세페이지에서 인스턴스 프로파일 ARN확인
    - 생성된 역할 상세페이지에서 확인할 수 있는 "**인스턴스 프로파일 ARN**" 값이 추후 사용됨.

→ **근데 사실 이거 잘못된것같다. EC2에는 아래의** **CodeDeploy-service-role(AWSCodeDeployRole 정책이 붙은)이 붙어야 하는 것으로 확인된다**

143 page

<aside>
💡 **※ CodeDeploy로 배포할 수 있는 인스턴스 조건**

1. 올바른 권한을 가진 역할이 필요
2. CodeDeploy Agent가 설치돼 있어야 함
3. 애플리케이션이 배포될 경로에 이미 저정돼 있는 파일이 없어야 함 (145페이지 보면 전부 삭제하고 있는걸 확인할수 있음)  ※ 기존에 파일이 있으면 에러가 나고 그런 개념이 아니라 배포될 경로가 지워졌다가 새 버전이 배포되는 개념인것으로 유추됨.(실제로 파일이 있어도 에러는 없었으나, 교재에서는 에러로 인식한다는 내용이 있음. 확인필요)

</aside>

## 서버에 Agent 설치

---

145 page
◼︎ Agent 설치
CodeDeploy Agent 설치위해 설치될 서버의 터미널에서

1. 설치스크립트 내려받기
2. 실행권한 추가
3. 스크립트 실행하면 현재 EC2에 맞게 Agent 설치 및 실행. (설치시 자동으로 시작 서비스에 등록됨)
4. Agent가 올바르게 실행됐는지 확인
5. AMI를 생성하기 위해 종료
- 실행코드
  
    ### 기존코드 삭제
    
    ```bash
    $ cd /var/www/ #기존 코드가 배포돼 있는 경로로 이동
    
    $ ls
    파일1 파일2 파일3 ...등등
    
    $ rm -rf 파일* #위와 같이 기존 코드들 삭제
    ```
    
    ### Agent설치
    
    ```bash
    #설치 스크립트 내려받기
    $ wget https://aws-codedeploy-ap-northeast-2.s3.amazonaws.com/latest/install
    
    #내려받은 스크립트파일 실행할 수 있게 파일의 권한 변경한다.
    $ chmod +x ./install
    
    # ruby 설치
    $ apt-get install ruby
    
    #스크립트를 싱행하여 현재 EC2 인스턴스에 맞는 CodeDeploy Agent를 설치하고 실행한다.
    #서비스 등록 등의 작업을 위해 root 권한을 사용한다.
    #설치 시 자동으로 시작 서비스에 등록된다.
    $ sudo ./install auto
    
    #설치된 CodeDeploy Agent가 올바르게 실행됐는지 확인해본다.
    $ sudo service codedeploy-agent status
    
    #다음과 같이 특정 Process ID로 실행되고 있다는 메시지가 나타나면 설치 성공.
    The AWS CodeDeploy agent is running as PID 2955
    ```
    
    ### 설치가 완료되면 AMI를 생성하기 위해 인스턴스 종료
    
    ```bash
    $ sudo shutdown -h now
    ```
    

(아래 내용 중 일부(146~151p)는 시작템플릿과 오토스케일링에 대한 내용이 다뤄진다. 그런데 최종 CodeDeploy 배포그룹 생성시 오토스케일링 그룹단위로 배포를 할지 EC2단위로 할지 선택할 수 있다. 따라서 아래의 내용이 오토스케일링이 적용되지 않은 프로젝트의 경우 필요 없을 수 있다는 점을 유의하자)

## 시작템플릿 생성 (오토스케일링에 사용 될 시작템플릿)

---

146~148 page 

1. AWS 콘솔로 돌아와 인스턴스의 상태가 [stopped] 로 변하는 것을 확인 후,
해당 인스턴스에 대한 [이미지 생성] 
(해당 인스턴스는 /var/www/경로에, 앞서 진행한 작업으로 인해 텅 비어있고, agent가 설치 되어있는 상태)
사이드바 [AMI] 메뉴에서 생성이 완료될때 까지 기다린 후 [available] 상태가 되면
시작템플릿에서 사용하기 위해 [**AMI ID**]를 복사해 둔다.
2. 생성된 이미지로 '**시작템플릿**' 생성 (사이드바의 Launch template 메뉴에서)
    - (기존에 사용하던 시작템플릿이 있을 경우)기존에 사용되던 시작템플릿을 소스템플릿으로 사용하여 기존 정보 불러온 후 AMI ID만 위에서 생성한 [**AMI ID**]로 변경
    - [고급 세부정보] 입력항목 쪽에서 "IAM 인스턴스 프로파일" 부분에 위에서 생성한 **인스턴스 프로파일 ARN** 값 붙여넣기(즉, Git과 S3를 읽을 수 있는 정책(policy)을 갖는 역할(권한=role)을 부여)

## 오토스케일링 수정 (위에서 생성한 시작템플릿 적용)

---

150 page
◼︎ Auto scaling 수정

1. 사이드바 > Auto Scaling group으로 이동
2. 기존에 사용되던 오토스케일링 클릭 > 하단의 '세부정보' 탭에있는 [편집] 클릭
3. 기존에 참조하는 시작템플릿 대신 위에서 생성한 ("Agent가 설치된 AMI"로 만든) **시작템플릿** 으로 변경
4. 인스턴스 목표용량, 최소, 최대의 값을 조절 후 저장하여(교재에서는 모두 3으로 높였다)
"Agent가 설치된 인스턴스"를 띄운다
(생성된 인스턴스의 상태가 [InService] 상태로 되면 CodeDeploy로 배포 가능한 인스턴스 생성작업 완료)

<aside>
💡 여기까지 했다면, 인스턴스가 생성 및 실행됐어도, 앞서 AMI를 만들 때 인스턴스 내 코드를 삭제했기 때문에 이 3대의 서버들은 아무런 애플리케이션도 서비스하지 않고 있다. 실제로 ELB의 주소로 접속해보면 Phusion Passenger 서버 에러가 발생하는 것을 확인할 수 있다. 이제부터 CodeDeploy 애플리케이션을 생성해야 한다.

</aside>

## CodeDeploy Application 생성

---

138 page
◼︎ "CodeDeploy"를 위한 역할생성
IAM → 역할 생성 → 이 역할을 사용할 서비스 선택 = CodeDeploy → 기본적으로 **AWSCodeDeployRole** 정책이 추가되어있는 것을 확인 → 생성완료 ( 역할 이름 : **CodeDeploy-service-role** )

152 page
◼︎ CodeDeploy 생성

CodeDeploy 서비스 > 애플리케이션 > [애플리케이션 생성]
코드디플로이는 일반적인 EC2에 배포할 수도 있고, Lamda와 같은 서버리스 컴퓨팅서비스에도 배포할 수 있다. 현재 실습하는 내용은 EC2이기 때문에
컴퓨팅 플랫폼 선택란에서 'EC2/온프레미스' 선택하고 [생성] 완료.

<aside>
💡 단지 이것만으로 CodeDeploy 애플리케이션 생성은 끝이다.

다만, 생성이 완료된 애플리케이션 안에는 배포그룹이라는게 있는데,
하나의 CodeDeploy 애플리케이션은 여러 개의 배포그룹을 가질 수 있다.

같은 소스코드를 배포 하더라도, 
1. 목적지가 여러 환경으로 구분되게끔 배포해야 한다거나(예를들어 개발계, 운영계 구분하듯이),
2. 배포방법(예를들어 현재위치, 블루그린방식)을 상황에따라 다르게 사용해야 할 수도 있기 때문이다.

</aside>

154 page

◼︎ 배포 대상과 배포 방식을 설정하는 '배포그룹' 생성

1. [서비스 역할 선택] 선택항목 >  앞서 생성했던 "CodeDeploy를 위해 생성했던 역할(**CodeDeploy-service-role)**" 적용
2. [배포유형 선택] 현재위치 or 블루/그린
3. [환경구성 선택] 배포그룹 대상이 되는 인스턴스를 지정하는 화면. EC2 인스턴스를 개별로 직접 지정할 수도 있고, Aouto Scaling그룹을 지정하여 그룹 내 여러 인스턴스들을 한꺼번에 자동으로 등록할 수도 있다
이번 실습에서는 위의 내용들과 같이 오토스케일링을 적용한 케이스이기 때문에 'Auto scaling 그룹' 체크 후 기존에 사용하던 대상그룹을 선택하였다.
4. [배포구성(배포설정) 선택] "한번에 하나, 절반씩, 한꺼번에" 방식 중 하나 선택. (직접 배포 구성도 가능)
5. [로드밸런서] 로드밸런서 활성화 체크, Application Load Balancer 체크, 기존에 사용하던 대상그룹(타겟그룹) 지정

157 page
◼︎ 배포할 프로젝트 출처 설정

1. 위에서 생성된 배포그룹의 상세화면에서 [배포생성]을 클릭 
(결과적으로 CodeDeploy라는 애플리케이션 ⇒ 배포그룹 ⇒ 배포생성 순으로 진행되었다.
[배포그룹]이 목적지와 방식에 대한 설정이었다면 [배포생성]은 소스를 내려받을 수 있는 리포지토리에 대한 설정이라고 보면 된다)
2. [계정유형] 어디서 프로젝트를 불러온 것인지에 대한 선택 (S3 or Github)
3. [GitHub 토큰이름] github 계정의 별칭 (아무렇게나 해도 된다는 글이 있다)
4. [리포지토리 이름]
5. [커밋 ID] (깃에서 배포하고자 하는 대상 소스의 커밋 id 를 확인하여 입력)



# End.

### 끝으로,

169 page
CodeDeploy에서 제공하는 블루/그린은 굉장히 많은 일을 자동으로 해주지만 몇 가지 단점이 있다.
자동으로 Auto Scaling그룹을 만들어 주지만 그 이름이 배포ID를 기반으로 생성되기 때문에 (Auto Scaling그룹 이름이)매번 새로운 이름으로 생성됨.
또한 기존 블루그룹을 삭제하기 때문에 연동돼있는 서비스들도 끊어져 버림
따라서 Auto Scaling그룹의 이름을 구분자로 사용하는 'AWS의 다른 서비스'들(하나의 예로 CloudWatch) 쪽에서
지속적으로 사용할 수 있도록 설정을 새로 해줘야 함.(현시점에서 수정되었는지는 확인 필요)
이를 해결하기 위한 방법으로 다음과 같은 '좀더 수동적인' 방법이 있다

1. Auto Scaling 그룹을 두개 생성(블루, 그린)
2. 그린그룹에 블루그룹과 같은 시작 템플릿, 인스턴스 수 설정
3. 그린그룹에 CodeDeploy의 "현재 위치 배포"로 배포 진행
4. 로드밸런서 트래픽 포워딩
5. 블루그룹 인스턴스 수 0으로 축소

위의 실습내용에는 CodeDeploy로 배포하기 위해 만들어둔 인스턴스에 아무런 코드가 포함돼 있지 않다. 그럼 Auto scaling을 통해 자동으로 인스턴스가 시작될 때 과연 어떻게 될까? 
→ 다행히 Auto Scaling그룹과 CodeDeploy는 AWS의 서비스이기 때문에 별다른 설정을 하지 않아도 클라이언트의 요청을 받기 이전에 자동으로 코드 배포가 이뤄진다. 그 원리는 바로 Auto Scaling 그룹의 생명주기와 후크에 있다. CodeDeploy에서 애플리케이션의 배포 그룹을 생성할 때 Auto Scaling 그룹을 지정하는데, 이때 해당 Auto Scaling 그룹에 생명주기 후크가 하나 추가된다. 이 후크는 인스턴스가 시작될 때를 나타내는 [autoscaling:EC2_INSTANCE_LAUNCHING]이라는 생명주기에 CodeDeploy로 배포를 진행하게 된다. 인스턴스가 시작되고 InService 상태로 변경되기 이전에 이 후크가 실행되는데, 그때 CodeDeploy에서는 해당 배포그룹에서 맨 마지막으로 배포에 성공한 개정(rivision)을 이 인스턴스에 배포하게 된다. 배포에 실패하는 경우에는 생명주기에 따라서 시작되던 인스턴스가 종료되고 Auto Scaling그룹에서 잠시 후 새로운 인스턴스를 실행한다. 만약 이 자동배포를 잠시 끄고 싶다면 Auto Scaling 그룹의 [생명주기후크] 탭에서 해당 후크를 삭제하면 된다. 다시 추가하고 싶다면 CodeDeploy의 배포그룹의 편집화면에서 저장하기만 하면 된다.

그럼 내 생각에, 테라폼을 사용하여 오토스케일링 그룹을 생성 한 후 코드디플로이 현재위치 배포로 진행하면 안될까?

CodeDeploy 배포 관련 로그 경로

- CodeDeploy Agent 로그(Agent가 실행되면서 쌓는 로그) :
 /var/log/aws/codedeploy-agent/codedeploy-agent.log
- 배포 스크립트 로그 (우리가 만든 배포 스크립트가 실행되면서 출력한 로그) : 
/opt/codedeploy-agent/deployment-root/deployment-logs/codedeploy-agent-deployments.log

코드디플로이를 통한 무중단배포가 짧은시간내에 이루어지는데 세션클러스터가 정상적으로 작동할 수 있는지?
[https://blog.deliwind.com/posts/268](https://blog.deliwind.com/posts/268)

**[AWS] Auto-Scaling CodeDeploy Blue/Green 자동화 배포**

[https://devlog-wjdrbs96.tistory.com/305](https://devlog-wjdrbs96.tistory.com/305)

**AWS Code Deploy를 통한 배포 자동화**

[https://blog.dramancompany.com/2017/04/aws-code-deploy를-통한-배포-자동화/](https://blog.dramancompany.com/2017/04/aws-code-deploy%EB%A5%BC-%ED%86%B5%ED%95%9C-%EB%B0%B0%ED%8F%AC-%EC%9E%90%EB%8F%99%ED%99%94/)