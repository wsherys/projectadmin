<template>
    <div class="app app-container">
        <h2>REST API</h2>
        <router-link  to="/api/create" >
            <el-button type="primary" icon="el-icon-edit" style="margin-bottom: 2%;">
                Tambah
            </el-button>
        </router-link>

        <template>
            <!-- <el-table :data="posts" >
                <el-table-column label="ID" prop="id" sortable="custom" align="center" width="80" ></el-table-column>
            </el-table> -->
            <table border="1" style="width: 50%;" 
            >
                <tr>
                    <td style="text-align: center;">&nbsp;ID</td>
                    <td style="text-align: center;">&nbsp;Nama</td>
                    <td style="text-align: center;">&nbsp;Email</td>
                    <td style="text-align: center;">&nbsp;Alamat</td>
                    <td>&nbsp;</td>
                </tr>
                <tr v-for="data in posts" :key="data">
                    <td style="text-align: center;">{{data.id}}</td>
                    <td style="text-align: center;">{{data.name}}</td>
                    <td style="text-align: center;">{{data.email}}</td>
                    <td style="text-align: center;">{{data.address}}</td>
                    <td>
                        &nbsp; 
                        <el-button type="primary" size="mini" v-on:click="PostEdit(data.id)" >
                            <!-- <router-link :to="{name: 'edit', params: { id: data.id }}" > -->
                            Edit
                            <!-- </router-link> -->
                        </el-button>
                        &nbsp;
                        <!-- <button v-on:click="PostDelete(data.id)" class="">Hapus</button> -->
                        <el-button v-on:click="PostDelete(data.id)" type="danger" size="mini">
                            Hapus
                        </el-button>
                        &nbsp;
                    </td>
                </tr>
            </table>
        </template>
        
    </div>

</template>


<script>
    // import Vue from 'vue'
    // import Router from 'vue-router'
    // Vue.use(Router)

    import axios from 'axios'

    export default {
        data() {
            return {
                posts: [],
            }
        },
        created() {
            axios.get('http://127.0.0.1:8000/api/customers').then(response => {
            this.posts = response.data.data;
            });
        },
        methods: {
            PostEdit(id){
                this.$router.push(`/api/edit/${id}`);
            },
            PostDelete(id)
            {
                axios.delete(`http://127.0.0.1:8000/api/customers/${id}`)
                .then(response => {
                this.posts.splice(this.posts.indexOf(id), 1);
                alert("data dihapus");
                location.reload(); 
                // console.log(response);
                }).catch(error => {
                // console.log(error.response);
                this.tampilkanpesan("error gak bisa hapus");
                    
                });
            },
            tampilkanpesan(isipesan){
                alert(isipesan);
                console.log(isipesan);

            }
        }
    }
</script>
