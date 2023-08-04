<template>
  <div id="home" class="divcol">
    <v-row class="center" style="max-width: 500px!important; min-width: 320px!important;">
      <v-col cols="12" class="center">
        <v-card class="card" style="background-color: var(--secondary)!important;">
            <h2 class="p">WALLET</h2>
            <span>{{ wallet }}</span>
        </v-card>
      </v-col>

      <hr>

      <v-col cols="12" class="center">
        <v-card class="card">
          <img src="~/assets/sources/icons/Bloque.svg" alt="Bloque" class="mb-2">
          <h2 class="p">{{ blocks | numericFormat(numericFormatConfig) }} BLOCKS</h2>
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

    <v-row class="center" style="max-width: 500px!important; min-width: 320px!important;  margin-top: 0px!important;">
      <v-col cols="12" class="center">
        <v-card class="card">
          <img src="~/assets/sources/icons/roi.svg" alt="Bloque" class="mb-2">
          <h2 class="p">RET. DE INVERSION</h2>
          <span>${{ roi | numericFormat(numericFormatConfig) }}</span>
          <v-btn class="btn mt-3" @click="$store.state.isLogged == false? $store.dispatch('modalConnect') : alertConnect()">RETIRAR</v-btn>
        </v-card>
      </v-col>

      <v-col cols="12" class="center">
        <v-card class="card">
          <img src="~/assets/sources/icons/residual.svg" alt="Bloque" class="mb-2">
          <h2 class="p">BONO RESIDUAL</h2>
          <span>${{ bonoResidual | numericFormat(numericFormatConfig) }}</span>
          <v-btn class="btn mt-3" @click="$store.state.isLogged == false? $store.dispatch('modalConnect') : alertConnect()">RETIRAR</v-btn>
        </v-card>
      </v-col>

      <v-col cols="12" class="center">
        <v-card class="card">
          <img src="~/assets/sources/icons/Referido.svg" alt="Bloque" class="mb-2">
          <h2 class="p">BONO REFERIDOS</h2>
          <span>${{ bonoReferidos | numericFormat(numericFormatConfig) }}</span>
          <v-btn class="btn mt-3" @click="$store.state.isLogged == false? $store.dispatch('modalConnect') : alertConnect()">RETIRAR</v-btn>
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

    <v-row class="center" style="max-width: 500px!important; min-width: 320px!important; margin-top: 0px!important; align-items: flex-start;">
      <v-col cols="12" class="center">
        <v-card class="card" style="background-color: var(--tertiary)!important; padding-block: 10px!important;">
            <div class="div-blue">
              <div class="divrow center" style="gap: 10px; max-height: 40px;">
                <h2 style="font-size: 16px!important;">1 BLKS ($50)</h2>
                <div class="vertical"></div>
                <h2 style="font-size: 16px!important;">ACTIVE</h2>
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
               27 de Julio del 2025 - 00:02:05<br>GMT-5 (hora oficial de Infinity Bloks) 
            </span>
        </v-card>
      </v-col>

      <v-col cols="12" class="center">
        <v-card class="card" style="background-color: var(--tertiary)!important; padding-block: 10px!important;">
            <div class="div-blue">
              <div class="divrow center" style="gap: 10px; max-height: 40px;">
                <h2 style="font-size: 16px!important;">$250 - RESIDUAL</h2>
                <div class="vertical"></div>
                <h2 style="font-size: 16px!important;">ACTIVE</h2>
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
               27 de Julio del 2025 - 00:02:05<br>GMT-5 (hora oficial de Infinity Bloks) 
            </span>
        </v-card>
      </v-col>

      <v-col cols="12" class="center">
        <v-card class="card" style="background-color: var(--tertiary)!important; padding-block: 10px!important;">
            <div class="div-blue">
              <div class="divrow center" style="gap: 10px; max-height: 40px;">
                <h2 style="font-size: 16px!important;">20 BLKS ($1000)</h2>
                <div class="vertical"></div>
                <h2 style="font-size: 16px!important;">ACTIVE</h2>
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
               27 de Julio del 2025 - 00:02:05<br>GMT-5 (hora oficial de Infinity Bloks) 
            </span>
        </v-card>
      </v-col>

      <v-col 
      v-for="item in depositos"
      :key="item.fecha"
      cols="12"
      class="center"
      >
        <v-card class="card" style="background-color: var(--tertiary)!important; padding-block: 10px!important;">
            <div class="div-blue">
              <div class="divrow center" style="gap: 10px; max-height: 40px;">
                <h2 style="font-size: 16px!important;">{{item.monto}} BLKS ($1000)</h2>
                <div class="vertical"></div>
                <h2 style="font-size: 16px!important;">{{item.estado}}</h2>
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
              {{item.tiempo}} (hora oficial de Infinity Bloks) 
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
const walletPruebas = '0x981374dE858078c0be8c0Bb51c4cDCe6393a7405'

export default {
  name: "HomePage",
  data() {
    return {
      numericFormatConfig: {
        decimalSeparator: ".",
        fractionDigitsMax: 2,
        fractionDigitsMin: 2,
        fractionDigitsSeparator: "",
        thousandsDigitsSeparator: ","
      },
      roi: 0,
      blocks: 0,
      invested: 0,
      bonoResidualActivo: 0,
      bonoReferidos: 0,
      bonoResidual: 0,
      active_slider: 60,
      depositos: [],
      wallet: localStorage.getItem("wallet") === null ? "": localStorage.getItem("wallet"),
    }
  },
  head() {
    const title = 'Home';
    return {
      title,
    }
  },
  mounted() {
    this.getBLKS()
    this.getROI()
    this.getInvestors()
    this.getAdInfinity()
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
    async getBLKS() {
      this.loading = true
      const tokenContract = new web3.eth.Contract(contractAbi, infinityBlocksAddres);
      try {
        const blks = await tokenContract.methods.blokes(localStorage.getItem("wallet"), 0).call({from: localStorage.getItem("wallet")})
        console.log(Number.isInteger(blks) ? blks : "")
        const formatedBlks = await blks[2] / Math.pow(10, 18)
        this.blocks = formatedBlks
      } catch (error) {
        console.log(error+"getBLKS")
        this.blocks = 0
      }
      
    },

    async getInvestors() {
      const tokenContract = new web3.eth.Contract(contractAbi, infinityBlocksAddres);
      try {
        const investors = await tokenContract.methods.investors(localStorage.getItem("wallet")).call({from: localStorage.getItem("wallet")})
        console.log(investors)
        console.log("---------------------------- investors")
        this.invested = investors.invested / Math.pow(10, 18)
        this.bonoReferidos = investors.balanceRef / Math.pow(10, 18)
      } catch (error) {
        console.log(error+"getInvestors")
        this.invested = 0
      }
      
    },

    async getROI() {
      try {
        const tokenContract = new web3.eth.Contract(contractAbi, infinityBlocksAddres);
        const adRoi = await tokenContract.methods.adRoi(localStorage.getItem("wallet")).call({from: localStorage.getItem("wallet")})
        this.roi = adRoi / Math.pow(10, 18)
      } catch (error) {
        this.roi = 0
      }
    },

    async getAdInfinity() {
      try {
        const tokenContract = new web3.eth.Contract(contractAbi, infinityBlocksAddres);
        const adInfinity = await tokenContract.methods.adInfinity(localStorage.getItem("wallet")).call({from: localStorage.getItem("wallet")})
        this.bonoResidual = adInfinity / Math.pow(10, 18)
      } catch (error) {
        this.bonoResidual = 0
      }
    },
    
    async withdrawBonoResidual() {
      const tokenContract = new web3.eth.Contract(contractAbi, infinityBlocksAddres);
      try {
        await tokenContract.methods.Withdraw(localStorage.getItem("wallet")).send({from: localStorage.getItem("wallet")})
      } catch (error) {
        console.log(error)
      }
    },

    async withdrawBonoReferidos() {
      const tokenContract = new web3.eth.Contract(contractAbi, infinityBlocksAddres);
      const bonoReferidos = await tokenContract.methods.withdraw2(localStorage.getItem("wallet")).send({from: localStorage.getItem("wallet")})
      return bonoReferidos
    },

    async withdrawTeam() {
      const tokenContract = new web3.eth.Contract(contractAbi, infinityBlocksAddres);
      const depositos = await tokenContract.methods.withdrawTeam(localStorage.getItem("wallet")).send({from: localStorage.getItem("wallet")})
      return depositos
    },

    async getDepositos() {
      const tokenContract = new web3.eth.Contract(contractAbi, infinityBlocksAddres);
      const historialDepositos = await tokenContract.methods.depositos(localStorage.getItem("wallet"), true).call({from: walletPruebas})
      console.log(historialDepositos)
      console.log("-----------historialDepositos")
      console.log("antes del for")
      for(let i = historialDepositos[0].length -5; i < historialDepositos[0].length-1; i++) {
        const monto = historialDepositos[0][i]
        const tiempo = new Date(historialDepositos[1][i] * 1000)
        const estado = historialDepositos[2][i]
        console.log(monto,"monto", tiempo,"tiempo", estado,"estado","assdadasdas")
        this.depositos.push({monto, tiempo, estado}) 
      }
      console.log("despues del for")
      console.log(this.depositos)
      console.log("-------------- depositos")
    },
    
    
  }
};
</script>

<style src="~/assets/styles/pages/index.scss" lang="scss" />
