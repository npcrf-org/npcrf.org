---
title: "{{ replace .File.ContentBaseName "-" " " | title }}"
date: {{ .Date }}
lastmod: {{ .Date }}
slug: "{{ .File.ContentBaseName }}"
tags: []
draft: true
description: ""
# Every article must list its sources (see /methodology/). One entry per
# source, with the URL and the date it was consulted (ISO 8601).
sources:
  - title: ""
    url: ""
    accessed: "{{ dateFormat "2006-01-02" .Date }}"
# Substantive post-publication corrections are recorded here and rendered
# on the article (see /methodology/). Remove the comment markers to use.
# corrections:
#   - date: "{{ dateFormat "2006-01-02" .Date }}"
#     note: ""
---
