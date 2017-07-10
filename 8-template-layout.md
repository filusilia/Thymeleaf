## 8.1 Including template fragments\(导入模板文件\)

### 定义与引用模板

页面中我们可能需要导入其他模板文件,类似于页眉页脚,菜单等.

在Thymeleaf 中，我们可以使用`th:fragment`属性来定义一个模板。

我们可以新建一个简单的页尾模板，如：/WEB-INF/templates/footer.html，内容如下：



```js
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
    <body>
        <div th:fragment="copy">
            &copy; 2011 The Good Thymes Virtual Grocery
        </div>
    </body>
</html>
```

上面定义了一个叫做

`copy`的片段，接着我们可以使用`th:insert`或者`th:replace`属性来使用它.\(同时`th:include`,也可以引入模板,但会在未来版本中删除这个属性\).



```js
<body>
 ...
 <div th:insert="~{footer :: copy}">
 </div>
</body>
```

`th:insert 引入需要表达式`\(`~{...}`\),上面例子中是一个非复杂的表达式,这种表达式也可以简写成:



```js
<body>
...
    <div th:insert="footer :: copy">
    </div>
</body>
```

### 模板语法

其中`th:insert `中的参数格式为templatename::\[domselector\],  
其中templatename是模板名（如footer），domselector是可选的dom选择器。如果只写templatename，不写domselector，则会加载整个模板。

~{templatename} 会加载整个模板

~{::selector}" 或 "~{this::selector}" 会加载当前页面中指定模板

### 不使用`th:fragment`引入模板

通过强大的dom选择器，我们可以在不添加任何Thymeleaf属性的情况下定义模板：



```js
...
    <div id="copy-section">
    &copy; 2011 The Good Thymes Virtual Grocery
    </div>
...
```

可以使用id等属性引用模板,引用方式与css类似.



```js
<body>
  ...  
 <div th:insert="~{footer :: #copy-section}">
 </div>
</body>
```



### `th:insert 与 th:replace`\(与`th:include`\)的不同点

And what is the difference between`th:insert`and`th:replace`\(and`th:include`, not recommended since 3.0\)?

* `th:insert 直接添加模板内信息到页面`

* `th:replace 替换标签内信息`

* `th:include 与insert类似,但是只会加载模板标签内的内容.`

下面是个例子:

模板文件如下:

```js
<footer th:fragment="copy">
  &copy; 2011 The Good Thymes Virtual Grocery
</footer>
```

引入:

```
<body>
    ...
    <div th:insert="footer :: copy">
    </div>
    
    <div th:replace="footer :: copy">
    </div>
    
    <div  th:include="footer :: copy">
    </div>
</body>
```

下面是引用结果:



```
<body>
    ...  
    <div>
        <footer>
        &copy; 2011 The Good Thymes Virtual Grocery
        </footer>
    </div>
    
    <footer>
        &copy; 2011 The Good Thymes Virtual Grocery  
    </footer>
        
    <div>
        &copy; 2011 The Good Thymes Virtual Grocery  
    </div>
</body>
```



