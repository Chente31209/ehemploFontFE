<template>
  <div>
    <div id="Formulario" class="container">
      <div id="Nobre">
        <div class="form-group">
          <fieldset>
            <label class="form-label mt-4" for="readOnlyInput">RFC</label>
            <input class="form-control"  type="text"  v-model="rfc">
          </fieldset>
        </div>
      </div>
      <div id="PassWord">
        <div class="form-group">
          <fieldset>
            <label class="form-label mt-4" for="readOnlyInput">Password</label>
            <input class="form-control"  type="password" v-model="password">
          </fieldset>
        </div>
      </div>
      <div id=".key">
        <div class="form-group">
          <label for="formFile" class="form-label mt-4">sube el archivo .key</label>
          <input class="form-control" type="file" accept=".key" id="key" >
        </div>
      </div>
      <div id=".car">
        <div class="form-group">
          <label for="formFile" class="form-label mt-4">sube el archivo .cer</label>
          <input class="form-control" type="file" accept=".cer" id="cer" >
        </div>
      </div>
      <div id="SUMIT">
      <button class="btn btn-primary" @click="btnEncriprar()"> Enviar </button>
    </div>
    </div>
    
  </div>
</template>

<script>

export default {
  name: 'App',
  data:function() {
    return {
      rfc:"",
    password:"",
    Eemail:"",
    Epassword:"",
    pkey:null,
    pcer:null,
    url : "http://trsffirmadigitalserviciocertificados-dev.eba-qrs52xqc.us-west-1.elasticbeanstalk.com/"
    
    }
  },
  methods:{
    btnEncriprar(){
      this.Epassword=this.$CryptoJS.AES.encrypt(this.password, "PaoabraSeceta",{ iv: this.password }).toString()
      console.log(this.Epassword )
      
      this.$http.get(this.url+"Certificados/GetKeyToUploadCertificate?rfc="+this.rfc).then((response) => {
        
        this.pkey = document.getElementById("key").files[0]
        this.pcer = document.getElementById("cer").files[0]
        
         

        this.$http.post(this.url+"Certificados",{
          RFC:this.rfc,
          UserId:response.data,
          Password:this.Epassword,
          filecer:this.pcer,
          filekey:this.pkey
        }).then((res)=>{
          console.log(res)
        })

      })
      

      
      


      
    }
  }
  
}
</script>
