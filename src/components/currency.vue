<template>
  <div class="container">
    <div id="head">
      <img src="../assets/logo.png" alt="NBU_logo">
      <p>Currency exchange NBU</p>
    </div>
    <div id="form">
      <b-form-input id="dateInput" type="date" v-model="curDate" :max="curDate" @change="getOnUpdate()"></b-form-input>

      <div id="currencies">
        <div id="from">
          <b-form-select v-model="curFrom" @input="exchange()" class="select" >
            <option :key="currency.cc" v-for="currency in currencies" :value="currency.r030">{{currency.txt}} ({{currency.cc}})</option>
          </b-form-select>
          <b-form-input type="text" v-model="valFrom" @input="exchange()"></b-form-input>
        </div>
        <div id="swap">
          <font-awesome-icon icon="exchange-alt" class="fa-2x" @click="changePosition()"/>
        </div>
        <div id="to">
          <b-form-select v-model="curTo" @input="exchange()" class="select">
            <option :key="currency.cc" v-for="currency in currencies" :value="currency.r030">{{currency.txt}} ({{currency.cc}})</option>
          </b-form-select>
          <b-form-input type="text" v-model="valTo" disabled></b-form-input>
        </div>
      </div>
    </div>         
  </div> 
</template>

<script>

export default {
 
 data(){
   return{
      curDate: new Date().toJSON().slice(0,10),
      curDateRequest:"",
      curFrom: 980,//"Українська гривня (UAH)",
      curTo: 978,//"Євро (EUR)",
      valFrom: 0,
      valTo: 0,
      currencies:[]
   }
  },
  methods:{
    createDate() {
        this.curDateRequest = new Date().toJSON().slice(0,10).replace(/-/g,'');
    },
    getCurrency(){
      this.$http.get('https://bank.gov.ua/NBUStatService/v1/statdirectory/exchange?date='+this.curDateRequest+'&json').then(function(data){
               return data.json()
              
      }).then((data)=>{
               let curArray = data;
               this.currencies = curArray;
              
              let uahData = {
                "cc":"UAH",
                "exchangedate": this.curDate,
                "r030": 980,
                "rate": 1,
                "txt":"Українська гривня"
              }
              this.currencies.push(uahData);//added currency to array
      })
    },
    getOnUpdate(){
              this.curDateRequest = this.curDate.split('-').join('');
              console.log("get on upd" +this.curDateRequest);
              this.getCurrency();
              this.exchange();
    },
    selectedOption(currency) {
              if (currency) {
                this.selectedFrom.cc=currency.cc;
                this.selectedFrom.exchangedate=currency.exchangedate;
                this.selectedFrom.r030=currency.r030;
                this.selectedFrom.rate=currency.rate;
                this.selectedFrom.txt=currency.txt;
              }
    },
    exchange(){
              const fromObject = this.currencies.find( currency => currency.r030 === this.curFrom );
              const toObject = this.currencies.find( currency => currency.r030 === this.curTo );

          //  console.log(this.curFrom +"-> "+fromObject.rate);
          //  console.log(this.curTo +"-> "+toObject.rate );
              const fromRate = fromObject.rate;
              const toRate = toObject.rate;

              this.valTo = Math.ceil(((fromRate*this.valFrom)/toRate)*100)/100;  //calculate value anr ceil it
            
    },
    changePosition(){
              const a = this.curFrom;
              const b = this.curTo;

              this.curTo=a;
              this.curFrom=b;
              this.exchange();
    }
  },
  created(){
          this.createDate();
          this.getCurrency();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  .container{
    width:50%;
    margin: 100px auto;
    height: 700px;
    border-radius: 10px;
    background: white;
  }
  #head{
    display: grid;
    grid-template-columns: 1fr 1fr;
    height: 100px;
    align-content:center;
    margin: 30px auto;
    padding: 120px 0 40px 0;
  }
  img{
    width:200px;
    margin: 0 auto;
  }
  p{
    font-size: 1.5em;
    color:#4b4c4e;
    margin:0 100px 0 0;
    align-self: center;
  }
  #form{
    padding-top: 30px;
  }
  #dateInput{
    width: 350px;
    margin: 0 20px 20px 20px;
  }
  #currencies{
    display:grid; 
    grid-template-columns: 5fr 1fr 5fr;
    margin: 0 20px;
  }
  .select{
      margin-bottom: 10px;
  }


  @media only screen and (max-width: 1680px)  {
    .container{
    width:60%;
    margin: 80px auto;
    height: 600px; 
  }
  }
  @media only screen and (max-width: 1440px)  {
    .container{
      width:65%;
      margin: 80px auto;
      height: 600px;  
    }
  }
  @media only screen and (max-width: 1280px)  {
    .container{
      width:85%;
      margin: 80px auto;
      height: 600px;     
    }
}
@media only screen and (max-width: 1024px)  {
    .container{
      width:90%;
      margin: 80px auto;
      height: 600px;
    }
    #head{
      grid-template-columns: 1fr; 
    }
    p{
      font-size: 1.5em;
      margin:20px auto;
      align-self: center;
    }
    #dateInput{
      width: 250px;
    }
}
@media only screen and (max-width: 768px)  {
    .container{
      max-width: 95%;
      width:95%;
      margin: 80px auto;
      height: 600px; 
    }
    #currencies{
      display:grid;
      grid-gap: 10px;
      grid-template-columns: 1fr ;
      margin: 0 5px;
    }
    #dateInput{
      width: 200px;
      margin: 0 5px 20px 5px; 
    }
    #head{
      grid-template-columns: 1fr; 
    }
    p{
      font-size: 1.2em;
      margin:20px auto;
      align-self: center;
    }
}
@media only screen and (max-width: 703px)  {
    .container{
      max-width: 95%;
      width:95%;
      margin: 20px auto;
      height: max-content;
      padding-bottom: 50px; 
    }
}
@media only screen and (max-width: 414px)  {
    .container{
      width:95%;
      margin: 20px auto;
      height: max-content;
      padding-bottom: 30px;  
    }
}

</style>
