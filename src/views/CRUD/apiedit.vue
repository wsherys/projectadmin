<template>
    <div class="app-container">
        <div style="margin-bottom:1%;">
        <router-link  to="/cth/API">
            <el-button type="primary"  icon="el-icon-back">
                Kembali
            </el-button>
        </router-link>
        </div>
        <div class="posts">
            <el-card style="width: 50%;">
            <h2>Update Customer</h2>
                <el-row>
                    <el-col>
                    <!-- jika errors maka membuat paragraph baru -->
                    <p v-if="errors" >
                        <!-- <b>Please correct the following error(s):</b> -->
                        <ul>
                            <!-- v-bind:key untuk membuat data binding utk memanipulasi value error  -->
                            <!-- v-for untuk loop error -->
                            <li v-for="error in errors" v-bind:key="error" >{{ error }}</li>
                        </ul>
                    </p>

                    <form @submit.prevent="PostUpdate">
                    <p>Nama : </p>
                    <el-input name="nama" v-model="post.name" placeholder="Masukkan Nama"></el-input>
                    
                    <p>Email : </p>
                    <el-input name="email" v-model="post.email" placeholder="Masukkan Email"></el-input>

                    <p>Address : </p>
                    <el-input name="adresss" v-model="post.address" placeholder="Masukkan Address"></el-input>
                    <p>
                        
                    <el-input type="submit" value="Update" ></el-input>
                    </p>

                    </form>
                    </el-col>

                </el-row>
            </el-card>
        </div>
    </div>
</template>


<script>

    import axios from 'axios'

    export default {
        data() {
            return {
                post: {},
                name: null,
                email: null,
                address: null,
                result: {},
                errors: [] //jika errors maka membuat paragraph baru
            }
        },
        created() {
            axios.get(`http://127.0.0.1:8000/api/customers/${this.$route.params.id}`)
                .then((response) => {
                this.post = response.data.data;
            });
        },
        methods: {
            PostUpdate () {
                this.errors=[];
                this.result = Object.assign({}, this.post);

                if(this.post.address && this.post.name && this.post.email){
                    // this.errors.push("sukses.");

                    axios.put(`http://127.0.0.1:8000/api/customers/${this.$route.params.id}`, this.post)
                    .then((response) => {
                        this.$router.push({
                            name: 'api'
                        });
                        console.log(response);
                    }).catch(error => {
                    this.validation = error.response.data.data;
                    });



                }
                else{

                    if (!this.post.name) {
                    this.errors.push("Mohon isi nama.");
                    }

                   
                    if (!this.post.email) {
                        this.errors.push("Mohon isi email dengan benar.");
                    }
                    
                    // if (!this.validEmail(this.post.email)) {
                    //     this.errors.push('Email tidak valid.');
                    // }

                     if (!this.post.address) {
                        this.errors.push("Mohon isi alamat.");
                    }


                }
            },
        }
    }
</script>