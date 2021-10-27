<template>
    <div class="app app-container">

        <el-row :gutter="2">
            <el-col :span="12">
                <h2>Data</h2>
            </el-col>
        </el-row>
        
        <el-row :gutter="2">
            <el-col style="margin-bottom: 2%;" :xs="24" :sm="24" :md="24" :lg="24" :xl="24" >
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
                                @click="PostEdit(scope.$index, scope.row.id)"
                                >Edit</el-button>

                                <el-button
                                size="mini"
                                type="danger"
                                @click="handleDelete(scope.$index, scope.row.id)">Delete</el-button>
                            </template>

                        </el-table-column>
                    </el-table>
                    
                    <ul>
                        <li v-for="pageNumber in totalPages" :key="pageNumber">
                        <a href="#" @click="setPage(pageNumber)">{{ pageNumber+1 }}</a>
                        </li>
                    </ul>

                </el-card>
            </el-col>
        </el-row>
    </div>
    
</template>

<script>
    import axios from 'axios'
    import formtambah from './formtambah.vue'


    export default {
        components: { 
            formtambah,
        },
        data() {
            return {
                // search: '',
                // multipleSelection: [],
                posts: [],
                name: null,
                email: null,
                address: null,
                formLabelWidth: '100px',

                // loading: false,
                // order: 1,
                // searchText: null,
                // ccn: null,
                // currentPage: 0,
                // itemsPerPage: 2,
                // resultCount: 0
            }
        },
        created() {
            axios.get('http://127.0.0.1:8000/api/customers').then(response => {
            this.posts = response.data.data;
            });
        },
        
        // computed: {
        //     totalPages: function() {
        //     console.log(Math.ceil(this.resultCount / this.itemsPerPage) + "totalPages");
        //     return Math.ceil(this.resultCount / this.itemsPerPage);

        //     }
        // },
        methods: {
            // setPage: function(pageNumber) {
            // this.currentPage = pageNumber;
            // console.log(pageNumber);
            // },
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
                this.$router.push(`/api/edit/${row}`);
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
