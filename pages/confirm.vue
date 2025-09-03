<script setup lang="ts">
import type { Database } from '~/types/supabase'
const user = useSupabaseUser()
const client = useSupabaseClient<Database>()

const checkIfExistingUser = async(role:"consumer" | "seller",userId:string) =>{
  let isFound = null
  if(role == "consumer"){
    const consumerProfile =await  client.from("consumers").select("user_id").eq("user_id",userId).maybeSingle()
    console.log('consumerProfile: ', consumerProfile);
    isFound = consumerProfile.data
  }else if(role="seller"){
    const consumerProfile = await client.from("sellers").select("user_id").eq("user_id",userId).maybeSingle()
    console.log('consumerProfile: ', consumerProfile);
    isFound = consumerProfile.data
  }
  return isFound == null ? false : true
}

onMounted(() => {
  console.log("on confirm");
  handleUser()
})
import { jwtDecode } from 'jwt-decode';
const supabase = useSupabaseClient()

const handleUser = async () => {
  const { data: { session } } = await supabase.auth.getSession();
  if (user.value && session) {
    //get user profile and check if he's a new user or existing one
      const accessToken = session.access_token;
      const decodedToken = jwtDecode(accessToken);
      // The custom claims are directly on the decoded payload
     const  userRole = decodedToken.user_role;
    const isExisting =await checkIfExistingUser(userRole,session.user.id)
    const toRoute = isExisting ? "/" : userRole === "consumer" ? "/cn" : "/sl" 
    console.log(isExisting,userRole,toRoute);
    return navigateTo(toRoute)
  }
}
</script>

<template>
 <template>
  <div class="flex items-center justify-center h-screen bg-gray-50">
    <div class="flex flex-col items-center space-y-4">
      <!-- Vuetify loader -->
      <v-progress-circular
        indeterminate
        color="primary"
        size="48"
      ></v-progress-circular>

      <!-- Text -->
      <p class="text-lg font-medium text-gray-700">يتم تسجيل دخولك</p>
    </div>
  </div>
</template>
</template>
