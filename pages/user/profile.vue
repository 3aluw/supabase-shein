<template>
    <div class="pick-role">
<v-radio-group inline label="اختر نوع حسابك" v-model="role">
  <v-radio label="مستخدم عادي" value="consumer"></v-radio>
  <v-radio label="تاجر" value="seller"></v-radio>
</v-radio-group>
</div>
<ProfileForm :role="role"/>

</template>
<script setup lang="ts">
import type { Database } from '~/types/supabase'
const role = ref<Database["public"]["Enums"]["user_role"]>("consumer")
</script>
<style scoped></style>