---
layout: post
title:  "Windows 사용자를 위한 Jekyll로 GitHub Page에 블로그 만들기"
date:   2018-02-20 15:32:00 +0900
categories: jekyll update
---

[window에서 jekyll 실행][window-jekyll] 가이드를 참고하여 필요한 것들을 설치합니다.
1. [Ruby][ruby-download]와 [Ruby Devkit][ruby-download] 설치

  * 1-1. 하단에 DevKit-mingw64. 로 시작되는 파일 설치 (32bits , 64bits 선택)

  * 1-2. Ruby Devkit 설치한 후 초기화하고 Ruby 설치에 바인딩 해야 한다.

  * 1-3. 터미널을 열어서
  <pre><code>
    cd C:\RubyDevKit    // Devkit을 추출한 폴더로 이동
    ruby dk.rb init     // 설정 파일에 추가
    ruby dk.rb install  // Devkit 을 설치하여 Ruby 설치에 바인딩
  </code></pre>


2) Jekyll 은 소프트웨어 패키지인 Ruby Gem 형태로 제공됨으로 Jekyll Gem을 설치 해야 합니다.
<pre><code> gem install jekyll</code></pre>

3) Syntax highlighter 설치
> Syntax Hightlighter란, 코드를 서로 다른 색상으로 표현하여 보기 좋게 만드는 것입니다.

<pre><code> gem install rouge</code></pre>

실행창에 install 한 후에 `_config.yml` 에 설정한다.
<pre><code> highlighter: rouge</code></pre>

4) [파이썬 설치][python-download] : 구문강조 표시를 위해 Python을 사용하려면 Python, pip 및 python.rb의 python 기반을 설치해야 합니다.

5) [Pip 설치][pip-download] : Pip은 Ruby Gams와 비슷한 Python 패키지를 설치하고 관리하는 도구입니다. pygments.rb가 코드를 강조 표시하는 데 사용하는 Python 패키지 인
Pygments를 설치하려면 패키지가 필요합니다.
<pre><code>
  cd C:\pip           // pip 폴더로 이동
  python get-pip.py   // 구성 요소를 자동으로 다운로드하고 설치
</code></pre>

6) Pygments의 Python 기반 설치
<pre><code> python -m pip install Pygments          
</code></pre>
실행창에서 설치 한 후 `_config.yml` 에 설정한다.

<pre><code> highlighter: pygments          
</code></pre>

7) wdm Gem 설치 :
<pre><code>
  gem install wdm   
  gem 'wdm', '~> 0.1.0' if Gem.win_platform?  // Gemfile에 wdm 필요

</code></pre>

> 엥 놋북에 셋팅하다가 gem install jekyll 에서 에러 생겨서 작업이 안된다???

<pre><code>gem install jekyll
jekyll new .
gem install jekyll bundler     
bundle install   (bundle install 다운 받지 않으면 에러남)
jekyll serve</code></pre>

참고 사이트 :

[jekyll-ko][jekyllrb-ko],
[window에서 jekyll 실행][window-jekyll],
[Ruby][ruby-download],
[Nolboo's Blog][nolboo-blog],
[xho95's Swift Life][xho95-blog],
[markdown 사용법(한글)][markdown-ko],
[파이썬 설치][python-download],
[Pip 설치][pip-download]


[pip-download]: https://pip.pypa.io/en/latest/installing/
[python-download]: https://www.python.org/downloads/
[jekyllrb-ko]: https://jekyllrb-ko.github.io/
[ruby-download]: https://rubyinstaller.org/downloads/
[window-jekyll]: http://jekyll-windows.juthilo.com/
[nolboo-blog]: https://nolboo.kim/blog/2013/10/15/free-blog-with-github-jekyll/
[xho95-blog]: http://xho95.github.io/blog/github/pages/jekyll/minima/theme/2017/03/04/Jekyll-Blog-with-Minima.html
[markdown-ko]: https://gist.github.com/ihoneymon/652be052a0727ad59601
