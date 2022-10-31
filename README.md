# 图片上传
## Vue3+ElementPlus+阿里云对象存储OSS
### 获取accessKeyId、accessKeySecret、以及域名可参考[https://blog.csdn.net/lovoo/article/details/118018387](https://blog.csdn.net/lovoo/article/details/118018387)

* 购买阿里云对象存储
* 查看app...key 和ID
* yarn add ali-oss/npm install ali-oss
``` javascript
const client = new OSS({
    accessKeyId: "*************", // 查看你自己的阿里云KEY
    accessKeySecret: "**************", // 查看自己的阿里云KEYSECRET
    bucket: "****", // 你的 OSS bucket 名称
    region: "oss-cn-***", // bucket 所在地址，我的是 华北2 北京
    cname: true, // 开启自定义域名上传
    endpoint: "*********", // 自己的域名
});
```
