#   Design
## Privilege

+  Admin

![design](./resources/AdminGrantsPrivilege.jpg)

+ change_status

![design](./resources/PrivilegedUserchangesState.jpg)

##  Project

+  register_login

![design](./resources/register_login.jpg)

+   create

![design](./resources/create_project.jpg)


+ AddUser

![design](./resources/addUserToProject.png)


+ Find

![design](./resources/find_issue.png)

**Usecase 4번은 Usecase 5번의 Find 기능에서 Filter, Key를 전체값을 검색한다는 방식으로 extend된 기능입니다.**


##  Issue
+   create

![design](./resources/report_issue.jpg)

+  report

![design](./resources/addIssue.jpg)

+  solve

![design](./resources/issueSolved.jpg)

+  description

![design](./resources/Issue_Description(SSD).png)

![design](./resources/Issue_Description.png)

+ statistics

![design](./resources/Report_Statistics(SSD).png)

![design](./resources/Report_Statistics.png)

+ allocate_user_to_issue,_user_recommendation

![design](./resources/TriagerAllocatesUser_Recommendation.jpg)

##  Comment
+   JPA의 도움으로, Project에 List<Comment> comments 필드를 넣고, comments.add()를 하면 자동으로 DB에 comment가 저장된다.

![design](./resources/Comment_Manage(SSD).png)

![design](./resources/Comment_Manage.png)

## User

+  list

![design](./resources/userList.jpg)

+  search

![design](./resources/userSearch.jpg)
