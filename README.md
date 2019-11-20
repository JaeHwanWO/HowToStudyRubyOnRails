# Ruby on Rails

## 0. 왜 써야하는가?

이 튜토리얼의 근간이 된 유투브 강좌입니다. 

```
 https://www.youtube.com/watch?v=wbZ6yrVxScM&t=925s
```

`github`, `twitter` 가 바로 레일즈로 만들어졌다. 

초보 개발자에게도 좋은 언어이다. php, js에 비해서 훨씬 간단하고, 세팅할 부분이 적다. 개인적으로는 사이드 프로젝트용 미니 서버를 만들기에 매우 좋다고 생각한다. (너무 어려운 프로젝트에서는, 오히려 커스터마이징 하기에 복잡할 수 있음.)
 
## 1. 작업 환경 세팅하기
1. 설치

> 사실 루비온레일즈 개발에서 가장 어려운 부분은 설치이다. Mac을 쓰고 있다면, 루비는 (파이썬처럼) 디폴트로 깔려져 있다. 하지만 Rails를 쓰려면, 다른걸 꽤 많이 깔아줘야 한다. 

```
www.installrails.com
```
이 사이트에 친절하게 나와있으니, 참고해서 설치하시면 된다. 

2. IDE

* RubyMine
    * 학생용 무료
* VSCode
    * 무료이지만, ruby 전용 툴은 아님. 
 
 
## 2. 만들 예제 소개하기
### 1. 블로그 CRUD

* Create, Read, Update, Delete가 가능한 블로그 포스팅 기능을 만들 예정이다. 

### 2. Bulma - for CSS Design
* 우리는 프론트 개발을 하려는 것이 아니니, Bulma라는 CSS 라이브러리를 사용할 예정이다. 부트스트랩과 비슷한 역할을 한다고 생각하면 된다. 

## 3. rails app 만들기

```
원하는 디렉토리로 이동
```
```
rails new demoBlog
```
```
cd demoBlog
```
```
rails s
```
 * 짠 ! 127.0.0.1:3000으로 접속했을 때, You're on Rails가 뜨면 성공적인 세팅이다. 

### gem file 설정

> rubygems.org 에서 잼 파일을 다 만들어준다.
> 참고하자!

```ruby
gem 'bulma-rails', '~>0.6.1'
```
* bulma 역시 잼을 통해서 설치해준다. 
* bulma는 한가지 더 해줄 것이 있는데, app > assets > stylesheets 폴더에서 application.css 파일에 
` @import "bulma"; ` 를 추가해줘야 한다. 
* 그 후, `application.css`를 `application.scss`로 rename 한다. 

```
gem 'simple_form', '~> 5.0', '>= 5.0.1'
```

* 굳이 서버를 껐다 켜지 않아도 변경사항이 반영되도록 live-reload 기능을 추가해준다. 
```
gem 'guard', '~> 2.16', '>= 2.16.1'
```
* guard는 오직 development에서만 필요하다. group :development라고 써있는 곳 안쪽에 넣어주면 된다. 

```
gem 'guard-livereload', '~> 2.5', require: false
```

* guard와 마찬가지로 developement 안에 넣어준다. 

```ruby
# Make erros better looking
```
# Make errors better looking
gem 'better_errors', '~>2.4'
```
* better_errors 역시 development에 넣어준다. 


버전은 문서를 쓰는 시점(2019-11-20)과 많이 다를 수 있으니, 꼭 rubygems에서 확인하길 바란다.


### install

```
bundle
```
* 추가한 gem들이 설치되었다!

* 서버를 열고, 다른 커멘드 탭에서 다음과 같은 작업을 계속한다. 

```
rails generate simple_form:install
```

```
guard init livereload
```
* guard file이 생긴것을 확인할 수 있다. 




### controller 생성

```
rails g controller posts
```
* posts와 관련된 view, controller, helper 등이 자동 생성된다. 

* controllers안에 `posts_controller.rb`가 있는 걸 확인할 수 있다. 
	* `posts_controller.rb` 안에 다음과 같은 함수를 추가해준다. 

```ruby
		 def index
		 
 		 end
```

index라는 함수를 추가해주고, 내용은 아직 채우지 않은 것이다. 

* views > posts 안에 `index.html.erb`라는 파일을 직접 추가해준다. 


```html
<h1 class = "title"> Blog Index </h1>
```
* 을 추가해준다. 


* 하지만, localhost:3000/posts로 이동하면 No route matches "/posts"라는 경고를 보게 된다.
* route를 추가해 줄 차례이다. 

config 폴더 안에 있는 `routes.rb` 파일에 

```
resources :posts

root "posts#index"

```
* 라고 써 준다. 
* 기본적으로 posts에 접근하게 해주는데, (아마 모델 얘기인듯)
* root로 들어왔을 때는 postController안에 있는 index 함수를 실행하라는 뜻이다. 
* localhost:3000/는 잘 작동하는 것을 확인할 수 있다. 

 `PostsController`에 def new end 와, def create end 를 추가해준다. 아까와 마찬가지로 함수 내용은 채우지 않는다. 
 

### model 생성
 
이제 model을 추가할 차례이다. 

```
rails g model Post title:string content:text
```
* title과 content의 타입을 정해준 모델을 생성했다. 
* 원한다면 마이그레이션 파일을 변경할 수 있다: db폴더 안에 migration file이 있다. 

```
rails db:migrate
```
* migration을 해준다. 


### controller 함수들 채우기

```ruby
def new
	@post = Post.new
end
```
* Post.new가 model에서 새로운 객체를 만들어준다. 

* @가 붙은 변수는, 인스턴스 안에 있는 변수이다. controller 안에서 쓸 변수를 선언해준거라고 생각하면 된다.
* 참고로 루비는 선언 할 때 let이나 var같은 타입도 지정하지 않는다. * @post라고 쓰는 것 자체가 선언이다.  

routing에 대해서 헷깔린다면?.?

터미널에 
```
rake routes
```
를 치면 다 나온다!


* controller에 있는 함수를 더 채워준다. 

```ruby
def create
   @post = Post.new(post_params)
   if @post.save
	   redirect_to @post
	else 
		render 'new'
	end	   
end

```

* create 함수는, post_params를 실행해서, 그 result를 매개변수로 해서 new를 실행시킨다. 
* 만약 post가 save라면?(정확한 의미 모르겠음) post로 redirect 시키고, 
* 아니면 new 뷰를 렌더링 해준다. 

```ruby
private

def post_params
	params.require(:post).permit(:title, :content)
end
```
* private으로 선언된 post_params 함수는, post 객체의 permission을 받아온다. title과 content에 대해서 받아왔다. 

### view 작성해주기
views > posts 안에 
` new.html.erb` 파일을 만들어준다. 

```html
<h1 class = "title"> New Posts </h1>

```

* 임시로 작성해둔다. 

* 잘 돌아가는것이 확인되었으면, 다음과 같은 내용을 덧붙인다. 

```erb
<h1 class = "title"> New Posts </h1>

<div class="section">
  <%= simple_form_for @post do |f| %>
  <div class="field">
    <div class="control">
      <%= f.input :title, input_html: { class: 'input' }, wrapper: false , label_html: { class: 'label'} %>
    </div>
  </div>

    <div class="field">
      <div class="control">
        <%= f.input :content, input_html: { class: 'textarea' }, wrapper: false , label_html: { class: 'label'} %>
      </div>
    </div>
    <%= f.button :submit, 'Create new post', class: "button is-primary" %>
  <% end %>
</div>
```
* erb가 어렵다면 ?? html을 erb로 바꿔주는 사이트들이 여러개 있다. 참고해보자. 
* <%= => <% end %> 가 루비 문법을 html에서 쓰려고, 표시하는 부분이다. 
* 한줄로 쓰고 싶다면 <%= %> 을 쓰자. 

### controller 작성해주기.

controller에 다음과 같은 함수를 추가한다. 

```ruby
def show
   @post = Post.find(params[:id])
end

```

* show함수는, param으로 받은 id를 써서, post db를 뒤져서 id와 일치하는? 포스트를 찾아서 갖고 오는 애이다. 
* 이 params가 어떻게 넘어왔는지는 뒤쪽에 나온다. 

### view 작성해주기

`show.html.erb` 파일을 만든다. 

```
<div class="section">
  <div class="container">
    <h1 class="title">
      <%= @post.title %>
    </h1>
      <div class="content">
        <%= @post.content %>
      </div>
    </h1>
  </div>
</div>
```

* show는 말그대로 포스팅을 보여준다. 

### controller 작성해주기

```
def index
    @posts = Post.all.order("created_at DESC")
end
```

* index 함수를 수정한다. 모든 포스팅을 보여주는 기능으로 변경되었다!

### view 작성해주기

`index.html.erb`를 수정해준다. 

```
<h1 class = "title"> Blog Index </h1>
<section class="section">
  <div class="container">
    <% @posts.each do |post| %>
      <div class="card">
        <div class="card-content">
          <div class="media">
            <div class="media-content">
              <p class="title is-4">
                <%= link_to post.title %>
              </p>
            </div>
          </div>

          <div class="content">
            <%=post.content %>
          </div>

<!--          <div class="comment-count">-->
<!--            <span class="tag is-rounded">-->
              <%# # comment count %>
<!--            </span>-->
<!--          </div>-->

        </div>
      </div>
    <% end %>
  </div>
</section>
```


### layout code 복붙하기


```html
<section class="hero is-primary is-medium">
	  <!-- Hero head: will stick at the top -->
	  <div class="hero-head">
	    <nav class="navbar">
	      <div class="container">
	        <div class="navbar-brand">
	          <%= link_to 'Demo Blog', root_path, class: "navbar-item" %>
	          <span class="navbar-burger burger" data-target="navbarMenuHeroA">
	            <span></span>
	            <span></span>
	            <span></span>
	          </span>
	        </div>
	        <div id="navbarMenuHeroA" class="navbar-menu">
	          <div class="navbar-end">
	            <%= link_to "Create New Post", new_post_path, class:"navbar-item" %>
	          </div>
	        </div>
	      </div>
	    </nav>
	  </div>

	  <!-- Hero content: will be in the middle -->
	  <div class="hero-body">
	    <div class="container has-text-centered">
	      <h1 class="title">
	        <%= yield :page_title %>
	      </h1>
	    </div>
	  </div>
	</section>
```


* 이 코드를 복붙해왔는데, bulma에 가면 다양한 snippet들이 있다. 원하는 걸 쓰면 될 듯 하다. 


> 여기부터 영상의 1시간 부분이다. 영상 링크에는 깃헙 링크도 있더라...

### edit과 post 

* edit과 post 기능을 추가해보자!
* edit을 하기 위해서는 두개의 action이 있어야한다. 
* 하나는 update, 하나는 edit.







