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
그럼 merge하고 `git pull origin main` 후에 파일에 내용을 추가하고 다시 push를 해보면 어떻게 될까?
![img](https://media.vlpt.us/images/langssi/post/0afb7f2c-bc99-4ac1-b4d5-a87ec7988b38/image.png)
merge 내역 + 새로운 커밋내용까지 남아있는 것을 볼 수 있따. 결론적으로는 커밋 기록이 더 많아졌다. 하지만 파일 변경 내역을 확인하면,
![img](https://media.vlpt.us/images/langssi/post/fc813d52-e4ff-4a36-81db-285c21a90640/image.png)
이전의 `feature/f1`의 변경 내역은 제외된 것을 볼 수 있다.
