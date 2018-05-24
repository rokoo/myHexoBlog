---
title: 使用node-forge库实现生成数字证书以及使用证书签名验证的代码示例
catalog: true
date: 2018-05-24 18:50:59
subtitle:
header-img: "/img/header_img/header-bg-railway.jpg"
tags:
- nodejs
- forge
- 数字证书
---
> 使用node-forge库创建数字证书，并使用私钥签名，用证书的公钥验证签名
## 具体实现步骤
### 根据用户名随机生成一对密钥，用公钥生成证书并用私钥签名

```javascript
function generateUserCert(username) {
    // generate a keypair or use one you have already
    var keys = pki.rsa.generateKeyPair(2048);

    // create a new certificate
    var cert = pki.createCertificate();

    // fill the required fields
    cert.publicKey = keys.publicKey;
    cert.serialNumber = '01';
    cert.validity.notBefore = new Date();
    cert.validity.notAfter = new Date();
    cert.validity.notAfter.setFullYear(cert.validity.notBefore.getFullYear() + 1);

    // use your own attributes here, or supply a csr (check the docs)
    var attrs = [];

    // here we set subject and issuer as the same one
    cert.setSubject(attrs);
    cert.setIssuer(attrs);

    // the actual certificate signing
    cert.sign(keys.privateKey);

    // now convert the Forge certificate to PEM format
    var pem = pki.certificateToPem(cert);
    let cerFilePath = "../certifications/" + username + ".cer";
    let privateKeyFilePath = "../certifications/" + username + ".key";
    var privateKey = forge.pki.privateKeyToPem(keys.privateKey);
    return saveFile(cerFilePath, pem).then(saveFile(privateKeyFilePath, privateKey)).then(console.log("all save success"));
}
```
### 将证书和私钥保存在文件中
```javascript
function saveFile(filepath, fileData) {
    return new Promise((resolve, reject) => {
        // 块方式写入文件
        fs.writeFile(filepath, fileData);
    });
}
```
### 用私钥给一段数据签名
```javascript
function signDocWithUserCer(docData, username) {
    var userPrivateKey = getPrivateKeyOfUser(username);
    var privateKeyPem = getPrivateKeyOfUser(username)
    var privateKey = forge.pki.privateKeyFromPem(privateKeyPem);
    var md = forge.md.sha1.create();
    md.update(docData, 'utf8');
    var signature = privateKey.sign(md);
    return signature;
}
```
### 用证书中的公钥调用verify函数验证，可以解开即证明该公钥是该用户提供。

```javascript
function verifySignature(docData, username, signature) {
    var priateKeyPem = getPemCertOfUser(username);
    var certfrompem = forge.pki.certificateFromPem(priateKeyPem);
    var md = forge.md.sha1.create();
    md.update(docData, 'utf8');
    try {
        return certfrompem.publicKey.verify(md.digest().bytes(), signature);
    } catch (e) {
        return false;
    }
}
```