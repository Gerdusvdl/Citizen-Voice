<template>
    <NuxtLayout name="default">
        <q-page style="display: flex;justify-content: center;align-items: center">
            <div class="padding-16">
                <center-div>
                    <div class="q-pa-md custom-login-form">
                        <h1 class="q-pb-md text-h6">Login or
                            <NuxtLink to="/register">create an account</NuxtLink>
                        </h1>
                        <q-form ref="myForm" class="q-gutter-md">

                            <q-input filled v-model="email" label="Email *" lazy-rules
                                :rules="[val => val && val.length > 0 || 'Please type something']" />

                            <q-input filled v-model="password" label="Password *" lazy-rules
                                :rules="[val => val !== null && val !== '' || 'Please enter your password']" />
                        </q-form>
                        <div class="q-mt-md">
                            <!-- :disabled="userStore.userData.pending" -->
                            <q-btn label="Submit" @click="onSubmit" color="primary" />
                            <q-btn label="Reset" @click="onReset" color="primary" flat class="q-ml-sm" />

                        </div>
                    </div>
                </center-div>
            </div>
        </q-page>
    </NuxtLayout>
</template>

<script setup>
import CenterDiv from "../layouts/centerDiv";
import { useUserStore } from "~/stores/user"

const email = ref(null)
const password = ref(null)

const userStore = useUserStore()

const onSubmit = async () => {
    await userStore.loginUser(email.value, password.value)
}

const onReset = () => {
    userStore.clearUser()
}


</script>

<style lang="scss" scoped>
.padding-16 {
    width: 100%;
    height: 100%;
    padding: 16px;
}

.custom-container {
    width: 100%;
    height: 100vh;
}

.custom-login-form {
    width: 33%;
    min-width: 100px;
}
</style>
