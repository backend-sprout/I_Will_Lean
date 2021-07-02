# Oh My Zsh 설정하기 
## 환경 정보 
* 기종 : MacBook Pro (13-inch, 2017, Two Thunderbolt 3 ports)
* OS : macOS BigSur 11.4

## 방법
### Iterm2 다운 
[Iterm2](https://iterm2.com/) 

### Homebrew 다운 
[Homebrew](https://brew.sh/index_ko)에 접속해서 `Homebrew`를 다운 받는다.   
      
<img width="1018" alt="스크린샷 2021-07-02 오후 12 07 19" src="https://user-images.githubusercontent.com/50267433/124239356-90eb2f00-db54-11eb-8b8a-bff921b344df.png">
     
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

<img width="1025" alt="스크린샷 2021-07-02 오후 12 09 44" src="https://user-images.githubusercontent.com/50267433/124239308-8761c700-db54-11eb-857f-01426f2c42b8.png">

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
# 이름 아에 지우기 
DEFAULT_USER="$(whoami)"

# 이름만 남기기 
prompt_context() {
  if [[ "$USER" != "$DEFAULT_USER" || -n "$SSH_CLIENT" ]]; then
    prompt_segment black default "%(!.%{%F{yellow}%}.)$USER"
  fi
}
```
```sh
source ~/.zshrc
```

### New Line 적용하기(옵션)
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

### Syntax Hightlight 적용하기 (옵션)
```sh
// brew를 통해 설치해줍니다.
brew install zsh-syntax-highlighting
```
```sh
// 매번 Iterm2 실행시 적용되도록 설정해주자  
vi ~/.zshrc
```
```sh    
// .zshrc 맨 아래에 플러그인을 적용하는 코드를 넣는다.  
source /usr/local/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

### 폰트 깨짐 해결
[ZeddiOS 님의 블로그](https://zeddios.tistory.com/1207)참조   

```sh
# 적절한 디렉터리에 
git clone https://github.com/powerline/fonts.git --depth=1
```
   
<img width="779" alt="스크린샷 2021-07-02 오후 4 02 36" src="https://user-images.githubusercontent.com/50267433/124238517-a6ac2480-db53-11eb-848f-da44d938f498.png">
  
Terminal의 서체를 `Ubuntu Mono derivative Powerline` 를 사용했다.   
  
<img width="1055" alt="스크린샷 2021-07-02 오후 4 03 00" src="https://user-images.githubusercontent.com/50267433/124238494-a2800700-db53-11eb-9c48-2b90dd243e99.png">
   
이후, Iterm2의 font 도 `Ubuntu Mono derivative Powerline` 를 사용했다.     

### 경로 자동 설정 (옵션)
> [참고 사이트](https://c0wb3ll.tistory.com/entry/%ED%84%B0%EB%AF%B8%EB%84%90-iterm2-zsh-oh-my-zsh%EB%A5%BC-%EA%B3%81%EB%93%A4%EC%9D%B8)   
  
```sh
// 경로 자동 설정 
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions  

// 앞서 설정한, 하이라이트도 이걸로 설정 가능하다.
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```
`.oh-my-zsh` 숨김 폴더에 설정되므로 경로를 신경 안써도 된다.      

```sh
vi ~/.zshrc
```
```sh
// 생략, 어딘가에 있다.  

plugins=(
git
zsh-autosuggestions
zsh-syntax-highlighting
)
```

### Terms2 테마 설치하기(옵션)
```sh
vi ~/.zshrc
```
```sh
# 생략, robyrussell 을 agnoster로 바꾼다.   
ZSH_THEME=”agnoster”    
```     
<img width="1025" alt="스크린샷 2021-07-02 오후 12 11 05" src="https://user-images.githubusercontent.com/50267433/124239229-71ec9d00-db54-11eb-8144-829b79cde6b9.png">
     
`agnoster` 기본 테마로 설정을 바꾼다.    



[iTerm2-Color-Schma](https://github.com/mbadolato/iTerm2-Color-Schemes)에 접속하면 iTerm2에서 사용가능한 다양한 컬러 스키마를 다운받을 수 있다.        
다운을 받은 후 iTerm2에서 `cmd` + `,`를 눌러 환경 설정을 띄운 후.    
`Profile -> colors`로 들어가면 아래와같은 화면을 볼 수 있다.  
    
<img width="1030" alt="스크린샷 2021-07-02 오후 3 40 00" src="https://user-images.githubusercontent.com/50267433/124238813-fb4f9f80-db53-11eb-91cf-237c333ae193.png">

스크린샷 2021-07-02 오후 3.41.27<img width="1040" alt="스크린샷 2021-07-02 오후 3 41 27" src="https://user-images.githubusercontent.com/50267433/124238765-ee32b080-db53-11eb-9824-ccb453ba10d6.png">  

이후 `Color Presets..` 에서 알맞은 테마를 사용하면 된다.      
필자는 `3024 Night`를 선택했다.    
  
### 이외의 다양한 플러그인 
[사이트](https://medium.com/harrythegreat/zsh%EC%99%80-%ED%95%A8%EA%BB%98-%EC%82%AC%EC%9A%A9%ED%95%A0-%ED%94%8C%EB%9F%AC%EA%B7%B8%EC%9D%B8-%EC%B6%94%EC%B2%9C-6%EA%B0%80%EC%A7%80-8f9b8b7f3c24)



