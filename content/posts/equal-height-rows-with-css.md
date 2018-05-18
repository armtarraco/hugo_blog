+++
date = "2018-05-07T09:46:03+02:00"
title = "Equal height rows with CSS"
tags = [
  "table",
]
categories = [
  "css",
]

+++
<!--more-->

Source: [http://blog.karenmenezes.com/2013/aug/9/all-things-are-not-created-equal/](http://blog.karenmenezes.com/2013/aug/9/all-things-are-not-created-equal/ "http://blog.karenmenezes.com/2013/aug/9/all-things-are-not-created-equal/")  
Demo: [http://karenmenezes.com/snippets/equalheightcss-margins/index.html](http://karenmenezes.com/snippets/equalheightcss-margins/index.html "http://karenmenezes.com/snippets/equalheightcss-margins/index.html")  
  
CSS

    tableBlock {
        display: table;
        border-collapse: separate;
        border-spacing: 30px;
        margin: 0 -30px;
    }
    
    .tableRow {
        display: table-row;
    }
    
    .tableCell {
        display: table-cell;
        padding: 10px;
        width: 33.33%;
        border: 1px solid;
    }
    

  
HTML

    <div class="tableBlock">    
      <div class="tableRow">
        <div class="tableCell">
    ...
    