---
layout: post
title:  "Azure Table Storage - Thinking about partition ordering"
date:   2017-09-30
categories: azure tablestorage
---

Normal approach: Row Keys == Guid.NewGuid()

Descending Ordered approach: `this.RowKey = string.Format("{0:D24}", DateTime.MaxValue.Ticks - DateTime.UtcNow.Ticks);`

Ascending Ordered approach: `this.RowKey = string.Format("{0:D24}", DateTime.UtcNow.Ticks);`
