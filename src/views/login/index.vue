<template>
  <div class="login">
    <div class="login-wrap">
      <div class="loginLogo">
        <img src="../../assets/logo_index.png" alt />
      </div>
      <el-form ref="form" :model="form" :rules="rules">
        <el-form-item prop="mobile">
          <el-input v-model="form.mobile" placeholder="请输入手机号"></el-input>
        </el-form-item>
        <el-form-item prop="code">
          <el-col :span="14">
              <el-input v-model="form.code" placeholder="验证码"></el-input>
          </el-col>
          <el-col :span="8" :push="2">
              <!-- 判断timer是否存在  清除计时器的不会清除timer的标识 不会为null -->
              <el-button :disabled="!!timer" class="sendCode" type="success" plain @click="send">{{timer ? `${time}秒后获取验证码` : '发送验证码'}}</el-button>
          </el-col>
        </el-form-item>
        <el-form-item prop="type">
          <el-checkbox v-model="form.type">
            我已阅读并同意
            <a href="#">用户协议</a>和
            <a href="#">隐私条款</a>
          </el-checkbox>
        </el-form-item>
        <el-form-item>
          <el-button @click="loginForm" :loading="loadType" class="loginBtn" type="success">登录</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
// 导入axios
import axios from 'axios'
export default {
  data () {
    return {
      // 数据
      timer: null,
      time: 60,
      checked: false,
      loadType: false,
      // 表单对象
      form: {
        code: '',
        mobile: '',
        type: false
      },
      // 定义规则
      rules: {
        mobile: [
          { required: true, message: '请输入正确手机号码', trigger: 'blur' },
          { min: 11, max: 11, message: '长度在11个字符', trigger: 'blur' }
        ],
        code: [
          { required: true, message: '请输入正确的验证码', trigger: 'blur' },
          { min: 6, max: 6, message: '长度在6位数', trigger: 'blur' }
        ],
        type: [
          { required: true, message: '请阅读用户协议', trigger: 'change' },
          // 用正则限制结果为true
          { pattern: /true/, message: '请先阅读用户协议', trigger: 'change' }
        ]
      }
    }
  },
  methods: {
    loginForm () {
      this.$refs['form'].validate(valid => {
        if (valid) {
          // 设置loading为true
          this.loadType = true
          // axios发送请求
          axios({
            url: 'http://ttapi.research.itcast.cn/mp/v1_0/authorizations',
            method: 'post',
            data: this.form
          })
            .then(backData => {
              console.log(backData)
              if (backData.status === 201) {
                setTimeout(() => {
                  // 获取到数据后修改loading显示
                  this.loadType = false
                  // 显示登录成功
                  this.$message({
                    message: '恭喜你登录成功',
                    type: 'success'
                  })
                  // 跳转到主界面
                  this.$router.push('/home')
                }, 500)
              }
            })
            .catch(err => {
              console.log(err)
              this.$message.error('登录失败')
            })
        } else {
        }
      })
    },
    send () {
      // 通过获取输入手机号码的提示消息,检验手机号码是否存在
      this.$refs['form'].validateField('mobile', errMsg => {
        if (errMsg.trim().length > 0) {
          // 提醒输入正确手机号码
          this.$message({
            message: '请输入手机号',
            type: 'warning'
          })
        } else {
          // 没有提醒消息的话就发送验证码,显示倒计时
          this.timer = setInterval(() => {
            this.time--
            if (this.time < 0) {
            //  清除计时器
              clearInterval(this.timer)
              // 把时间归位
              this.time = 60
              // 把句柄重新设置为null
              this.timer = null
            }
          }, 1000)
        }
      })
    }
  }
}
</script>

<style lang="less" scoped>
.login {
  height: 100%;
  background: url("../../assets/login_bg.jpg") no-repeat center;
  background-size: 100% 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  .login-wrap {
    width: 350px;
    padding: 50px;
    background-color: #fff;
    .loginLogo {
      text-align: center;
      img {
        width: 200px;
        margin-bottom: 30px;
      }
    }
    .loginBtn {
      width: 100%;
    }
    .sendCode {
      width: 100%;
      padding: 12px 0;
    }
  }
}
</style>
