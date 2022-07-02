## Git의 관리에서 특정 파일/폴더를 배제해야 할 경우

- 포함할 필요가 없을 때
    - 자동으로 생성 또는 다운로드되는 파일들(빌드 결과물, 라이브러리)
- 포함하지 말아야 할 때
    - 보안상 민감한 정보를 담은 파일

### >> .gitignore 파일을 사용해서 배제할 요소들을 지정할 수 있다.

## .gitignore 형식

```jsx
//모든 file.c
file.c

// 최상위 폴더의 file.c
/file.c

//모든 .c 확장자 파일
*.c

# .c 확장자지만 무시하지 않을 파일
!not_ignore_this.c

# logs란 이름의 파일 또는 폴더와 그 내용들
logs

# logs라 이름의 폴더와 그 내용들
logs/

# logs 폴더 바로 안의 debug.log와 .c 파일들
logs/debug.log
logs/*.c

# logs 폴더 바로 안, 또는 그 안의 다른 폴더(들) 안의 debug.log
logs/**/debug.log
```

## 프로젝트의 변경사항들을 타임캡슐(버전)에 담기

### git status

- 현재 버전(타임캡슐)의 상태를 확인한다.

![image](https://user-images.githubusercontent.com/81351313/175913212-58606434-82fc-43b0-b7e2-ba3dc855a086.png)

- No commit yet : 아직 커밋(=버전 = 타임캡슐)이 없다.
- untracked : 아직 깃의 관리에 들어간 적 없는 파일

## git add

- 파일을 담는다. (캡슐 안에 파일을 담겠다)

```jsx
git add 파일이름 // 파일 이름에 해당하는 파일을 캡슐에 담는다.
git add * // 모든 파일 담기
git add . // 모든 파일 담기
```

git add 후에 git status로 현재 상태를 확인하면

![image](https://user-images.githubusercontent.com/81351313/175913359-6e3db5e4-37f6-4b4e-8521-21018a9a4b14.png)

- No commits yet이 뜨지만 Changes to be commited 즉, 커밋 준비가 됐습니다. ( 캡슐을 땅에 묻을 준비가 됐습니다.)

## git commit

- 파일을 묻는다.(캡슐을 묻는다.)

```jsx
git commit
```

- vi 입력 모드로 진입합니다.
    - i : 입력 시작
    - ESEC : 입력 종료
    - :q : 저장 없이 종료(입력이 없을 때)
    - :q! : 저장 없이 종료(입력이 있을 때)
    - :wq : 저장하고 종료
- 여기서 커밋 메세지 내용을 입력 후 저장 종료를 하게되면 우리는 그 메세지 내용을 버전으로 하는 타임 캡슐을 깃에 묻게 된 것입니다.

- 커밋 메세지와 함께 작성하기

```jsx
git commit -m "first commit"
```

- 소스트리로 확인하기

```jsx
git log
```

- 새로 추가된 (untracked)파일이 없을 때 add와 커밋을 한꺼번에 할 수 있다.

```jsx
git commit -am "(메시지)"
```

맨 처음 first commit 한 두개의 파일 중 하나는 삭제하고 하나는 수정해보겠습니다. 거기에 새로운 파일도 하나 만들어보면 git status가 어떻게 보일까요?

![image](https://user-images.githubusercontent.com/81351313/175913435-59af5bb8-2aa9-46f2-b1ee-85211cbb23dd.png)

lions는 삭제됐고, tigers는 수정됐어, 하지만 untracked file인(내가 알지 못하는) leopards가 추가됐어 라고 알려주고 있습니다.

## 깃을 사용해 과거로 돌아가기

다음과 같은 커밋 내용이 쌓여있다고 생각해보겠습니다.

![image](https://user-images.githubusercontent.com/81351313/175913546-a42173da-00c4-4f8f-999c-2de3b7df34f3.png)

이 커밋이라는 것들 하나하낙 묻어놓은 타임캡슐(버전)이라고 볼 수 있습니다. 나중에 타임캡슐을 파낼 때 안에 뭐가 든 건지 미리 알 수 있게 캡슐마다 작업한 내용을 커밋 메세지로 남겨 둔 것입니다.

깃에서 프로젝트를 과거로 돌리는 방식은 두가지가 있습니다.

# Reset vs Revert

## Reset

현재 Replace Lions with Leopards라는 커밋을 Reset을 통해 돌아간다면 다음과 같이 일어나게된다.

![image](https://user-images.githubusercontent.com/81351313/175913600-b7ba2c2f-9d50-4b5f-8bb7-68e006487fb9.png)

즉, Replace Lions with Leopards 커밋 이후의 내용을 모두 reset 함으로써 그 전으로 돌아가게 된다.

하지만 git reset은 사용시 주의 사항이 있다.

내 로컬에선 FIRST COMMIT을 제외한 커밋들이 사라졌지만, 원격 저장소에는 내 로컬에 없는 4개의 커밋이 남아있기 때문이다. 따라서 이 상태로 원격 저장소에 push하게 되면 충돌이 발생한다. 커밋 히스토리가 불일치 하기 때문이다.

그럼 공유하는 브랜치에서 이전 커밋을 수정하고 싶을 때는 어떻게 해야할까? 이 때 사용하는 것이 git revert 이다.

## Revert

이전 커밋 내역을 그대로 남겨 둔 채 새로운 커밋을 만든다.

![image](https://user-images.githubusercontent.com/81351313/175913648-ccaf4696-c5ad-4fb8-8a5e-3b9803ede368.png)

Replace Lions with Leopards 커밋만 -해줌으로써 그 이후에 있었던 캠슐은 온전히 지킬 수 있다. 이러면 커밋 히스토리는 바뀌지 않기 때문에 충돌이 발생하지 않는다.
