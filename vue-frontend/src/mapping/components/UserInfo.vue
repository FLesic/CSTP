<script setup>
import {ref} from "vue";
import {ArrowDown, ArrowRight, Avatar, Star, SwitchButton, User} from "@element-plus/icons-vue";
import {ElMessage} from "element-plus";
import {useRouter} from "vue-router";


const userInfo = ref({
  user_id: 2,
  user_name: "任飞扬",
  email: "876320233@qq.com",
  phone: "18962391106",
  password: "123456"
})
const newUserInfo = ref({
  user_name: "任飞扬",
  email:"876320233@qq.com",
  phone:"18962391106",
  oldPassword:"",
  newPassword:"",
})
const formRef = ref(null);
const UserInfoVisible = ref(false)
const UserInfoUpdateVisible = ref(false)
const ShowUpdateUserInfo = ()=>{
  UserInfoUpdateVisible.value = true
  UserInfoVisible.value = false
  newUserInfo.value.newPassword = ""
  newUserInfo.value.oldPassword = ""
  newUserInfo.value.user_name = userInfo.value.user_name
  newUserInfo.value.email = userInfo.value.email
  newUserInfo.value.phone = userInfo.value.phone
}
const handleUpdateUserInfo = ()=>{
  const { user_name,email, phone,oldPassword,newPassword} = newUserInfo.value;

  console.log('用户名:', user_name,'邮箱：', email, '密码：', newPassword, '电话: ',newPassword);
  formRef.value.validate((valid) => {
    if (valid) {
      // 表单验证通过，执行提交操作
      // TODO 提交到后端账户数据 需要检查独立性问题
      ElMessage.success("编辑成功")
      console.log('success');
      UserInfoUpdateVisible.value = false
    } else {
      // 表单验证失败，阻止提交
      console.log('error');
    }
  });
}
const rules = {
  user_name: [
    { required: true, message: '请输入用户名', trigger: 'blur' },
    { min: 3, message: '用户名不少于3个字符', trigger: ['blur', 'change'] },
  ],
  email: [
    { required: true, message: '请输入邮箱', trigger: 'blur' },
    { type: 'email', message: '邮箱格式不正确', trigger: ['blur', 'change'] },
  ],
  phone:[
    { required: true, message: '请输入电话', trigger: 'blur' },
    {
      validator: (rule, value, callback) => {
        const reg = /^1[3-9]\d{9}$/;
        if (reg.test(value)) {
          return callback();
        } else {
          return callback(new Error('请输入正确的11位手机号码'));
        }
      },
      trigger: 'blur',
    }
  ],
  oldPassword:[
    { required: true, message: '请输入原始密码确认身份', trigger: 'blur'},
    {
      validator: (rule, value, callback) => {
        if (value !== userInfo.value.password) {
          callback(new Error('密码不正确'));
        } else {
          callback();
        }
      },
      trigger: 'blur',
    },
  ],
  newPassword:[
    { required: false, message: '请确认密码', trigger: 'blur'},
    { min: 6, message: '密码不少于6个字符', trigger: ['blur', 'change'] },
    {
      validator: (rule, value, callback) => {
        if (value === userInfo.value.password) {
          callback(new Error('新旧密码一致'));
        } else {
          callback();
        }
      },
      trigger: 'blur',
    },
  ],
};
</script>

<template>
  <el-dropdown class="top-right-container" trigger="hover">
    <el-avatar
        src="https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png"
    />
    <template #dropdown>
      <el-dropdown-menu>
        <!--TODO 具体逻辑-->
        <el-text class="mx-2" size="large" style="text-align: center;" tag="b">欢迎，{{userInfo.user_name}}</el-text>
        <div style="margin-bottom: 10px;"></div>
        <el-dropdown-item @click="UserInfoVisible = true">
          <el-icon style="margin-left:15px"><User /></el-icon>
          <span style="margin-left:8px; margin-right: 100px">个人中心</span>
          <el-icon><ArrowRight /></el-icon>
        </el-dropdown-item>
        <el-dropdown-item>
          <el-icon style="margin-left:15px"><SwitchButton /></el-icon>
          <a href="/" style="margin-left:8px; margin-right: 100px">退出登录</a>
        </el-dropdown-item>
      </el-dropdown-menu>
    </template>
  </el-dropdown>
  <el-dialog v-model="UserInfoVisible" width = "30%" align-center :show-close="false">
    <div style="font-size: 25px; font-weight: bold; color: #181818">用户ID： {{userInfo.user_id}}</div>
    <div v-if="UserInfoVisible" style="text-align: left">
      <el-divider/>
      <p style = "margin-left: 1vw; font-size: 1rem; margin-top: 5px; color: #666666">
        名称：
        <span style="font-weight: bold; color: #181818">{{userInfo.user_name}}</span>
      </p>
      <p style = "margin-left: 1vw; font-size: 1rem; margin-top: 5px; color: #666666">
        邮箱：
        <span style="font-weight: bold; color: #181818">{{userInfo.email}}</span>
      </p>
      <p style = "margin-left: 1vw; font-size: 1rem; margin-top: 5px; color: #666666">
        电话号码：
        <span style="font-weight: bold; color: #181818">{{userInfo.phone}}</span>
      </p>
    </div>
    <template #footer>
      <el-button type="danger" @click="ShowUpdateUserInfo">
        编辑
      </el-button>
      <el-button type="primary" @click="UserInfoVisible=false">
        确认
      </el-button>
    </template>
  </el-dialog>
  <el-dialog v-model="UserInfoUpdateVisible" width = "30%" align-center :show-close="false">
    <div style="font-size: 25px; font-weight: bold; color: #181818">用户ID： {{userInfo.user_id}}</div>
    <div v-if="UserInfoUpdateVisible" style="text-align: left">
      <el-divider/>
      <el-form :model="newUserInfo" :rules="rules" ref="formRef" label-width="100px">
        <el-form-item label="密码" prop="oldPassword">
          <el-input type="password" v-model="newUserInfo.oldPassword" placeholder="请输入原始密码确认身份"></el-input>
        </el-form-item>
        <el-form-item label="用户名" prop="user_name">
          <el-input v-model="newUserInfo.user_name" maxlength="30"></el-input>
        </el-form-item>
        <el-form-item label="邮箱" prop="email">
          <el-input v-model="newUserInfo.email"></el-input>
        </el-form-item>
        <el-form-item label="电话号码" prop="phone">
          <el-input v-model="newUserInfo.phone"></el-input>
        </el-form-item>
        <el-form-item label="修改密码" prop="newPassword">
          <el-input type="password" v-model="newUserInfo.newPassword" placeholder="请修改密码（如有需要）"></el-input>
        </el-form-item>
      </el-form>
    </div>
    <template #footer>
      <el-button type="danger" @click="UserInfoUpdateVisible=false">
        取消
      </el-button>
      <el-button type="primary" @click="handleUpdateUserInfo">
        确认
      </el-button>
    </template>
  </el-dialog>
</template>

<style scoped>
.top-right-container {
  position: absolute;
  top: 4vh; /* 根据需要调整 */
  right: 9vw; /* 根据需要调整 */
}
.product_item {
  display: flex;
  align-items: center;
}
a {
  text-decoration: none;
  color: #606266; /* 替换为你希望的颜色 */
}

/* 移除悬停、点击和访问过状态下的下划线 */
a:hover,
a:active,
a:visited {
  text-decoration: none;
}
.custom-link {
  color: #f57777;
}
.custom-link:hover {
  color: white;
}
</style>