---
author: ""
title: {{ replace .Name "-" " " | title }}
draft: true
date: {{ dateFormat "2006-01-02" .Date }}
description: ""
images: []
categories: []
tags: []
archives: [{{ dateFormat "2006/01" .Date }}]
---