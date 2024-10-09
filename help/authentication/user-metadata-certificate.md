---
title: 用于加密的用户元数据证书
description: 用于加密的用户元数据证书
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a2
source-git-commit: 8c5203aa4a897a5e119a9886afc64a1b1556ee4f
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---


# 用于加密的用户元数据证书

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

对于加密的用户元数据Adobe Pass身份验证集成，您需要具有私钥/公钥对。

本文档介绍了生成公钥证书以用于Adobe Pass身份验证的过程。 这里描述的过程使用OpenSSL工具包。

## 证书生成过程演练(#generation)

1. 下载并安装OpenSSL工具包(http://www.openssl.org)。

1. 生成证书签名请求(CSR)：

   * 生成密钥对。  打开命令/终端窗口并运行以下命令：

     ```bash
     openssl genrsa -des3 -out mycompany-license.key 2048
     ```

   * 生成CSR。 在命令行上，运行以下命令：

     ```bash
     openssl req -new -key mycompany-license.key -out mycompany-license.csr -batch
     ```

     系统将提示您输入私钥的密码。

   * 创建私钥和密码的备份副本。 示例CSR：

     ```
     -----BEGIN CERTIFICATE REQUEST-----
     MIIBnTCCAQYCAQAwXTELMAkGA1UEBhMCU0cxETAPBgNVBAoTCE0yQ3J5cHRvMRIw
     EAYDVQQDEwlsb2NhbGhvc3QxJzAlBgkqhkiG9w0BCQEWGGFkbWluQHNlcnZlci5l
     eGFtcGxlLmRvbTCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEAr1nYY1Qrll1r
     uB/FqlCRrr5nvupdIN+3wF7q915tvEQoc74bnu6b8IbbGRMhzdzmvQ4SzFfVEAuM
     MuTHeybPq5th7YDrTNizKKxOBnqE2KYuX9X22A1Kh49soJJFg6kPb9MUgiZBiMlv
     tb7K3CHfgw5WagWnLl8Lb+ccvKZZl+8CAwEAAaAAMA0GCSqGSIb3DQEBBAUAA4GB
     AHpoRp5YS55CZpy+wdigQEwjL/wSluvo+WjtpvP0YoBMJu4VMKeZi405R7o8oEwi
     PdlrrliKNknFmHKIaCKTLRcU59ScA6ADEIWUzqmUzP5Cs6jrSRo3NKfg1bd09D1K
     9rsQkRc9Urv9mRBIsredGnYECNeRaK5R1yzpOowninXC
     -----END CERTIFICATE REQUEST-----
     ```

1. 将CSR发送给证书颁发机构(CA)（例如Verisign）。

1. ca将以.p7b格式（PKCS#7，加密消息语法标准）向您发送证书

1. 部署.p7b证书。 使用私钥将PKCS#7 (.p7b)文件转换为PKCS#12 （ PFX文件、个人信息交换语法标准），并生成PEM文件（连接的证书容器文件）：

   * 将PKCS#7文件转换为临时PEM文件。 在命令行上，运行以下命令：

     ```
     openssl pkcs7 -in mycompany-license.p7b -inform DER -out mycompany-license-temp.pem -outform PEM -print_certs
     ```

   * 将临时PEM文件转换为PFX文件。  在命令行上，运行以下命令：

     ```
     openssl pkcs12 -export -inkey mycompany-license.key -in mycompany-license-temp.pem -out mycompany-license.pfx -passin pass:private_key_password -passout pass:pfx_password
     ```

   * 将临时PEM文件转换为最终PEM文件。 在命令行上，运行以下命令：

     ```
     openssl x509 -in mycompany-license-temp.pem -inform PEM -out mycompany-license.pem -outform PEM
     ```

1. 将最终PEM文件发送到Adobe以进行配置。

   * 最终需要接收PEM文件的人员是分配给您的集成/验证的Adobe支持工程师。 如果您没有直接与该人员合作，则可以从Adobe代表处了解将文件发送给谁。
   * Adobe同时支持主证书和备份证书。 如果您的主证书遭到任何形式的破坏，您可以撤销该证书，然后切换到辅助证书以在您的应用程序中签名请求者ID。 这将确保在生产中实现证书之间的平稳过渡，对客户的影响最小。
   * Adobe收到PEM文件后，身份验证工程师会将其添加到服务器端配置中，并确认收到该文件。
