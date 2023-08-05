<template>
  <div id="home" class="divcol">
    <v-row class="center" style="max-width: 500px!important; min-width: 320px!important; align-items: flex-start; max-height: 600px!important;">
      <v-col cols="12" class="center">
        <v-card class="card" style="background-color: var(--secondary)!important;">
            <h2 class="p">WALLET</h2>
            <span>{{ wallet }}...</span>
        </v-card>
      </v-col>

      <hr>

      <v-col cols="12" class="center">
        <v-card class="card">
          <img src="~/assets/sources/icons/Bloque.svg" alt="Bloque" class="mb-2">
          <h2 class="p">{{ blocks }} BLOCKS</h2>
          <span>${{invested | numericFormat(numericFormatConfig)}}</span>
        </v-card>
      </v-col>
      
      <hr class="delete-mobile">

      <v-col cols="12" class="center delete-mobile">
        <v-card class="card" style="background-color: var(--secondary)!important;">
          <h2 class="p tcenter">BONO RESIDUAL<br>ACTIVO</h2>
          <v-sheet class="sheet-white mt-3 center">
            <span>${{ bonoResidualActivo | numericFormat(numericFormatConfig) }}</span>
          </v-sheet>
        </v-card>
      </v-col>
    </v-row>

    <v-row class="center" style="max-width: 500px!important; min-width: 320px!important; margin-top: -13px!important; max-height: 740px;">
      <v-col cols="12" class="center">
        <v-card class="card" style="max-height: 215px!important;">
          <img src="~/assets/sources/icons/roi.svg" alt="Bloque" class="mb-2">
          <h2 class="p">RET. DE INVERSION</h2>
          <span>${{ roi | numericFormat(numericFormatConfig) }}</span>
          <v-btn class="btn mt-3" @disabled="registered" @click="withdrawROI()">RETIRAR</v-btn>
        </v-card>
      </v-col>

      <v-col cols="12" class="center">
        <v-card class="card">
          <img src="~/assets/sources/icons/residual.svg" alt="Bloque" class="mb-2">
          <h2 class="p">BONO RESIDUAL</h2>
          <span>${{ bonoResidual | numericFormat(numericFormatConfig) }}</span>
          <v-btn class="btn mt-3" @disabled="registered" @click="withdrawBonoResidual()">RETIRAR</v-btn>
        </v-card>
      </v-col>

      <v-col cols="12" class="center">
        <v-card class="card">
          <img src="~/assets/sources/icons/Referido.svg" alt="Bloque" class="mb-2">
          <h2 class="p">BONO REFERIDOS</h2>
          <span>${{ bonoReferidos | numericFormat(numericFormatConfig) }}</span>
          <v-btn class="btn mt-3" @disabled="registered" @click="withdrawReferidos()">RETIRAR</v-btn>
        </v-card>
      </v-col>

      <hr class="show-mobile">

      <v-col cols="12" class="center show-mobile">
        <v-card class="card" style="background-color: var(--secondary)!important;">
          <h2 class="p tcenter">BONO RESIDUAL<br>ACTIVO</h2>
          <v-sheet class="sheet-white mt-3 center">
            <span>${{ bonoResidualActivo | numericFormat(numericFormatConfig) }}</span>
          </v-sheet>
        </v-card>
      </v-col>

      <hr class="show-mobile">
    </v-row>

    <v-row class="center" style="max-width: 500px!important; min-width: 320px!important; margin-top: -13px!important; align-items: flex-start;">

      <v-col 
      v-for="item in depositos"
      :key="item.fechaFinalizacion"
      cols="12"
      class="center"
      >
        <v-card v-show="item.monto" class="card" style="background-color: var(--tertiary)!important; padding-block: 10px!important;">
            <div class="div-blue">
              <div class="divrow center" style="gap: 10px; max-height: 40px;">
                <h2 style="font-size: 16px!important;">{{ item.monto | numericFormat(numericFormatConfig) }} BLKS (${{ valorBLKS }})</h2>
                <div class="vertical"></div>
                <h2 style="font-size: 16px!important;">{{ item.estado }}</h2>
              </div>
              <v-slider
              v-model="active_slider"
              color="var(--primary)"
              thumb-label="none" 
              hide-details 
              class="slider"
              readonly
               ></v-slider>
            </div>

            <span class="tcenter mt-2" style="color: var(--secondary); font-size: 14px!important;">
              <span class="bold tcenter" style="color: var(--secondary); font-weight: 700!important;">Finaliza:</span> 
              {{item.fechaFinalizacion}}  
            </span>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
import contractAbi from '~/static/ABIS/infinity_blocks_abi.json';
const Web3 = require('web3');
const web3 = new Web3(window.ethereum);
const infinityBlocksAddres = '0x2A97A853261e6338a0663f17e81fC3d6dF9e4f41';
// const walletPrueba = '0x981374dE858078c0be8c0Bb51c4cDCe6393a7405'
 const wallet1 = '0x6924Ff38aDFd2E93dD29c603c762650d4e5981e7'
// const wallet2 = '0x11907e222f22b659f9F2192d2aAF1c26bDeAc2DB'
const precioBLKS = 50

export default {
  name: "HomePage",
  data() {
    return {
      numericFormatConfig: {
        decimalSeparator: ".",
        fractionDigitsMax: 2,
        fractionDigitsMin: 0,
        fractionDigitsSeparator: "",
        thousandsDigitsSeparator: ","
      },
      roi: 0,
      blocks: 0,
      invested: 0,
      registered: false,
      bonoResidualActivo: 0,
      bonoReferidos: 0,
      bonoResidual: 0,
      active_slider: 60,
      valorBLKS: 0,
      depositos: [],
      wallet: localStorage.getItem("wallet") === null ? "": localStorage.getItem("wallet").substring(1, 20),
    }
  },
  head() {
    const title = 'Home';
    return {
      title,
    }
  },
  mounted() {
    this.getAccount()
    this.getROI()
    this.getUserData()
    this.getBonoResidual()
    this.getDepositos()
    if (localStorage.getItem("wallet") !== null) {
      this.updateWallet();
    }
  },
  methods: {
    async getAccount() {
      const accounts = await window.ethereum
        .request({ method: 'eth_requestAccounts' })
        .catch((err) => {
          if (err.code === 4001) {
            this.$alert('error',{desc:"Por favor conecta con metamask para poder utilizar la Dapp"})
          } else {
            console.error(err)
          }
        })
      const account = accounts[0]
      localStorage.setItem('isLogged', window.ethereum.isConnected())
      if(localStorage.getItem('wallet') !== account){
        localStorage.setItem('wallet', account)
        window.location.reload();
      }
      localStorage.setItem('wallet', account)
    },
    updateWallet() {
      window.ethereum.on('accountsChanged', (accounts) => {
        const currentAddress = accounts[0];
        currentAddress === undefined ? localStorage.removeItem("wallet") : localStorage.setItem("wallet", currentAddress);
        window.location.reload();
      });
    },
    alertConnect(){
      return this.$alert('error',{desc:"Su wallet ya esta conectada"})
    },

    async getUserData() {
      const tokenContract = new web3.eth.Contract(contractAbi, infinityBlocksAddres);
      
      try {
        const user = await tokenContract.methods.investors(localStorage.getItem('wallet')).call({from: localStorage.getItem('wallet')})
        console.log(user)
        console.log("---------------------------- investors")
        this.invested = user.invested / Math.pow(10, 18)
        this.blocks = this.invested / precioBLKS
        this.bonoResidual = user.balanceInfinit
        this.bonoReferidos = user.balanceRef / Math.pow(10, 18)
        this.registered = user.registered
        console.log(user.registered)
      } catch (error) {
        console.log(error+"getUserData")
        this.invested = 0
      }
      
    },

    async getROI() {
      try {
        const tokenContract = new web3.eth.Contract(contractAbi, infinityBlocksAddres);
        const roi = await tokenContract.methods.withdrawable(localStorage.getItem('wallet'), false).call({from: localStorage.getItem('wallet')})
        this.roi = roi / Math.pow(10, 18)
        console.log("---getROI----")
        console.log(roi)
        console.log("-------")
      } catch (error) {
        this.roi = 0
      }
    },

    async getBonoResidual() {
      try {
        const tokenContract = new web3.eth.Contract(contractAbi, infinityBlocksAddres);
        const bonoResidual = await tokenContract.methods.withdrawable(localStorage.getItem('wallet'), true).call({from: localStorage.getItem('wallet')})
        this.bonoResidual = bonoResidual / Math.pow(10, 18)
      } catch (error) {
        this.bonoResidual = 0
      }
    },
    
    async withdrawROI() {
      const tokenContract = new web3.eth.Contract(contractAbi, infinityBlocksAddres);
      try {
        await tokenContract.methods.withdraw().send({from: localStorage.getItem('wallet')})
      } catch (error) {
        console.log(error) 
      }
    },

    async withdrawBonoResidual() {
      try {
        const tokenContract = new web3.eth.Contract(contractAbi, infinityBlocksAddres);
        await tokenContract.methods.withdraw2().send({from: localStorage.getItem('wallet')})
      } catch (error) {
        console.log(error)
      }
    },

    async withdrawReferidos() {
      try {
        const tokenContract = new web3.eth.Contract(contractAbi, infinityBlocksAddres);
        await tokenContract.methods.withdrawTeam().send({from: localStorage.getItem('wallet')})
      } catch (error) {
        console.log(error)
      }

    },

    addDaysToDate(date, days){
      const res = date;
      res.setDate(res.getDate() + days);
      return res;
    },

    async getDepositos() {
      const diasFinalizacion = 900 
      try {
        const tokenContract = new web3.eth.Contract(contractAbi, infinityBlocksAddres);
        const depositos = await tokenContract.methods.depositos( wallet1, false).call({from:  wallet1})
        const depositosInfinit = await tokenContract.methods.depositos( wallet1, true).call({from:  wallet1})

        for(let i = 0 ; i < depositos[0].length; i++) {
          const monto = depositos[0][i] / Math.pow(10, 18)
          const fechaInicio = new Date(depositos[1][i] * 1000)
          const fechaFinalizacion = this.addDaysToDate(fechaInicio, diasFinalizacion)
          const estado = depositos[2][i] ? "ACTIVE" : "INACTIVE"
          const data = {monto, fechaFinalizacion, estado}
          this.depositos.push(data)
          console.log("-----fecha-----")
          console.log(this.addDaysToDate(fechaInicio, 900))
          console.log("----------")
        }
        for(let i = 0 ; i < depositosInfinit[0].length; i++) {
          const monto = depositosInfinit[0][i] / Math.pow(10, 18)
          const fechaInicio = new Date(depositosInfinit[1][i] * 1000)
          const fechaFinalizacion = this.addDaysToDate(fechaInicio, diasFinalizacion)
          const estado = depositosInfinit[2][i] ? "ACTIVE" : "INACTIVE"
          const data = {monto, fechaFinalizacion, estado}
          this.depositos.push(data)
          console.log("-----fecha-----")
          console.log(this.addDaysToDate(fechaInicio, 900))
          console.log("----------") 
        }
      } catch (error) {
        console.log(error)
      }
    },
    
  }
};
</script>

<style src="~/assets/styles/pages/index.scss" lang="scss" />
