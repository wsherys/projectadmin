<template>
    <div>    
        <h1>Export</h1>

        <h4>Contoh Declarative Rendering</h4>
        <h4>Nama Saya {{ nama }}</h4>
        <h4>Umur Saya {{ umur }} tahun</h4>

        <!-- <h4>Contoh Loops Menggunakan v-bind</h4>
        <table border=1>
            <tr v-for="hari in namaHari" v-bind:key="hari">
                <td>{{ hari }}</td>
            </tr>
        </table> -->
    </div>
</template>

<script>
export default {
     data(){
        return{
            nama : "TEST",
            umur : 17   
        }
    },
    // mounted(){
    //     this.umur = this.umur + 3;        
    // },

    // data2(){
    //     return{
    //         namaHari : ["Senin", "Selasa", "Rabu", "Kamis",
    //                     "Jumat", "Sabtu", "Minggu"
    //                     ]
    //     }
    // }
}
</script>

