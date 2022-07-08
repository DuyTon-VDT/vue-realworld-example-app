<template>
  <v-modal :visible="visible" title="Wrong Network" :showClose="false">
    <div class="wrong-network-loading">
      <i class="ion-refresh"></i>
      Wrong network. Change network to {{ chainName }}
    </div>
  </v-modal>
</template>

<script>
import VModal from "./VModal.vue";
import { DEFAULT_CHAIN_ID, DEFAULT_CHAIN_NAME } from "../utils/constants";
import connectWalletMixin from "../mixins/connectWallet";

export default {
  name: "ModalWrongNetwork",
  components: { VModal },
  mixins: [connectWalletMixin],
  data() {
    return {
      visible: false,
      chainName: DEFAULT_CHAIN_NAME
    };
  },
  watch: {
    async chainId(value) {
      if (value && value != DEFAULT_CHAIN_ID && this.account) {
        this.visible = true;
        try {
          await window.myProvider
            .request({
              method: "wallet_switchEthereumChain",
              params: [{ chainId: DEFAULT_CHAIN_ID }]
            })
            .then(() => {
              this.setChainId(DEFAULT_CHAIN_ID);
              this.visible = false;
            });
        } catch (error) {
          if (error.code === 4902) {
            try {
              await window.myProvider
                .request({
                  method: "wallet_addEthereumChain",
                  params: [
                    {
                      chainId: DEFAULT_CHAIN_ID,
                      rpcUrl: process.env.RPC_URL_RINKEBY_TESTNET
                    }
                  ]
                })
                .then(() => {
                  this.setChainId(DEFAULT_CHAIN_ID);
                  this.visible = false;
                });
            } catch (addError) {
              console.error(addError);
            }
          } else if (error.code === 4001) {
            this.disconnectWallet();
            this.visible = false;
          }
          console.log(error);
        }
      } else {
        this.visible = false;
      }
    }
  }
};
</script>
<style>
.wrong-network-loading {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.wrong-network-loading .ion-refresh {
  font-size: 50px;
  animation: rotating 0.5s linear infinite;
}

@keyframes rotating {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
