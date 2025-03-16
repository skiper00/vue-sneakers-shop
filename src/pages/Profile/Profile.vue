<template>
    <div class="shadow-lg bg-slate-50 py-6">
        <h1 class="text-center font-bold text-xl text-[#363638] mb-5">Регистрация</h1>
        <form @submit.prevent="saveUser">
            <div class="flex flex-col items-center gap-4">
                <input v-model="name" class="shadow p-3 outline-none" type="text" placeholder="Name">
                <input v-model="email" class="shadow p-3 outline-none" type="email" placeholder="Email">
                <button class="bg-[#745eff] text-white px-4 py-2 rounded" type="submit">Сохранить</button>
            </div>
        </form>
    </div>
</template>

<script setup>
import { ref, inject } from 'vue';
import {useRouter} from 'vue-router'

const useData = inject('useData')
const router = useRouter();
const name = ref('');
const email = ref('');

const saveUser = () => {
    useData.name = name.value
    useData.email = email.value
    name.value = '';
    email.value = '';
    router.push('/');
}


</script>
