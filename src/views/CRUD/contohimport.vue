<template>
      <!-- <judul title="Contoh Import" /> -->
      <judul title="Contoh Penggunaan Components" />

</template>

<script>
//passing data di src/components/Title.vue
import judul from '@/components/Title.vue'

export default {
    name:'App',
    components:{
        judul,
    },
}


</script>
