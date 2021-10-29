<template>
    <div class="app-container">
        <!-- filter -->
        <div class="filter-container">
            <el-input v-model="listQuery.name" placeholder="Nama" style="width: 200px;" class="filter-item" @keyup.enter.native="handleFilter"  />

            <el-button v-waves class="filter-item" type="primary" icon="el-icon-search" @click="handleFilter">
                Search
            </el-button>

            <el-button class="filter-item"  style="margin-left: 10px;" type="primary" icon="el-icon-edit" @click="formtambah = true">
                Tambah
            </el-button>
        </div>

        <el-dialog 
        title="Tambah Data" 
        :visible.sync="formtambah" 
        width="30%"
        :before-close="handleClose"
        >
            <form @submit.prevent="onsubmit">
                <el-form :model="post">
                    <el-form-item 
                    label="Nama" 
                    :label-width="formLabelWidth"
                    prop="name"
                    :rules="[
                        { required: true, message: 'Masukkan nama', trigger: 'blur' },
                    ]"
                    >
                        <el-input v-model="post.name" placeholder="masukkan nama" autocomplete="off"></el-input>
                    </el-form-item>

                    <el-form-item 
                        label="Email" 
                        :label-width="formLabelWidth"
                        prop="email"
                        :rules="[
                            { required: true, message: 'Masukkan email', trigger: 'blur' },
                            { type: 'email', message: 'Masukkan email dengan benar', trigger: ['blur', 'change'] }
                        ]"
                    >
                        <el-input v-model="post.email" placeholder="masukkan email"></el-input>
                    </el-form-item>

                    <el-form-item label="Alamat" :label-width="formLabelWidth">
                        <el-input
                        type="textarea"
                        :autosize="{ minRows: 2, maxRows: 4}"
                        placeholder="Masukkan alamat"
                        v-model="post.address">
                        </el-input>
                    </el-form-item>
                </el-form>

                <el-input type="submit" value="Simpan" ></el-input>

            </form>
            <span slot="footer" class="dialog-footer"></span>
        </el-dialog>

    </div>
</template>

<script>
    export default 
    {
        data() {
            return {
            formtambah: false,
            post: {},
            name: null,
            email: null,
            address: null,
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
                this.result = Object.assign({}, this.post);

                if(this.post.address && this.post.name && this.post.email){
                    axios.post('http://localhost:8000/api/customers', this.post)
                        .then((response) => {
                        //redirect page
                        this.$router.push({
                            name: 'CRUD'
                        });
                        location.reload();
                        }).catch(error => {
                        this.validation = error.response.data.data;
                    });
                }
            },
            
        }
    }
</script>
