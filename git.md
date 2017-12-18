#### 커밋 : 프로젝트의 스냅샷

> 브랜치를 많이 만들어도 메모리나 디스크 공간에 부담이 되지 않기 때문에, 여러분의 작업을 커다른 브랜치로 만들기 보다, 작은 단위로 잘게 나누는 것이 좋습니다.

```
git commit : 커밋 생성
git branch [branchName] : 브랜치 생성
git checkout [branchName] : 브랜치 이동
브랜치 이동 후
git merge [merge하려는 branchName]
```

#### 리베이스를 쓰면 저장소의 커밋 로그와 이력이 한결 깨끗해집니다.

> 리베이스를 하면 커밋들의 흐름을 보기 좋게 한 줄로 만들 수 있다는 장점이 있습니다. 

```
git checkout [branchName]
git rebase [가리키려는 branchName]
ex) git checkout [bugFix]; git rebase [master]
```

#### HEAD : 현재 체크아웃된 커밋

HEAD를 분리한다는 것은 HEAD를 브랜치 대신 커밋에 붙이는 것을 의미

```
git checkout [commitName]
```

```
상대참조
git checkout [name]^
git checkout [name]~[num]
git checkout [name]^[num] //num번째 부모
```

```
브랜치 강제로 옮기기
git branch -f [name] [name]~[num]
```

Git 작업 되돌리기 : 낮은 수준의 일(개별 파일이나 묶음을 스테이징 하는 것)과 높은 수준의 일(실제 변경이 복구되는 방법)

`git reset`은 브랜치로 하여금 예전의 커밋을 가리키도록 이동시키는 방식으로 변경 내용을 되돌립니다. 이런 관점에서 "히스토리를 고쳐쓴다" **단, 다른 사람이 작업하는 리모트 브랜치에는 쓸 수 없습니다.**

변경분을 되돌리고, 이 되돌린 내용을 다른 사람들과 *공유하기*위해서는, `git revert`를 써야합니다. (내용 이해가 좀 안된다 다시 보자.)

```
git reset [name]~[num] or ^
git revert [name]
```

#### cherry-pick : 현재 위치(`HEAD`) 아래에 있는 일련의 커밋들에대한 복사본을 만들겠다

```
git checkeout [name]
git cherry-pic [commitName] [commitName] ...
```

#### 인터렉티브 리베이스 : 원하는 커밋을 모르는 상황에서 cherry-pick 대용

```
git rebase -i [해당하는 name]
```

1. `git rebase -i` 명령으로 우리가 바꿀 커밋을 가장 최근 순서로 바꾸어 놓습니다
2. `commit --amend` 명령으로 커밋 내용을 정정합니다
3. 다시 `git rebase -i` 명령으로 이 전의 커밋 순서대로 되돌려 놓습니다

#### Commit 내용 정정

```
git commit --amend
```

#### Tag

```
git tag [tagName] [name]
```

#### Describe

커밋 트리에서 태그가 훌륭한 "닻"역할을 하기 때문에, git에는 여러분이 가장 가까운 "닻(태그)"에 비해 상대적으로 어디에 위치해있는지 *describe(묘사)*해주는 명령어가 있습니다. 이 명령어는 `git describe`

```
git describe [name]
```

`<ref>`에는 commit을 의미하는 그 어떤것이던 쓸 수 있습니다. 만약 ref를 특정 지어주지 않으면, git은 그냥 지금 체크아웃된곳을 사용합니다 (`HEAD`).

명령어의 출력은 다음과 같은 형태로 나타납니다:

`<tag>_<numCommits>_g<hash>`

`tag`는 가장 가까운 부모 태그를 나타냅니다. `numCommits`은 그 태그가 몇 커밋 멀리있는지를 나타냅니다. `<hash>`는 묘사하고있는 커밋의 해시를 나타냅니다.

### Remote

#### `origin/master` : 원격브랜치

원격 브랜치는 체크 아웃을 하게 되면 분리된 `HEAD` 모드로 가게되는 특별한 속성이 있다.

`o/master`가 원격 저장소가 갱신될때만 갱신된다.

#### `git fetch` : 원격 저장소에서 데이터를 가져오는 방법

- 원격 저장소에는 있지만 로컬에는 없는 커밋들을 다운로드 받습니다. 그리고...
- 우리의 원격 브랜치가 가리키는곳을 업데이트합니다 (예를들어, `o/master`)
- *로컬*에서 나타내는 원격 저장소의 상태를 *실제* 원격 저장소의 (지금)상태와 동기화합니다.

**실제로 로컬 파일들이나 브랜치를 변경하지는 않습니다**

#### `git pull` : fetch + merge

#### `git fakeTeamwork [num]` : 원격저장소에 페이크 커밋을 생성

#### git crash

```
git fetch
git rebase o/master
git push

Or

git pull --rebase
git push
```

큰 프로젝트를 개발할때 작업을 feature 브랜치(=토픽브랜치 / `master`브랜치가 아닌 작업을 위해 임시로 만든 브랜치를 말합니다)들에 하고 준비가 되면 그 작업을 통합

장점: rebase는 여러분의 커밋 트리를 깔끔하게 정리해서 보기가 좋습니다 모든게 한 줄에 있기때문이죠.

단점: rebase를 하게 되면 커밋 트리의 (보이는)히스토리를 수정합니다.

``` 
git checkout -b [name] o/master : 추적 
(= git branch -u o/master foo)
git pull
```



repository push된 내용 이전 commit으로 바꾸기

```shell
git reset --hard HEAD
git push origin +master
```



1. git@github.com:[your]/parent.git
2. git@github.com:[your]/child.git
3. working directory : ~/mywork

```shell
~/mywork $ git clone git@github.com:<your>/parent.git
~/mywork $ cd parent
~/mywork/parent $ git submodule add [git repository url] //submoule 생성
```

```shell
~/mywork/parent $ git status //상태조회
~/mywork/parent $ git cat .gitmodules //서브모듈 정보
~/mywork/parent $ git diff --cached child //커밋 해시 확인
(출력물)
diff --git a/child b/child
new file mode 160000
index 0000000..fcdb9d3
--- /dev/null
+++ b/Child_Test_Git
@@ -0,0 +1 @@
+Subproject commit fcdb9d3850d143bcddfe99be17916cad2e01e887 : 부모 repository에서 하위 repository [fcdb9d3850d143bcddfe99be17916cad2e01e887] 커밋을 바라보고 있다.
```

```
~/mywork/parent $ git commit -am "Commit Message"
```

