---
layout: default
title: Trang chủ
use_math: true
---

# Chào mừng đến với trang của tôi!

Đây là trang chủ.

Ở đây tôi sẽ viết một số chủ đề liên quan đến kinh nghiệm lập trình, thuật toán mà tôi đã từng lĩnh hội được hồi còn học cấp 3 và Đại học

# Các chủ đề

{% assign navpages = site.pages | where_exp: "p", "p.nav_title" | sort: "nav_order" %}
{% assign grouped = navpages | group_by: "nav_group" %}
{% for group in grouped %}
{% if group.name != nil %}
## {{ group.name }}
{% endif %}
{% for p in group.items %}
- [{{ p.nav_title }}]({{ p.url | relative_url }})
{% endfor %}
{% endfor %}