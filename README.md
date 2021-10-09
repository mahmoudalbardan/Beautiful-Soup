# Beautiful-Soup

## Introduction 
This is an HTML document I’ll be using as an example:

```
html_doc = """<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title"><b>The Dormouse's story</b></p>

<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>

<p class="story">...</p>
"""
 ```
 
 
 Output:
 
 
 
 ```
 from bs4 import BeautifulSoup
soup = BeautifulSoup(html_doc, 'html.parser')

print(soup.prettify())
# <html>
#  <head>
#   <title>
#    The Dormouse's story
#   </title>
#  </head>
#  <body>
#   <p class="title">
#    <b>
#     The Dormouse's story
#    </b>
#   </p>
#   <p class="story">
#    Once upon a time there were three little sisters; and their names were
#    <a class="sister" href="http://example.com/elsie" id="link1">
#     Elsie
#    </a>
#    ,
#    <a class="sister" href="http://example.com/lacie" id="link2">
#     Lacie
#    </a>
#    and
#    <a class="sister" href="http://example.com/tillie" id="link3">
#     Tillie
#    </a>
#    ; and they lived at the bottom of a well.
#   </p>
#   <p class="story">
#    ...
#   </p>
#  </body>
# </html>
 ```


This is how we navigate through this HTML document
```
soup.title
# <title>The Dormouse's story</title>

soup.title.name
# u'title'

soup.title.string
# u'The Dormouse's story'

soup.title.parent.name
# u'head'

soup.p
# <p class="title"><b>The Dormouse's story</b></p>

soup.p['class']
# u'title'

soup.a
# <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>

soup.find_all('a')
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

soup.find(id="link3")
# <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
```


How to get all URL links in this html file ?

```
for link in soup.find_all('a'):
    print(link.get('href'))
    
http://example.com/elsie
http://example.com/lacie
http://example.com/tillie

```

How to extract all the text from a page ?

print(soup.get_text())

The Dormouse's story

The Dormouse's story
Once upon a time there were three little sisters; and their names were
Elsie, Lacie and Tillie; and they lived at the bottom of a well.


# Objects
Beautiful Soup transforms a complex HTML document into a complex tree of Python objects. But you’ll only ever have to deal with about four kinds of objects: Tag, NavigableString, BeautifulSoup, and Comment.

#### Tags and names 
A Tag object corresponds to an XML or HTML tag in the original document
```
frm bs4 import BeautifulSoup as BS
soup = BS('<b class="boldest">Extremely bold</b>', 'html.parser')
tag = soup.b
print (type(tag))
# <class 'bs4.element.Tag'>
print (tag.name)
# b
tag.name = "a" # we can chage the name 
print (tag)
# <a class="boldest">Extremely bold</a>
```

### Attributes
A tag may have any number of attributes. The tag `<b id="boldest">`  has an attribute "id" whose value is "boldest". You can access a tag’s attributes by treating the tag like a dictionary
 
```
from bs4 import BeautifulSoup as BS
tag = BS('<b id="boldest" link ="www.link.com"> bold </b>', 'html.parser').b
print (tag['id'])
# boldest
print (tag.attrs)
{'id': 'boldest', 'link': 'www.link.com'}


tag['id'] = 'verybold'
tag['link'] = "www.link.fr"
print (tag)
# <b id="verybold" link="www.link.fr"> bold </b>

del tag['id']
del tag['link']
print (tag)
# <b>bold</b>

print (tag['id'])
# KeyError: 'id'
print (tag.get('id'))
# None

 
# class is a multi valued attribute defined by HTML4 and HTML5, thus if you want to access its value it returns a list
soup = BS('<p class="body strikeout"></p>', 'html.parser')
soup.p['class']
# ['body', 'strikeout']
```
 

