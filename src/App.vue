<template>
  <b-container fluid>
    <b-col  class="mt-3">
      <div class="text-end">
          <button class="btn btn-danger"  @click="() => spreadsheet.deleteRow()" >
            Delete row
          </button>
          <button style="margin-left:3px" class="btn btn-primary"  @click="() => spreadsheet.insertRow()" >
            Add new row
          </button>
          <button style="margin-left:3px" class="btn btn-success"  @click="() => spreadsheet.insertRow()" >
            Save data
        </button>
      </div>
    </b-col>
    <b-col class="mt-2">
      <div>
          <div width="100%" id="app" ref="spreadsheet"></div>
      </div>
    </b-col>
  </b-container>
  
</template>

<script>
import jexcel from 'jexcel'
import 'jexcel/dist/jexcel.css'


export default {
  name: 'App',
  data: () => ({
    spreadsheet: null,
    options: {}
  }),
  mounted: function () {
 this.options = {
    data: [
      [''],
      ['']
    ],
    tableOverflow:true ,
    fullscreen:true,
    allowToolbar:true,
    columns: [
      { type: 'calendar', title: 'Tanggal Order', width: '250px' },
      { type: 'dropdown', title: 'Kirim dari', width: '120px',  source: [ 'Pusdiklat IT', 'Purbalingga'] },
      { type: 'text', title: 'Nama Pembeli', width: '250px'},
      { type: 'numeric', title: 'Nomor HP', width: '100px'},
      { type: 'numeric', title: 'Code Pos', width: '100px'},
      { type: 'text', title: 'Alamat Detail', width: '300px'},
      { type: 'dropdown', title: 'Product', width: '120px',  source: [ 'Baju', 'Celana'] },
      { type: 'dropdown', title: 'Variasi', width: '120px',  source: [ 'Baju - waran-hitam - ukuran L', 'Baju - waran-merah - ukuran XL'] },
      { type: 'numeric', title: 'Kuantitas', width: '60px' },
      { type: 'dropdown', title: 'Metode Pembayaran', width: '120px',  source: [ 'COD', 'BANK TRANSFER'] },
      { type: 'dropdown', title: 'Expedisi', width: '120px',  source: [ 'JNE', 'Si Cepat'] },
      { type: 'numeric', title: 'Nilai Pembayaran', width: '150px', mask: 'Rp #.###,00', decimal: ',' }
    ]
  }

    let spreadsheet = jexcel(this.$el, this.options)
    Object.assign(this, { spreadsheet })

    fetch('https://jsonplaceholder.typicode.com/users')
      .then(response => response.json())
      .then(json => {
        spreadsheet.setData(json)
      })
  }
}
</script>