<template>
    <div class="app app-container">

        <el-row :gutter="2">
            <el-col :span="24">
                <h2>Data</h2>
            </el-col>

            <el-col style="margin-bottom: 2%;" :xs="24" :sm="24" :md="24" :lg="24" :xl="24">
                <!-- <el-button type="primary" @click="formtambah = true">Tambah</el-button> -->
                <formtambah></formtambah>
            </el-col>

            <el-col :xs="24" :sm="24" :md="24" :lg="24" :xl="24">
                <el-card>
                    <el-table
                        :data="posts.filter(data => !search || data.name.toLowerCase().includes(search.toLowerCase()))"
                        style="width: 100%"
                        @selection-change="handleSelectionChange"
                        :default-sort = "{prop: 'id', order: 'descending'}"
                        >

                        <el-table-column
                        type="selection"
                        width="55">
                        </el-table-column>

                        <el-table-column
                        label="ID"
                        prop="id">
                        </el-table-column>

                        <el-table-column
                        label="Nama"
                        prop="name">
                        </el-table-column>

                        <el-table-column
                        label="Email"
                        prop="email">
                        </el-table-column>

                        <el-table-column
                        label="Alamat"
                        prop="address">
                        </el-table-column>
                        
                        <el-table-column
                        align="right"
                        fixed="right"
                        width="200"
                        >
                            <template slot="header" slot-scope="{}">
                                <el-input
                                v-model="search"
                                size="mini"
                                placeholder="Type to search"/>
                            </template>
                            
                            <template slot-scope="scope" >
                                <!-- <el-button
                                size="mini"
                                @click="handleEdit(scope.$index, scope.row.id)" 
                                >Edit</el-button> -->
                                <el-button
                                size="mini"
                                @click="PostEdit(scope.$index, scope.row.id), formedit = true"
                                >Edit</el-button>

                                <el-button
                                size="mini"
                                type="danger"
                                @click="handleDelete(scope.$index, scope.row.id)">Delete</el-button>
                            </template>
                        </el-table-column>
                    </el-table>
                </el-card>
            </el-col>
        </el-row>

         <!-- dialog edit -->
        <el-dialog 
        title="Edit Data" 
        :visible.sync="formedit" 
        width="30%"
        :before-close="handleClose"
        >
            <!-- <div v-for="data in posts" :key="data">
                <div v-if="data.id == id">{{ data.name }}</div>
            </div> -->
            <!-- {{ id }} -->
            
            <div v-for="data in posts" :key="data">
                <div v-if="data.id == id">
                    <form @submit.prevent="PostUpdate">

                        <el-form :model="post">
                            <el-form-item 
                            label="Nama" 
                            :label-width="formLabelWidth"
                            prop="name"
                            :rules="[
                                { required: true, message: 'Masukkan nama', trigger: 'blur' },
                            ]"
                            >
                                <el-input v-model="post.name" :placeholder="data.name" autocomplete="off"></el-input>
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
                                <el-input v-model="post.email" :placeholder="data.email"></el-input>
                            </el-form-item>

                            <el-form-item label="Alamat" :label-width="formLabelWidth">
                                <el-input
                                type="textarea"
                                :autosize="{ minRows: 2, maxRows: 4}"
                                :placeholder="data.address"
                                v-model="post.address">
                                </el-input>
                            </el-form-item>
                        </el-form>

                        <el-input type="submit" value="Simpan" ></el-input>

                    </form>
                </div>
            </div>
            <span slot="footer" class="dialog-footer">
                <!-- <el-button @click="tambah = false">Cancel</el-button> -->
                <!-- <el-button type="submit">Confirm</el-button> -->
                <!-- <el-input type="submit" value="Simpan"></el-input> -->
            </span>
        </el-dialog>
        <!-- dialog edit -->
        
    </div>

    
</template>

<script>
    import axios from 'axios'
    import formtambah from './formtambah.vue'

    export default {
        components: { 
            formtambah
        },

        data() {
            return {
                post: {},
                posts: [],
                search: '',
                multipleSelection: [],
                name: null,
                email: null,
                address: null,
                formedit: true,
                formLabelWidth: '100px',

                id: this.$route.params.id //this is the id from the browser

            }
        },

        created() {
            axios.get('http://127.0.0.1:8000/api/customers').then(response => {
            this.posts = response.data.data;
            });
        },

        methods: {
            handleClose(done) {
                this.$confirm('Apakah Anda yakin untuk menutup dialog ini?')
                .then(_ => {
                    done();
                })
                .catch(_ => {});
            },
            toggleSelection(rows) {
                if (rows) {
                rows.forEach(row => {
                    this.$refs.multipleTable.toggleRowSelection(row);
                });
                } else {
                this.$refs.multipleTable.clearSelection();
                }
            },
            handleSelectionChange(val) {
                this.multipleSelection = val;
            },
            PostEdit(index, row){
                // console.log(index, row);
                // alert(`edit ${row}`);
                this.$router.push(`/api/edit/${row}`);
            },
           
            PostUpdate () {
                this.errors=[];
                this.result = Object.assign({}, this.post);

                if(this.post.address && this.post.name && this.post.email){
                    // this.errors.push("sukses.");

                    // axios.put(`http://127.0.0.1:8000/api/customers/${this.$route.params.id}`, this.post)
                    axios.put(`http://127.0.0.1:8000/api/customers/${this.$route.params.id}`, this.post)
                    .then((response) => {
                        this.$router.push({
                            name: 'CRUD'
                        });
                        location.reload();
                        // console.log(response);
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
            handleDelete(index, row) {
                this.$confirm(`Ini akan menghapus ID ${row} secara permanen. Lanjutkan?`, 'Peringatan',{
                confirmButtonText: 'OK',
                cancelButtonText: 'Cancel',
                type: 'warning'
                }).then(() => {

                    axios.delete(`http://127.0.0.1:8000/api/customers/${row}`)
                    .then(response => {
                    this.posts.splice(this.posts.indexOf(row), 1);
                    // alert(`data dihapus ${row}`);
                    location.reload(); 
                    // console.log(response);
                    }).catch(error => {
                    // console.log(error.response);
                    this.tampilkanpesan("error gak bisa hapus");
                    });

                    this.$message({
                        type: 'success',
                        message: `Hapus ID ${row} sukses`
                    });
                }).catch(() => {
                this.$message({
                    type: 'info',
                    message: `Hapus ID ${row} Gagal`
                });          
                });
            },
            tampilkanpesan(isipesan){
                alert(isipesan);
                console.log(isipesan);

            }
        }
    }
</script>
