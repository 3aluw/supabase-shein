<template>
    <div>
        <nav class="flex px-4 sm:h-16 items-center">
            <v-btn variant="flat" color="red-accent-2" class="font-bold px-4 mr-auto rounded" @click="signInWithGoogle">
                تسجيل الدخول
            </v-btn>
        </nav>
        <div class="product-categories flex flex-wrap gap-8">
            <div class="category-card w-36 flex flex-col items-center gap-2" v-for="category in productCategories">
                <div class="cat-img-cont w-28 h-28"><img class="w-full h-full rounded-full"
                        :src="`/product-categories/${category.picture}`" alt=""></div>
                <span>{{ category.name }}</span>
            </div>
        </div>
    </div>
</template>
<script setup lang="ts">
import { productCategories } from "~/data/data"
import { jwtDecode } from 'jwt-decode';
const supabase = useSupabaseClient()
const user = useSupabaseUser()
console.log(user.value);
 const { data: { session } } = await supabase.auth.getSession();
 if (session) {
    const accessToken = session.access_token;
    const decodedToken = jwtDecode(accessToken);

    // The custom claims are directly on the decoded payload
    const userRole = decodedToken.user_role;
    console.log("User role from JWT:", userRole);
 }
const signInWithGoogle = async () => {
  const { error } = await supabase.auth.signInWithOAuth({
  provider: 'google',
  options:{
       redirectTo: 'http://localhost:3000/confirm' 
  }
})
  if (error) console.log(error)
}

</script>
