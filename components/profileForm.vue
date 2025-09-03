<template>
  <form @submit.prevent="handleSubmit" class="max-w-lg mx-auto p-6 bg-white rounded shadow">
    <h2 class="text-2xl font-bold mb-6 text-red-700 capitalize">
      {{ role === 'seller' ? 'تسجيل البائع' : 'تسجيل المستخدم' }}
    </h2>

    <!-- Common fields (consumer) -->
    <template v-if="role === 'consumer'">
      <div class="mb-4">
        <label class="block font-medium mb-1">رقم واتساب</label>
        <input v-model="form.whatsapp_number" type="text" class="w-full border px-3 py-2 rounded" required />
      </div>

      <div class="mb-4">
        <label class="block font-medium mb-1">الحي</label>
        <input v-model="form.district" type="text" class="w-full border px-3 py-2 rounded" />
      </div>

      <div class="mb-4">
        <label class="block font-medium mb-1">العنوان</label>
        <input v-model="form.address" type="text" class="w-full border px-3 py-2 rounded" />
      </div>

      <div class="mb-4">
        <label class="block font-medium mb-1">إحداثيات الموقع (JSON)</label>
        <textarea v-model="form.map_address" class="w-full border px-3 py-2 rounded" placeholder='{"lat": 0, "lng": 0}'></textarea>
      </div>
    </template>

    <!-- Seller fields -->
    <template v-else-if="role === 'seller'">
      <div class="mb-4">
        <label class="block font-medium mb-1">رقم الهاتف</label>
        <input v-model="form.phone_number" type="text" class="w-full border px-3 py-2 rounded" required />
      </div>

      <div class="mb-4">
        <label class="block font-medium mb-1">اسم المتجر</label>
        <input v-model="form.store_name" type="text" class="w-full border px-3 py-2 rounded" required />
      </div>

      <div class="mb-4">
        <label class="block font-medium mb-1">كود المتجر</label>
        <input v-model="form.store_code" type="text" class="w-full border px-3 py-2 rounded" required />
      </div>

      <div class="mb-4">
        <label class="block font-medium mb-1">رابط شعار المتجر</label>
        <input v-model="form.logo_url" type="text" class="w-full border px-3 py-2 rounded" />
      </div>

      <div class="mb-4">
        <label class="block font-medium mb-1">الحي</label>
        <input v-model="form.district" type="text" class="w-full border px-3 py-2 rounded" />
      </div>

      <div class="mb-4">
        <label class="block font-medium mb-1">مدة التوصيل</label>
        <input v-model="form.delivery_time" type="text" class="w-full border px-3 py-2 rounded" />
      </div>

      <div class="mb-4 flex items-center space-x-2">
        <input v-model="form.delivery_to_home" type="checkbox" class="form-checkbox h-5 w-5 text-red-600" />
        <label class="font-medium">التوصيل إلى المنزل</label>
      </div>
    </template>

    <v-btn type="submit" class="bg-red-600 hover:bg-red-700 text-white font-bold py-2 px-4 rounded">
      إرسال
    </v-btn>
  </form>
</template>

<script setup lang="ts">
import type { Database } from '~/types/supabase'
const props = defineProps<{ role: Database["public"]["Enums"]["user_role"]}>()

// Unified type with optional fields for both roles
interface FormData {
  // common
  user_id?: string

  // consumer fields
  whatsapp_number?: string
  district?: string
  address?: string
  map_address?: string

  // seller fields
  phone_number?: string
  store_name?: string
  store_code?: string
  logo_url?: string
  delivery_time?: string
  delivery_to_home?: boolean
}

const form = ref<FormData>({
  // initialize everything as empty
  whatsapp_number: '',
  district: '',
  address: '',
  map_address: '',
  phone_number: '',
  store_name: '',
  store_code: '',
  logo_url: '',
  delivery_time: '',
  delivery_to_home: false
})

// Only send the relevant fields based on role
const filteredForm = computed(() => {
  if (props.role === 'seller') {
    const { phone_number, store_name, store_code, logo_url, district, delivery_time, delivery_to_home } = form.value
    return { phone_number, store_name, store_code, logo_url, district, delivery_time, delivery_to_home }
  } else {
    const { whatsapp_number, district, address, map_address } = form.value
    return { whatsapp_number, district, address, map_address }
  }
})

const handleSubmit = ()=>{

}

</script>
