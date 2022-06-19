<h1>블로그 프로젝트 소개</h1>

- 프로젝트 제작에 사용한 것들:
  - IDE: RubyMine 2021.2.2
  - SDK: Ruby 2.7.5
  - Gem Packages: bundler, jekyll, jekyll-paginate
  - Theme: Lanyon
  - Comments Handler: Disqus


- 프로젝트 제작 과정:
  1. Jekyll 초기화
  2. Lanyon 테마 파일 적용
  3. Gemfile 설정 (jekyll-paginate 플러그인 적용)
    ```ruby
    source "https://rubygems.org"
    # Hello! This is where you manage which Jekyll version is used to run.
    # When you want to use a different version, change it below, save the
    # file and run `bundle install`. Run Jekyll with `bundle exec`, like so:
    #
    #     bundle exec jekyll serve
    #
    # This will help ensure the proper Jekyll version is running.
    # Happy Jekylling!
    gem "jekyll", "~> 4.2.1"
    # This is the default theme for new Jekyll sites. You may change this to anything you like.
    gem "minima", "~> 2.5"
    # If you want to use GitHub Pages, remove the "gem "jekyll"" above and
    # uncomment the line below. To upgrade, run `bundle update github-pages`.
    # gem "github-pages", group: :jekyll_plugins
    # If you have any plugins, put them here!
    group :jekyll_plugins do
      # gem "jekyll-feed", "~> 0.12"
      gem "jekyll-paginate"
    end
    
    # Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
    # and associated library.
    platforms :mingw, :x64_mingw, :mswin, :jruby do
      gem "tzinfo", "~> 1.2"
      gem "tzinfo-data"
    end
    
    # Performance-booster for watching directories on Windows
    gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]
    ```
  4. _config.yml 설정 (변수 설정)
    ```yaml
    # Setup
    title:               sti09181의 블로그
    tagline:             ''
    description:         '유레카프로젝트 과제 제출을 위한 블로그입니다. 오픈 소스 프로젝트이며, GPLv3의 의해 보호되고 있습니다.'
    url:                 https://sti09181.github.io
    baseurl:             ''
    paginate:            5
    permalink:           pretty
    
    # About/contact
    author:
      name:              sti09181
      url:               https://github.com/sti09181
      email:             sti09181@gmail.com
    
    # GitHub
    github:
      repo: https://github.com/sti09181/sti09181.github.io
    
    # Gems
    plugins:
      - jekyll-paginate
    
    # Custom vars
    google_analytics_id: #UA-XXXX-Y
    
    # Disqus settings
    comment:
      provider: "disqus"
      disqus:
        shortname: "sti09181"
    ```
  5. index.html 수정
  6. about.md 수정
  7. 404.md 수정
  8. _layouts/post.html 수정 (post 레이아웃에 관해 댓글 기능 추가)
    ```html
    ---
    layout: default
    ---
    
    <div class="post">
      <h1 class="post-title">{{ page.title }}</h1>
      <span class="post-date">{{ page.date | date_to_string }}</span>
      {{ content }}
    </div>
    
    {% if site.related_posts.size >= 1 %}
    <div class="related">
      <h2>다른 관련 글</h2>
      <ul class="related-posts">
        {% for post in site.related_posts limit:3 %}
          <li>
            <h3>
              <a href="{{ site.baseurl }}{{ post.url }}">
                {{ post.title }}
                <small>{{ post.date | date_to_string }}</small>
              </a>
            </h3>
          </li>
        {% endfor %}
      </ul>
    </div>
    {% endif %}
    
    {% if page.comments %}
    <h2>댓글</h2>
    <div id="disqus_thread"></div>
    <script>
        /**
         *  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
         *  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables    */
        let PAGE_URL = "{{site.url}}{{page.url}}"
        let PAGE_IDENTIFIER = "{{page.url}}"
        var disqus_config = function () {
        this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
        this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
        };
    
        (function() { // DON'T EDIT BELOW THIS LINE
            var d = document, s = d.createElement('script');
            s.src = 'https://sti09181.disqus.com/embed.js';
            s.setAttribute('data-timestamp', +new Date());
            (d.head || d.body).appendChild(s);
        })();
    </script>
    <noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
    {% endif %}
    ```
  9. _includes/sidebar.html 수정 (전반적인 사이드바 제작)
    ```html
    <!-- Target for toggling the sidebar `.sidebar-checkbox` is for regular
         styles, `#sidebar-checkbox` for behavior. -->
    <input type="checkbox" class="sidebar-checkbox" id="sidebar-checkbox">
    
    <!-- Toggleable sidebar -->
    <div class="sidebar" id="sidebar">
      <div class="sidebar-item">
        <p>{{ site.description }}</p>
      </div>
    
      <nav class="sidebar-nav">
        <a class="sidebar-nav-item{% if page.title == 'Home' %} active{% endif %}" href="{{ '/' | absolute_url }}">메인페이지</a>
    
        {% comment %}
          The code below dynamically generates a sidebar nav of pages with
          `layout: page` in the front-matter. See readme for usage.
        {% endcomment %}
    
        {% assign pages_list = site.pages | sort:"url" %}
        {% for node in pages_list %}
          {% if node.title != null %}
            {% if node.layout == "page" %}
              <a class="sidebar-nav-item{% if page.url == node.url %} active{% endif %}" href="{{ node.url | absolute_url }}">{{ node.title }}</a>
            {% endif %}
          {% endif %}
        {% endfor %}
    
        <a class="sidebar-nav-item" href="{{ site.github.repo }}/archive/refs/heads/main.zip">소스 다운로드</a>
        <a class="sidebar-nav-item" href="{{ site.github.repo }}">원격 저장소</a>
      </nav>
    
      <div class="sidebar-item">
        <p>{{ site.title }}</p>
      </div>
    </div>
    ```
  10. 포스트 추가