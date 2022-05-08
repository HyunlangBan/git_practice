# git_practice

`git push`가 무서운 쫄보 깃린이의 깃연습 공간👩🏻‍💻

### Practice 1.
어떠한 branch에 push까지 했지만 다시 잘못된 것을 발견해서 다시 push해야 하는 상황.(요새 자주 겪는다..^^)
pr을 close하고 계속 새로운 브랜치를 따서 새로 push하자니 의구심이 들기 시작했다.
그래서 생각한 다른 방법을 시도해보았다.
1. 잘못 push한 PR은 close한다.
2. 해당 local branch에서 원하는 만큼 git reset으로 commit을 돌리고 다시 add, commit한다.
3. 원래 잘못 푸시했던 원격 브랜치로 force push를 한다.
이대로 해보니 원래의 잘못된 커밋 내역은 사라지고(closed PR에 존재) 새로 올라온 커밋내용이 PR로 올라온 것을 확인했다.

### Practice 2.
`feature/f1`이 마스터에 머지가 되지 않은 상태에서 이 변경 사항을 반영한 새로운 작업을 하고 싶다. 그럼 그냥 `feature/f1`에서 브랜치를 새로 따도 상관이 없을까? 한번 시험해보자.</br>
와우. 이전 브랜치의 머지되지 않은 커밋 내역이 딸려온 것을 확인할 수 있다.
![img](https://media.vlpt.us/images/langssi/post/574860f3-d85e-4242-9209-bcf20833ec85/image.png)
second commit이라는 커밋은 현재 브랜치에서 남긴 커밋내역이 아니다. `feature/f1` 브랜치에서 머지가 되어도 `feature/f1_2`에 남은  커밋내역은 그대로 남아있다. 또한 파일 변경 내역을 보아도 이전의 브랜치의 변경 내역까지 포함되어있다.<br>
그럼 merge하고 `git pull origin main` 후에 파일에 내용을 추가하고 다시 push를 해보면 어떻게 될까?(생각해보니 `origin/main` -> `local/main`으로 pull하고 `local/main` -> `local/feature1_2`를 merge 하는게 맞는 것 같다...)
![img](https://media.vlpt.us/images/langssi/post/0afb7f2c-bc99-4ac1-b4d5-a87ec7988b38/image.png)
merge 내역 + 새로운 커밋내용까지 남아있는 것을 볼 수 있다. 결론적으로는 커밋 기록이 더 많아졌다. 하지만 파일 변경 내역을 확인하면,
![img](https://media.vlpt.us/images/langssi/post/fc813d52-e4ff-4a36-81db-285c21a90640/image.png)
이전의 `feature/f1`의 변경 내역은 제외된 것을 볼 수 있다.<br>
커밋내역을 깔끔하게 하고 싶다면 [sqush](https://meetup.toast.com/posts/39) + force push 를 사용해서 정리해줄 수 있다. 리모트 메인에서 README를 수정하는 동시에 로컬 브랜치에서도 파일을 push했더니 커밋이 완전 꼬이고 꼬여버렸다;; 이것의 해결은 나중으로 ~~~ 😂
### -> update
업무하면서 경험한 결과 practice2와 같은 경우 즉, 그냥 머지되지 않은 작업1 위에 또 다른 작업을 해야할 경우 작업1 브랜치에서 새로운 브랜치를 생성해도 상관이 없었다. 다만, master에 작업1 브랜치를 머지 한 후 작업2 브랜치를 머지해야지 작업 1의 내용이 딸려오지 않았다.

### Practice 3 - 일부 커밋 먼저 머지하는 방법?
- development 브랜치에 이미 merge된 commit 중(일반 commit + merge commit) 일부분만 main branch로 먼저 가져오면 어떻게 될까?
  - developmet -> main merge시, conflict를 해결해주어야 한다.(미리 main에 반영한 내용이 중복되어서) 
- 그냥 이미 devel에 머지 되었더라도 해당 feature 브랜치를 main으로 pr, merge하면 나중에 devel -> main 머지시에는 머지 커밋만 넘어가게 되므로 내가 원하는 결과가 나옴 ✅ 

### Practice 4 - Hotfix
<img width="556" alt="image" src="https://user-images.githubusercontent.com/45524783/167299997-9be0caeb-feab-4668-8ec5-1bacab59857c.png">      

- hotfix란 빠르게 해결해야 하는 버그나 바로 적용해야하는 작업이 있을때 feature branch -> development -> master 순으로 하는게 아니라 작업 내용을 바로 master, development에 반영하는 것을 의미한다.      
- 처음에 생각해봤을때는 devel에 바로 merge(pr X) -> devel에서 main으로 pr, merge 해서 변경 사항을 적용해주면 되겠거니했는데 아니었다.(이미 pr에 길들어진 몸이라 pr빼고 생각이 안되었다...)        
- HOW TO     
   - 먼저 **main**에서 hotfix 브랜치를 따온다.
   - 작업을 진행 한 뒤 commit을 하고 여기서 push가 아니라(push는 한 브랜치에만 할 수 있으므로) 위의 그림처럼 바로 main, development에 머지를 해주어야 한다.
   - `git checkout main` -> `git merge hotfix_branch` -> `git push origin main`
   - `git checkout development` -> `git merge hotfix_branch` -> `git push origin development`
   - 마지막으로 hotfix 브랜치를 삭제해준다. `git branch -D hotfix_branch`
