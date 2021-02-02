<template>
<div id="hello">
  <div id="title">
  <h2 class="qq">Калькулятор сети</h2>
  </div>
  <div>
  <div>
  <b-form inline @submit="onSubmit">
    <label for="inline-form-input-name">IPv4 адрес: </label>
    <b-form-input
      id="inline-form-input-name"
      class="mb-2 mr-sm-2 mb-sm-0"
      v-model="form.ipv4"
    ></b-form-input>

    <label  for="inline-form-input-username">Маска:  </label>
    <b-form-select v-model="form.mask" :options="options"></b-form-select>
    <b-button type="submit" variant="primary" :disabled="$v.$invalid">Расчитать</b-button>
  </b-form>
</div>
  <div class="under">
  <b-table-simple hover small striped responsive bordered>
    <b-thead head-variant="dark">
      <b-tr>
        <b-th>Имя</b-th>
        <b-th>Значение</b-th>
        <b-th>Бинарное значение</b-th>
        <b-th>16-ричный код</b-th>
      </b-tr>
    </b-thead>
    <b-tbody>
      <b-tr>
        <b-td>Address:</b-td>
        <b-td>{{ipv4.join(".")}}</b-td>
        <b-td>{{ipv4bin.join(".")}}</b-td>
        <b-td>{{ipv4_16.join(".")}}</b-td>
      </b-tr>
      <b-tr>
        <b-td>Mask</b-td>
        <b-td>{{mask.join(".")}}</b-td>
        <b-td>{{maskbin.join(".")}}</b-td>
        <b-td>{{mask_16.join(".")}}</b-td>
      </b-tr>
      <b-tr>
        <b-td>Network:</b-td>
        <b-td>{{network.join(".")}}</b-td>
        <b-td>{{networkbin.join(".")}}</b-td>
        <b-td>{{network_16.join(".")}}</b-td>
      </b-tr>
      <b-tr>
        <b-td>Wildcard:</b-td>
        <b-td>{{wildcard.join(".")}}</b-td>
        <b-td>{{wildcardbin.join(".")}}</b-td>
        <b-td>{{wildcard_16.join(".")}}</b-td>
      </b-tr>
      <b-tr>
        <b-td>Broadcast:</b-td>
        <b-td>{{broadcast.join(".")}}</b-td>
        <b-td>{{broadcastbin.join(".")}}</b-td>
        <b-td>{{broadcast_16.join(".")}}</b-td>
      </b-tr>
      <b-tr>
        <b-td>HostMin:</b-td>
        <b-td>{{validator('hostmin')}}</b-td>
        <b-td>{{validator('hostminbin')}}</b-td>
        <b-td>{{validator('hostmin_16')}}</b-td>
      </b-tr>
      <b-tr>
        <b-td>HostMax:</b-td>
        <b-td>{{validator1('hostmax')}}</b-td>
        <b-td>{{validator1('hostmaxbin')}}</b-td>
        <b-td>{{validator1('hostm_16')}}</b-td>
      </b-tr>
      <b-tr>
        <b-td>Hosts/Net:</b-td>
        <b-td>{{Hosts}}</b-td>
      </b-tr>
    </b-tbody>
  </b-table-simple>
  </div>
</div>


</div>

</template>

<script>
import { required, minLength } from 'vuelidate/lib/validators'
  export default {
    data() {
      return{
        form:{
          ipv4:'',
          mask:''
        },
        n:null,
        ipv4:[],
        ipv4bin:[],
        ipv4_16:[],
        mask:[],
        maskbin:[], 
        mask_16:[], 
        network:[], //[192,168,0,0]
        networkbin:[],//["101101001",101001101,0000000,0000000]
        network_16:[],
        wildcard:[],
        wildcardbin:[],
        wildcard_16:[],
        broadcast:[],
        broadcastbin:[],
        broadcast_16:[],
        HostMin:[],
        HostMinbin:[],
        HostMin_16:[],
        HostMax:[],
        HostMaxbin:[],
        HostMax_16:[],
        Hosts:null,
        zero:null,
        options: [
          { value: '0.0.0.0', text: '0.0.0.0' },
           { value: '128.0.0.0', text: '128.0.0.0' },
           { value: '192.0.0.0', text: '192.0.0.0' },
           { value: '224.0.0.0', text: '224.0.0.0' },
           { value: '240.0.0.0', text: '240.0.0.0' },
            { value: '248.0.0.0', text: '248.0.0.0' },
           { value: '252.0.0.0', text: '252.0.0.0' },
           { value: '254.0.0.0', text: '254.0.0.0' },
           { value: '255.0.0.0', text: '255.0.0.0' },
          { value: '255.128.0.0', text: '255.128.0.0' },
           { value: '255.192.0.0', text: '255.192.0.0' },
           { value: '255.224.0.0', text: '255.224.0.0' },
           { value: '255.240.0.0', text: '255.240.0.0' },
         { value: '255.248.0.0', text: '255.248.0.0' },
          { value: '255.252.0.0', text: '255.252.0.0' },
          { value: '255.254.0.0', text: '255.254.0.0' },
          { value: '255.255.0.0', text: '255.255.0.0' },
          { value: '255.255.128.0', text: '255.255.128.0' },
          { value: '255.255.192.0', text: '255.255.192.0' },
          { value: '255.255.224.0', text: '255.255.224.0' },
          { value: '255.255.240.0', text: '255.255.240.0' },
          { value: '255.255.248.0', text: '255.255.248.0' },
          { value: '255.255.252.0', text: '255.255.252.0' },
          { value: '255.255.254.0', text: '255.255.254.0' },
          { value: '255.255.255.0', text: '255.255.255.0' },
          { value: '255.255.255.128', text: '255.255.255.128' },
          { value: '255.255.255.192', text: '255.255.255.192' },
          { value: '255.255.255.224', text: '255.255.255.224' },
          { value: '255.255.255.240', text: '255.255.255.240' },
          { value: '255.255.255.248', text: '255.255.255.248' },
          { value: '255.255.255.252', text: '255.255.255.252' },
          { value: '255.255.255.254', text: '255.255.255.254' },
          { value: '255.255.255.255', text: '255.255.255.255' },

        ]

      }
    },
    validations: {
    form: {
      ipv4:{
        required,
        minLength: minLength(4),
        validNumber: val =>  /^(25[0-5]|2[0-4][0-9]|1?[0-9][0-9]{1,2})(\.(25[0-5]|2[0-4][0-9]|1?[0-9]{1,2})){3}$/.test(val),
      },
      mask:{
        required,
      },
    }
    },
    methods: {
      onSubmit(event) {
        event.preventDefault()
        this.$v.$touch()
        this.reset();
        let a =  this.form.ipv4.split('.');
        this.ipv4bin = a.map(function (x) {  return parseInt(x, 10).toString(2).padStart(8,"0");});
        this.ipv4_16 = a.map(function (x) {  return parseInt(x, 10).toString(16).padStart(2,"0").toUpperCase();});
        this.ipv4 = a.map(function (x) {  return parseInt(x, 10).toString();});
        let b =  this.form.mask.split('.');
        this.maskbin = b.map(function (x) {  return parseInt(x, 10).toString(2).padStart(8,"0");});
        this.mask_16 = b.map(function (x) {  return parseInt(x, 10).toString(16).padStart(2,"0").toUpperCase();});
        this.mask = b.map(function (x) {  return parseInt(x, 10).toString();});
         //Hosts
        if(this.form.mask !='255.255.255.255' | '255.255.255.254'){
          this.n=0;
          let f = this.maskbin.join('');
          for(let i=0; i<f.length; i++){
            if(f[i]=='1'){this.n++;}
          }
          this.zero = 32- this.n;
          this.Hosts = Math.pow(2,this.zero)-2
          }else{
            this.Hosts = "N/A"
        }
        
        //Network
        console.log(parseInt(this.ipv4bin[3], 2) & parseInt(this.maskbin[3], 2));
        for(let i=0; i<4; i++){
          this.network.push(parseInt(this.ipv4bin[i], 2) & parseInt(this.maskbin[i], 2));
        }
        console.log(this.network[3]);
        this.networkbin = this.network.map(function (x) {  return parseInt(x, 10).toString(2).padStart(8,"0");});
        this.network_16 = this.network.map(function (x) {  return parseInt(x, 10).toString(16).padStart(2,"0").toUpperCase();});
        // Wildcard
        let g;
        for(let i=0; i<4; i++){
          g = this.maskbin[i].replace(/1/gi,"2");
          g = g.replace(/0/gi,"1");
          g = g.replace(/2/gi,"0");
          this.wildcardbin.push(g);
        }
        this.wildcard = this.wildcardbin.map(function (x) {  return parseInt(x, 2).toString();});
        this.wildcard_16 =this.wildcard.map(function (x) {  return parseInt(x, 10).toString(16).padStart(2,"0").toUpperCase();});
        //Broadcast
        for(let i=0; i<4; i++){
          this.broadcast.push(this.network[i] | this.wildcard[i]);
        }
        this.broadcastbin = this.broadcast.map(function (x) {  return parseInt(x, 10).toString(2).padStart(8,"0");});
        this.broadcast_16 = this.broadcast.map(function (x) {  return parseInt(x, 10).toString(16).padStart(2,"0").toUpperCase();});
        //Hostmin
        if(this.form.mask !='255.255.255.255' && this.form.mask !='255.255.255.254'){
        this.HostMin = this.network.slice();
        this.HostMin[3] = (parseInt(this.network[3], 10) + 1);
        this.HostMinbin = this.HostMin.map(function (x) {  return parseInt(x, 10).toString(2).padStart(8,"0");});
        this.HostMin_16 = this.HostMin.map(function (x) {  return parseInt(x, 10).toString(16).padStart(2,"0").toUpperCase();});
        }else{
          this.HostMin = 'N/A';
          this.HostMinbin = 'N/A';
          this.HostMin_16 = 'N/A';
          console.log("111")
        }
        //Hostmax
        if(this.form.mask !='255.255.255.255' && this.form.mask !='255.255.255.254'){
        this.HostMax = this.broadcast.slice();
        this.HostMax[3] = (parseInt(this.broadcast[3], 10) - 1);
        this.HostMaxbin = this.HostMax.map(function (x) {  return parseInt(x, 10).toString(2).padStart(8,"0");});
        this.HostMax_16 = this.HostMax.map(function (x) {  return parseInt(x, 10).toString(16).padStart(2,"0").toUpperCase();});
        }else{
          this.HostMax = 'N/A';
          this.HostMaxbin = 'N/A';
          this.HostMax_16 = 'N/A';
        }
      },
      reset(){
this.n=null,
this.ipv4bin=[],
this.ipv4_16=[],
this.maskbin=[],
this.mask_16=[],
this.network=[], //[192,168,0,0]
this.networkbin=[],//["101101001",101001101,0000000,0000000]
this.network_16=[],
this.wildcard=[],
this.wildcardbin=[],
this.wildcard_16=[],
this.broadcast=[],
this.broadcastbin=[],
this.broadcast_16=[],
this.HostMin=[],
this.HostMinbin=[],
this.HostMin_16=[],
this.HostMax=[],
this.HostMaxbin=[],
this.HostMax_16=[],
this.Hosts=null,
this.zero
}, 
 validator(id){
   if(id=="hostmin"){
     if(this.HostMin instanceof Array ){ 
       return this.HostMin.join(".")
      }
      else{
        console.log("mass")
         return this.HostMin
      };
   }
   if(id=="hostminbin"){
     if(this.HostMinbin instanceof Array ){ 
       return this.HostMinbin.join(".")
      }
      else{
         return this.HostMinbin
      };
   }
   if(id=="hostmin_16"){
     if(this.HostMin_16 instanceof Array ){ 
       return this.HostMin.join(".")
      }
      else{
         return this.HostMin_16
      };
   }
 },
 validator1(id){
   if(id=="hostmax"){
     if(this.HostMax instanceof Array ){ 
       return this.HostMax.join(".")
      }
      else{
         return this.HostMax
      };
   }
   if(id=="hostmaxbin"){
     if(this.HostMaxbin instanceof Array ){ 
       return this.HostMaxbin.join(".")
      }
      else{
         return this.HostMaxbin
      };
   }
   if(id=="hostmax_16"){
     if(this.HostMax_16 instanceof Array ){ 
       return this.HostMax.join(".")
      }
      else{
         return this.HostMax_16
      };
   }
 }
    }
  }
</script>

<style>
#hello {
  width: 1000px;
  margin:0 auto;
  background-color: #FAEBD7;
  border-radius: 15px;
  padding-bottom: 15px;
  padding-left: 5px;
  padding-right: 5px;
  margin-top: 15px;
}
#title{
text-align: center;
overflow: hidden;
}
.qq{
  margin-top:40px;
  margin-bottom:40px;
}
.under{
  background-color: white;
}
</style>