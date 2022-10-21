##### 实现一个简单的弹框界面

```
<template>
    <div class="modalMask" v-if="open">
        <div class="modal">
            <h3><strong>修改项目配置</strong></h3>
            <form :model="form">
                <div class="formItem">
                    <div style="flex: 1;">
                        <span>标题</span>
                        <input v-model.trim="form.title" placeholder="请输入项目标题" />
                    </div>
                    <div style="flex:1;">
                        <span>项目id</span>
                        <input v-model.trim="form.projectID" placeholder="请输入项目id" />
                    </div>
                </div>
                <div class="formItem">
                    <div style="flex: 1;">
                        <span>省份</span>
                        <input v-model.trim="form.province" placeholder="请输入省份" />
                    </div>
                    <div style="flex:1;">
                        <span>城市</span>
                        <input v-model.trim="form.city" placeholder="请输入项目城市" />
                    </div>
                </div>
                <div class="formItem">
                    <div style="flex: 1;">
                        <span>服务器ip</span>
                        <input v-model.trim="form.url" placeholder="请输入服务器ip" />
                    </div>
                    <div style="flex:1;">
                        <span>用户名</span>
                        <input v-model.trim="form.u" placeholder="请输入用户名" />
                    </div>
                </div>

                <div class="formItem">
                    <div style="flex: 1;">
                        <span>用户密码</span>
                        <input type="password" v-model.trim="form.p" placeholder="请输入用户密码" />
                    </div>
                    <div style="flex:1;">
                        <span>banner类型</span>
                        <select v-model.trim="form.type" placeholder="请选择banner类型">
                            <option v-for="bannerType in bannerTypes" :key="bannerType.id" :label="bannerType.name"
                                :value="bannerType.id">
                            </option>
                        </select>
                    </div>
                </div>
                <div class="formItem">
                    <div style="flex: 1;">
                        <span>banner名字</span>
                        <input v-model.trim="form.bannerName" placeholder="请输入banner名字" />
                    </div>
                </div>
            </form>
            <div class="bottom">
                <button class="button1" @click="submitForm">确 定</button>
                <button class="button2" @click="cancel">取 消</button>
            </div>
        </div>
    </div>
</template>
<script setup lang="ts">
import { watch, ref } from 'vue'

const form = ref()
const props = defineProps({
    open: {
        type: Boolean,
        default: false
    },
})

const bannerTypes = [
    {
        id: 'image',
        name: "image"
    }, {
        id: 'video',
        name: "video"
    }
];

watch(() => props.open, (newValue, oldValue) => {
    if (newValue) {
        const BaseConfig = JSON.parse(localStorage.getItem('projectConfig') || "");
        form.value = {
            title: BaseConfig.title,
            weatherUrl: BaseConfig.lives.weatherUrl,
            province: BaseConfig.lives.province,
            city: BaseConfig.lives.city,
            projectID: BaseConfig.projectID,
            url: BaseConfig.url,
            u: BaseConfig.user.u,
            p: BaseConfig.user.p,
            type: BaseConfig.banner.type,
            bannerName: BaseConfig.banner.bannerName
        }
    }
})

const emits = defineEmits(["dismiss"])
const cancel: any = () => {
    emits("dismiss");
};

const submitForm: any = async () => {
    const { title, weatherUrl, province, city, projectID, url, u, p, type, bannerName } = form.value;
    if (!title || !weatherUrl || !province || !city || !projectID || !url || !u || !p || !type || !bannerName) {
        alert("所有项均为必填！请检查后提交！")
    } else {
        const data = {
            title: title,
            lives: {
                weatherUrl: weatherUrl,
                province: province,
                city: city
            },
            projectID: projectID,
            url: url,
            user: {
                u: u,
                p: p,
            },
            banner: {
                type: type,
                bannerName: bannerName
            }
        }
        localStorage.setItem('projectConfig', JSON.stringify(data));
        window.location.reload();
    }
};
</script>
<style scoped>
.button1 {
    border: none;
    text-align: center;
    width: 100px;
    border-radius: 3px;
    color: white;
    background: rgb(13, 104, 224);
    margin: 10px;
    padding: 10px;
}

.button2 {
    border: none;
    border-radius: 3px;
    text-align: center;
    width: 100px;
    color: white;
    background: gray;
    margin: 10px;
    padding: 10px;
}

.formItem {
    display: flex;
    margin-top: 5px;
}

span {
    color: black;
    text-align: center;
    width: 90px;
    padding: 3px;
    margin-right: 5px;
}

input {
    border: 1px solid gray;
    border-radius: 5px;
    width: 150px;
    padding: 10px;
}

select {
    border-radius: 5px;
    border: 1px solid gray;
    width: 150px;
    padding: 10px;
}

option {
    width: 150px;
    padding: 10px;
}

h3 strong {
    font-size: 20px;
    color: black;
}

.modalMask {
    position: fixed;
    top: 10%;
    right: 0;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 100000;
    background: rgba(0, 0, 0, 0.45);
}

.modal {
    padding: 15px 15px 60px 15px;
    border-radius: 8px;
    background: white;
    width: 600px;
    margin: 0 auto;
    color: rgba(0, 0, 0, .65);
    font-size: 14px;
    line-height: 2;
}

.bottom {
    float: right;
}
</style>
```