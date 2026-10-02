# Lab 6

## 1. Why Version Control?

작업하다 보면 파일을 여러 번 수정한다. `최종`, `최종_수정`, `진짜최종`처럼 파일을 계속 복사하면 어느 파일이 최신인지 헷갈리고, 다른 사람의 수정 사항을 합치기도 어렵다. 버전 관리 시스템(Version Control System, VCS)은 변경 기록을 체계적으로 저장해 이런 문제를 줄여 준다.

Git은 파일을 매번 통째로 복사하는 방식보다 **프로젝트의 시점별 상태(snapshot)**를 저장하는 방식으로 변경 이력을 다룬다. 필요할 때 이전 상태와 현재 상태를 비교할 수 있다.

## 2. Types of Version Control Systems

| 종류 | 저장 방식 | 간단한 설명 |
| --- | --- | --- |
| 로컬 버전 관리 | 내 컴퓨터 | 한 컴퓨터 안에 버전 기록을 저장한다. |
| 중앙 집중식 | 중앙 서버 | 서버에 기록을 모으고 사용자가 서버와 주고받는다. |
| 분산식 | 각 사용자의 컴퓨터에도 저장소 전체 | 각 사용자가 프로젝트 이력을 가진다. Git은 분산 버전 관리 시스템이다. |

## 3. Three Areas in Git

Git에서 파일 변경은 보통 다음 순서로 기록한다.

```text
Working Directory       Staging Area          Git Repository
작업 폴더       --git add-->  기록할 변경 모음  --git commit-->  저장된 이력
```

| 영역 | 뜻 |
| --- | --- |
| Working Directory | 내가 파일을 만들고 수정하는 폴더 |
| Staging Area | 다음 커밋에 포함할 변경을 골라 모아 두는 곳 |
| Git Repository | 커밋한 이력이 저장되는 저장소 (`.git` 폴더에 관련 정보가 있음) |

파일 상태를 간단히 말하면 다음과 같다.

- **Modified**: 파일을 수정했지만 아직 스테이징하지 않은 상태
- **Staged**: 다음 커밋에 들어가도록 `git add`한 상태
- **Committed**: `git commit`으로 저장소 이력에 기록한 상태

`git add`는 “이 변경을 다음 기록에 넣겠다”는 뜻이고, `git commit`은 스테이징한 변경을 이력으로 저장하는 단계다. 파일을 수정했다고 자동으로 커밋되는 것은 아니다.

## 4. Installing and Configuring Git

먼저 설치 상태를 확인한다.

```bash
git --version
```

Windows에서는 Git Bash를 사용할 수 있다. Linux와 macOS는 설치 여부를 확인하고, 필요하면 사용하는 운영체제의 안내에 따라 설치한다.

커밋 작성자를 구분할 수 있도록 이름과 이메일을 설정한다. 처음에는 보통 현재 사용자의 모든 저장소에 적용되는 `--global` 설정을 사용한다.

```bash
git config --global user.name "내 이름"
git config --global user.email "내 이메일 주소"
git config --global init.defaultBranch main
```

설정 확인:

```bash
git config --list
git config --list --show-origin
```

Git 설정에는 세 단계가 있다.

| 단계 | 옵션 | 적용 범위 |
| --- | --- | --- |
| System | `--system` | 컴퓨터의 모든 사용자와 저장소 |
| Global | `--global` | 현재 사용자의 저장소 |
| Local | `--local` | 현재 저장소 하나 |

여러 단계에 같은 설정이 있으면 `system → global → local` 순서로 더 구체적인 값이 우선한다. 즉, 현재 저장소에 설정한 값은 global 값보다 우선할 수 있다.

## 5. Initializing a Repository

프로젝트 폴더로 이동한 다음 초기화한다.

```bash
cd my-project
git init
```

`git init`은 현재 폴더를 Git 저장소로 준비한다. 이후 상태를 확인할 때는 다음 명령을 사용한다.

```bash
git status
```

`git status`는 현재 브랜치와 수정·스테이징된 파일, 아직 추적하지 않는 파일 등을 알려 준다. 자주 실행해도 안전한 확인용 명령이다.

## 6. Staging and Committing Changes

### Staging One File

```bash
git add README.md
git status
```

새 파일이나 수정한 파일을 `git add`하면 그 시점의 변경이 스테이징된다. 파일을 더 수정했다면 다시 `git add`해야 새 수정 내용도 스테이징된다.

### Staging All Files

```bash
git add .
```

현재 폴더 아래의 변경을 모두 스테이징한다. 무엇이 포함되는지 확인한 뒤 사용하는 습관을 들이자.

### Creating a Commit

```bash
git commit -m "Add lecture notes"
```

`-m` 뒤에는 이번 변경을 설명하는 짧은 메시지를 쓴다. 예를 들면 `Fix typo in README`처럼 변경 목적을 알 수 있게 적는다.

커밋한 뒤 상태와 기록을 확인할 수 있다.

```bash
git status
git log
```

작업 폴더가 깨끗하면 `nothing to commit, working tree clean`과 비슷한 안내가 보인다.

### Basic Workflow

```bash
git status
git add 파일이름
git status
git commit -m "변경 내용 요약"
git status
```

## 7. Unstaging a File

새 파일을 실수로 스테이징했다면 저장소에는 두되, 스테이징 영역에서만 빼낼 수 있다.

```bash
git rm --cached 파일이름
git status
```

`--cached`를 사용하면 Git의 추적 대상에서 빼고 파일은 작업 폴더에 남긴다. 파일 자체를 지우는 명령과 다르므로 옵션을 빠뜨리지 않도록 주의한다.

## 8. Ignoring Files with `.gitignore`

```gitignore
# 모든 .a 파일은 무시
*.a

# 위에서 .a 파일을 무시하더라도 lib.a는 추적
!lib.a

# 현재 디렉터리의 TODO만 무시하고, 하위 디렉터리의 TODO는 무시하지 않음
/TODO

# 이름이 build인 모든 디렉터리의 파일을 무시
build/

# doc 디렉터리 바로 아래의 .txt 파일을 무시하지만,
# doc/server/arch.txt는 무시하지 않음
doc/*.txt

# doc 디렉터리와 그 하위 디렉터리 안의 모든 .pdf 파일을 무시
doc/**/*.pdf
```

| 패턴 | 의미 |
| --- | --- |
| `*.a` | `.a`로 끝나는 파일을 무시한다. |
| `!lib.a` | 앞서 무시한 파일 중 `lib.a`는 예외로 두어 추적할 수 있게 한다. `!`는 무시 규칙의 예외를 뜻한다. |
| `/TODO` | `.gitignore`가 있는 현재 디렉터리의 `TODO`만 무시한다. 하위 폴더의 `TODO`에는 적용되지 않는다. |
| `build/` | 어느 위치에 있든 이름이 `build`인 디렉터리와 그 안의 파일을 무시한다. |
| `doc/*.txt` | `doc` 바로 아래의 `.txt` 파일에 적용된다. 하위 디렉터리 안의 파일에는 적용되지 않는다. |
| `doc/**/*.pdf` | `doc` 아래 여러 단계의 하위 디렉터리를 포함해 `.pdf` 파일을 무시한다. |

## 9. Renaming a Branch

현재 브랜치 이름을 확인하고 `master`를 `main`으로 바꾸는 예시는 다음과 같다.

```bash
git branch
git branch -m master main
git branch
git status
```

별표(`*`)가 표시된 줄이 현재 브랜치다. 저장소를 처음 만들 때 기본 브랜치 이름을 `main`으로 설정하려면 앞에서 본 `git config --global init.defaultBranch main`을 쓸 수 있다.

## 10. Command Summary

| 명령어 | 하는 일 |
| --- | --- |
| `git --version` | 설치된 Git 버전 확인 |
| `git config --global ...` | 현재 사용자에 대한 기본 설정 |
| `git config --list` | 설정 확인 |
| `git init` | 현재 폴더를 저장소로 초기화 |
| `git status` | 저장소와 파일 상태 확인 |
| `git add 파일이름` | 파일 변경을 스테이징 |
| `git add .` | 현재 폴더 아래 변경을 모두 스테이징 |
| `git rm --cached 파일이름` | 파일은 두고 Git 추적/스테이징에서 제거 |
| `git commit -m "메시지"` | 스테이징한 변경을 이력으로 저장 |
| `git log` | 커밋 기록 확인 |
| `git branch` | 브랜치 확인 |
| `git branch -m 이전이름 새이름` | 현재 브랜치 이름 변경 |
