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
          <h2 class="p">{{ getBLKS() }} BLOCKS</h2>
          <span>${{ getInvestors()}}</span>
        </v-card>
      </v-col>
      
      <hr class="delete-mobile">

      <v-col cols="12" class="center delete-mobile">
        <v-card class="card" style="background-color: var(--secondary)!important;">
          <h2 class="p tcenter">BONO RESIDUAL<br>ACTIVO</h2>
          <v-sheet class="sheet-white mt-3 center">
            <span>$250.00</span>
          </v-sheet>
        </v-card>
      </v-col>
    </v-row>

    <v-row class="center" style="max-width: 500px!important; min-width: 320px!important;  margin-top: 0px!important;">
      <v-col cols="12" class="center">
        <v-card class="card">
          <img src="~/assets/sources/icons/roi.svg" alt="Bloque" class="mb-2">
          <h2 class="p">RET. DE INVERSION</h2>
          <span>${{ getROI() }}</span>
          <v-btn class="btn mt-3" @click="$store.state.isLogged == false? $store.dispatch('modalConnect') : alertConnect()">RETIRAR</v-btn>
        </v-card>
      </v-col>

      <v-col cols="12" class="center">
        <v-card class="card">
          <img src="~/assets/sources/icons/residual.svg" alt="Bloque" class="mb-2">
          <h2 class="p">BONO RESIDUAL</h2>
          <span>$0.00</span>
          <v-btn class="btn mt-3" @click="$store.state.isLogged == false? $store.dispatch('modalConnect') : alertConnect()">RETIRAR</v-btn>
        </v-card>
      </v-col>

      <v-col cols="12" class="center">
        <v-card class="card">
          <img src="~/assets/sources/icons/Referido.svg" alt="Bloque" class="mb-2">
          <h2 class="p">BONO REFERIDOS</h2>
          <span>$50.00</span>
          <v-btn class="btn mt-3" @click="$store.state.isLogged == false? $store.dispatch('modalConnect') : alertConnect()">RETIRAR</v-btn>
        </v-card>
      </v-col>

      <hr class="show-mobile">

      <v-col cols="12" class="center show-mobile">
        <v-card class="card" style="background-color: var(--secondary)!important;">
          <h2 class="p tcenter">BONO RESIDUAL<br>ACTIVO</h2>
          <v-sheet class="sheet-white mt-3 center">
            <span>$250.00</span>
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
    </v-row>
  </div>
</template>

<script>
import faucetAbi from '~/static/ABIS/infinity_blocks_abi.json';
const Web3 = require('web3');
const web3 = new Web3(window.ethereum);
const infinityBlocksAddres = '0x2A97A853261e6338a0663f17e81fC3d6dF9e4f41';

export default {
  name: "HomePage",
  data() {
    return {
      active_slider: 60,
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
      const tokenContract = new web3.eth.Contract(faucetAbi, infinityBlocksAddres);
      try {
        const blks = await tokenContract.methods.blokes(localStorage.getItem("wallet"), 0).call({from: localStorage.getItem("wallet")})
        console.log(Number.isInteger(blks) ? blks : "")
        return Number.isInteger(blks) ? blks : ""
      } catch (error) {
        console.log(error+"getBLKS")
        return ""
      }
      
    },

    async getInvestors() {
      const tokenContract = new web3.eth.Contract(faucetAbi, infinityBlocksAddres);
      try {
        const investors = await tokenContract.methods.investors(localStorage.getItem("wallet")).call({from: localStorage.getItem("wallet")})
        return investors
      } catch (error) {
        console.log(error+"getInvestors")
        return 0
      }
      
    },

    async getROI() {
      const tokenContract = new web3.eth.Contract(faucetAbi, infinityBlocksAddres);
      const ROI = await tokenContract.methods.adRoi(localStorage.getItem("wallet")).call({from: localStorage.getItem("wallet")})
      return ROI
    },
    async withdrawBonoResidual() {
      const tokenContract = new web3.eth.Contract(faucetAbi, infinityBlocksAddres);
      const bonoResidual = await tokenContract.methods.Withdraw().call()
      return bonoResidual
    },
    async getAdInfinity() {
      const tokenContract = new web3.eth.Contract(faucetAbi, infinityBlocksAddres);
      const adInfinity = await tokenContract.methods.adInfinity().send()
      return adInfinity
    },
    async withdrawBonoReferidos() {
      const tokenContract = new web3.eth.Contract(faucetAbi, infinityBlocksAddres);
      const bonoReferidos = await tokenContract.methods.withdraw2().send()
      return bonoReferidos
    },
    async getDepositos() {
      const tokenContract = new web3.eth.Contract(faucetAbi, infinityBlocksAddres);
      const depositos = await tokenContract.methods.depositos().send()
      return depositos
    },
  }
};
</script>

<style src="~/assets/styles/pages/index.scss" lang="scss" />
