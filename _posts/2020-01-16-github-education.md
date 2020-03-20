---
layout: post
title: NHN Basecamp Github 교육
author: YunSeoWon
date: '2020-01-16 19:30:00'
category: etc
summary: Github 교육을 들었습니다.
thumbnail: github.png
---



# NHN Basecamp Github 교육

> 교육 내용 출처: NHN 기술교육 - Git 이론과 Gituhb 실습 (신승엽 책임)



NHN 신입 첫 교육으로 GitHub 기초 교육을 들었다. 11시부터 16시30분까지 진행되는 아주 긴 교육이지만, 곧 있을 팀 프로젝트를 하기위해서는 반드시 필요하고 중요한 교육이라고 생각된다. 학생 때 부터 GitHub를 종종 이용해보았지만, 잘 사용해보지는 않았던 것으로 기억이 난다. 오히려 충돌이 나서 그 것을 해결하다가 엇나가기도 하는 등 망한 적도 많았다.

이 긴 시간동안 GitHub의 역사부터 시작해서 add, commit, push, fetch, pull, merge, collision 등 여러 가지를 배우고 개인 또는 조별로 실습을 하면서 여러 상황을 직면해보는 연습도 하였다. 실습은 SourceTree로 하였지만 [git-fork](https://git-fork.com/)라는 툴도 좋다는 말씀을 하셨다.

  

## GitHub

먼저 GitHub는 대표적인 분산 버전 관리 시스템으로써, 원격 저장소와 로컬 저장소로 분리하여 버전을 관리한다. 보통 로컬 저장소에서 작업을 한 후 작업에 이상이 없으면 원격 저장소로 소스코드를 올린다. 로컬 저장소는 여러 개가 있을 수 있다.

  

### 버전 관리

Git은 버전 관리를 할 때 스냅샷으로 관리한다. 버전을 업데이트 할 때, 파일이 달라지는 부분만 따로 저장하고, 파일이 달라지지 않은 부분은 이전 버전의 링크로 저장한다. 이렇게 저장하기 때문에 좀 더 가볍게 버전을 관리할 수 있다.

  

### 파일의 상태

Git에서 파일을 관리할 때 파일의 상태는 3가지가 있다.

* Commiitted
  * 데이터가 로컬 데이터베이스에 안전하게 저장됨
  * commit 명령어를 치는 순간 Git이 로컬 저장소에서의 버전을 관리하기 위해  commit된 파일을 데이터베이스에 저장하게 된다.
* Modified
  
  * 수정한 파일을 아직 로컬 데이터베이스에 커밋하지 않음
* Staged
  * 수정한 파일을 곳 커밋할 것이라고 표시
  * add 명령어를 사용하면 변경된 파일이 Modified에서 Staged로 변경된다.

  

  

### Tracked, Untracked

워킹 디렉토리의 파일은  Tracked와  Untracked로 나뉜다.

  

**Tracked**

* 이미 스냅샷에 포함된 파일 [버전관리가 되고 있는 파일]
  * Unmodified (파일이 변경되지 않은 상태)
  * Modified (파일이 변경되어있지만 add를 하지 않은 상태)
  * Staged (파일이 변경되었고 add를 한 상태)

**Untracked**

* 스냅샷에 포함되지 않은 파일 [버전관리가 되고 있지 않은 파일]
  * 한 번도 add가 되지 않았거나
  * .gitignore로 필터링 되거나

  

**.gitignore**: 버전관리를 강제로 하지 않겠다고 선언하는 설정파일

[https://www.gitignore.io/](https://www.gitignore.io/) 에서 자동으로 만들 수도 있다.

  

  



## 버전관리

### 브랜치

브랜치란, 가상의 작업공간으로 하나의 독립적인 작업을 위한 공간이라고 생각하면 된다. 브랜치와 브랜치 끼리 병합(merge) 또한 가능하다.

Git이 브랜치를 어떻게 다루는지 알려면 Git이 데이터를 저장하는 방법을 알아야 한다.

  

#### Git이 데이터를 저장하는 방법

 https://yunseowon.github.io/etc/2020/01/16/github-education/200116-git/commit-and-tree.png

먼저 다음 명령어를 입력했다고 하자.

```shell
$ git add README test.rb LICENCE
$ git commit -m "The initial commit of my project"
```

add 명령어를 수행하면 각 3개의 파일은 staged 상태로 변하며, Git 저장소에 파일을 저장하고(blob) staging area에 해당 파일의 check-sum을 저장한다. [5b1d3, 911e7, cba0a]

그러고 난 후 commit 명령어를 수행하면 먼저 루트 디렉토리와 각 하위 디렉토리의 트리 개체를 check-sum과 함께 저장소에 저장된다. [92ec2] 그 다음에 커밋 객체를 만들고 메타데이터와 루트 디렉토리 트리 개체를 가리키는 포인터 정보를 커밋 객체에 넣어 저장한다.[98ca9]

{:class="img-fluid"}![commit-and-tree](/assets/img/posts/200116-git/commit-and-tree.png)





그리고 다시 파일을 수정하고 커밋하면 이전 커밋이 무엇인지도 저장한다.

![](/assets/img/posts/200116-git/commits-and-parents.png){:class="img-fluid"}

Git이 이러한 방식으로 데이터를 저장하기 때문에 커밋 상태를 이전 커밋 상태로 되돌리거나 복구할 수 있다.



**그리고 Git의 브랜치는 커밋을 가리키는 포인터이다.** 기본적으로 Git은 master 브랜치를 만든다. 

![](/assets/img/posts/200116-git/branch1.png){:class="img-fluid"}



![](/assets/img/posts/200116-git/branch2.png){:class="img-fluid"}

  

  

  



### Merge

한 브랜치의 내용을 다른 브랜치에 합치는 것을 merge라고 한다.

예를 들어, branch의 내용을 master에 합친다고 한다면 다음과 같다.(branch 브랜치를 master 브랜치로 머지)

  

#### Fast-forward

병합시킬 브랜치(branch)의 부모 커밋(98cc9c)이 병합할 브랜치(master)의 커밋(98cc9c)일 경우, 병합할 브랜치를 병합시킬 브랜치의 커밋으로 가리키기만 하면 merge가 완료된다. 이를 fast-forward라고 한다.

이는 가리키는 것을 바꾸기만 하면 되기 때문에 매우 빠르게 merge가 된다.

  

**merge 전**

![](/assets/img/posts/200116-git/pre-merge.png){:class="img-fluid"}

  

**merge 후**

![](/assets/img/posts/200116-git/post-merge.png){:class="img-fluid"}

  

  

  

## Git-flow

브랜치 관리 모델 중 하나로, Vincent Driessen이 주장한다. 이 모델의 요소는 다음과 같이 구성된다.

  

**메인 브랜치**

* master
  
  * 배포된 소스가 있다.
* develop
  * 다음 배포를 위한 소스가 있다.
  * 개발이 완료됨면 일련의 과정을 통해 master로 머지한다.

  

**서포팅 브랜치**

master와 develop 외에 팀 멤버들이 병렬로 일할 수 있도록 도와주는 브랜치 (working branch)이다. 이 브랜치는 작업이 완료되면 삭제한다.

* feature

  * 생성: develop
  * 머지: develop
  * 명명: feature/{name}
  * 역할: 특정 기능 하나에 대하여 develop으로부터 생성하여 개발하며, 개발이 완료되면 다시 develop 브랜치로 머지한다.

* release

  * 생성: develop
  * 머지: develop, master
  * 명명: release/{name}
  * 역할
    * 배포를 위한 준비를 할 수 있는 브랜치이다.
    * 여기서는 각종 메타 데이터를 변경하거나 작은 버그를 수정한다.
    * 배포 준비가 완료되면 release 브랜치를 master와 develop에 각각 머지한다.
    * master에는 버전 태그를 붙인다.

* hotfix

  * 생성: master
  * 머지: develop, master
  * 명명: hotfix/{name}
  * 역할
    * 배포된 버전에 긴급한 변경 사항이 있을 때 활용
    * 긴급한 이슈가 해결되면 다시 master와 develop에 머지한다.

    

  ![](/assets/img/posts/200116-git/gitflow.png){:class="img-fluid"}

  

  

Git-flow 전략을 이용했을 때, 이득이라고 생각되는 부분은 먼저, 어떤 버전에 대한 계획을 짤 때 Task를 설계하고 최대한 독립적인 태스크를 묶어서 개발자들에게 역할을 분담한 후 개발을 시작할 것인데, Git-flow 전략을 따르면 각각 업무를 완료하면 develop 브랜치에 머지하여 개발 태스크를 관리하고, 모든 태스크가 완료되었을 때, 최종 테스트를 거친 후 master 브랜치에 머지하여 배포를 하게 된다.

따라서, 계획에 맞게 버전과 태스크를 관리할 수 있다는 장점이 있다고 생각한다.

  

  

   

## 원격 저장소

로컬 저장소가 아닌 웹 상에서 관리되는 저장소이다.  

   

### Fetch

원격 저장소의 변경 사항을 내려받는 것을 fetch라고 한다. 하지만, 변경 사항만을 내려받았을 뿐 아직 로컬 브랜치에 반영이 되지 않은 상태이다. 따라서 fetch를 수행하면 수동으로 원격 브랜치를 로컬 브랜치로 머지해야 한다.  

  

### Pull

pull은 fetch + merge이다. 따라서 원격 저장소의 변경 사항을 로컬 저장소로 반영한다.

  

### Push

push는 로컬 저장소의 변경 사항을 원격 저장소로 올리는 것을 말한다.

  

### Pull Request

여러 명의 개발자들이 하나의 원격 저장소를 이용하여 협업을 할 때, 개발자들이 따로 develop 브랜치에서 feature 브랜치를 파서 작업을 하게 된다. 작업을 완료하면 원격 저장소에 push를 한다. 그러면 원격 저장소에는 feature 브랜치가 생성되고 그에 따른 변경사항이 저장된다.

하지만, 이 변경사항은 develop에 반영이 되어야 하는데 feature 브랜치에서 push한다고 해서 develop에 반영이 되지는 않는다. 즉, 원격 저장소에 올려져 있는 feature 브랜치를 develop 브랜치에 merge해야 하는데, 이를 요청하는 과정을 **pull request**라고 한다.

![](/assets/img/posts/200116-git/pre-pr.png){:class="img-fluid"}

  

하지만, 이 때 1번 개발자가 개발을 완료했다고 해서 마음대로 develop에 머지하면 문제가 생길 수도 있다. 이 개발 코드가 맞는지 여러 개발자가 따져봐야 하는데, pull request로 "머지를 하려고 하는데 제 코드를 봐주세요"라고 다른 개발자들에게 요청하는 것이다. 그러면 다른 개발자들은 1번의 코드를 보고 리뷰를 하고, 수정사항이 필요하다. 또는 merge해도 괜찮다. 등 여러 의견을 제시할 수 있다. 여러 의견을 조합한 후 모두 다 괜찮다고 생각되면 PR을 close할 수 있다. close가 되는 순간 merge가 된다. 그리고 git-flow의 전략에 의해 feature/1을 삭제한다.(이건 수동으로..)

![](/assets/img/posts/200116-git/post-pr.png){:class="img-fluid"}

  

  

### 충돌

위의 그림처럼 1번 개발자가 PR을 통해서 merge를 했고, 3번 개발자가 개발을 완료해서 원격 저장소에 올린 상태라고 가정하자. 다행히도, ccab와 ccaa 커밋의 코드들이 서로 독립적으로 변경되었다면 (서로 다른 파일이 변경되었다면), 3번 개발자가 PR을 통해 merge가 되어도 아무런 문제가 없다. (실제로 merge가 된다.) 하지만, 서로 공통된 부분을 다르게 수정하였다면, merge가 되기는 하지만 충돌이 났다고 메세지가 뜬다.

![](/assets/img/posts/200116-git/conflict.png){:class="img-fluid"}

  

그리고 충돌난 파일을 살펴보면 다음과 같이 충돌이 어디서 발생했는지가 표시된다.

![](/assets/img/posts/200116-git/conflictfile.png){:class="img-fluid"}

  

* HEAD: 내가 수정한 내용
* develop(머지한 브랜치): 머지한 브랜치에서 수정한 내용

  

충돌 표시를 지우고 적절히 두 변경사항을 반영하여 수정한다. **(이 때, 직접 소통하면서 변경하는 것이 가장 중요하다..!)** 수정한 후에 commit하고 push를 하면 PR에 변경된 사항이 반영되어 merge를 할 수 있다.

  

  

## 후기

대학교 프로젝트에서 git을 사용해본 적이 몇 번 있지만, Git-Flow 전략과 PR을 잘 사용해보지 못했던 것 같다. 단순히 원격 저장소에 저장하는 용도로 사용을 하였고, 가끔씩 브랜치를 나눠 개발을 수행하기도 했지만, 충돌이 난 이후에 해결을 하지 못하여 다른 팀원들에게 별도로 변경된 파일을 전송하여 개발을 하는 등 git을 제대로 사용해본 적이 많이 없던 것 같다.

이번 교육을 통해 git-flow 전략을 배우며 실습을 하였고, PR이라는 좋은 기능을 배운 것 같다. 이번 9주 동안 팀원들과 프로젝트를 하는데, 현재 기능을 명세하고 역할 분담을 정하는 것 까지 한 상태이다. 그리고 곧 있으면 개발을 할 것 같은데, Git-flow 전략을 잘 이용하여 버전을 관리하는 것을 연습함과 동시에 PR을 통해 코드리뷰를 하여 다른 팀원들의 코드를 검토하는 것 까지 연습을 하면 현업에 굉장히 도움이 될 것 같다는 생각을 하였다.

하지만 무엇보다, Git-Flow 전략을 잘 이용하기 위해서는, 기능 분할을 명확히 하여 코드 개발 및 수정 영역이 겹치지 않도록 하는 것이 중요하다는 생각을 하였다.

  

아무튼, 이번 프로젝트부터 수습기간이 끝나고 현업에 뛰어들 예정인데, Git을 제대로 이용하지 못하면 팀원들에게 굉장한 민폐를 끼치게 되므로 Git을 제대로 사용해보자.. ~~(특히 실수하더라도 수습기간에 해야..)~~

