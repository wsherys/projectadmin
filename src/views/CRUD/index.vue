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

            <span slot="footer" class="dialog-footer">
                <!-- <el-button @click="tambah = false">Cancel</el-button> -->
                <!-- <el-button type="submit">Confirm</el-button> -->
                <!-- <el-input type="submit" value="Simpan"></el-input> -->
            </span>
        </el-dialog>
    </div>

    <!-- tabel -->
    <!-- :data="tableData.slice((currentPage-1)*pagesize,currentPage*pagesize)" -->
    <el-table
      :data="tableData.slice(pagesize * currentPage - pagesize, pagesize * currentPage)"
      border
      fit
      highlight-current-row
      style="width: 100%;"
      :default-sort="{prop: 'id', order: 'ascending'}"
    >
      <el-table-column label="ID" min-width="150px" prop="id" sortable >
        <template slot-scope="{row}">
          <span class="link-type" >{{ row.id }}</span>
        </template>
      </el-table-column>

      <el-table-column label="Nama" min-width="150px">
        <template slot-scope="{row}">
          <span class="link-type" >{{ row.name }}</span>
        </template>
      </el-table-column>

      <el-table-column label="Email" min-width="150px">
        <template slot-scope="{row}">
          <span class="link-type" >{{ row.email }}</span>
        </template>
      </el-table-column>

      <el-table-column label="Alamat" min-width="150px">
        <template slot-scope="{row}">
          <span class="link-type" >{{ row.address }}</span>
        </template>
      </el-table-column>

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

    <!-- pagination -->
    <!-- <el-pagination layout="prev, pager, next" :total="this.tableData.length" @current-change="setPage"></el-pagination> -->
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="currentPage"
      :page-sizes="[10, 20, 30, 40]"
      :page-size="pagesize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="tableData.length">
    </el-pagination>
    
  </div>
</template>

<script>
import axios from 'axios'
import waves from '@/directive/waves' // waves directive

export default {
  name: 'CRUD',
  components: {},
  directives: { waves },
  
  data() {
    return {
      listLoading: true,
      tableData:[],
      currentPage: 1,
      pagesize:10,

      formtambah: false,
      labelPosition: 'left',
      post: {},
      name: null,
      email: null,
      address: null,
      formLabelWidth: '100px',
      listQuery: {
          page: 1,
          // limit: 20,
          // importance: undefined,
          name: undefined,
          // type: undefined,
          // sort: '+id'
      },
     
     
    }
  },
  computed: {
    pagedTableData() {
      return this.tableData.slice(this.pageSize * this.page - this.pageSize, this.pageSize * this.page)
    },
  },
  created() {
    this.getList()
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
        this.errors=[];
        
        this.result = Object.assign({}, this.post);

        if(this.post.address && this.post.name && this.post.email){
            // this.errors.push("sukses");
            axios.post('http://localhost:8000/api/customers', this.post)
                .then((response) => {
                    //redirect page
                this.$router.push({
                    name: 'CRUD'
                });
                location.reload();
                // console.log(response.data.data);
                }).catch(error => {
                this.validation = error.response.data.data;
            });
        }
        // else{
        //     // alert("gagal");
        // }
    },
    handleFilter() {
        this.listQuery.page = 1
        this.getList()
    },

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
          // refresh
          // this.$forceUpdate();

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

    tampilkanpesan(isipesan){
      // alert(isipesan);
      console.log(isipesan);
    }
    
  }
}
</script>
