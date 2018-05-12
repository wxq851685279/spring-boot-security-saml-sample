Spring Boot Sample SAML 2.0 Service Provider
====================
#### 注册SSOCircle

* 访问[https://www.ssocircle.com](https://www.ssocircle.com)网站注册一个SSOCircle账号<br/>
* 点击Sign in/Register菜单下的Register<br/>
* 填写账号信息，进行注册，邮箱不能使用qq邮箱<br/>
* 注册成功后会收到一条激活邮件，点击收到的链接进行激活<br/>

#### 启动项目

* 修改WebSecurityConfig文件的metadataGenerator()方法的enittyId，确保是唯一的<br/>
* 运行Application以main方法方式启动<br/>
* 运行时如果出现以下异常，需要更新SSOCircle证书<br/> 
`javax.net.ssl.SSLPeerUnverifiedException: SSL peer failed hostname validation for name: null`<br/>
* 切换到cd src/main/resources/saml 目录下，执行 ./update-certifcate.sh脚本进行更新<br/>
* 访问[http://localhost:8080/saml/metadata](http://localhost:8080/saml/medadata)下载SP metadata<br/>
* 访问[https://www.ssocircle.com](https://www.ssocircle.com)网站，进行登陆<br/>
* 点击Manage Metadata ==> Add new Service Provider添加SP metadata到IDP<br/>
* FQDN 填写entityId，以及把刚才下载的下载SP metadata文件中内容复制进Insert the SAML Metadata information of your SP中<br/>
* 点击Submit提交，注意，entityId必须在SSOCircle中是唯一的<br/>
* 访问[http://localhost:8080](http://localhost:8080)点击SSO Login Page<br/>
* 选择idp进行登陆，会跳转到SSOCircle进行登陆<br/>
* 登陆后会跳转到SAML Consent Page页面，此页面需要翻墙，不然显示不出谷歌的验证码<br/>

#### SAML SSO流程
1. 当用户访问一个受保护的SP资源时，会检测是否被认证<br/>
2. 如果发现用户是未认证的，会被重定向到IDP，要求用户进行身份鉴别<br/>
3. IDP对用户身份进行认证<br/>
4. 认证通过后，会带着SAML artifact被重定向到SP，SAML artifact是认证的标识<br/>
5. 当收到SAML artifact后，SP会发送会IDP，IDP根据SAML artifact找到认证信息，最后通过SAML Artifact Response发送给SP<br/>