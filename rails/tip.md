### 레일즈 앱에 마크다운 업로드 기능 추가하기

예전부터 마크다운을 여기 데브로그에 추가하려고 마음을 엄청 먹고 있었는데, 바빠서 미루다가 이제서야 마크다운 에디터를 추가해 버렸다.  지금 쓰고 있는 이 글도 마크다운으로 작성중이다. 원래는 마크다운이 위지윅보다 글 쓸때는 약간 불편하다고 생각했지만, 마크다운으로 쓰다 버릇 하니 마크다운이 너무 편하고 포맷팅도 깔끔한 것이 느껴진다.

#### 1. 마크다운 파싱해 주는 젬 설치하기

알아보니 Redcarpet 이라는 젬도 있지만, 둘 다 읽어보니 사용법은 대략 비슷한거 같고 나는 깃헙 지킬에서 쓰는 Kramdown 이라는 모듈을 사용해 보기로 했다.

https://github.com/gettalong/kramdown

언제나 그렇듯이 Gemfile에 아래 문장을 추가한다.
~~~ruby
gem 'kramdown'
~~~

그리고 언제나 그렇듯 `bundle install`

#### 2. 사용법은 초간단

그냥 아래와 같이 쓰면 된다..ㅋㅋㅋㅋㅋ 진짜 레일즈는 너무 사용자 친화적이다

~~~ruby
require 'kramdown'

Kramdown::Document.new(text).to_html
~~~

여기 데브로그에서 쓴 코드를 인용하자면, 아래와 같다.  
나는 데브로그 show 액션에서 사용했고 아래의 `markdown_text` 는 데브로그 테이블의 칼럼이자, 마크다운 텍스트 내용이다.  
아걸 그냥 `Kramdown::Document` 클래스 생성자에 넣고, 생성된 객체를 to_html 메서드로 변환하여 주면 끝이다.

~~~ruby
def show
  @post = Devlog.find_by(id: params[:id])
  if @post.markdown_text.present?
    @markdown = Kramdown::Document.new(@post.markdown_text).to_html
  end
end
~~~

아무튼 변수에 담긴 html 텍스트를 화면에 뿌려주면 잘 나올 것이다.

#### 3. 스타일링

근데 이대로 화면에 뿌려보면, 무언가 깃허브 마크다운만큼 안이쁘다.  
그러면 이쁘게 꾸미면 된다. 아래에 깃허브 마크다운 css 파일이 있다. 

https://github.com/sindresorhus/github-markdown-css

위 링크 디렉토리에 있는 css 파일을 잘 긁어서 적절한 곳에 잘 붙여넣고,  
마크다운 텍스트가 들어갈 구역의 클래스에 `.markdown-body` 클래스를 이쁘게 박아주면 된다.

아래를 참고하도록 하자

~~~html
<div class="markdown_body">
  <%= @markdown.html.safe %>
</div>
~~~

끝! 깃허브 처럼 이쁘게 나온다.