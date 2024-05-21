#   Design
##  Project
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

![design](./resources/addIssue.png)

+  solve

![design](./resources/issueSolved.png)

+  description

![design](./resources/Issue_description_SSD.png)

![design](./resources/Issue_description.png)

+ statistics

![design](./resources/Issue_staticstics_SSD.png)

![design](./resources/Issue_statistics.png)

##  Comment
+   JPA의 도움으로, Project에 List<Comment> comments 필드를 넣고, comments.add()를 하면 자동으로 DB에 comment가 저장된다.

![design](./resources/Comment_SSD.png)

![design](./resources/Comment.png)
