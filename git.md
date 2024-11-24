## 여러 원격 저장소에 연결

### 원격 저장소 목록 확인
```
git remote -v
```
### 원격 저장소 추가
```
git remote add [name] [url]
```
### 원격 저장소 삭제
```
git remote remove [name]
```
### 원격 저장소 URL 변경
```
git remote set-url [name] [newurl]
```

## 여러 원격 저장소에 병합

### 로컬에서 병합할 브랜치로 체크아웃
먼저 병합하고자 하는 브랜치로 체크아웃합니다. 예를 들어 feature 브랜치로 이동하려면:
```
git checkout feature
```
##3 팀의 원격 저장소에서 최신 변경 사항 가져오기
teamCheerping 원격 저장소의 main 브랜치를 가져옵니다. 이를 위해 원격 저장소의 브랜치를 로컬로 가져옵니다:
```
git fetch teamCheerping
```
### 병합 수행
이제 teamCheerping의 main 브랜치를 현재 체크아웃한 feature 브랜치에 병합합니다:
```
git merge teamCheerping/main
```
이 명령어를 실행하면 teamCheerping의 main 브랜치의 변경 사항이 현재 브랜치에 병합됩니다.

### 병합 충돌 해결 (필요한 경우)
병합 중에 충돌이 발생할 수 있습니다. 이 경우 Git이 충돌을 알려주고, 충돌이 발생한 파일을 수정해야 합니다. 수정이 완료되면:
```
git add [충돌 해결한 파일]
git commit -m "Merge teamCheerping/main into feature"
```
### 병합 결과 확인
병합이 완료되면 git status를 사용하여 상태를 확인합니다:
```
git status
```
### 원격 저장소에 푸시 (선택 사항)
병합이 완료된 후, 변경 사항을 원격 저장소에 푸시하려면:
```
git push origin feature
```
여기서 origin은 기본 원격 저장소의 이름이고, feature는 현재 브랜치의 이름입니다. 다른 브랜치로 푸시하고 싶다면 브랜치 이름을 변경하세요.
