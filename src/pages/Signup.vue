<template>
    <div class="signup">
        <h1>注册</h1>
        <el-input
            id="username"
            size="large"
            placeholder="请输入用户名"
            v-model="username"
        >
            <template #prepend>
                <label for="username">用户名</label>
            </template>
        </el-input>
        <el-input
            id="password"
            size="large"
            placeholder="请输入密码"
            v-model="password"
            show-password
        >
            <template #prepend>
                <label for="password">密码</label>
            </template>
        </el-input>
        <el-input
            id="passwrod-again"
            size="large"
            placeholder="请重复输入密码"
            v-model="passwordAgain"
            show-password
        >
            <template #prepend>
                <label for="passwrod-again">重复密码</label>
            </template>
        </el-input>
        <el-button class="btn" type="primary" size="large" round @click="signup"
            >注册</el-button
        >
    </div>
</template>
<script>
import { ElMessage } from "element-plus"
export default {
    data() {
        return {
            username: "",
            password: "",
            passwordAgain: "",
        }
    },
    methods: {
        async signup() {
            if (this.password != this.passwordAgain) {
                ElMessage.error("前后密码不一致。")
                return
            }

            try {
                const response = await this.$api.signup(
                    this.username,
                    this.password
                )
                this.userStore.setToken(response.data.token)
            } catch (error) {
                if (error.response.data.code === 2000) {
                    ElMessage.error("用户名或密码错误。")
                }
            } finally {
                if (this.$route.query.redirect) {
                    // 如果有之前的页面
                    // 则跳到之前的页面
                    this.$router.push({
                        path: this.$route.query.redirect,
                    })
                } else {
                    this.$router.push({
                        name: "home",
                    })
                }
            }
        },
    },
}
</script>
<style scoped>
.signup {
    width: 360px;
    padding: 20px;
    border-radius: 8px;

    margin: 0 auto;

    background-color: var(--bg-color);

    display: flex;
    flex-direction: column;
    align-items: center;

    gap: 20px;
}
h1 {
    margin: 0;
}

label {
    width: 50px;
    text-align: center;
}
.btn {
    width: 100%;
}
</style>
