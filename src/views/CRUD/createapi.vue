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
            <h2>Tambah Customer</h2>
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

                        <!-- <form> -->
                        <form @submit.prevent="onsubmit">
                        <p>Nama : </p>
                        <el-input  name="name" type="text" v-model="post.name" placeholder="Masukkan Nama"></el-input>
                        
                        <p>Email : </p>
                        <el-input name="email" type="email" v-model="post.email" placeholder="Masukkan Email"></el-input>

                        <p>Alamat : </p>
                        <el-input name="address" type="text" v-model="post.address" placeholder="Masukkan Alamat"></el-input>
                        <p>
                            
                        <el-input type="submit" value="Simpan" ></el-input>
                        </p>

                        </form>

                        {{ result }}
                    
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
        methods: {
            onsubmit () {
                this.errors=[];
                
                this.result = Object.assign({}, this.post);

                if(this.post.address && this.post.name && this.post.email){
                    // this.errors.push("sukses.");

                    axios.post('http://localhost:8000/api/customers', this.post)
                        .then((response) => {
                            //redirect page
                        this.$router.push({
                            name: 'api'
                        });
                            // this.$router.push({
                            //     name: 'posts'
                            // });
                            // console.log(response.data.data);
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