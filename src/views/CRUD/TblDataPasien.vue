<template>
  <div class="app-container">
      <h1>Data Pasien</h1>
    <!-- filter -->
    <div class="filter-container">

        <el-button class="filter-item"  style="margin-left: 10px;" type="primary" icon="el-icon-edit" @click="formtambah = true">
            Tambah
        </el-button>

        <el-dialog 
        title="Pendaftaran Pasien Baru" 
        :visible.sync="formtambah" 
        width="70%"
        :before-close="handleClose"
        
        >
            <form @submit.prevent="onsubmit" >
              <el-form :model="post" :label-position="labelPosition" >
                <el-row>
                  <el-col :span="12" class="app-container">
                    <el-form-item 
                    label="Nama Lengkap" 
                    :label-width="formLabelWidth"
                    prop="nama"
                    :rules="[
                        { required: true, message: 'Masukkan nama', trigger: 'blur' },
                    ]"
                    >
                        <el-input v-model="post.nama" placeholder="masukkan nama" autocomplete="off"></el-input>
                    </el-form-item>

                    <el-form-item label="Tempat/Tanggal Lahir" :label-width="formLabelWidth" prop="tgllahir" >
                        <el-col :span="11">
                            <el-input v-model="post.tempatlahir" placeholder="Masukkan Tempat Lahir" autocomplete="off"></el-input>
                        </el-col>
                        <el-col class="line" :span="2">-</el-col>
                        <el-col :span="11">
                            <el-date-picker  v-model="post.tgllahir" type="date"  placeholder="Masukkan Tgl Lahir" style="width: 100%;"></el-date-picker>
                        </el-col>
                    </el-form-item>

                    <el-form-item label="Jenis Kelamin" :label-width="formLabelWidth" >
                      <!-- <p>jenis kelamin</p> -->
                      <el-radio-group v-model="post.jeniskelamin">
                        <el-radio label="Laki Laki"></el-radio>
                        <el-radio label="Perempuan"></el-radio>
                      </el-radio-group>
                    </el-form-item>

                    <el-form-item label="Agama" :label-width="formLabelWidth">
                        <el-select v-model="post.agama" placeholder="Pilih Agama">
                            <el-option
                            v-for="item in agama"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value">
                            </el-option>
                        </el-select>
                    </el-form-item>

                    <el-form-item label="Pendidikan" :label-width="formLabelWidth">
                        <el-select v-model="post.pendidikan" placeholder="Pilih Pendidikan">
                            <el-option
                            v-for="item in pendidikan"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value">
                            </el-option>
                        </el-select>
                    </el-form-item>

                    <el-form-item label="Pekerjaan" :label-width="formLabelWidth">
                        <el-select v-model="post.pekerjaan" placeholder="Pilih Pekerjaan">
                            <el-option
                            v-for="item in pekerjaan"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value">
                            </el-option>
                        </el-select>
                    </el-form-item>
                  </el-col>

                  <el-col :span="12" class="app-container">
                    <el-form-item label="Status" :label-width="formLabelWidth">
                        <el-select v-model="post.status" placeholder="Pilih Status">
                            <el-option
                            v-for="item in status"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value">
                            </el-option>
                        </el-select>
                    </el-form-item>
                    
                    <el-form-item 
                    label="No.Telepon Rumah" 
                    :label-width="formLabelWidth"
                    >
                        <el-input v-model="post.notelepon" placeholder="masukkan nomor telepon rumah" autocomplete="off"></el-input>
                    </el-form-item>

                    <el-form-item 
                    label="No.Handphone" 
                    :label-width="formLabelWidth"
                    >
                        <el-input v-model="post.nohandphone" placeholder="masukkan nomor handphone" autocomplete="off"></el-input>
                    </el-form-item>

                    <el-form-item label="Alamat" :label-width="formLabelWidth">
                        <el-input
                        type="textarea"
                        :autosize="{ minRows: 2, maxRows: 4}"
                        placeholder="Masukkan alamat"
                        v-model="post.address">
                        </el-input>
                    </el-form-item>

                    <el-form-item label="Jenis Pembayaran" :label-width="formLabelWidth">
                        <el-select v-model="post.pembayaran" placeholder="Pilih Pembayaran">
                            <el-option
                            v-for="item in pembayaran"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value">
                            </el-option>
                        </el-select>
                    </el-form-item>
                  </el-col>
                </el-row>
              </el-form>

              <!-- <el-input type="submit"  value="Simpan" ></el-input> -->

              <el-button class="filter-item" type="primary" @submit.prevent="onsubmit">
                Simpan
              </el-button>

            </form>

            <span slot="footer" class="dialog-footer">
            </span>
        </el-dialog>
    </div>

    <!-- tabel -->
    <el-table
      :data="pagedTableData"
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
          <span class="link-type" >{{ row.nama }}</span>
        </template>
      </el-table-column>

      <el-table-column label="Tempat Tgl Lahir" min-width="150px">
        <template slot-scope="{row}">
          <span class="link-type" >{{ row.tempat_lahir }} {{ row.tgl_lahir }}</span>
        </template>
      </el-table-column>
      
      <el-table-column label="Jenis Kelamin" min-width="150px">
        <template slot-scope="{row}">
          <span class="link-type" >{{ row.jenis_kelamin }}</span>
        </template>
      </el-table-column>

      <el-table-column label="Agama" min-width="150px">
        <template slot-scope="{row}">
          <span class="link-type" >{{ row.agama }}</span>
        </template>
      </el-table-column>

      <el-table-column label="Pendidikan" min-width="150px">
        <template slot-scope="{row}">
          <span class="link-type" >{{ row.pendidikan }}</span>
        </template>
      </el-table-column>

      <el-table-column label="Pekerjaan" min-width="150px">
        <template slot-scope="{row}">
          <span class="link-type" >{{ row.pekerjaan }}</span>
        </template>
      </el-table-column>

      <el-table-column label="Status" min-width="150px">
        <template slot-scope="{row}">
          <span class="link-type" >{{ row.status }}</span>
        </template>
      </el-table-column>
      

      <el-table-column label="No.Telepon" min-width="150px">
        <template slot-scope="{row}">
          <span class="link-type" >{{ row.no_telepon }}</span>
        </template>
      </el-table-column>

      <el-table-column label="No.Handphone" min-width="150px">
        <template slot-scope="{row}">
          <span class="link-type" >{{ row.no_handphone }}</span>
        </template>
      </el-table-column>

      <el-table-column label="Alamat" min-width="150px">
        <template slot-scope="{row}">
          <span class="link-type" >{{ row.alamat }}</span>
        </template>
      </el-table-column>

      <el-table-column label="Pembayaran" min-width="150px">
        <template slot-scope="{row}">
          <span class="link-type" >{{ row.pembayaran }}</span>
        </template>
      </el-table-column>
      

      <el-table-column label="Actions" align="center" width="230" fixed="right"
      class-name="small-padding fixed-width">
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
  name: 'TblDataPasien',
  components: {},
  directives: { waves },
  
  data() {
    return {
        search: null,

        listLoading: true,
        // tableData:[],
        tableData:[{
          id:'1', 
          nama:'Andi', 
          tgl_lahir:'jakarta 11 januari 1997', 
          jenis_kelamin:'L',
          agama:'islam',
          pendidikan:'S1',
          pekerjaan:'wiraswasta',
          }],
        currentPage: 1,
        pagesize:10,

        formtambah: false,
        labelPosition: 'left',
        post: {},
        formLabelWidth: '160px',

        listQuery: {
        name: undefined,
        },

        agama: [{
          value: 'Islam',
          label: 'Islam'
        }, {
          value: 'Kristen',
          label: 'Kristen'
        }, {
          value: 'Katolik',
          label: 'Katolik'
        }, {
          value: 'Hindu',
          label: 'Hindu'
        }, {
          value: 'Budha',
          label: 'Budha'
        }, {
          value: 'Lain-Lain',
          label: 'Lain-Lain'
        }],

        pendidikan: [{
          value: 'tidak sekolah',
          label: 'tidak sekolah'
        }, {
          value: 'tidak tamat SD',
          label: 'tidak tamat SD'
        }, {
          value: 'SD',
          label: 'SD'
        }, {
          value: 'SMP',
          label: 'SMP'
        }, {
          value: 'SMA/SMK',
          label: 'SMA/SMK'
        },  {
          value: 'Akademi/ D3',
          label: 'Akademi/ D3'
        },  {
          value: 'Sarjana',
          label: 'Sarjana'
        }, {
          value: 'Lain-Lain',
          label: 'Lain-Lain'
        }],

        pekerjaan: [{
          value: 'pegawai negeri',
          label: 'pegawai negeri'
        }, {
          value: 'TNI/POLRI',
          label: 'TNI/POLRI'
        }, {
          value: 'pensiunan',
          label: 'pensiunan'
        }, {
          value: 'swasta',
          label: 'swasta'
        }, {
          value: 'pelajar/mahasiswa',
          label: 'pelajar/mahasiswa'
        }, {
          value: 'Lain-Lain',
          label: 'Lain-Lain'
        }],

        status: [{
          value: 'Belum Menikah',
          label: 'Belum Menikah'
        }, {
          value: 'Nikah',
          label: 'Nikah'
        }, {
          value: 'Janda',
          label: 'Janda'
        }, {
          value: 'Duda',
          label: 'Duda'
        }],

        pembayaran: [{
          value: 'Tunai',
          label: 'Tunai'
        }, {
          value: 'Asuransi lain/ASKES',
          label: 'Asuransi lain/ASKES'
        }, {
          value: 'Jaminan Perusahaan',
          label: 'Jaminan Perusahaan'
        }, {
          value: 'Karyawan/Keluarga',
          label: 'Karyawan/Keluarga'
        }],

    }
  },
  computed: {
    pagedTableData() {
        // pagedTableData(dataTable, query) {
      // return this.tableData.slice(this.pageSize * this.page - this.pageSize, this.pageSize * this.page)
      return this.tableData.slice(this.pagesize * this.currentPage - this.pagesize, this.pagesize * this.currentPage)
    },
  },
  created() {
    this.getList()
  },
  methods: {
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
    onsubmit () {
        this.errors=[];
        
        this.result = Object.assign({}, this.post);

        if(this.post.nama && this.post.tempat_lahir && this.post.tgl_lahir&& this.post.jenis_kelamin && this.post.agama && this.post.pendidikan && this.post.pekerjaan && this.post.status && this.post.no_telepon && this.post.no_handphone && this.post.alamat && this.post.pembayaran ){
            this.errors.push("sukses");
            // axios.post('http://localhost:8000/api/customers', this.post)
            //     .then((response) => {
            //         //redirect page
            //     this.$router.push({
            //         name: 'CRUD'
            //     });
            //     location.reload();
            //     // console.log(response.data.data);
            //     }).catch(error => {
            //     this.validation = error.response.data.data;
            // });
        }
        
    },
    handleFilter() {
      // this.listQuery.page = 1
      // this.getList()
      // this.$router.go()

      // http://localhost:8000/api/customers/?email=tes@gmail.com
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
      //   axios.get('http://127.0.0.1:8000/api/customers').then(response => {
      //   this.tableData = response.data.data;
      //   });
      // Just to simulate the time of the request
      setTimeout(() => {
      this.listLoading = false
      }, 1.5 * 1000)
    },

    PostEdit(row){
      // console.log(index, row);
      this.$router.push(`/api/edit_pasien/${row}`);
    },
    handleDelete(row) 
    {
    //   this.$confirm(`Ini akan menghapus ID ${row} secara permanen. Lanjutkan?`, 'Peringatan',{
    //   confirmButtonText: 'OK',
    //   cancelButtonText: 'Cancel',
    //   type: 'warning'
    //   }).then(() => 
    //   {
    //       axios.delete(`http://127.0.0.1:8000/api/customers/${row}`)
    //       .then(response => {
    //           this.$router.push({
    //               name: 'CRUD'
    //           });
              
    //       // reload page
    //       this.$router.go()
    //       // refresh
    //       // this.$forceUpdate();

    //       this.tampilkanpesan("bisa hapus");
    //       console.log(response);

    //       })
          
    //       this.$message({
    //           type: 'success',
    //           message: `Hapus ID ${row} sukses`
    //       });

    //   }).catch(() => {
    //       this.tampilkanpesan("error gak bisa hapus");

    //       this.$message({
    //       type: 'info',
    //       message: `Hapus ID ${row} Gagal`
    //       });
    //   });
    },

    // tampilkanpesan(isipesan){
    //   // alert(isipesan);
    //   console.log(isipesan);
    // }
    
  }
}
</script>
