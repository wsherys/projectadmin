<template>
    <div class="app app-container">
        <!-- import file index CRUD -->
        <indexCRUD />

        <!-- dialog edit -->
        <el-dialog 
        title="Edit Data Pasien" 
        :visible.sync="formedit" 
        width="70%"
        :before-close="handleClose"
        >
            <!-- :key  diisi sesuai dengan objek data array. -->
            <div v-for="data in tableData" :key="data.id">
                <div v-if="data.id == id">
                    <form @submit.prevent="PostUpdate">
                        <!-- jika ada data yang sama akan muncul disini -->
                        <el-input type="submit" value="Simpan" ></el-input>
                    </form>
                </div>
            </div>

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

               <el-button class="filter-item" type="primary" @submit.prevent="onsubmit">
                Update
              </el-button>
            
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
                tableData: {},
                labelPosition: 'left',
                formLabelWidth: '160px',

                post: {},
                multipleSelection: [],
                formedit: true,
                id: this.$route.params.id, //this is the id from the browser

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

        // mounted () {
        //     axios
        //     .get(`http://127.0.0.1:8000/api/customers/${this.$route.params.id}`)
        //     .then(response => (
        //         this.post.name = response.data.data.name,
        //         this.post.email = response.data.data.email,
        //         this.post.address = response.data.data.address
        //     ))
        // },

        // computed: {
        //     pagedTableData() {
        //     return this.tableData.slice(this.pageSize * this.page - this.pageSize, this.pageSize * this.page)
        //     },
        // },

        // created() {
        //     this.getList()
        // },

        methods: {
            // handleSizeChange(size) {
            // this.pagesize = size;
            // },
            // handleCurrentChange(currentPage) {
            // this.currentPage = currentPage;
            // },
            
            // getList() {
            //     this.listLoading = true //data loading 
            //     //get data API with axios
            //     // axios.get('http://127.0.0.1:8000/api/customers').then(response => {
            //     // this.tableData = response.data.data;
            //     // });
            // // Just to simulate the time of the request
            // setTimeout(() => {
            //     this.listLoading = false
            //     }, 1.5 * 1000)
            // },
            
            handleClose(done) {
                this.$confirm('Apakah Anda yakin untuk menutup dialog ini?')
                .then(_ => {
                    done();
                    //refresh page CRUD index
                    this.$router.push({
                        name: 'DataPasien'
                    });
                    //refresh
                    // this.$forceUpdate();
                    this.$router.go()

                })
                .catch(_ => {});
            },
           
            PostUpdate () {
                this.errors=[];
                this.result = Object.assign({}, this.post);

                // if(this.post.address && this.post.name && this.post.email){

                //     axios.put(`http://127.0.0.1:8000/api/customers/${this.$route.params.id}`, this.post)
                //     .then((response) => {
                //         this.$router.push({
                //             name: 'CRUD'
                //         });
                //         //reload page
                //         this.$router.go()
                //         // console.log(response);
                //     }).catch(error => {
                //     this.validation = error.response.data.data;
                //     });
                // }
            },
            
        }
    }
</script>
