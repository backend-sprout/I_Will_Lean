# Oh My Zsh 설정하기 
## 환경 정보 
* 기종 : MacBook Pro (13-inch, 2017, Two Thunderbolt 3 ports)
* OS : macOS BigSur 11.4

## 방법
### Homebrew 다운 
[Homebrew](https://brew.sh/index_ko)에 접속해서 `Homebrew`를 다운 받는다.   
   
```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Zsh Update 
```sh
zsh --version
zsh 5.8 (x86_64-apple-darwin20.0)
```
설치된 zsh 버전을 확인한다.   
  
```sh
brew update
brew upgrade zsh
zsh --version
zsh 5.8 (x86_64-apple-darwin20.0)
```
최신 버전이 아닌 경우 위와 같이 업데이트를 진행해주면 된다.   

### Oh My Zsh 설치   
