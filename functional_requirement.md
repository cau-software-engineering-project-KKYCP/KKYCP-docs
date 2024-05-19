#   Functional Requirement

## Use case

1. 회원가입과 로그인
2. 관리자가 프로젝트를 개설
3. 관리자가 유저를 프로젝트에 추가
4. 프로젝트의 유저가 이슈를 조회
5. 프로젝트의 유저가 이슈를 검색
6. 프로젝트의 유저가 전체 발생한 이슈의 통계를 조회
7. 프로젝트의 유저가 이슈의 상세 정보를 확인

  \- 이슈의 상태와 코멘트를 확인 가능

7. 프로젝트의 유저가 이슈에 코멘트를 달고, 자신이 단 코멘트를 관리함
8. Reporter가 이슈를 등록
9. Triager가 이슈를 특정 유저에게 할당  \- 할당에 추천시스템을 활용할 수 있음

10. Assignee가 이슈를 해결함
11. Tester가 Assginee가 이슈를 제대로 해결했는지 확인함
12. Verifier가 resolved된 이슈를 최종 확인 후 close 함



a. 권한이 있는 User가 이슈의 상태를 변경#

##  회원가입과 로그인
+   Primary Actor: User

+   Postcondition: 회원가입이 완료되면 유저는 시스템에 등록되고 로그인할 수 있음.

+   Basic Flow:

        1.   User는 회원가입/로그인 페이지에 접속한다.

        2.   User는 회원가입을 선택하고, 필요한 정보(이름, 이메일 등)를 입력한다.

        3.   시스템은 입력된 정보를 확인하고, 유저를 시스템에 등록한다.

        4.   시스템은 유저를 로그인시킨다.
+   Alternative Flow:

        3a. 입력된 정보가 유효하지 않은 경우: 시스템은 에러 메시지를 보여주고, 회원가입을 중단한다.

##  관리자가 프로젝트를 개설

+   Primary Actor: Admin

+   Basic Flow:

        1. Admin이 프로젝트 개설 버튼을 누름
    
        2. Admin이 프로젝트 이름을 작성
    
        3. 시스템은 프로젝트를 개설하고, 프로젝트에 할당된 ID를 Admin에게 반환

+   Alternative Flow:

        2a. Admin 잘못된 프로젝트 이름을 입력한 경우, 유저에게 알려 올바르게 프로젝트 이름을 입력하게 함.

+   ETC: Admin에게 반환된 프로젝트 ID를 이용해 다른 사용자가 프로젝트에 참여 가능

## 관리자가 유저를 프로젝트에 추가

+   Primary Actor: Admin

+   Postcondition: 유저가 프로젝트에 추가된다.

+   Basic flow

        1. Admin이 관리자 페이지에서 "프로젝트에 유저 추가" 페이지로 들어간다.

        2. Admin이 유저 추가 페이지에서 프로젝트 id와 해당 프로젝트에 추가할 유저의 username을 기입후, "프로젝트에 유저 추가" 버튼을 누른다.

        3. 시스템은 해당 유저를 프로젝트에 등록한 후, 성공 응답을 Admin에게 보내준다.

+    Alternate flow

        3a. 해당 프로젝트나 유저를 시스템 상에서 찾을 수 없을 경우, 에러 응답을 Admin에게 보내준다.

## 이슈 조회

+   Primary Actor: User

+   Precondition: user가 해당 프로젝트에 참가해야 함

+   Basic flow

        1. User가 프로젝트 홈페이지에 접근한다.
    
        2. <<extend>> 검색 조건 설정
    
        3. 시스템은 User에게 최근 n개의 프로젝트 정보를 알려준다.

##  이슈 검색

+   Primary Actor: User

+   Precondition: user가 해당 프로젝트에 참가해야 함

+   Basic flow

        1. User가 필터 대상을 선택한다.
    
        2. User가 검색 키워드를 작성한다.
    
        3. User가 검색 버튼을 누른다.
    
        4. 시스템은 검색 결과를 유저에게 제공한다.

+   Alternative flow

        4a. 검색 결과가 없다면 검색 결과가 없다고 알린다.

+   필터 대상
    + Assignee
    + Reporter
    + Priority
    + Status
    + Type
+   검색 키워드 대상
    + Title

## 프로젝트의 유저가 전체 발생한 이슈의 통계를 조회

+   Primary Actor: Project's User

+   Precondition: user가 로그인 되어있어야 함, user가 해당 프로젝트에 참가해 있어야 함

+   Basic flow

        1. 유저가 통계 버튼을 클릭

        2. 시스템은 일일 발생 레포트 통계를 기본으로 보여줌

+   Alternate flow

        2a. 유저가 일일/월별/연별/상태별/중요도별 통계 중 어떤 통계를 보여줄 지를 선택하면, 시스템은 해당 통계를 유저에게 보여준다.

##  프로젝트의 유저가 이슈의 상세 정보를 확인

+   Primary Actor: Project’s User

+   Precondition: user가 시스템에 접속한 상태(로그인) 및 이슈가 등록된 상태, user가 해당 프로젝트에 참가해야 함

+   Postcondition: 이슈의 상세정보 UI가 제공됨

+   Basic flow

        1. Project의 User(PL1, Reporter, Fixer, etc)가 Issue list에서 issue를 선택한다.
    
        2. 선택된 issue의 상세정보(Title, Description, Priority, Type)를 제공한다. 

+   Alternative flow

        2a. 선택된 이슈의 상세정보를 불러오는데 실패한다면, 유저에게 알린다.

##  유저가 이슈에 코멘트를 달고, 자신이 단 코멘트를 관리함

+   Primary Actor: User

+   Precondition: 유저는 시스템에 로그인되어 있어야 함, 해당 이슈가 존재해야 함, user가 해당 프로젝트에 참가해야 함

+   Postcondition: 이슈에 코멘트가 추가됨

+   Basic Flow:

        1.    유저가 시스템에 로그인
    
        2.    유저가 이슈를 검색
    
        3.    이슈의 상세 페이지에 코멘트를 입력하고 제출 버튼 누름
    
        4.    시스템이 코멘트를 DB에 추가하고 이슈와 같이 표시함

+   Alternative Flow:

        3a. 유저가 코멘트를 입력하지 않고 "제출" 버튼을 클릭하는 경우: 시스템은 코멘트 입력을 요구하는 오류 메시지 표시
        
        4a. 유저가 코멘트를 수정, 삭제하는 경우:
    시스템은 수정 또는 삭제된 코멘트를 처리하고, 이슈 페이지에 코멘트를 업데이트

##  Reporter가 이슈등록

+   Primary Actor: Reporter

+   Precondition : User가 시스템에 접속한 상태(로그인), user가 해당 프로젝트에 참가해야 함

+   Postcondition: Report된 이슈가 시스템 상에 영구히 저장된다.

+   Basic flow

        1. Reporter가 인터페이스를 통해 시스템에서 “새 Issue 등록” 을 시도한다.
    
        2. Reporter가 Issue를 작성한다.
        -   Title
            -   Describtion
        -   Priority
            -   Type
    
        3. 시스템에 작성된 이슈를 저장한다.

+   Alternative flow

        3a. 만약 시스템이 이슈 저장을 실패한다면 유저에게 알린다.

##  Triager가 이슈를 특정 유저에게 할당

+   Primary Actor : User(Triager)

+   Precondition : User가 시스템에 접속한 상태, 시스템에서 검색기능 사용가능, Triagger Use인 경우만 해당함

+   Post condition : 할당된 이슈를 할당 받은 User가 조회할 수 있다.

+   Basic Flow

        1. PA(Primary Actor)가 인터페이스를 통해 할당되지 않은 이슈를 검색한다.
    
        2. PA가 할당되지 않은 이슈에 대한 정보를 확인한다.
    
              - 이슈의 상태로 필터링

        3. PA가 이슈를 유저에게 할당한다.

         - 추천 기능 활용 가능

+   Alternative flow

        2a. 할당되지 않은 이슈가 없다면 할당되지 않은 이슈가 없다고 알린다.
        
        3a. 이슈를 할당하는 것을 실패했다고 유저에게 알린다.

## Assignee가 이슈를 해결함

+ Primary Actor : User

+ Precondition : User가 시스템에 접속한 상태, assigned된 상태의 issue를 불러올 수 있는 기능이 있음.

+ Postcondition : 할당된 이슈가 fixed상태로 바뀐다.

+ Basic Flow

      1. User가 인터페이스를 통해 자신에게 할당된 이슈를 조회한다.

      2. User가 할당된 이슈를 해결 후, 해결했다고 시스템에 알린다.

      3. 시스템은 해당 이슈를 fixed 상태로 변경시키로 해당 이슈의 fixer를 User로 갱신한다.

          -  (실제로 코드를 고치는 행동은 본 프로젝트에 적용되지 않으므로 생략되었음)

+ Alternative flow

      1a. 할당된 이슈가 없는 경우 할당된 이슈가 없음을 알린다.

      3a. User에게 할당 된 이슈가 아닐 경우, 자신에게 할당된 이슈가 아니라 고칠 수 없음을 알린다.


##  권한 있는 유저의 이슈 상태 변경

+   Primary Actor: user

+   Precondition: user는 상태를 수정할 권리를 가지고 있다. 프로젝트 내의 Issue가 최소 1개 이상 생성 되어있다.
    
+   Postcondition: 이슈의 상태가 변경되었다.

+   Basic Flow:

        1.  User가 issue 상태를 선택한다.
    
        2.  Apply 버튼을 누른다.
    
        3.  System은 User가 권한을 가지고 있는지 확인한다.
    
        4.  Issue의 상태가 변경되어 표시된다.

+   Alternative Flow

 	    4a. user의 권한이 없다고 표시한다.

+   etc

    Status는 new/assigned/resolved/closed/reopened 중 1


## Tester(reporter)는 Fixer(Assginee)가 이슈를 제대로 해결했는지 확인

+   Primary Actor: Tester(=issue Reporter)

+   Precondition: 로그인 상태 / Reporter가 report한 issue가 등록되어 있음 / 담당자(assignee = fixer)가 문제를 해결하여, 해당 issue에 코멘트가 추가되고, issue의 상태가 fixed인 상태
    
+   Postcondition: 이슈의 상태가 resolved.

+   Basic Flow:

        1.  Tester가 Issue Search 기능을 활용하여, reporter가 자기자신인 이슈들을 확인
    
        2.  검색 결과 issue들 중 상태가 fixed인 이슈의 코멘트 확인 및 문제가 해결되었는지 테스트
    
        3.  issue의 상태를 resolved로 변경.

+   Alternative Flow

 	    3a. 문제가 해결되지 않은 경우, 코멘트를 추가 후 issue 상태를 다시 assigned로 변경.


## Verifier가 resolved된 이슈를 최종 확인 후 close 함

+  Primary Actor: Verifier(Verified User)

+  Precondition: 이슈가 최소 1개 이상 만들어져 있다. / 현재 이슈의 상태가 resolved 이다.

+  Postcondition: 이슈의 상태가 closed이다.

+  Basic Flow:

       1.   Verifer는 Issue Id를 통해 project에 속해있는 Issue를 선택한다.
   
       2.   Issue의 상태가 resolved 인지 확인한다.
   
       3.   resolved인 경우 Veifier는 Issue의 상태를 closed로 변경한다.

+  Alterantive Flow

        3a.  그렇지 않으면, closed로 변경할 수 없다.
