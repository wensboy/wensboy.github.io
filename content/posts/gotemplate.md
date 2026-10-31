---
draft: false
searchHidden: false
ShowToc: false
TocOpen: false
TocFloat: true
ShowBreadCrumbs: false
comments: false
date: '2026-10-30'
author: ["wendisx"]
title: 'Go Std - text/template'
tags: ["golang"]
cover:
  image: ""
  alt: ""
  caption: ""
  relative: true
editPost:
  URL: "https://github.com/wensboy/wensboy.github.io/blob/main/content"
  Text: "Correct"
  appendFilePath: true
---

{{< tldr >}}
Go Std 系列之: `text/template` 模板库. `text/template` 定义了一套用于处理文本模板数值替换的操作接口, 通常遵循特定的模板规则, 与语言语法类似, 具备一定的可编程性, 但更多用于实现模板字符串的动态插值, 在动态翻译, 文本生成等场景下较多使用. 这里侧重按照官方[doc](https://pkg.go.dev/text/template)来记录: 语法, 使用示例, 拓展相关.
{{< /tldr >}}

## Syntax

fixed rules:

- `{{}}` 切片替换
- . 总是代表context对象, 一个包含所有访问信息的结构.
- function 规定只能返回一个或者两个值, 第二个值必须为error作为断路的判断依据.
- 所有的 command 本质上是类似函数式编程概念中的函数调用.

|所属|语法|解释|
|:-|:-|:-|
|action| `{{/*  */}}` | 注释 |
|action| `{{- /*  */ -}}` | trim space 注释|
|action| `{{pipeline}}` | 值 |
|action| `{{if pipeline}} T1 {{end}}` | if |
|action| `{{if pipeline}} T1 {{else}} T0 {{end}}` | if...else |
|action| `{{if pipeline}} T1 {{else if pipeline}} T0 {{end}}` | if...elseif | 
|action| `{{range pipeline}} T1 {{end}}` | for _,v := range |
|action| `{{range pipeline}} T1 {{else}} T0 {{end}}` | for _, v := range...else |
|action| `{{break}}` | 终止 range |
|action| `{{continue}}` | 继续下一个 range |
|action| `{{template "name"}}` | execute template with nil |
|action| `{{template "name" pipeline}}` | execute template with pipeline |
|action| `{{block "name" pipeline}} T1 {{end}}` | define and execute template shorthand |
|action| `{{with pipeline}} T1 {{end}}` | method |
|action| `{{with pipeline}} T1 {{else}} T0 {{end}}` | method...else |
|action| `{{with pipeline}} T1 {{else with pipeline}} T0 {{end}}` | method...elsemethod |
| argument | nil | map golang nil |
| argument | `.` | 当前操作结构上下文 |
| argument | .Key | map value |
| argument | .Field | struct field value |
| argument | .Method | method |
| argument | funcname | function |
| variables | $<variable_name> | 变量 |
| variables | $<variable_name> := value | 变量定义 |
| variables | $<variable_name> = value | 变量赋值 |
| pipeline | `\|` | 管道符号, 类似bash管道 |
| function | and, or, not | 使用格式: command arg1 arg2 ... |
| function | call | 调用function |
| function | html, js, urlquery | 转义处理 |
| function | len, slice, index | 针对 slice等可迭代类型 |
| function | print, printf, println | 打印操作, `fmt` 下函数调用 |
| function | eq, ne, lt, gt, le, ge | 比较函数 |
| template | `{{define <name>}}{{end}}` | 定义模板 |
| template | `{{template <name>}}` | 执行模板 |

## Example

核心场景: 代码切片生成, 动态配置, 动态复杂值, 动态页面等.

以下挑两个用得比较多的给个简单示例, 实际上由于可编程性比较高, 本质上是设计了一套专门处理文本插值的dsl, 灵活运用即可.

### 代码切片

```go
func main() {
  quick_sort_str := `
    func find_base({{.nums}} []{{.type}}, l, r {{.type}}) {{.type}} {
      base := {{.nums}}[l]
      for l < r {
        for l < r && {{.nums}}[r] >= base {
          r--
        }
        {{.nums}}[l] = {{.nums}}[r]
        for l < r && {{.nums}}[l] <= base {
          l++
        }
        {{.nums}}[r] = {{.nums}}[l]
      }
      {{.nums}}[l] = base
      return l
    }

    func quick_sort({{.nums}} []{{.type}}, l, r {{.type}}) {
      if l>=r {
        return
      }
      p := find_base({{.nums}},l,r)
      quick_sort({{.nums}}, l, p-1)
      quick_sort({{.nums}}, p+1, r)
    }
  `
  quick_sort_option := map[string]any{
    "nums": "list",
    "type": "int",
  }
  tmpl, err := template.New("quick_sort_snipet").Parse(quick_sort_str)
  if err != nil { panic(err) }
  err = tmpl.Execute(os.Stdout, quick_sort_option)
  if err != nil { panic(err) }
}

/*
  打印效果:
      func find_base(list []int, l, r int) int {
      base := list[l]
      for l < r {
        for l < r && list[r] >= base {
          r--
        }
        list[l] = list[r]
        for l < r && list[l] <= base {
          l++
        }
        list[r] = list[l]
      }
      list[l] = base
      return l
    }

    func quick_sort(list []int, l, r int) {
      if l>=r {
        return
      }
      p := find_base(list,l,r)
      quick_sort(list, l, p-1)
      quick_sort(list, p+1, r)
    }
*/
```

### 动态配置

类似上面的代码切片, 主要用于无法在编译器确定的动态配置, 例如:
- 根据 os 信息动态生成符合当前环境的配置.
- 默认配置模板根据环境变量动态确定值.

关于这里做配置的好处:
- 灵活
- 统一管理
- 逻辑集中

## Reference

- [html/template](https://pkg.go.dev/html/template)