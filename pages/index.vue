<template>
  <div id="home" class="divcol">
    <v-row class="center"
      style="max-width: 500px!important; min-width: 320px!important; align-items: flex-start; max-height: 600px!important;">
      <v-col cols="12" class="center">
        <v-card class="card" style="background-color: var(--secondary)!important;">
          <h2 class="p">WALLET</h2>
          <span class="wallet-text">{{ wallet }}</span>
        </v-card>
      </v-col>

      <hr>

      <v-col cols="12" class="center">
        <v-card class="card">
          <img src="~/assets/sources/icons/Bloque.svg" alt="Bloque" class="mb-2">
          <h2 class="p">{{ blocks }} BLKS</h2>
          <span>${{ invested | numericFormat({
        decimalSeparator: ".",
        fractionDigitsMax: 0,
        fractionDigitsMin: 0,
        fractionDigitsSeparator: "",
        thousandsDigitsSeparator: ","
      }) }}</span>
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

    <v-row class="center"
      style="max-width: 500px!important; min-width: 320px!important; margin-top: -13px!important; max-height: 740px;">
      <v-col cols="12" class="center">
        <v-card class="card" style="max-height: 215px!important;">
          <img src="~/assets/sources/icons/roi.svg" alt="Bloque" class="mb-2">
          <h2 class="p">RET. DE INVERSION</h2>
          <span>${{ roi | numericFormat(numericFormatConfig) }}</span>
          <v-btn v-show="!test" class="btn mt-3" @disabled="registered" @click="withdrawROI()">RETIRAR</v-btn>
        </v-card>
      </v-col>

      <v-col cols="12" class="center">
        <v-card class="card">
          <img src="~/assets/sources/icons/residual.svg" alt="Bloque" class="mb-2">
          <h2 class="p">BONO RESIDUAL</h2>
          <span>${{ bonoResidual | numericFormat({
            decimalSeparator: ".",
            fractionDigitsMax: 3,
            fractionDigitsMin: 0,
            fractionDigitsSeparator: "",
            thousandsDigitsSeparator: ","
          }) }}</span>
          <v-btn v-show="!test" class="btn mt-3" @disabled="registered" @click="withdrawBonoResidual()">RETIRAR</v-btn>
        </v-card>
      </v-col>

      <v-col cols="12" class="center">
        <v-card class="card">
          <img src="~/assets/sources/icons/Referido.svg" alt="Bloque" class="mb-2">
          <h2 class="p">BONO REFERIDOS</h2>
          <span>${{ bonoReferidos | numericFormat(numericFormatConfig) }}</span>
          <v-btn v-show="!test" class="btn mt-3" @disabled="registered" @click="withdrawReferidos()">RETIRAR</v-btn>
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

    <v-row class="center"
      style="max-width: 500px!important; min-width: 320px!important; margin-top: -13px!important; align-items: flex-start;">

      <v-col v-for="(item, index) in depositos" :key="index" cols="12" class="center">
        <v-card v-show="item.monto" class="card"
          style="background-color: var(--tertiary)!important; padding-block: 10px!important;">
          <div class="div-blue">
            <div class="divrow center" style="gap: 10px; max-height: 40px;">
              <h2 style="font-size: 16px!important;">{{ item.monto | numericFormat(numericFormatConfig) }} {{ item.desc }}
              </h2>
              <div class="vertical"></div>
              <h2 style="font-size: 16px!important;">{{ item.estado }}</h2>
            </div>
            <v-slider v-model="item.progreso" min="0" max="100" color="var(--primary)" hide-details class="slider"
              readonly></v-slider>
          </div>

          <span class="tcenter mt-2" style="color: var(--secondary); font-size: 14px!important;"
          v-html="`<span class='bold tcenter' style='color: var(--secondary); font-weight: 700!important;'>Finaliza: </span>${ item.stringFecha }`">
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
// const wallet1 = '0x6924Ff38aDFd2E93dD29c603c762650d4e5981e7'
// const wallet2 = '0x11907e222f22b659f9F2192d2aAF1c26bDeAc2DB'
const precioBLKS = 50
const today = new Date(Date.now())

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
      registered: false,
      bonoResidualActivo: 0,
      bonoReferidos: 0,
      bonoResidual: 0,
      active_slider: 60,
      valorBLKS: 0,
      depositos: [],
      wallet: "",
      test: false,
    }
  },
  head() {
    const title = 'Home';
    return {
      title,
    }
  },
  mounted() {
    if (this.$route.query.view) {
      this.wallet = this.$route.query.view
      this.test = true
    } else if (localStorage.getItem("wallet") !== null) {
      this.wallet = localStorage.getItem("wallet")
    }

    this.getAccount()
    this.updateData()
    if (localStorage.getItem("wallet") !== null) {
      this.updateWallet();
    }
  },
  methods: {

    todayIs(){
      this.today = this.convertEpochToOficialTimezone(Date.now())
    },

    async getAccount() {
      const accounts = await window.ethereum
        .request({ method: 'eth_requestAccounts' })
        .catch((err) => {
          if (err.code === 4001) {
            this.$alert('error', { desc: "Por favor conecta con metamask para poder utilizar la Dapp" })
          } else {
            console.error(err)
          }
        })
      const account = accounts[0]
      localStorage.setItem('isLogged', window.ethereum.isConnected())
      if (localStorage.getItem('wallet') !== account) {
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

    async getUserData() {
      const tokenContract = new web3.eth.Contract(contractAbi, infinityBlocksAddres);

      try {
        const user = await tokenContract.methods.investors(this.wallet).call({ from: this.wallet })
        this.invested = user.invested / Math.pow(10, 18)
        this.blocks = this.invested / precioBLKS
        this.bonoResidual = user.balanceInfinit
        this.bonoReferidos = user.balanceRef / Math.pow(10, 18)
        this.registered = user.registered
      } catch (error) {
        console.log(error + "getUserData")
        this.invested = 0
      }

    },

    async getROI() {
      try {
        const tokenContract = new web3.eth.Contract(contractAbi, infinityBlocksAddres);
        const roi = await tokenContract.methods.withdrawable(this.wallet, false).call({ from: this.wallet })
        this.roi = roi / Math.pow(10, 18)
      } catch (error) {
        this.roi = 0
      }
    },

    async getBonoResidual() {
      try {
        const tokenContract = new web3.eth.Contract(contractAbi, infinityBlocksAddres);
        const bonoResidual = await tokenContract.methods.withdrawable(this.wallet, true).call({ from: this.wallet })
        this.bonoResidual = bonoResidual / Math.pow(10, 18)
      } catch (error) {
        this.bonoResidual = 0
      }
    },

    async withdrawROI() {
      const tokenContract = new web3.eth.Contract(contractAbi, infinityBlocksAddres);
      try {
        await tokenContract.methods.withdraw().send({ from: this.wallet })
      } catch (error) {
        console.log(error)
      }
    },

    async withdrawBonoResidual() {
      try {
        const tokenContract = new web3.eth.Contract(contractAbi, infinityBlocksAddres);
        await tokenContract.methods.withdraw2().send({ from: this.wallet })
      } catch (error) {
        console.log(error)
      }
    },

    async withdrawReferidos() {
      try {
        const tokenContract = new web3.eth.Contract(contractAbi, infinityBlocksAddres);
        await tokenContract.methods.withdrawTeam().send({ from: this.wallet })
      } catch (error) {
        console.log(error)
      }

    },

    addDays(date, days) {
      const result = new Date(date);
      result.setDate(result.getDate() + days);
      return result;
    },

    diferenciaDias(date1, date2) {
      const difference = date1.getTime() - date2.getTime();
      const TotalDays = Math.ceil(difference / (1000 * 3600 * 24));
      return TotalDays;
    },

    convertEpochToOficialTimezone(timeEpoch) {
      const offset = -5
      const d = new Date(timeEpoch * 1000);
      const utc = d.getTime() + (d.getTimezoneOffset() * 60000);  // This converts to UTC 00:00
      const nd = new Date(utc + (3600000*offset));
      return nd;
    },

    async getDepositosBLKS(diasFinalizacion) {
      const tokenContract = new web3.eth.Contract(contractAbi, infinityBlocksAddres);
      const response = await tokenContract.methods.depositos(this.wallet, false).call({ from: this.wallet })
      let depositosBLKS = []
      try {
        for (let i = 0; i < response[0].length; i++) {
          const monto = (response[0][i] / Math.pow(10, 18)) / 120
          const desc = " BLKS ($" + precioBLKS * monto + ")"
          const fechaInicio = this.convertEpochToOficialTimezone(response[1][i])
          const epoch = response[1][i]
          const fechaFinalizacion = this.addDays(fechaInicio, diasFinalizacion)
          const calProgreso = (this.diferenciaDias(today, fechaInicio) / diasFinalizacion) * 100
          const progreso = calProgreso > 100 ? 100 : calProgreso
          const stringFecha = fechaFinalizacion.toLocaleString() + " GTM -5 <br> (Hora oficial Infinity Blocks)"
          const estado = progreso < 100 ? "ACTIVE" : "INACTIVE"
          const data = { monto, stringFecha, estado, progreso, fechaInicio, desc, epoch }
          depositosBLKS.push(data)
        }
        depositosBLKS = depositosBLKS.sort((a, b) => a.epoch - b.epoch)
        depositosBLKS.reverse()

        return depositosBLKS
      } catch (error) {
        console.log(error)
        return []
      }
    },
    async getDepositosInfinity(diasFinalizacion) {
      const tokenContract = new web3.eth.Contract(contractAbi, infinityBlocksAddres);
      let montoTotalInfinit = 0
      const depositosInfinity = []
      
      try {
        const response = await tokenContract.methods.depositos(this.wallet, true).call({ from: this.wallet })
        for (let i = 0; i < response[0].length; i++) {
          const monto = response[0][i] / Math.pow(10, 18)
          const desc = "USD | infinity "
          const fechaInicio = this.convertEpochToOficialTimezone(response[1][i])
          const epoch = response[1][i]
          const fechaFinalizacion = this.addDays(fechaInicio, diasFinalizacion)
          const calProgreso = (this.diferenciaDias(today, fechaInicio) / diasFinalizacion) * 100
          const progreso = calProgreso > 100 ? 100 : calProgreso
          const stringFecha = fechaFinalizacion.toLocaleString() + " GTM -5 <br> (Hora oficial Infinity Blocks)"
          const estado = progreso < 100 ? "ACTIVE" : "INACTIVE"
          const data = { monto, stringFecha, estado, progreso, fechaInicio, desc, epoch }
          montoTotalInfinit += monto
          depositosInfinity.push(data)
        }
      this.bonoResidualActivo = montoTotalInfinit > 0 ? montoTotalInfinit : this.bonoResidualActivo
      depositosInfinity.sort((a, b) => a.epoch - b.epoch)
      depositosInfinity.reverse()
      return depositosInfinity  
      } catch (error) {
        console.log(error)
        return []
      }
    },

    async getDepositos() {
      const diasFinalizacion = 900      
      try {
        const depositosBLKS = await this.getDepositosBLKS(diasFinalizacion)
        const depositosInfinity = await this.getDepositosInfinity(diasFinalizacion)
        this.depositos.push(...depositosBLKS, ...depositosInfinity)
      } catch(error) {
        console.log(error)
      }
    },



    updateData() {
      setInterval(() => {
        this.todayIs()
        this.getROI()
        this.getUserData()
        this.getBonoResidual()
        this.getDepositos()
      }, (5000));
    }

  }
};
</script>

<style src="~/assets/styles/pages/index.scss" lang="scss" />
