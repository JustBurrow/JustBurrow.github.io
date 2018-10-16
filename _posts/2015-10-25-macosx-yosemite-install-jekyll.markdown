---
layout: post
title: MacOS X Yosemite에 jekyll 설치하기
---

다음을 위해 한 일을 기록한 글입니다.

1. GitHub Pages를 사용한 블로그 만들기.
2. 툴을 사용한 포스팅 편의성과 블로그 꾸미기.
3. Github Flavored Markdown을 사용할 수 있도록 설정하기.

## 설치

`sudo gem install jekyll`로 설치할 수 있다고 하는데, 다음과 같은 에러가 난다.

```
Justui-MacBook-Pro:~ justburrow$ sudo gem install jekyll
Building native extensions.  This could take a while...
ERROR:  Error installing jekyll:
	ERROR: Failed to build gem native extension.

    /System/Library/Frameworks/Ruby.framework/Versions/2.0/usr/bin/ruby -r ./siteconf20151025-39611-mdll1l.rb extconf.rb
creating Makefile

make "DESTDIR=" clean

make "DESTDIR="
make: *** No rule to make target `/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.11.sdk/System/Library/Frameworks/Ruby.framework/Versions/2.0/usr/include/ruby-2.0.0/universal-darwin14/ruby/config.h', needed by `porter.o'.  Stop.

make failed, exit code 2

Gem files will remain installed in /Library/Ruby/Gems/2.0.0/gems/fast-stemmer-1.0.2 for inspection.
Results logged to /Library/Ruby/Gems/2.0.0/extensions/universal-darwin-14/2.0.0/fast-stemmer-1.0.2/gem_make.out
Justui-MacBook-Pro:~ justburrow$ gem install jekyll
ERROR:  While executing gem ... (Gem::FilePermissionError)
    You don't have write permissions for the /Library/Ruby/Gems/2.0.0 directory.
Justui-MacBook-Pro:~ justburrow$ 
```

이유는, XCode 업데이트 후 커맨드라인 개발도구를 설치하지 않았기 때문.

```
Justui-MacBook-Pro:~ justburrow$ xcode-select --install
xcode-select: note: install requested for command line developer tools
Justui-MacBook-Pro:~ justburrow$ sudo gem install jekyll
```

이제 설치된다.

## 설정

`_config.yml` 파일에 `kramdown.input: GFM`을 추가한다. 깃허브 스타일의 마크다운을 쓸 수 있다. 단, 파일 확장자는 `*.md`가 아닌 `*.markdown`이다.

```yaml
markdown: kramdown
kramdown:
  input: GFM
```

## 확인

`jekyll serve --watch` 명령어로 확인할 수 있다. 이 명령어로 jekyll 웹서버를 실행하면 브라우저로 http://localhost:4000 에서 확인할 수 있다.
포스트는 저장시 실시간으로 갱신하지만, 설정파일(`_config.yml`)을 수정하면 새로 시작해야 된다.

```
Justui-MacBook-Pro:JustBurrow.github.com justburrow$ jekyll serve --watch
Configuration file: /Users/justburrow/git/github.com/JustBurrow/JustBurrow.github.com/_config.yml
            Source: /Users/justburrow/git/github.com/JustBurrow/JustBurrow.github.com
       Destination: /Users/justburrow/git/github.com/JustBurrow/JustBurrow.github.com/_site
      Generating... 
                    done.
 Auto-regeneration: enabled for '/Users/justburrow/git/github.com/JustBurrow/JustBurrow.github.com'
Configuration file: /Users/justburrow/git/github.com/JustBurrow/JustBurrow.github.com/_config.yml
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
      Regenerating: 2 file(s) changed at 2015-10-25 17:56:16 ...done in 0.085741 seconds.
      Regenerating: 1 file(s) changed at 2015-10-2
```

## 참고

* [지킬로 깃허브에 무료 블로그 만들기](https://nolboo.github.io/blog/2013/10/15/free-blog-with-github-jekyll)
* [mac yosemite, gem install error solution](http://blog.caesarchi.com/2014/12/mac-yosemite-gem-install-error-solution.html)
