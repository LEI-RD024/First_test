# 数据模型化(RModel)说明文档
## 一.简介
RModel是一个数据库,OData,WebService数据添加,修改,删除,变量映射,缓存序列化框架。
## 二.安装
### A.主程序安装
将 Models.dll 文件置于 asp.net 站点根目录Bin中再将Models.config放入根目录。
### B.Web API配置
新建Gateway.aspx 加载文件引入<%@ Page Language="C#"  ValidateRequest="false"   Debug="true"   Inherits="MrTe.Models.Gateway" %>
### C.RestFull配置
如是.net 2.0 web.config请在/configuration/system.web加入<br/>
```C#
<httpHandlers>
    <add verb="*"  validate="false" path="/api*" type="MrTe.Models.RestfullApi,Models />
</httpHandlers>
```
如是.net 4.0请在/configuration/加入<br/>
```C#
<system.webServer>
   <modules>
     <remove name="WebDAVModule" />
   </modules>
   <handlers>
     <remove name="WebDAV" /> 
     <add name="RestfullApi" verb="*" path="/Core/api*"  type="MrTe.Models.RestfullApi,App_Code" preCondition="integratedMode" />
   </handlers>
   <validation validateIntegratedModeConfiguration="false" />
 </system.webServer>
 ```
 最后将Routes.config加入到你的站点根目录下。
 ## 三.XML配置(Models.config)
 ### A.Config根节点
 配置文件根节点
 ### B.Adapter适配器
 属性 name 为所用 适配器名称 default为Command默认连接；<br/>
 节点内容为连接操作数据库URI
 #### Sqlserver
 ```C#
 <Adapter name="mssql">mssql://sa:pwd@localhost/master</Adapter>
 ```
 #### Mysql
 ```C#
 <Adapter name="mysql">mysql://root:pwd@localhost/mysql?charset=UTF8</Adapter>
 ```
 #### Postgresql
 ```C#
 <Adapter name="pgsql">pgsql://postgres:pwd@localhost/postgres?charset=UTF8</Adapter>
 ```                                                      
 #### Oracle
 ```C#
 <Adapter name="oracle">ora://system:pwd@localhost:1521/Sample</Adapter>
 ```
 #### Sybase
 ```C#
 <Adapter name="sybase">ase//sa:pwd@localhost/master</Adapter>
 ```
 #### DB2
 ```C#
 <Adapter name="db2">db2://db2admin:pwd@localhost/Sample</Adapter>
 ```
 #### Http
 ```C#
 <Adapter name="odata">http//auth:pwd@localhost/master</Adapter>
 ```
 ### C.CFG配置变量
 定义配置变量表的前缀
 ```C#
 <CFG name="TBPRE">dc_</CFG>
 ```
 ### D.General常规设置
 属性:primarykey 主键关键字,createdkey创建时间关键字,modifyedkey:修改时间关键字。
 ### E.Command操作指令
 属性:name 调用名,type 操作类型(getRows,getRow,getResult,setRows,Remove)<br/>
 primarykey 主键关键字,createdkey创建时间关键字,modifyedkey:修改时间关键字<br/>
 Command 子节点 map使用<br/>
 在getRows中为将 Command 连接到父节点内容中<br/>
 在setRows中map为将 name名为JsonData值中<br/>
 getRows append="true"为所有内容并行加入<br/>
 getRows,setRows redirect 重定Command 位置<br/>
 #### Command/Verify检验
 属性:name在基本操作所对用的名中(Command[@name="Base"])<br/>
 检验表达式内容为JSON对象
 ```JSON
 {
  Id:{title:"ID标识",patterns:[["Required","不能为空"],["/[0-9a-f\-]{16,36}/i","必需为16个字符"]]},
  Modifyed:{title:"修改时间",patterns:[["Required","不能为空"],["/[0-9]{4,4}\-[0-9]{1,2}\-[0-9]{1,2}/","格式有误"]]},
  Mobile:{title:"手机",patterns:[["Required","不能为空"],["not this.Checked('Unique.Members_Mobile',$jvars)","已注册"]]},
  SMSCode:{title:"手机验证码",patterns:[["Required","不能为空"],
  ["this.Condition(\"'{ROOT.Session.SMSCode}'=='{SMSCode}'\")","无效"]
  ]},
  Password:{title:"密码",patterns:[["Required","不能为空"]]},
  RePassword:{title:"确认密码",patterns:[["Required","不能为空"],["this.Confirm($value,$jvars,'Password')","两次密码输入不符"]]},
  RecommendCode:{title:"邀请码",patterns:[["this.Checked('Unique.Members_RecommendCode',$jvars)","无效"]]}
 }
 ```
 JSON键为输入JSON键title为键标题patterns判定部分 Required空值判定 判定部分第一个值为正则表达式时按正则判定.如果第一个值有圆括号为判段函数。<br/>
 系统内置函数<br/>
 this.Checked 可以从Models.conf获取内容如果为空则返回false<br/>
 ["this.Checked('ref getRow:App.Member_Property_Check_{Type}',$jvars)","验证失败"]<br/>
 Ref 将Checked获取内容载入 输入JsonData中<br/>
 this.Condition 条件表达式判定
 #### Command/Field字段(setRows专用)

