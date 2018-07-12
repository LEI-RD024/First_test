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
 <Adapter name="mssql">mssql://sa:pwd@localhost/master</Adapter>
 ### Mysql
 <Adapter name="mysql">mysql://root:pwd@localhost/mysql?charset=UTF8</Adapter>


                                                       

