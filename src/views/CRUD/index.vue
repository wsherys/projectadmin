<template>
  <div class="app-container">
    <!-- filter -->
    <div class="filter-container">
      <!-- <el-input v-model="listQuery.name" placeholder="Nama" style="width: 200px;" class="filter-item" @keyup.enter.native="handleFilter"  /> -->

        <el-input
          v-model="search"
          size="mini"
          style="width: 200px;"
          placeholder="cari nama disini"/>
      <!-- tambah data -->
      <el-button class="filter-item"  style="margin-left: 10px;" type="primary" icon="el-icon-edit" @click="formtambah = true">
          Tambah
      </el-button>

      <!-- dialog tambah data -->
      <el-dialog 
      title="Tambah Data" 
      :visible.sync="formtambah" 
      width="30%"
      :before-close="handleClose"
      >
          <form @submit.prevent="tambahdata">
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
          </span>
      </el-dialog>

      <!-- dialog edit data -->
      <el-dialog 
      title="Edit Data" 
      :visible.sync="formedit" 
      width="30%"
      :before-close="handleClose"
      >
        <el-form :model="update">

          <el-form-item 
          label="Nama" 
          :label-width="formLabelWidth"
          prop="name"
          :rules="[{ required: true, message: 'Masukkan nama', trigger: 'blur' },]"
          >
            <el-input v-model="update.name" autocomplete="off"></el-input>
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
              <el-input v-model="update.email" ></el-input>
          </el-form-item>

          <el-form-item label="Alamat" :label-width="formLabelWidth">
              <el-input type="textarea" :autosize="{ minRows: 2, maxRows: 4}" v-model="update.address"></el-input>
          </el-form-item>
        </el-form>

        <el-button class="filter-item"  style="margin-left: 10px;" type="primary" @click="PostUpdate">
          Update
        </el-button>
                
      <span slot="footer" class="dialog-footer"></span>
      </el-dialog>
    </div>

    <!-- tabel -->
    <!-- <el-table
      :data="pagedTableData"
      border
      fit
      highlight-current-row
      style="width: 100%;"
      :default-sort="{prop: 'id', order: 'ascending'}"
    > -->
    <el-table
      :data="pagedTableData.filter(data => !search || data.name.toLowerCase().includes(search.toLowerCase()))"
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
          @click="PostEdit(row.id,row.name,row.email,row.address)"
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
      search: null,

      listLoading: true,
      tableData:[],

      post: {name: null, email: null, address: null },
      update: {name: null, email: null, address: null },
      id: null,
      name: null,
      email: null,
      address: null,
      
      currentPage: 1,
      pagesize:10,

      formtambah: false,
      formedit: false,
      labelPosition: 'left',
     
      formLabelWidth: '100px',

      listQuery: {
        name: undefined,
      },
    }
  },
  computed: {
    pagedTableData(dataTable, query) {
      return this.tableData.slice(this.pagesize * this.currentPage - this.pagesize, this.pagesize * this.currentPage)
    },
  },
  created() {
    this.getList()
  },
  methods: {
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
     searchData(dataTable, query) {
      return dataTable.filter(data => !search || `data.${query}.toLowerCase().includes(search.toLowerCase())`)
    },
    handleClose(done) {
        this.$confirm('Apakah Anda yakin untuk menutup dialog ini?')
        .then(_ => {
            done();
        })
        .catch(_ => {});
    },
    tambahdata () {
      
      if(this.post.address && this.post.name && this.post.email){
          ///
          axios.post('http://localhost:8000/api/customers', {
              name: this.post.name,
              email: this.post.email,
              address: this.post.address
          })
          .then(res => {
              // handle success
              this.getList();
              this.post.name = '';
              this.post.email = '';
              this.post.address = '';
              this.formtambah = false;
          })
          .catch(err => {
              // handle error
              console.log(err);
          })

      }
    },

    
    PostEdit: function(row,name,email,address) {
      this.formedit = true;
      this.id = row;
      this.update.name = name;
      this.update.email = email;
      this.update.address = address;
    },

    PostUpdate () {
      axios.put(`http://127.0.0.1:8000/api/customers/${this.id}`, {
          name: this.update.name,
          email: this.update.email,
          address: this.update.address,
      })
      .then(res => {
          // handle success
          this.getList();
          this.formedit = false;
      })
      .catch(err => {
          // handle error
          console.log(err);
      })
    },

    

    handleDelete(row) 
    {
      this.$confirm(`Ini akan menghapus ID ${row} secara permanen. Lanjutkan?`, 'Peringatan',{
      confirmButtonText: 'OK',
      cancelButtonText: 'Cancel',
      type: 'warning'
      }).then(() => 
      {
        // axios.delete(`product/delete/${this.productIdDelete}`)
        axios.delete(`http://127.0.0.1:8000/api/customers/${row}`)
        .then(res => {
            // handle success
            this.getList();
        })
        .catch(err => {
            // handle error
            console.log(err);
        })
      }).catch(() => {
          // this.tampilkanpesan("error gak bisa hapus");
          this.$message({
          type: 'info',
          message: `Hapus ID ${row} Gagal`
          });
      });

    },

    filterHandler(value, row, column) {
      const property = column['property'];
      return row[property] === value;
    },

    handleSizeChange(size) {
      this.pagesize = size;
    },
    handleCurrentChange(currentPage) {
      this.currentPage = currentPage;
    },

    tampilkanpesan(isipesan){
      // alert(isipesan);
      console.log(isipesan);
    }
    
  }
}
</script>
