<style scoped>
#all_container {
    text-align: center;
    height: 100vh;
    background-color: #d9d9d9;
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
    z-index: 1;
    font-family: 'Noto Sans TC', sans-serif;
}
#login_section {
    width: 80vw;
    height: 300px;
    background-color: white;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
    border-radius: 1%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    max-width: 500px;
}
#account {
    margin: 15px 5px 5px 5px;
}

#password {
    margin: 5px 5px 15px 5px;
}
.login_button {
    width: 150px;
    height: 30px;
    border-radius: 3px;
    text-align: center;
    cursor: pointer;
    line-height: 40px;
    color: #fff;
    background-color: #000;
    display: flex;
    align-items: center;
    justify-content: center;
    margin: auto;
}
.login_button:hover {
    background-color: rgb(38 38 38);
}
.custom-input {
    border: none;
    border-bottom: 1px solid #000;
    outline: none;
    padding: 5px;
}
h5 {
    color: #8c8c8c;
    padding: 20px;
}
h5 a {
    color: #8c8c8c;
}
</style>

<template>
    <div id="all_container">
        <div id="login_section">
            <div id="title"><h4>管理員登入</h4></div>
            <form @submit.prevent="login">
                <div id="account">
                    <label for="account">帳號：</label>
                    <input
                        type="text"
                        id="account"
                        v-model="user.account"
                        class="custom-input"
                        required
                        minlength="4"
                        maxlength="12"
                    />
                </div>
                <div id="password">
                    <label for="password">密碼：</label>
                    <input
                        type="password"
                        id="password"
                        v-model="user.password"
                        class="custom-input"
                        required
                        minlength="4"
                        maxlength="12"
                    />
                </div>
                <button type="submit" class="login_button">登入</button>
            </form>
        </div>
    </div>
</template>
<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { save_status } from '@/stores/counter' //引入pinia

const store_status = save_status()
const router = useRouter()
const user = ref({
    account: '',
    password: '',
})
console.log('pinia登入狀態::' + store_status.login_status)

const login = async () => {
    try {
        const response = await fetch('http://localhost:3100/stepbrothers/console/login', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify(user.value),
        })

        if (response.ok) {
            const data = await response.json()
            if (data.login_status === false) {
                alert('登入失敗，請檢查帳號與密碼!')
                return
            }
            console.log(data)
            alert('成功登入!')
            router.push('/order_page')
        } else {
            alert('登入時發生錯誤!')
        }
    } catch (error) {
        alert('登入時發生錯誤!')
        console.error('登入時發生錯誤：', error)
    }
}
</script>
