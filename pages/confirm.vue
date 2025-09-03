<script setup lang="ts">
import type { Database } from '~/types/supabase'
const user = useSupabaseUser()
const client = useSupabaseClient<Database>()

onMounted(() => {
  console.log("mounted");
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
  }
}
</script>

<template>
  <div>Waiting for login...</div>
</template>
