<template>
    <div class="app-container">
        <!-- filter -->
        ......

        <el-table
        :data="tableData.slice(pagesize * currentPage - pagesize, pagesize * currentPage)"
        border
        fit
        highlight-current-row
        style="width: 100%;"
        :default-sort="{prop: 'id', order: 'ascending'}"
        >
        
            ......

            <el-table-column label="Actions" align="center" width="230" class-name="small-padding fixed-width">
                <template slot-scope="{row}">

                <el-button
                type="primary"
                size="mini"
                @click="PostEdit(row.id)"
                >Edit</el-button>
                
                <el-button
                type="danger"
                size="mini"
                @click="handleDelete(row.id)"
                >Delete</el-button>

                </template>
            </el-table-column>
        
        </el-table>
    </div>
</template>

<script>
   
    import axios from 'axios'

    export default {
        name: 'CRUD',
            
        data() {
            return {
            // .....
            }
        },
        computed: {
            // .....
        },
        created() {
            // .....
        },
        methods: {
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
                this.$router.go()

                this.tampilkanpesan("bisa hapus");
                console.log(response);

                })
                
                this.$message({
                    type: 'success',
                    message: `Hapus ID ${row} sukses`
                });

            }).catch(() => {
                this.tampilkanpesan("error gak bisa hapus");

                this.$message({
                type: 'info',
                message: `Hapus ID ${row} Gagal`
                });
            });
            },
        }
    }
</script>