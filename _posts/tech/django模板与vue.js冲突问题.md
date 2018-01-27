# 问题：

django模板与vue.js的变量都是使用“{{”和“}}”包裹起来的，在渲染django模板时会先替代掉所有的“{{”和“}}”及被包裹在其中的内容，使得vue.js没有使用"{{"、"}}"来绑定变量。

# 处理方法：

## 方法1：修改vue.js的默认的绑定符号

```
Vue.config.delimiters = ["[[", "]]"];
```

执行这个之后，你就可以使用“[[”、“]]”来绑定变量的数据了

## 方法2：使用模板的标签来输出“{{”、“ }}”

django模板的`templatetag `可以渲染模板时输出模板标签，标签参数及输出如下：

| 参数              | 输出   |
| --------------- | ---- |
| `openblock`     | `{%` |
| `closeblock`    | `%}` |
| `openvariable`  | `{{` |
| `closevariable` | `}}` |
| `openbrace`     | `{`  |
| `closebrace`    | `}`  |
| `opencomment`   | `{#` |
| `closecomment`  | `#}` |

我们可以在模板中用

```
{% templatetag openvariable %} 
{% templatetag closevariable %}
```

来替代"{{"、"}}"

## 方法3：禁用django模板渲染

django标签 verbatim可以使包裹其中的代码不进行渲染保持原样输出

```
{% verbatim %}
    {{ vue }}
{% endverbatim %}
```

我们可以在需要用于vue.js的地方使用{% verbatim %} {% endverbatim %}包裹。