## 操作场景

Tencent Serverless Toolkit for VS Code 是腾讯云 Serverless 产品的 VS Code（Visual Studio Code）IDE 的插件。该插件可以让您更好的在本地进行 Serverless 项目开发和代码调试，并且轻松将项目部署到云端。

通过该 VS Code 插件，您可以：

- 拉取云端的云函数列表，并触发云函数。
- 在本地快速创建云函数项目。
- 使用模拟的 COS、CMQ、CKafka、API 网关等触发器事件来触发函数运行。
- 上传函数代码到云端，更新函数配置。
- 在云端运行、调试函数代码。

## 前提条件
Tencent Serverless 可在 Windows， MacOS 中安装。在安装 Tencent Serverless 之前，需要确保系统中已有以下组件/信息：
- 已注册腾讯云帐户。若未注册腾讯云账户，可 [点此](https://cloud.tencent.com/register) 进入注册页面。
- VS Code ：在 [VS Code下载页面](https://code.visualstudio.com/) 下载对应的 IDE 并安装，其**版本要求为 v1.33.0 +**。


## 操作步骤

### 安装插件
可通过以下两种方式安装 SCF VS Code 插件：  

**a. 通过插件市场直接安装**
进入 [插件市场](https://marketplace.visualstudio.com/items?itemName=tencentcloud.tencent-cloud-vscode-toolkit) 单击【install】进行安装。

**b. 通过 VS Code IDE 安装**
1. 运行 VS Code IDE。
2. 打开 VS Code 插件市场。
3. 在搜索框中输入 “Tencent Serverless”，单击搜索框下方列表中的 Tencent Serverless 插件查看详情并选择【install】。如下图所示：      
  ![](https://main.qcloudimg.com/raw/4d629d80bb03d4957213af44a4fb524c.png)    
  安装完成后，左侧栏中会展示已安装完毕的 Tencent Serverless 插件。


### 配置插件


1. 单击左侧导航栏的<img src="https://main.qcloudimg.com/raw/4395057dfb3a8f4a92c90ba7dff9b1c1.png" style="margin:-3px 0;">，打开已安装好的 Tencent Serverless 插件。
2. 单击创建一个腾讯云用户凭证。如下图所示：  
![](https://main.qcloudimg.com/raw/fca11ef6e54287f2ad400d34123872c9.png)
3. 根据提示依次输入账号的 APPID，SecretId 及 SecretKey 信息，作为插件调用云 API 时的认证信息。并在认证成功后，选择您希望部署函数的地域。配置信息获取途径如下：
 - 账号的 APPID：通过访问控制台中的 [账号信息](https://console.cloud.tencent.com/developer)，可以查看您的 APPID。
 - SecretId 及 SecretKey：通过访问控制台中的 [API 密钥管理](https://console.cloud.tencent.com/cam/capi)，获取相关密钥或创建相关密钥。
 - 地域：地域列表及对应的英文写法可 [点此](https://cloud.tencent.com/document/product/213/6091#.E4.B8.AD.E5.9B.BD.E5.A4.A7.E9.99.86.E5.8C.BA.E5.9F.9F) 参阅。

4. 为提升函数上传效率，您可以在 VS Code 中 [设置开启 COS 上传](#openCOS) 。


### 创建函数

1. 单击已配置账户函数列表上方的<img src="https://main.qcloudimg.com/raw/799da4ba342886d96d6a3e10681a2560.png" style="margin:-3px 0;">，在本地初始化新的函数项目。
2. 根据提示依次选择函数运行时 Runtime，并输入函数名。
3. 函数信息录入成功后，将开始创建。
4. 函数创建成功后，会跳转到工作区打开函数的入口文件。
5. 将 `index.py` 中的代码替换为如下内容：   
```
   import sys
   import logging
   print('Loading function')
   logger = logging.getLogger()
   def main_handler(event, context):
       logger.info("start main handler")
       print(event)
       body = 'API GW Test Success'
       response = {
           "isBase64": False,
           "statusCode": 200,
           "headers": {"Content-Type": "text",    "Access-Control-Allow-Origin": "*"},
           "body": body
       }
       return response
```


### 部署函数（含配置触发器）


> 完成函数代码的编辑后步骤后，可通过插件将函数一键部署到云端。

1. 单击左侧列表中的函数名称，可以进入该函数基本信息页面，查看函数信息。
![](https://main.qcloudimg.com/raw/0b830929feab34d609b00f59806a48b9.png)
2. 单击列表中函数名称右侧的【上传到云端】按钮，等待函数上传完毕。在这个过程中，可以通过 OUTPUT 输出窗口，看到函数的部署过程中的相关信息。
![](https://main.qcloudimg.com/raw/a597768d791603180f828deb1d7c197d.png)
3. 部署完成后，可在 【云端函数】 列表页中查看到函数的相关信息。您也可以登录 [云函数控制台](https://console.cloud.tencent.com/scf)，到相应地域查看已成功部署的函数。

>- 使用 COS 部署函数最高能提升80%的速率，大大提高了工作效率。但在部署频次、部署包很大时，可能会产生 COS 计费。
>- 您可以在 VS Code 中 [设置开启 COS 上传](#openCOS) 。


### 云端测试

单击左侧列表右侧的<img src="https://main.qcloudimg.com/raw/fef0ef2e04f094c5b3a390e6d78672c0.png" style="margin:-3px 0;">，即可在页面中查看到函数在云端运行的相关信息。如下图所示：    
![](https://main.qcloudimg.com/raw/2c7fb7f915028f4187265af6aef44aaf.png)



### 更多功能

## 常见问题  

安装或使用过程中有遇到问题，可参考 [SCF 工具类常见问题](https://cloud.tencent.com/document/product/583/33456) 解决，您也可以通过 [提交issue](https://github.com/tencentyun/tencent-cloud-vscode-toolkit/issues/new) 进行问题的交流和沟通。    

## 相关操作

<span id="openCOS"></span>
### 设置开启 COS 上传
1. 选择左下角的<img src="https://main.qcloudimg.com/raw/20fd46098cf037eb003dc41f1f913313.png" style="margin:-3px 0px;"/> >【Settings】。如下图所示：
    ![](https://main.qcloudimg.com/raw/e9e1f63819d29d86d8f9cae9cbb9e31a.png)
2. 在“Settings”页面，选择【Extensions】>【Scf】并勾选【Enable deployed by COS】。如下图所示：
    ![](https://main.qcloudimg.com/raw/05ca88747213e5a102747683dc20233a.png)



## 欢迎交流<span id="welcome"></span>
如果您对 Tencent Serverless 感兴趣，您可以加入QQ群（537539545）与我们交流。    
![](https://main.qcloudimg.com/raw/bc881547d1cd2043ecf1b286c70f7319.png)