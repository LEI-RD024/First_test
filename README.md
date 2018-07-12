# 数据模型化(RModel)说明文档
## 一.简介
RModel是一个数据库,OData,WebService数据添加,修改,删除,变量映射,缓存序列化框架。
## 二.安装
### A.主程序安装
将 Models.dll 文件置于 asp.net 站点根目录Bin中再将Models.config放入根目录。
### B.Web API配置
新建Gateway.aspx 加载文件引入<%@ Page Language="C#"  ValidateRequest="false"   Debug="true"   Inherits="MrTe.Models.Gateway" %>
### C.RestFull配置
如是.net 2.0 web.config在/configuration/system.web加入<br/>
<httpHandlers>
    <add verb="*"  validate="false" path="/api*" type="MrTe.Models.RestfullApi,Models />
 </httpHandlers>
                                                       

