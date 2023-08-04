<template>
  <v-dialog
    v-model="modalConnect"
    content-class="modal-connect divcol relative isolate"
  >
    <aside class="space">
      <span class="h9_em">Connect Wallet</span>

      <v-btn icon @click="modalConnect = false">
        <v-icon size="1.5em">mdi-close</v-icon>
      </v-btn>
    </aside>

    <v-sheet class="grid" color="transparent">
      <v-btn plain @click="connectMetamask()">
        <img
          src="~/assets/sources/logos/metamask-icon.svg"
          alt="metamask logo"
        />

        <div class="divcol astart" style="gap: 5px">
          <span class="h12_em bold">metamask</span>
          <span class="h13_em">metamask.org</span>
        </div>
      </v-btn>
    </v-sheet>
  </v-dialog>
</template>

<script>
export default {
  name: 'ConnectModal',
  data() {
    return {
      modalConnect: false
    }
  },

  mounted() {
    this.getChainId()
  },

  methods: {
    async connectMetamask() {
      if (typeof window.ethereum !== 'undefined') {
        console.log('MetaMask is installed!')
        await this.getAccount()
      } else {
        console.log('Por favor instala la extensión de Metamask')
      }
    },

    async getAccount() {
      const accounts = await window.ethereum
        .request({ method: 'eth_requestAccounts' })
        .catch((err) => {
          if (err.code === 4001) {
            // EIP-1193 userRejectedRequest error
            // If this happens, the user rejected the connection request.
            console.log('Please connect to MetaMask.')
          } else {
            console.error(err)
          }
        })
      const account = accounts[0]
      localStorage.setItem('isLogged', window.ethereum.isConnected())
      localStorage.setItem('account', account)
    },

    getChainId() {
      if (window.ethereum.networkVersion !== "56") {
        this.changeUserCurrentChain()
      }
    },

    async changeUserCurrentChain() {try {
      await window.ethereum.request({
        method: 'wallet_switchEthereumChain',
        params: [{ chainId: '0x38' }],
      });
      
    } catch (switchError) {
      if (switchError.code === 4902) {
        try {
          await window.ethereum.request({
            method: 'wallet_addEthereumChain',
            params: [
                {
                  chainId: '0x38',
                  chainName: 'BNB Smart Chain Mainnett',
                  rpcUrls: ['https:bsc-dataseed.binance.org'],
                  blockExplorerUrls: ['https:bscscan.com'],
                  nativeCurrency: {
                    symbol: 'BNB',
                    decimals: 18
                  }
                }
              ]
          });
        } catch (err) {
          this.$alert('error', { desc: 'Por favor agregue la red de BNB para poder usar la dapp' })
        }
      }
    }
    }
  }
}
</script>

<style lang="scss">
.modal-connect {
  @include card;
  --min-w: 330px;
  --w: max-content;
  --br: 30px;
  --bg: #272727;
  --p: 30px;
  --tt: capitalize;
  gap: 20px;

  &::before {
    content: '';
    position: absolute;
    inset: 0;
    border-radius: inherit;
    padding: 2px;
    background-clip: content-box, border-box;
    background-image: linear-gradient(var(--bg), var(--bg)),
      linear-gradient(135deg, rgba($primary, 0.2), rgba($accent, 0.2));
    z-index: -1;
  }

  i {
    color: hsl(225 225% 225% / 0.5) !important;
  }

  .v-sheet.grid {
    @include media(min, 500px) {
      --gtc: 1fr 1fr;
    }
    gap: 20px;
    .v-btn {
      --fs: 20px;
      width: 100%;
      min-height: 70px;
      border-radius: 10px;
      background-color: hsl(0 0% 0% / 0.2);
      transition: 0.2s $ease-return;
      &:hover {
        background-color: hsl(0 0% 0% / 0.4);
        transform: translateY(-5px) !important;
      }
      &__content {
        justify-content: flex-start;
        gap: 10px;
        img {
          --w: 40px;
          --of: cover;
        }
        span + span {
          --c: hsl(225 225% 225% / 0.5);
        }
      }
    }
  }
}
</style>
