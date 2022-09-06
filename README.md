# Solution-Odoo-Graviton2


Odoo-Graviton2将通过跨境电商独立站解决方案进行示范，帮助使用者了解`电商独立站`、`企业ERP`、`在线培训平台`、`CRM平台`、`企业web门户`等高可用弹性三层架构适配的解决方案架构推荐组件

## demo site
https://www.awspsa.com/ 

## 部署手册
按照下述部署手册，我们会将一些流行技术运用到您的模拟项目中，包括容器编排技术、缓存技术、数据库高可用技术、压力测试技术等。如果您在这些方面有一定基础或学习热情，相信您会获得收获。如果您是更侧重业务的架构师，您也将通过简单的配置、点击操作体会技术架构的重要性，并轻松的亲手实现出业务信息化轮廓，期望能为您的业务方向带来灵感。

![Image](/Architecture.png)


### 架构架构特点
快速交付稳定、可靠、弹性、低成本的优良架构准生产系统。


- 所有实例节点基于Graviton2提供超高性价比

- EKS+HAP+CA+ELB作为弹性应用层

- Aurora postgreSQL多副本的数据库层Cancel changes

- ElasticCache Redis存储session和缓存

- EFS和S3实现共享存储

- 使用Cloudformation 向 configmap传递参数实现多样化一键部署

- 可选Glue Athena等数据分析workload连接

- 可选ACM ELB ssl加密和卸载

- 可选使用Spot实例运行EKS WorkNode




### 预期费用

您需要在您的AWS账户中运行此动手训练营时所使用的AWS服务的成本支付费用。截至发布之日，按计划中的实验基准成本应为：

* S3 ：< 0.1 $
* EC2：< 5 $
* EKS：< 1 $
* RDS < 1 $
* ElasticCache < 1 $
* EFS < 0.1 $
* ELB < 1 $
* VPC < 1 $

{{% notice tip %}}
成本管理标签：我们建议您无论何时创建云资源，都对其进行标记。请您尝试在实验期间为实验资源设置统一的标记字段，例如项目：awschinaworkshop
{{% /notice %}}


***使用 cloudformation 部署Odoo E-commerce on Amazon Graviton2***
*   以下指导文档 我们将通过cloudformation或手动搭建我们的测试环境，环境将使用流行的开源ERP软件`Odoo`作为应用端示范，她将运行在`AWS EKS`环境中，`RDS postgreSQL`作为默认的数据库层，`ElasticCache Redis`存储session和缓存,`EFS`和`S3`实现共享存储。以上架构在企业实际应用中常常被作为典型的应用负载框架广泛部署。兼顾弹性，稳定性，易用性，易维护性，并且所有实例节点基于Graviton2提供超高性价比

### 资源清单

我们将使用cloudformation模板配置以下资源：

1. Amazon S3存储桶，用于保存ERP或电商独立站静态数据数据。
2. IAM角色，用于管理整套环境服务权限的IAM角色，以访问和供应其他AWS资源
3. 新建VPC，包含两个公有子网、两个私有子网，Internet网关和安全组以对外提供服务并支持安全访问控制。
4. 一台堡垒机EC2实例，创建将被放置在公有子网，对下游组件它将向架构前后端进行配置管理，对公网您可以配置只有您的本地IP地址可以与之通讯
5. EKS 集群和工作组，工作组将被放置在私有子网，运行Odoo模块化容器应用
6. 您可以自定义Aurora postgreSQL或RDS postgreSQL的多副本不同高可用级别和性能级别数据库层
7. 您可以自定义不同高可用级别和性能级别的ElasticCache Redis存储会话和缓存数据
8. 新建EFS共享存储，将共享存储Odoo应用中的静态文件。

## 实验选项:
如果您是一`个人使用自己的AWS账户`或是`使用PSA导师发送的AWS Event Engine账号`进行实验，建议您使用`cloudformation`启动环境。而如果您是`多人使用同一账户的不同IAM User`进行实验，建议您`手动搭建环境`进行实验，并在必要时明确区别您和他人的资源命名，例如在任何资源名后加入`-user1`。
  - [cloudformation搭建环境](#使用cloudformation搭建环境)




### 使用cloudformation搭建环境

1.使用有效凭证登录AWS控制台

2.选择AWS区域。 此实验室的首选区域是`us-east-1`。

* 您可以点击以下的 Launch Stack ，这样将直接跳转到首选区域的堆栈创建页面

| 账户所属 | 实验模板 |
|:-------------:|:-------------:|
|    海外区域账户  | [![](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=Graviton2PartnerWorkshop&templateURL=https://awspsa-quickstart.s3.amazonaws.com/awspsa-odoo/templates/odoo-master.template) |


3.若您选择自定义的区域后，请导航到cloudformation服务控制台。

![Image](https://s3.amazonaws.com/graviton2.awspsa.com/images/cloudformation-001.png)
4.填写对应的堆栈URL，单击“创建堆栈”。

5.单击“启动堆栈”按钮以启动堆栈，然后单击“下一步”：

![Image](https://s3.amazonaws.com/graviton2.awspsa.com/images/cloudformation-002.png) 

6.你可以自定义指定为StackName，然后单击“下一步”。
![Image](https://s3.amazonaws.com/graviton2.awspsa.com/images/cloudformation-003.png)
7.填写`AZ`信息，选择您`Key pair name`的单击“下一步”。
![Image](https://s3.amazonaws.com/graviton2.awspsa.com/images/cloudformation-004.jpeg)
您也可以在其他的选项中配置您需要的架构，例如：`postgreSQL、redis、堡垒机、EKS的实例类型`。`RDS的自动备份策略`、`是否多AZ部署`、`EKS集群的节点数量`，`Odoo的管理员密码`，等等。

8.确认使用自定义名称创建IAM资源的权限，然后单击“创建”。


![Image](https://s3.amazonaws.com/graviton2.awspsa.com/images/cloudformation-005.png)
9.等待cloudformation供应所有资源。 大约需要35分钟才能完成执行


10.cloudformation部署成功，包含8个内嵌stack，主stack为`Graviton2PartnerWorkshop`

![Image](https://s3.amazonaws.com/graviton2.awspsa.com/images/cloudformation-done.png)

11.点击`Graviton2PartnerWorkshop`进入output，即可查看您Odoo应用的URL。

![Image](https://s3.amazonaws.com/graviton2.awspsa.com/images/cloudformation-output.jpeg)




{{% notice info %}}
如果您遇到`Graviton2PartnerWorkshop`已存在的报错，可能是您在之前的使用中已经自动创建了这个模板并且未完成删除，请您替换模板名称或是导航至cloudformation控制台完成删除并重新创建。
{{% /notice  %}}

### 登录OpenERP后台管理系统

在您的任意浏览器输出访问output的URL，这是一个ELB负载均衡器，后面连接至EKS 集群内的各个odoo Pod的地址，例如`http://ae43138847e0343bda0586735063757c-223344533.us-east-1.elb.amazonaws.com/`

输入您的自定义密码，如果cloudformation填写参数页面您没有进行改动，默认用户名密码为`admin/admin`
![Image](https://s3.amazonaws.com/graviton2.awspsa.com/images/odoo-登录.png)
ps.初次登陆时会触发应用在数据库建表，这个过程大概需要30秒等待

登录后您可以看见odoo流行的企业应用列表。除了这些，odoo拥有活跃的开源社区，提供上千个成熟的企业级应用，并将这些应用封装为模块化插件，您只需点击任意模块的`install`即可进行模块安装，并主动跳转到您登陆权限对应的该应用后台管理界面。有关更多关于Odoo的信息，请您查阅 [odoo_user_doc](https://www.odoo.com/documentation/user/14.0/)

{{% notice info %}}
Odoo is a suite of web based open source business apps.
The main Odoo Apps include an Open Source CRM, Website Builder, eCommerce, Warehouse Management, Project Management, Billing & Accounting, Point of Sale, Human Resources, Marketing, Manufacturing, ...
Odoo Apps can be used as stand-alone applications, but they also integrate seamlessly so you get a full-featured Open Source ERP when you install several Apps.
{{% /notice  %}}

![Image](https://s3.amazonaws.com/graviton2.awspsa.com/images/odoo-app-list.png)

现在我们点击`website`---》`install`，稍等30秒左右将会完成模块安装，并跳转到主题选择页面

![Image](https://s3.amazonaws.com/graviton2.awspsa.com/images/install-website.png)

进入到website编辑页面，您可以通过简单的拉拽完成自定义门户网站搭建，使用体验类似日常的办公文档编辑软件，包括双击替换图片、调整字体、替换视频URL等。当然同时您也可以直接在这个控制台浏览器窗口修改或编写html代码，以完成您需要的其他功能

![Image](https://s3.amazonaws.com/graviton2.awspsa.com/images/odoo编辑网页.png)

点击左上角的主图标，您可以选择其他项目，例如点击`settings`设置系统为中文，点击`apps`安装其他应用，或许您现在就可以在10分钟内完成一个在线教育平台的demo。

![Image](https://s3.amazonaws.com/graviton2.awspsa.com/images/odoo安装其他应用.jpeg)


----
### (可选)配置堡垒机之外的Role获得EKS集群控制权
Cloudformation帮助我们创建了EKS集群等资源，这些资源默认只能在内网进行通讯。我们仅能通过ssh至堡垒机从而对这些资源进行后台管理。这也是我们安全性最佳实践的原则。

如果您出于对实验便利的需要，想从控制台界面操作EKS集群。你需要为EKS集群的`configmap`进行编辑，以添加您当前使用的除了堡垒机role之外的用户访问EKS的授权。要完成这个操作您可以进行如下配置：


1，ssh至您的堡垒机,例如（请把`c2-54-176-92-104.us-east-1.compute.amazonaws.com`替换成你自己的堡垒机地址，`ee-default-keypair.pem`是你在创建Cloudformation时选择的的Key,如果你不是使用默认模板做的选择，请替换为你自己的Key name。通过eventengine做实验的同事请在`https://dashboard.eventengine.run/dashboard`页面下载ee-default-keypair.pem）
```
ssh -i "ee-default-keypair.pem" ec2-user@ec2-54-176-92-104.us-east-1.compute.amazonaws.com
```

若您成功登录，您应该是第一次登陆到堡垒机，现在我们需要通过aws eks命令将堡垒机的kubeconfig文件进行更新，使得堡垒机可以管控EKS集群
```
aws eks --region us-east-1 update-kubeconfig --name odoo  
```


由于EKS集群由堡垒机创建，堡垒机默认就有EKS集群的管理权限，您可以通过`kubectl get pods -n odoo` 命令，查看我们已经通过脚本部署的2个odoo应用。
```
kubectl get pods -n odoo
NAME                        READY   STATUS    RESTARTS   AGE
odoo-arm-85b78fbb5d-62bw6   1/1     Running   0          9d
odoo-arm-85b78fbb5d-6wzq6   1/1     Running   0          9d

```

2，在堡垒机运行以下命令，编辑configmap以为用户添加权限，请将`${ACCOUNT_ID}`替换为您的账号，将`${AWS_REGION}`替换为您实验的region

```
eksctl create iamidentitymapping --cluster odoo \
--arn arn:aws:iam::${ACCOUNT_ID}:role/TeamRole \
--group system:masters \
--username admin \
--region ${AWS_REGION}
```
编辑完成后您可以通过控制台查看您的集群

![Image](https://s3.amazonaws.com/graviton2.awspsa.com/images/EKS控制台.png)

如果您还需要通过你自己的笔记本管理集群，你需要在本地配置了相应的aksk，完成后可以在本地运行`kubectl get nodes`，进行测试

1，在您的本地终端安装 AWS CLI，配置用户的AKSK和默认region，如果您没有配置过AWS CLI您可以参考[AWS CLI安装](https://docs.aws.amazon.com/zh_cn/cli/latest/userguide/install-cliv2.html)完成安装和配置CLI工作。如果您之前安装过CLI，您只需要更换AKSK为您实验的AKSK即可

2，运行以下命令，请将`region`和`name`字段替换为您的环境,例如下面示范的，us-east-1 和 odoo(如何您使用默认参数)
```
aws eks --region us-east-1 update-kubeconfig --name odoo  
```
3，运行以下命令以获取并记录您的 `IAM ARN `和`username`
```
aws sts get-caller-identity

{
    "UserId": "AIDAT2RQT5XMFX7535ZCV",
    "Account": "xxxxxxxx",
    "Arn": "arn:aws:iam::xxxxxxxxx:user/jiade"
}
(END)

```
4，在您的本地执行以下命令测试您配置的本地管理
```
kubectl get nodes

NAME                                        STATUS   ROLES    AGE   VERSION
ip-10-0-28-210.us-east-1.compute.internal   Ready    <none>   9d    v1.18.9-eks-d1db3c
ip-10-0-45-169.us-east-1.compute.internal   Ready    <none>   9d    v1.18.9-eks-d1db3c
```
