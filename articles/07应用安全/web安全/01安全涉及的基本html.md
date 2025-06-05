---
layout: default
title: 安全涉及的基本html
---

# html基本结构

```html
<!DOCTYPE html>  <!-- html5,较新，最广泛 -->
<html lang="en">  <!-- 根标签，语言为英语 -->
<head>  <!-- 头标签，存放元数据和页面配置 -->
    <meta charset="UTF-8">  <!-- 元素标签，编码为UTF-8（支持中文等特殊字符） -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">  
    <!-- 移动端适配：视口宽度=设备宽度，初始缩放比例1.0 -->
    <title>文档标题</title>  <!-- 在顶菜单显示的 -->
</head>
<body>   <!-- 可见内容 -->
<!-- ctrl + ? 生成注释 -->
<!-- ! + enter 生成头部 -->
</body>
</html>
```

# 标题标签

```html
<body>
    <h1>一级标题</h1>
    <h2>二级标题</h2>
    <h3>三级标题</h3>
    <h4>四级标题</h4>
    <h5>五级标题</h5>
    <h6>六级标题</h6>
</body>
```

# 段落标签

```html
<body>
    <p>第一段</p>
    <p>
      第二段<br>  <!-- 换行标签 -->
      第二段
    </p>
    <p>第三段</p>
</body>
```

# span标签

```html
<body>
<!-- span标签，特殊标记文本块 -->
    <p>
      这没有特殊标记<br>
      <span>这是特殊文本块</span>
    </p>
</body>
```

# 特殊字符实体编码

```html
<body>
<!-- 特殊字符实体编码 -->
    <p>
      尖括号是：<><br>
      空格是：   空格了<br>
      版权符是：©<br>
      人民币符号是：¥15000 <br>
    </p>
</body>
```

# div标签

```html
<body>
<!-- div标签，容器标签，搭配样式使用，一个容器一个样式 -->
<!-- 最典型就是顶部、内容、页脚div块 -->
    <p>
      <div style="background-color: bisque;height: 100px;"></div>
      <!-- style内嵌样式，background-color背景颜色，height高度100像素 -->
    </p>
</body>
```

# 图片标签

```html
<body>
<!-- 图片标签 -->
    <p>
      <img src="https://bpic.588ku.com/element_origin_min_pic/19/04/09/e89f042551e34852436b4d6e40a1ae49.jpg" 
      alt="大剑图片" height="25%" width="25%" title="大保健">
      <!-- src是url -->
      <!-- alt是url加载失败替代显示的 -->
      <!-- title是鼠标悬停显示的 -->
    </P>
</body>
```

# 超链接

```html
<body>
<!-- 超链接 -->
    <p>
      <a href="https://lu494.github.io" target="_blank">御剑的技术博客</a>
      <!-- href指的是url -->
      <!-- targer指的是在自己页面打开，还是新页面打开 -->
      <!-- target默认是self，blank是在新页面打开 -->
    </p>
</body>
```

# 图片链接

```html
<body>
<!-- 图片链接 -->
    <p>
      <a href="https://lu494.github.io" target="_blank">
      <img src="https://bpic.588ku.com/element_origin_min_pic/19/04/09/e89f042551e34852436b4d6e40a1ae49.jpg" 
      alt="大剑图片" height="10%" width="10%" title="御剑">
      </a>
    </p>
</body>
```

# 表单、表单控件text、password、submit

```html
<body>
<!-- 表单、表单控件text、password、submit -->
    <form action="#" method="post" enctype="">
      <!-- action标明提交的目标url -->
      <!-- # 代表提交给自己 -->
      <!-- method标明数据提交方法 -->
      <!-- 提交方法有get、post -->
      <!-- enctype代表编码类型 -->
      <!-- 如果提交文件，则method为post方法，enctype为multipart/form-data -->
      <p>
        <!-- input输入控件 -->
        账户：    <input type="text" name="uname">
        <!-- type 类型文本 -->
        <!-- 控件名name，非常重要，数据输入就用name来标识，uname=zhangsan -->
      </p>
      <p>
        <!-- input密码控件 -->
        密码：    <input type="password" name="upasswd">
        <!-- 类型password -->
        <!-- 控件名upasswd -->
      </p>
      <p>
        手机号：<input type="text" name="phonenumber">
      </p>
      <p>
        <!-- input提交控件 -->
        <input type="submit" value="注册">
        <!-- value是提交按钮显示的文本 -->
      </p>
    </form>
</body>
```

# 查看通过get方法提交的参数

```html
<!-- 查看通过get方法提交的参数 -->
    <!-- file:///C:/Users/xlu/Desktop/code/%E5%9F%BA%E6%9C%AC%E7%B
      B%93%E6%9E%84.html?uname=123&upasswd=wqerqwer&phonenumber=12341234 -->
    <!-- .html 是url -->
    <!-- ?代表参数 -->
    <!-- get方法提交的参数会放在url后边 -->
    <!-- 参数名是input控件name -->
    <!-- 参数值是从input控件框输入的值 -->
```

# 查看通过post方法提交的参数

```html
<!-- 查看通过post方法提交的参数 -->
    <!-- 通过post方法提交数据查看 -->
    <!-- f12，网络抓包，捕获 -->
    <!-- 右击：查看网页源代码 -->
```

# 自己做一个搜索框，调用百度引擎的后端，实现搜索功能

```html
<body>
<!-- 自己做一个搜索框，调用百度引擎的后端，实现搜索功能 -->
    <!-- ctrl + u 查看百度搜索框前端源码 -->
    <!-- ctrl + f 搜索<form -->
    <!-- 分析form表单，发现目标地址是https://www.baidu.com/s -->
    <!-- 方法是get -->
    <!-- 搜索框input控件类型是text，控件名是wd -->
    <form action="https://www.baidu.com/s" method="get" target="_blank">
      <input type="text" name="wd">
      <input type="submit" value="仿冒百度">
    </form>
</body>
```

# 表单控件：单选按钮、复选按钮、下拉列表、多行文本

```html
<body>
<!-- 表单控件：单选按钮、复选按钮、下拉列表、多行文本 -->
<!-- 通过数据采集案例实现 -->
    <form action="#" method="post">
      <p>
        账户：<input type="text" name="uname">
      </p>
      <p>
        密码：<input type="password" name="pwd">
      </p>
      <p>
        性别：
        <input type="radio" name="sex" value="male">男
        <input type="radio" name="sex" value="female">女
        <!-- radio input控件，单选按钮 -->
        <!-- 两个控件名都是sex，参数名就是sex -->
        <!-- value是参数值，是固定的male/female -->
        <!-- 性别：、input控件、男、input控件、女不换行 -->
      </p>
      <p>
        兴趣爱好：
        <input type="checkbox" name="hobby" value="game">游戏
        <input type="checkbox" name="hobby" value="movie">电影
        <input type="checkbox" name="hobby" value="music">音乐
        <!-- checkbox复选input控件 -->
        <!-- 控件名都是hobby -->
        <!-- value也是参数值 -->
        <!-- 可以重复选择 -->
        <!-- 兴趣爱好：、input控件、游戏、input控件、电影、input控件、音乐不换行 -->
      </p>
      <p>
        所在城市：
        <select name="city">
          <option value="beijing">北京</option>
          <option value="shanghai">上海</option>
          <option value="shenzhen">深圳</option>
          <!-- select下拉列表控件 -->
          <!-- name是参数名 -->
          <!-- option是下拉列表的选项 -->
          <!-- value是参数的值 -->
          <!-- option控件中间的文本是这个option选项显示的文本 -->
          <!-- 所在城市：select下拉列表控件不换行 -->
        </select>
      </p>
      <p>
        个人介绍：<br>
        <textarea name="resume" cols="30" rows="10"></textarea>
        <!-- textarea是多行文本 -->
        <!-- 参数名是resume -->
        <!-- column是30个字符 -->
        <!-- row行是10行，超过10行要滚动条了 -->
        <!-- textarea多行文本控件默认不换行 -->
      </p>
      <p>
        <input type="submit" value="提交">
      </p>
    <!-- 通过get方法传递数据验证 -->
      <!-- 可以看到数据在url中提交了 -->
      <!-- url有自己的编码，是为了防止字符冲突之类的 -->
      <!-- 可以看到%0D%0A是回车换行 -->
      <!-- 空格是+ 或者%20 -->
    <!-- 通过post方法传递数据验证 -->
      <!-- F12打开开发者选项 -->
      <!-- 选择网络 -->
      <!-- 提交数据 -->
      <!-- 查看数据 -->
    </form>
</body>
```

# iframe标签

```html
<body>
<!-- iframe标签：内嵌框架，在一个网页中嵌入另一个网页 -->
    <iframe src="https://www.163.com" frameborder="0" 
    width="100%" height="500px"></iframe>
    <!-- 内嵌页面url，边框0 -->
    <!-- 宽度100%，高度500像素 -->
</body>
```
