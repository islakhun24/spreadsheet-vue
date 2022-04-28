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
    options: {},
  }),
  mounted: function () {
 

    fetch('https://0f6c-36-73-34-68.ap.ngrok.io/api/v1/order/to-sheets')
      .then(response => response.json())
      .then(json => {
        return json.data;
      }).then(json => {
        // spreadsheet.setData(json)
        console.log(json);
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
      { type: 'dropdown', title: 'Kirim dari', width: '120px',  source: json.addresses },
      { type: 'text', title: 'Nama Pembeli', width: '250px'},
      { type: 'numeric', title: 'Nomor HP', width: '100px'},
      { type: 'numeric', title: 'Code Pos', width: '100px'},
      { type: 'text', title: 'Alamat Detail', width: '300px'},
      { type: 'dropdown', title: 'Product', width: '120px',  source: json.products},
      { type: 'dropdown', title: 'Variasi', width: '120px',  source: json.variant },
      { type: 'numeric', title: 'Kuantitas', width: '60px' },
      { type: 'dropdown', title: 'Metode Pembayaran', width: '120px',  source: json.payment_method },
      { type: 'dropdown', title: 'Expedisi', width: '120px',  source: json.shipments},
      { type: 'numeric', title: 'Nilai Pembayaran', width: '150px', mask: 'Rp #.###,00', decimal: ',' }
    ]
  }

    let spreadsheet = jexcel(this.$el, this.options)
    Object.assign(this, { spreadsheet })
      })
  }
}
</script>