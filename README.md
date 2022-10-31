# 图片上传
## Vue3+ElementPlus+阿里云对象存储OSS
### 获取accessKeyId、accessKeySecret、以及域名可参考[https://blog.csdn.net/lovoo/article/details/118018387](https://blog.csdn.net/lovoo/article/details/118018387)

* 购买阿里云对象存储
* 查看app...key 和ID
* yarn add ali-oss/npm install ali-oss
``` template
<el-upload
        action="#"
        list-type="picture-card"
        :auto-upload="true"
        :show-file-list="true"
        :http-request="fnUploadRequest"
        :before-upload="beforeAvatarUpload"
    >
        <el-icon>
            <Plus />
        </el-icon>

        <template #file="{ file }">
            <div>
                <img class="el-upload-list__item-thumbnail" :src="file.url" alt="" />
                <span class="el-upload-list__item-actions">
                    <span class="el-upload-list__item-preview">
                        <el-icon>
                            <zoom-in />
                        </el-icon>
                    </span>
                </span>
            </div>
        </template>
    </el-upload>
```

``` JavaScript
import { ref, reactive, getCurrentInstance } from "vue";
import { ElMessage } from "element-plus";
import OSS from "ali-oss";
const client = new OSS({
    accessKeyId: "**", // 查看你自己的阿里云KEY
    accessKeySecret: "******", // 查看自己的阿里云KEYSECRET
    bucket: "****", // 你的 OSS bucket 名称
    region: "****", // bucket 所在地址，我的是 华北2 北京
    cname: true, // 开启自定义域名上传
    endpoint: "****", // 自己的域名
});
// 上传图片
// 检测上传图片的格式
const beforeAvatarUpload = file => {
    console.log(file);
    const isJPEG = file.name.split(".")[1] === "jpeg";
    const isJPG = file.name.split(".")[1] === "jpg";
    const isPNG = file.name.split(".")[1] === "png";
    const isWEBP = file.name.split(".")[1] === "webp";
    const isGIF = file.name.split(".")[1] === "gif";
    const isLt500K = file.size / 1024 / 1024 / 1024 / 1024 < 4;
    if (!isJPG && !isJPEG && !isPNG && !isWEBP && !isGIF) {
        ElMessage.error("上传图片只能是 JPEG/JPG/PNG 格式!");
    }
    if (!isLt500K) {
        ElMessage.error("单张图片大小不能超过 4mb!");
    }
    return (isJPEG || isJPG || isPNG || isWEBP || isGIF) && isLt500K;
};
// 上传图片返回链接
const fnUploadRequest = async options => {
    try {
        let file = options.file; // 拿到 file
        let fileName = file.name;
        let date = new Date().getTime();
        let fileNames = `${date}_${fileName}`; // 拼接文件名，保证唯一，这里使用时间戳+原文件名
        // 上传文件,这里是上传到OSS的 uploads文件夹下
        client.put("img/" + fileNames, file).then(res => {
            if (res.res.statusCode === 200) {
                // 把返回的链接赋值给本地变量
            } else {
                ElMessage.error("删除失败");
            }
        });
    } catch (e) {
        ElMessage.error("删除失败");
    }
};
```
