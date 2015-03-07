---
layout: post
title: jekyll でブログ作った．
date: 2015-02-21
tags: [ごあいさつ,tech]
---

マークアップで作るのおもろいな。<br/>
暫く個人的な開発のことはこちらに記載していこうと思う。<br/>

![_config.yml]({{ site.baseurl }}/images/jekyll-logo.JPG)

##Try Ruby
{% highlight ruby linenos %}
class Person
  def initialize(name)
    @name = name
  end
  
  def hello
    "Hello, friend!\nMy name is #{@name}!"
  end
end

charlie = Person.new('Charlie')
puts charlie.hello

# >> Hello, friend!
# >> My name is Charlie!
{% endhighlight %}

<br/>
{% render_time %}
{¥% tweet https://twitter.com/DEVOPS_BORAT/status/159849628819402752 align='center' ¥%}
