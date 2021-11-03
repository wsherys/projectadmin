<template>
    <div class="app app-container">
        <!-- import file index CRUD -->
        <indexCRUD />

        <!-- dialog edit -->
        <el-dialog 
        title="Edit Data" 
        :visible.sync="formedit" 
        width="30%"
        :before-close="handleClose"
        >
            <!-- :key  diisi sesuai dengan objek data array. -->
            <div v-for="data in tableData" :key="data.id">
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
            <span slot="footer" class="dialog-footer"></span>
        </el-dialog>
        <!-- dialog edit -->
    </div>
</template>

<script>
    import axios from 'axios'

    // import file index CRUD
    import indexCRUD from './TblDataPasien.vue'

    export default {
        name: 'CRUD',
        components: { indexCRUD },
        data() {
            return {
                listLoading: true,
                tableData:[{
                    id: "",
                    name: "",
                    email: "",
                    address: ""
                }],
                currentPage: 1,
                pagesize:10,
                formLabelWidth: '100px',
                
                post: {name: null, email: null, address: null },
                
                multipleSelection: [],
                formedit: true,
                id: this.$route.params.id //this is the id from the browser
            }
        },

        mounted () {
            axios
            .get(`http://127.0.0.1:8000/api/customers/${this.$route.params.id}`)
            .then(response => (
                this.post.name = response.data.data.name,
                this.post.email = response.data.data.email,
                this.post.address = response.data.data.address
            ))
        },

        // created() {
        //     axios.get('http://127.0.0.1:8000/api/customers').then(response => {
        //     this.posts = response.data.data;
        //     });
        // },

        computed: {
            pagedTableData() {
            return this.tableData.slice(this.pageSize * this.page - this.pageSize, this.pageSize * this.page)
            },
        },
        created() {
            this.getList()
        },

        methods: {
            handleSizeChange(size) {
            this.pagesize = size;
            },
            handleCurrentChange(currentPage) {
            this.currentPage = currentPage;
            },
            
            getList() {
                this.listLoading = true //data loading 
                //get data API with axios
                axios.get('http://127.0.0.1:8000/api/customers').then(response => {
                this.tableData = response.data.data;
            });
            // Just to simulate the time of the request
            setTimeout(() => {
                this.listLoading = false
                }, 1.5 * 1000)
            },

            PostEdit(row){
            // console.log(index, row);
            this.$router.push(`/api/edit/${row}`);
            },
            handleClose(done) {
                this.$confirm('Apakah Anda yakin untuk menutup dialog ini?')
                .then(_ => {
                    done();
                    //refresh page CRUD index
                    this.$router.push({
                        name: 'CRUD'
                    });
                    //reload page
                    // location.reload();
                    //refresh
                    this.$forceUpdate();

                })
                .catch(_ => {});
            },
            
            // PostEdit(row){
            //     // console.log(index, row);
            //     // alert(`edit ${row}`);
            //     this.$router.push(`/api/edit/${row}`);
            // },
           
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
                        //reload page
                        this.$router.go()
                        //refresh
                        // this.$forceUpdate();
                        // console.log(response);
                    }).catch(error => {
                    this.validation = error.response.data.data;
                    });
                }
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

            handleDelete(row) 
            {
            this.$confirm(`Ini akan menghapus ID ${row} secara permanen. Lanjutkan?`, 'Peringatan',{
            confirmButtonText: 'OK',
            cancelButtonText: 'Cancel',
            type: 'warning'
            }).then(() => 
            {
                axios.delete(`http://127.0.0.1:8000/api/customers/${row}`)
                .then(response => {
                    this.$router.push({
                        name: 'CRUD'
                    });
                    // reload page
                    // location.reload();
                    // refresh
                    this.$forceUpdate();

                    this.tampilkanpesan("bisa hapus");
                    console.log(response);
                })

                this.$message({
                    type: 'success',
                    message: `Hapus ID ${row} sukses`
                });

            }).catch(() => {
                this.tampilkanpesan("error gak bisa hapus");
                console.log(error.response);

                this.$message({
                type: 'info',
                message: `Hapus ID ${row} Gagal`
                });
            });
            },
            tampilkanpesan(isipesan){
            // alert(isipesan);
            console.log(isipesan);
            }
        }
    }
</script>
