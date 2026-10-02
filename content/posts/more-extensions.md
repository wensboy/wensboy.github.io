---
draft: false
searchHidden: false
ShowToc: true
TocOpen: false
ShowBreadCrumbs: true
comments: true
date: '2026-06-18'
author: ["wendisx"]
title: 'More Extensions'
tags: ["hugo", "markdown extensions"]
cover:
  image: ""
  alt: ""
  caption: ""
  relative: true
editPost:
  URL: "https://github.com/wensboy/wensboy.github.io/blob/main/content"
  Text: "Extend"
  appendFilePath: true
---

# layout::_markup

## native codeblock

issues:

1. No language label.
2. There is a display issue with the copy button.

current:

1. isolation of the top status bar code section.
2. language label.
3. **Copy** icon button.

```cpp
#include<iostream>
using namespace std;

int main() {
  cout << "it looks like this!" << '\n';
}
```

# layout::partials

## comments

This blog uses the [Giscus](https://giscus.app/) comment system.

# layout::shortcodes

## closeable

```html
< closeable open=false class="" hint="" >
...
< /closeable >
```

|attribute|value|description|
|:-:|:-:|:-:|
|open|false, true|open status in default|
|class| "" | render style |
|hint| "" | hint text |

{{< closeable open=false hint="support languages" >}}
- golang
- rust
- lua
- shell
- python
- javascript
- nix
- java
- c/cpp
{{< /closeable >}}

## codeblock

```html
< codeblock syntax=true tab-position="top" overflow="scroll" copy=true >

---go
func main() {
  fmt.Print("hello", "world!")
}
---

---lua
print('hello world!\n')
---

---js
console.log("hello world!\n")
---

< /codeblock >
```

|attribute|value|description|
|:-:|:-:|:-:|
|syntax|false, true|syntax highlight|
|tab-position| "top, bottom, left, right" | tab position |
|overflow| "scroll, switch" | overflow tab render |
|copy| false, true | enable copy or not |

{{< codeblock syntax=true tab-position="top" overflow="scroll" copy=true >}}

---go
func main() {
  fmt.Print("hello", "world!")
}
---

---lua
print('hello world!\n')
---

---js
console.log("hello world!\n")
---

{{< /codeblock >}}

## tip

```html
< tip type="" class="" topic="" >
...
< /tip >
```

|attribute|value|description|
|:-:|:-:|:-:|
|type| "quote(gray), info(blue), safe(green), warn(yellow), danger(red)" | tip type | 
|class| "" | render style |

{{< tip type="quote" topic="quote tip" >}}
this is quote tip
{{< /tip >}}

{{< tip type="info" topic="info tip" >}}
this is info tip
{{< /tip >}}

{{< tip type="safe" topic="safe tip" >}}
this is safe tip
{{< /tip >}}

{{< tip type="warn" topic="warn tip" >}}
this is warn tip
{{< /tip >}}

{{< tip type="danger" topic="danger tip" >}}
this is danger tip
{{< /tip >}}



## tldr

```html
< tldr class="" >
...
< /tldr >
```

|attribute|value|description|
|:-:|:-:|:-:|
|class| "" | render style |

{{< tldr >}}
this is too long don't read section.
{{< /tldr >}}