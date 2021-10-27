<template>
    <div>
        <el-button type="primary" @click="formtambah = true">Tambah</el-button>

        <el-dialog 
        title="Tambah Data" 
        :visible.sync="formtambah" 
        width="30%"
        :before-close="handleClose"
        >
            <el-form :model="form" :label-position="labelPosition">
                <el-form-item label="Nama" :label-width="formLabelWidth">
                    <el-input v-model="form.name" autocomplete="off"></el-input>
                </el-form-item>

                <el-form-item 
                    label="Email" 
                    :label-width="formLabelWidth"
                    prop="email"
                    :rules="[
                    { required: true, message: 'Masukkan email address', trigger: 'blur' },
                    { type: 'email', message: 'Masukkan email address dengan benar', trigger: ['blur', 'change'] }
                    ]"
                >
                    <el-input v-model="form.email"></el-input>
                </el-form-item>
                
                <el-form-item label="Alamat" :label-width="formLabelWidth">
                    <el-input
                    type="textarea"
                    :autosize="{ minRows: 2, maxRows: 4}"
                    placeholder="Masukkan alamat"
                    v-model="form.alamat">
                    </el-input>
                </el-form-item>

            </el-form>
            <span slot="footer" class="dialog-footer">
                <!-- <el-button @click="tambah = false">Cancel</el-button> -->
                <el-button type="primary" @click="formtambah = false">Confirm</el-button>
            </span>
        </el-dialog>
    </div>
</template>

<script>
    // import axios from 'axios'

    export default {
        components: { 
        },

        data() {
            return {
                formtambah: false,
                labelPosition: 'left',
                form: {
                    name: '',
                    email: '',
                    alamat: '',
                    delivery: false,
                    type: [],
                    // resource: '',
                    // desc: '',
                },
                formLabelWidth: '100px',
            }
        },
        methods: {
            handleClose(done) {
                this.$confirm('Apakah Anda yakin untuk menutup dialog ini?')
                .then(_ => {
                    done();
                })
                .catch(_ => {});
            },
            onsubmit () {
                // this.errors=[];
                
                // this.result = Object.assign({}, this.post);

                // if(this.post.address && this.post.name && this.post.email){
                //     // this.errors.push("sukses.");

                //     axios.post('http://localhost:8000/api/customers', this.post)
                //         .then((response) => {
                //             //redirect page
                //         this.$router.push({
                //             name: 'api'
                //         });

                //             // this.$router.push({
                //             //     name: 'posts'
                //             // });
                //             // console.log(response.data.data);
                //         }).catch(error => {
                //         this.validation = error.response.data.data;
                //     });
                // }
                // else{

                //     if (!this.post.name) {
                //     this.errors.push("Mohon isi nama.");
                //     }

                   
                //     if (!this.post.email) {
                //         this.errors.push("Mohon isi email dengan benar.");
                //     }
                    
                //     // if (!this.validEmail(this.post.email)) {
                //     //     this.errors.push('Email tidak valid.');
                //     // }

                //      if (!this.post.address) {
                //         this.errors.push("Mohon isi alamat.");
                //     }
                // }
            },
        }
    }
</script>
