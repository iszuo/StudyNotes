

# 控制器向视图传值（ViewData、ViewBag、TempData）

1. ViewData
   只在一次HTTP请求中有效，当请求结束后会自动清空其值；
   生命周期短，类似WebForm的Page对象，因此无法跨Action方法传递数据。
2. ViewBag
   ViewBag属性返回的实际类型为Object，此类型允许动态添加成员，因此将返回类型定义为dynamic可以避免编译期的语法检查。
3. TempData
   TempData利用Session保存数据；
   生命周期长，可以跨Action方法传递数据，但读取一次后会自动清除。

```c#
例
// 后端
public ActionResult Index()
{
	// 获取所有管理员
	fashionshoppingDBEntities db = new fashionshoppingDBEntities();
	var list = db.Sysusers.ToList();
	// 将管理员保存到viewbag或viewdata中
	ViewData["sysUsers"] = list;
	return View();
}
// 前端
@foreach (var item in ViewData["sysUsers"] as List<WebApplication1.Models.Sysuser>)
    {
        <tr>
            <td>@item.id</td>
            <td>@item.username</td>
            <td><a href="#">操作</a></td>
        </tr>
    }
// 将数据库中的信息循环输出
```

------



# 跨页面获取_输出数据（get、post)

前端

```html
<div> 
      <form action="Save" method="get（post）">
          <p>
              账号：<input id="uname" type="text" name="uname" value="" />
          </p>
          <p>
              密码：<input id="upwd" type="password" name="upwd" value="" />
          </p>
          <p>
              <input type="submit" name="name" value="提交" />
          </p>
      </form>
</div>
```

后端

```c#
 // 传参数获取数据
 public ActionResult Save(string uname, string upwd)
 {
     ViewBag.name = uname;
     ViewBag.pwd = upwd;
     return View();
 }
// post\get获取数据
// 添加[HttpPost]后只能通过post的形式获取参数
// 添加[HttpGet]后只能通过get的形式获取参数
 public ActionResult Save()
 {
     ViewBag.uname = Request["uname"];
     ViewBag.upwd = Request["upwd"];
     return View();
 }
```

信息显示

```html
<p>传参数显示</p>
<div>
    用户名：<span>@ViewBag.uname</span>
    <br />
    密码：<span>@ViewBag.upwd</span>
</div>
<p>post/get显示</p>
<div>
    用户名：<span>@ViewBag.name</span>
    <br />
    密码：<span>@ViewBag.pwd</span>
</div>
```

------

# 简单数据绑定

- 通过获取前端控件name属性值名进行与后端的绑定。
- 表单控件的name属性和模型对象属性的名称必须相同，否则无法完成绑定。

```c#
// 后端
[HttpPost]
// 简单数据类型绑定
public ActionResult Index(string uname,int? uage)
{
    ViewBag.msg = "姓名：" + uname + "、年龄：" + uage;
    return View();
}
// 注：  int?   表示可空数据类型，若数据类型不符合则默认设置为  null

// 模型绑定，通过类进行绑定
public ActionResult Index(Student stu)
{
    ViewBag.msg = "姓名：" + stu.uname + "、年龄：" + stu.uage;
    return View();
}
```

```html
// 前端
<div class="jumbotron">
    <h1>添加学生信息</h1>
    <form action="" method="post">
        姓名：<input type="text" name="uname" value="" />
        <br />
        年龄：<input type="text" name="uage" value="" />
        <br />
        <input class="btn btn-primary"  type="submit" name="name" value="提交" />
    </form>
    @ViewBag.msg
</div>
```

# 复杂数据类型绑定

1. 场景：表单中存在多个name属性值相同的表单控件，如：checkbox
2. 实现：Action方法中，以表单控件的name属性值命名、List<相应数据类型>声明参数变量

- **通过使用List对内容进行输出**
- 页面1

```c#
// 前端
<h2>邮箱列表</h2>
<form action="Send" method="post">
    @{ int index = 0;}
// 第一种方法
    @foreach (var item in ViewBag.stus as IList<WebApplication1.Models.Student>)
    {
        <input class="form-control" placeholder="姓名" type="text" name="stu[@index].uname" value="@item.uname" />
        <input class="form-control" placeholder="邮箱" type="text" name="stu[@index].email" value="@item.email" /><hr />
        index++;
    }
// 第二种方法
    @*@{ var stu = ViewBag.stus as IList<WebApplication1.Models.Student>; }
    @for (int i = 0; i < stu.Count; i++)
    {
        <input class="form-control" placeholder="姓名" type="text" name="stu[@i].uname" value="@stu[i].uname" />
        <input class="form-control" placeholder="邮箱" type="text" name="stu[@i].email" value="@stu[i].email" /><hr />
    }*@
    <input type="submit" name="name" value="群发" />
</form>

// 后端
public ActionResult email()
{
    var stu = new Student[] { 
        new Student(){uname="小白",email="xb@qq.com"},
        new Student(),
        new Student(),
        new Student(),
        new Student()
    };
    ViewBag.stus = stu;
    return View();
}
```

- 页面2

```c#
// 前端
<h2>发送列表</h2>
<table border="1">
    <tr>
        <th>姓名</th>
        <th>邮箱</th>
    </tr>
    @foreach (var item in ViewBag.stus as IList<WebApplication1.Models.Student>)
    {
        <tr>
            <td>@item.uname</td>
            <td>@item.email</td>
        </tr>
    }
</table>

// 后端
//[HttpPost]
public ActionResult Send(List<Student> stu)
{
    ViewBag.stus = stu;
    return View();
}
```

# 文件上传

1. 上传文件之前需要在站点根目录建立存放上传文件的目录，例如：UploadFile。
2. 为了接收上传的文件对象，需要定义一个控制器方法并接收一个HttpPostedFileBase类型、文件上传控件的name属性值命名的参数
3. form标签的enctype属性值必须为“multipart/form-data”

```c#
// 后端
public ActionResult Upload()
{
    return View();
}
[HttpPost]
public ActionResult Upload(HttpPostedFileBase file)
{
    if (file.FileName != null && file.ContentLength > 0)
    {
        file.SaveAs(Server.MapPath("~/UpLoads/") + file.FileName);
        ViewBag.msg = "上传成功";
    }
    else
    {
        ViewBag.msg = "上传失败";
    }
    return View();
}
// 前端
<h2>上传文件</h2>
<form enctype="multipart/form-data" method="post">
    <input type="file" name="file" value="" />
    <input type="submit" name="uppic" value="上传" />
</form>
@ViewBag.msg
```

# 输出超链接

```c#
@Html.ActionLink("联系我们","Contact","Home",new { id = 1 },new { style = "color:red" })
//链接文字  页面  控制器   参数   css样式
```

# MVC框架输出表单

```c#
Html.BeginForm() 			// 输出<form>标签
Html.CheckBox() 			// 输出<input type="checkbox">标签
Html.DropDownList() 		// 输出<select>标签
Html.Password()				// 输出<input type="password">标签
Html.RadioButton()			//  输出<input type="radio">标签
Html.TextArea()				// 输出<textarea/>标签
Html.TextBox()				// 输出<input type="text">标签
Html.Hidden()				// 输出<input type=”hidden”/>标签

// 例
@using (Html.BeginForm("Index", "Home", "Post"))
{
    <p>姓名： @Html.TextBox("uname1")</p>
    <p>地址： @Html.TextBox("address", null, new { @class = "form-control" })</p>
    <p>密码： @Html.Password("pwd")</p>
    <p>
        性别：男 @Html.RadioButton("sex", "男")
        女 @Html.RadioButton("sex", "女")
    </p>
    <p>
        爱好：
        读书： @Html.CheckBox("hobbit1", true, "read")
        游戏： @Html.CheckBox("hobbit2", new { value = "game" })
        学习： @Html.CheckBox("hobbit3", new { value = "study" })
    </p>
    <p>
        职业：
        @Html.DropDownList("job", new SelectListItem[] {
            new SelectListItem{Text="北京",Value="010"},
            new SelectListItem{Text="北京",Value="011"},
            new SelectListItem{Text="北京",Value="012"}
        })
    </p>
    <p>
        简介
        @Html.TextArea("text")
    </p>
}
```

# 加载分部视图

1. 在需要呈现分部视图的位置使用**Html.Partial("分部视图名")**方法加载分部视图。
2. 分部视图文件通常放置于 ~/Views/Shared目录下，以便复用。

# 使用jquery进行表单验证

## 使用jquery

- 需要先引入jq文件，下面代码中的jq在模板页中引用，没有引入语句

```html
<div>
    <h1>使用jQuery进行表单验证</h1>
    @using (Html.BeginForm())
    {
        <p>
            账号：
            @Html.TextBox("uname", null, new { onblur = "checkName()" })
            <span id="uname_msg"></span>
        </p>
        <p>
            密码：
            @Html.TextBox("pwd", null, new { onblur = "checkPwd()" })
            <span id="pwd_msg"></span>
        </p>
        <p>
            确认密码：
            @Html.TextBox("pwd_2", null, new { onblur = "checkPwd2()" })
            <span id="pwd_2_msg"></span>
        </p>
        <p>
            <input type="submit" name="name" value="提交" />
        </p>
    }
</div>

@section scripts{
    <script>
        $(function () {
            $("form").bind("submit", checkForm)
        })
        function checkForm() {
            return checkName() && checkPwd() && checkPwd2();
        }
        function checkName() {
            if ($("#uname").val().trim().length < 1) {
                $("#uname_msg").text("账号不能为空");
                $("#uname").focus();
                return false;
            } else {
                $("#uname_msg").text("");
                return true;
            }
        }
        function checkPwd() {
            if ($("#pwd").val().trim().length < 1) {
                $("#pwd_msg").text("密码不能为空");
                $("#pwd").focus();
                return false;
            } else {
                $("#pwd_msg").text("");
                return true;
            }
        }
        function checkPwd2() {
            if ($("#pwd_2").val().trim().length < 1) {
                $("#pwd_2_msg").text("账号不能为空");
                $("#pwd_2").focus();
                return false;
            } else if ($("#pwd_2").val().trim() != $("#pwd").val().trim()) {
                $("#pwd_2_msg").text("两次密码不一致");
                $("#pwd_2").focus();
                return false;
            } else {
                $("#pwd_2_msg").text("");
                return true;
            }
        }
    </script>
}
```

## 使用jquery  Validate插件

- 先依次导入

  ```html
  <script src="~/Scripts/jquery-1.8.3.min.js"></script>
  <script src="~/Scripts/jquery.validate.js"></script>
  <script src="~/Scripts/jquery.metadata.js"></script>
  <script src="~/Scripts/jquery.validate.messages_cn.js"></script> //支持中文信息
  ```

- 实际操作

  ```html
  @section scripts{
      @Scripts.Render("~/bundles/jqueryval")
  	<script>
          $("#formstudent").validate({  //注意不是ta,是te结尾
              rules: {
                  login_name: { required: true },
                  login_pwd: { required: true,rangelength:[3,12] }
              },
              messages:{
                  login_name: { required: "账号不能为空"},
                  login_pwd: { required: "密码不能为空",rangelength:"长度3-12位" }
              }
          })
      </script>
  }
  
  ```


# 注解模型

1. 模型中添加注解

   在模型字段前，通过注释，添加验证

   1. [Required(ErrorMessage="错误信息")]
   2. [StringLength(50,ErrorMessage="错误信息")]
   3. [StringLength(12,MinimumLength =3, ErrorMessage = "密码长度3-12位")]
   4. [Range(1,50,ErrorMessage="错误信息")]
   5. [Range(typeof(decimal),1,50,ErrorMessage="错误信息")]
   6. [Compare("表单字段",ErrorMessage="错误信息")]
   7. [RegularExpression("pattern",ErrorMessage="错误信息")]  匹配正则表达式 

   - **设置别名[Display(Name="  ")]**

2. 服务器端控制器视图动作中

   ```
   if ( ModelState.IsValid ) {
   	ModelState.AddModelError(" ","用户名密码不匹配")；
   }
   ```

3. 视图中

   @Html.ValidationSummary( )方法集中显示错误提示信息

   @Html.ValidationMessage("id") 显示相应字段的错误提示信息

例：

```c#
// 先引入命名空间
using System.ComponentModel.DataAnnotations;
// 在类中使用
public class Student
{
    public string id{ get; set; }
    
    [Required(ErrorMessage = "姓名必须填写")]
    [StringLength(50,ErrorMessage = "长度在3-50之间",MinimumLength = 3)]
    public string name{ get; set; }
    
    [Range(1,150,ErrorMessage = "年龄在1~150直接")]
    public string age { get; set; }
    
    [Range(50,250,ErrorMessage = "身高在50-250之间")
    public decimal height { get; set; }
    
    [RegularExpression("^[12][0-9][0-9][0-9][.-/][01][0-9][.-/][0123][0-9]$", ErrorMessage = "邮箱格式不正确")]
    public DateTime birthday { get; set; }
    
    [RegularExpression("^[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(\\.[a-zA-Z0-9_-]+)+$", ErrorMessage = "邮箱格式不正确")]
        public string email { get; set; }
    }
     
// 控制器
[HttpPost]
public ActionResult Contact(Student s)
{
    if (ModelState.IsValid)
    {
        ViewBag.msg = "注册成功";
    }
    return View();
}
     
// 视图
@using (Html.BeginForm("Contact", "Home", FormMethod.Post))
{
    <p>
        姓名：@Html.TextBox("name")
        @Html.ValidationMessage("name")
    </p>
    <p>
        年龄：@Html.TextBox("age")
        @Html.ValidationMessage("age")
    </p>
    <p>
        身高：@Html.TextBox("height")
        @Html.ValidationMessage("height")
    </p>
    <p>
        生日：@Html.TextBox("birthday")
        @Html.ValidationMessage("birthday")
    </p>
    <p>
        邮箱：@Html.TextBox("email")
        @Html.ValidationMessage("email")
    </p>
    <input type="submit" value="注册" />
    @Html.ValidationSummary();
    <br />
    @ViewBag.msg
}
```

# Html辅助类型及强方法

## 普通Html辅助方法

1. @Html.Lable(元素名称，元素值，元素属性)

   ```c#
   @Html.Lable("GenreName","流行",new { @class="validation" })
   // 渲染之后代码如下
   <label class="validation" for="GenreName">流行</label>
   ```

2. @Html.TextBox("元素名称"，元素值，元素格式，元素属性)

   ```c#
   @Html.TextBox("title", "2.5",string.Format("{0:C}",2.5) new { @class = "validation" })
   // 渲染之后源码如下
   <input class="validation" id="title" name="title" type="text" value="¥2.50" />
   ```

3. @Html.Editor(元素名称，模板名称，Id名称，指定模板)

   ```c#
   @Html.Editor("title",Model.Title,"Title",Model)
   // 渲染之后源码如下
   <input class="text-box single-line" id="Title" name="Title" type="text" value="Details" />
   ```

4. @Html.Display(元素名称，模板名称，Id名称，指定模板)

   ```c#
   @Html.Display("title", "", "Title", Model)
   // 渲染之后源码如下
   Details
   // 原因：该辅助方法返回为纯文本。
   ```

## 强类型Html辅助方法

1. @Html.LableFor(for名称，元素值，属性)

   ```c#
   @Html.LabelFor(mode => mode.GenreId, "Genre", new { @class="validation" })
   // 渲染之后源码如下
   <label class="validation" for="GenreId">Genre</label>;
   ```

2. @Html.TextBoxFor("元素名称"，元素值，元素格式，元素属性)

   ```c#
   @Html.TextBoxFor(model => model.Title, string.Format("{0:C}", 2.5), new { @class = "validation" });
   // 渲染之后源码如下
   <input class="validation" id="Title" name="Title" type="text" value="" />;
   ```

3. @Html.Editor(元素名称，模板名称，Id名称，指定模板)

   ```c#
   @Html.EditorFor(model=>model.Price,"","Price",Model)
   // 渲染之后源码如下
   <input class="text-box single-line" data-val="true" data-val-number="The field Price must be a number." data-val-range="Price must be between 0.01 and 100.00" data-val-range-max="100" data-val-range-min="0.01" data-val-required="Price is Required" id="Price" name="Price" type="text" value="1.00" />
   ```

4. @Html.Display(元素名称，模板名称，Id名称，指定模板)

   ```c#
   @Html.DisplayFor(model => model.Title, "", "Title", Model)
   // 渲染之后源码如下
   Details
   ```



# Ajax

**需要提前引入jquery。**

## jquery方法

| 名称                         | 值/描述                               |
| ---------------------------- | ------------------------------------- |
| url                          | 规定发送请求的 URL。默认是当前页面。  |
| type                         | 规定请求的类型（GET 或 POST）。发     |
| jsonp                        | 在一个 jsonp 中重写回调函数的字符串。 |
| jsonpCallback                | 在一个 jsonp 中规定回调函数的名称。   |
| success(*result,status,xhr*) | 当请求成功时运行的函数。              |
| error(*xhr,status,error*)    | 如果请求失败要运行的函数。            |

```javascript
// 例
$("#btn").click(function () {
    $.ajax({
        // 提交地址
        url: "/Home/ValidateUser",
        // 提交类型
        type: "post",
        // 固定写法
        contentType: "application/x-www-form-urlencoded",
        // 前端向后端传递数据，多个参数用“，”分割
        data: {
            "username": $("#username").val()
        },
        // 成功后返回数据（data可以随便声明）
        success: function (data) {
            console.log(data);
            if (data == "true") {
                $("#msg").text("验证通过");
            } else {
                $("#msg").text("验证不通过");
            }
        },
        // 错误后返回数据
        error: function (data) {
            if (data == "true") {
                $("#msg").text("信息有误");
            }
        }
    })
})
```

## Mvc辅助方法

| 名字                   | 返回值类型 | 含义                                                         |
| ---------------------- | ---------- | ------------------------------------------------------------ |
| Confirm                | string     | 在请求之前会弹出一个提示框，是否确认提交                     |
| HttpMethod             | string     | 设置请求类型 Get、Post                                       |
| UpdateTargetId         | String     | 标明html中一元素的id，把请求返回的数据/元素更新到该元素中    |
| InsertionMode          | enum       | 把请求结果以何种方式更新到Dom元素中①Replace②InsertBefore③InsertAfter不设置的情况下，默认是Replace，只有在UpdateTargetId被设置后才有效 |
| LoadingElementId       | string     | 标明html中一元素的id，在请求过程中，该元素会显示出来，请求结束后又隐藏 |
| LoadingElementDuration | Int        | 控制Loading动画在显示/隐藏时的动画持续时间，单位为毫秒；默认情况下，动画将淡入淡出；这个时间即淡入淡出的时间（但经测试无效！！！） |
| OnBegin                | string     | 标明js中一function的名称，在Ajax请求发送前，执行该方法，对应JQuery的beforeSend |
| OnComplete             | String     | 标明js中一function的名称，在请求成功时，执行该方法，对应JQuery的complete |
| OnFailure              | String     | 标明js中一function的名称，在请求失败时，执行该方法，对应JQuery的error |
| OnSuccess              | String     | 标明js中一function的名称，无论请求成功与否，都在请求完成时，执行该方法，对应JQuery的success |
| Url                    | String     | 请求的地址                                                   |
| AllowCache             | Bool       | 是否使用缓存                                                 |

```c#
// 例
@Ajax.ActionLink("ActionLink", "Index", "Home", "https", "www.ayilaile.com:90", "title", new { id = 1, type = 1 }, new AjaxOptions { HttpMethod = "Post", UpdateTargetId = "detailsID", InsertionMode = InsertionMode.Replace }, new{ id = "testid"})
// 生成的链接为：https://www.ayilaile.com:90/Home/Index/1?type=1#title
```



# Api

Http动词

| 操作   | http动词     | 说明     |
| ------ | ------------ | -------- |
| Create | Post         | 新增数据 |
| Read   | Get          | 读取资源 |
| Update | Put（Patch） | 更新资源 |
| Delete | Delete       | 删除资源 |



# ActionResult派生类型

|                    | 作用                         |
| ------------------ | ---------------------------- |
| ViewResult()       | 用于返回标准的视图页面       |
| EmptyResult()      | 不返回任何数据               |
| ContentResult()    | 以字符串的形式指定响应的内容 |
| FileResult()       | 返回任意文档内容             |
| JavaScriptResult() | 响应一段JavaScript脚本内容   |
| view()             | 显示页面                     |
| content()          | 返回文本                     |

