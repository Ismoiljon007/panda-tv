<template>
    <main>
        <div class="profile">
            <div class="container">
                <div class="profile__top">
                </div>
                <div class="profile__bottom">
                    <Nav />
                    <div class="profile__bottom-wrapper">
                        <button class="edit-profile" @click="update = !update">
                            <svg id="Layer_1" width="20" height="20" version="1.1" viewBox="0 0 24 24"
                                xml:space="preserve" xmlns="http://www.w3.org/2000/svg"
                                xmlns:xlink="http://www.w3.org/1999/xlink">
                                <path fill="#fff"
                                    d="M19.607,18.746c0,0.881-0.716,1.624-1.597,1.624H5.231c-0.881,0-1.597-0.743-1.597-1.624V5.967  c0-0.881,0.716-1.571,1.597-1.571h7.454V3.332H5.231c-1.468,0-2.662,1.168-2.662,2.636v12.778c0,1.468,1.194,2.688,2.662,2.688  h12.778c1.468,0,2.662-1.221,2.662-2.688v-7.428h-1.065V18.746z" />
                                <path fill="#fff"
                                    d="M20.807,3.17c-0.804-0.805-2.207-0.805-3.012,0l-7.143,7.143c-0.068,0.068-0.117,0.154-0.14,0.247L9.76,13.571  c-0.045,0.181,0.008,0.373,0.14,0.506c0.101,0.101,0.237,0.156,0.376,0.156c0.043,0,0.086-0.005,0.129-0.016l3.012-0.753  c0.094-0.023,0.179-0.072,0.247-0.14l7.143-7.143c0.402-0.402,0.624-0.937,0.624-1.506S21.21,3.572,20.807,3.17z M13.016,12.467  l-2.008,0.502l0.502-2.008l5.909-5.909l1.506,1.506L13.016,12.467z M20.054,5.428l-0.376,0.376l-1.506-1.506l0.376-0.376  c0.402-0.402,1.104-0.402,1.506,0c0.201,0.201,0.312,0.468,0.312,0.753C20.366,4.96,20.255,5.227,20.054,5.428z" />
                            </svg>
                        </button>
                        <h3 class="profile__bottom-title">Foydalanuvchi haqida ma’lumot</h3>
                        <div class="profile__info">
                            <div class="profile__img" style="text-transform: uppercase;">
                                <span>{{ store.userInfo?.username.split("")[0] }}</span>
                            </div>
                            <!-- <input type="file" id="img"> -->
                            <label class="profile__img upload-img" for="img" v-if="false
                            " style="text-transform: uppercase;">
                                <span>{{ store.userInfo?.username.split("")[0] }}</span>
                                <button>
                                    <svg height="48" viewBox="0 0 48 48" width="48" xmlns="http://www.w3.org/2000/svg">
                                        <path d="M0 0h48v48H0z" fill="none" />
                                        <path fill="#fff"
                                            d="M8 12H4v28c0 2.21 1.79 4 4 4h28v-4H8V12zm32-8H16c-2.21 0-4 1.79-4 4v24c0 2.21 1.79 4 4 4h24c2.21 0 4-1.79 4-4V8c0-2.21-1.79-4-4-4zm-2 18h-8v8h-4v-8h-8v-4h8v-8h4v8h8v4z" />
                                    </svg>
                                </button>
                            </label>
                            <form @submit.prevent="" class="profile__info-list">
                                <div class="profile__info-list-item">
                                    <label for="name">Ism:</label> <input
                                        :style="update == false ? 'border-color: #fff;' : 'transparent'"
                                        :disabled="update" id="name" type="text" v-model="name">
                                </div>
                                <div class="profile__info-list-item">
                                    <label for="login">Login:</label> <input disabled id="login" type="text"
                                        v-model="login">
                                </div>
                                <div class="profile__info-list-item">
                                    <label for="tel">tel:</label> <input disabled id="tel" type="text" v-model="tel">
                                </div>
                                <button @click="updateUserInfo()" v-if="!update">ma'lumotni yangilash</button>
                            </form>
                        </div>
                        <div class="profile__footer">
                            <h4 class="profile__id">ID: <span>{{ store.userInfo?.id }}</span></h4>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </main>
</template>

<script setup lang="ts">
import { useToast } from 'vue-toastification';
import userInfo from '~/services/userInfo';
import { useStore } from '~~/store/store';
import { useAuthStore } from "~~/store/auth"
const store = useStore()
const authStore = useAuthStore()
const update = ref < boolean > (true)
const name = ref < string > (store.userInfo?.name)
const login = ref < string > (store.userInfo?.username)
const tel = ref < string > (store.userInfo?.phone_number)
store.loader = false;

interface UpdateForm {
    phone_number: string;
    name: string;
}
async function updateUserInfo() {
    const body: UpdateForm = {
        'phone_number': tel.value,
        'name': name.value,
    }
    const data: any = await userInfo.updateUserInfo(authStore.token, body)
    if (data.status == "success") {
        update.value = true
        useToast().success('profilingiz muvafaqiyatli tahrirlandi')
    }
}

watchEffect(() => {
    name.value = store.userInfo?.name
    login.value = store.userInfo?.username
    tel.value = store.userInfo?.phone_number
})
</script>

<style lang="scss" scoped></style>