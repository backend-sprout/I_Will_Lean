# Oh My Zsh 설정하기 
## 환경 정보 
* 기종 : MacBook Pro (13-inch, 2017, Two Thunderbolt 3 ports)
* OS : macOS BigSur 11.4

## 방법
### Iterm2 다운 
[Iterm2](https://iterm2.com/) 

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

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

쉘을 이용하여 `Oh My Zsh`를 설치한다.   

### 기본 shell 을 zsh 로 바꾸기
```sh
chsh -s `which zsh`
```
### 불필요한 이름 지우기
```sh
vi ~/.zshrc
```
```sh
# 방법1
DEFAULT_USER="$(whoami)"

# 방법2
prompt_context() {
  if [[ "$USER" != "$DEFAULT_USER" || -n "$SSH_CLIENT" ]]; then
    prompt_segment black default "%(!.%{%F{yellow}%}.)$USER"
  fi
}
```
```sh
source ~/.zshrc
```

### 개행
```sh
vi ~/.oh-my-zsh/themes/agnoster.zsh-theme
```
```sh
## Main prompt
// 생략
prompt_newline() {
  if [[ -n $CURRENT_BG ]]; then
    echo -n "%{%k%F{$CURRENT_BG}%}$SEGMENT_SEPARATOR
%{%k%F{blue}%}$SEGMENT_SEPARATOR"
  else
    echo -n "%{%k%}"
  fi

  echo -n "%{%f%}"
  CURRENT_BG=''
}

build_prompt() {
  RETVAL=$?
  prompt_status
  prompt_virtualenv
  prompt_aws
  prompt_context
  prompt_dir
  prompt_git
  prompt_bzr
  prompt_hg
  prompt_newline //이부분을 추가 꼭 순서 지키기
  prompt_end
}

// 생략
```


### Terms2 테마 설치하기(옵션)
[iTerm2-Color-Schma](https://github.com/mbadolato/iTerm2-Color-Schemes)에 접속하면 iTerm2에서 사용가능한 다양한 컬러 스키마를 다운받을 수 있다.        
다운을 받은 후 iTerm2에서 `cmd` + `,`를 눌러 환경 설정을 띄운 후.    
`Profile -> colors`로 들어가면 아래와같은 화면을 볼 수 있다.    





