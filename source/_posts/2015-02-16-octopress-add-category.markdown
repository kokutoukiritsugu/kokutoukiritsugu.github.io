---
layout: post
title: "octopress 添加分类列表"
date: 2015-02-16 08:21:51 +0800
comments: true
categories: octopress
---
发现octopress居然没分类列表，于是google下，添加了一个：  
[链接](http://codemacro.com/2012/07/18/add-category-list-to-octopress/)  
步骤为：

增加category_list插件   
保存以下代码到plugins/category_list_tag.rb：

```
module Jekyll
  class CategoryListTag < Liquid::Tag
    def render(context)
      html = ""
      categories = context.registers[:site].categories.keys
      categories.sort.each do |category|
        posts_in_category = context.registers[:site].categories[category].size
        category_dir = context.registers[:site].config['category_dir']
        category_url = File.join(category_dir, category.gsub(/_|\P{Word}/, '-').gsub(/-{2,}/, '-').downcase)
        html << "<li class='category'><a href='/#{category_url}/'>#{category} (#{posts_in_category})</a></li>\n"
      end
      html
    end
  end
end

Liquid::Template.register_tag('category_list', Jekyll::CategoryListTag)
```
以上生成list，以下让它显示出来  
增加aside  
复制以下代码到source/_includes/asides/category_list.html。

{% raw %}
    <section>
      <h1>Categories</h1>
        <ul id="categories">
          {% category_list %}
      </ul>
    </section>
{% endraw %}

修改 _config.yml 文件中的 default_asides 项：  
加入 asides/category_list.html 即可

    default_asides: [asides/category_list.html, asides/recent_posts.html ... ... ]

  


