<template>
  <nav
    class="w-full h-14 bg-white border-b flex items-center px-4 border-header text-[#7a6eaa] dark:bg-[#1c1c28] dark:text-[#6f6e84] dark:border-[#6f6e84]"
  >
    <div
      class="mx-auto w-full px-4 sm:max-w-[576px] md:max-w-[720px] lg:max-w-[1140px] items-center flex justify-between"
    >
      <router-link
        class="float-left py-1 mr-4 text-xl text-black font-bold font-display dark:text-white"
        :to="{ name: 'home' }"
      >
        conduit
      </router-link>

      <ul
        v-if="!isAuthenticated"
        class="flex flex-row space-x-3 float-right items-center justify-center"
      >
        <li class="">
          <router-link
            class="py-1 px-2 h-12 text-[#7a6eaa] hover:bg-[#eff4f5] hover:rounded-2xl dark:hover:bg-[#9191ad] dark:hover:text-white hover:no-underline"
            active-class="active"
            exact
            :to="{ name: 'home' }"
          >
            Home
          </router-link>
        </li>
        <li class="">
          <router-link
            class="py-1 px-2 h-12 text-[#7a6eaa] hover:bg-[#eff4f5] hover:rounded-2xl dark:hover:bg-[#9191ad] dark:hover:text-white hover:no-underline"
            active-class="active"
            exact
            :to="{ name: 'login' }"
          >
            <i class="ion-compose"></i>Sign in
          </router-link>
        </li>
        <li class="">
          <router-link
            class="py-1 px-2 h-12 text-[#7a6eaa] hover:bg-[#eff4f5] hover:rounded-2xl dark:hover:bg-[#9191ad] dark:hover:text-white hover:no-underline"
            active-class="active"
            exact
            :to="{ name: 'register' }"
          >
            <i class="ion-compose"></i>Sign up
          </router-link>
        </li>
      </ul>
      <ul
        v-else
        class="flex flex-row space-x-3 float-right items-center justify-center"
      >
        <li class="">
          <router-link
            class="py-1 px-2 h-12 text-[#7a6eaa] hover:bg-[#eff4f5] hover:rounded-2xl dark:hover:bg-[#9191ad] dark:hover:text-white hover:no-underline"
            active-class="active"
            exact
            :to="{ name: 'home' }"
          >
            Home
          </router-link>
        </li>
        <li class="">
          <router-link
            class="py-1 px-2 h-12 hover:bg-[#eff4f5] hover:rounded-2xl dark:hover:bg-[#9191ad] dark:hover:text-white hover:no-underline"
            active-class="active"
            :to="{ name: 'article-edit' }"
          >
            <i class="ion-compose"></i>&nbsp;New Article
          </router-link>
        </li>
        <li class="">
          <router-link
            class="py-1 px-2 h-12 text-[#7a6eaa] hover:bg-[#eff4f5] hover:rounded-2xl dark:hover:bg-[#9191ad] dark:hover:text-white hover:no-underline"
            active-class="active"
            exact
            :to="{ name: 'settings' }"
          >
            <i class="ion-gear-a"></i>&nbsp;Settings
          </router-link>
        </li>
        <li class="" v-if="currentUser.username">
          <router-link
            class="py-1 px-2 h-12 text-[#7a6eaa] hover:bg-[#eff4f5] hover:rounded-2xl dark:hover:bg-[#9191ad] dark:hover:text-white hover:no-underline"
            active-class="active"
            exact
            :to="{
              name: 'profile',
              params: { username: currentUser.username }
            }"
          >
            {{ currentUser.username }}
          </router-link>
        </li>
        <li>
          <button
            v-if="!address"
            @click="connect"
            class="h-12 bg-[#1fc7d4] px-6 text-white font-bold rounded-2xl text-base"
          >
            connect
          </button>
          <div v-else class="flex flex-row items-center justify-center">
            <div
              class="p-2 -mr-3 flex items-center justify-center rounded-full border bg-white border-[#1fc7d4] z-10 dark:bg-[#372f47]"
            >
              <svg
                viewBox="0 0 24 24"
                color="primary"
                width="24px"
                xmlns="http://www.w3.org/2000/svg"
                class="fill-[#1fc7d4]"
              >
                <path
                  fill-rule="evenodd"
                  clip-rule="evenodd"
                  d="M17 4C18.5 4 19 4.5 19 6L19 8C20.1046 8 21 8.89543 21 10L21 17C21 19 20 20 17.999 20H6C4 20 3 19 3 17L3 7C3 5.5 4.5 4 6 4L17 4ZM5 7C5 6.44772 5.44772 6 6 6L19 6L19 8L6 8C5.44772 8 5 7.55229 5 7ZM17 16C18 16 19.001 15 19 14C18.999 13 18 12 17 12C16 12 15 13 15 14C15 15 16 16 17 16Z"
                ></path>
              </svg>
            </div>
            <p
              class="bg-[#eff4f5] pl-4 px-2 text-justify rounded-2xl text-[#280d5f] font-bold shadow-2xl border border-gray-200 dark:bg-[#372f47] dark:text-[#a87ebc] dark:border-gray-200"
            >
              {{ address }}
            </p>
          </div>
        </li>
      </ul>
      <div class="flex justify-start items-center gap-3">
        <button
          v-if="!account"
          class="py-1 px-2 text-[#FFFFFF] bg-[#a371ff] rounded-md"
          @click="showModalConnectWallet()"
        >
          Connect Wallet
        </button>
        <template v-else>
          <div class="account-address">
            <h5>{{ formatAccount(account) }}</h5>
            <i @click="isShowMenu = !isShowMenu" class="ion-arrow-down-b"></i>
            <div
              v-if="isShowMenu"
              class="dropdown-wallet bg-white dark:bg-[#1c1c28]"
            >
              <button class="btn btn-danger" @click="disconnectWallet()">
                Disconnect
              </button>
            </div>
          </div>
        </template>
        <i
          v-if="!isDark"
          @click="toggleTheme"
          class="ion-ios-moon w-5 h-5 text-4xl cursor-pointer flex items-center"
        ></i>
        <i
          v-else
          @click="toggleTheme"
          class="ion-android-sunny w-5 h-5 text-3xl cursor-pointer flex items-center"
        ></i>
      </div>
    </div>
    <modal-connect-wallet
      v-if="isShowModalConnectWallet"
      :visible="isShowModalConnectWallet"
      @close="handleCloseModalConnectWallet()"
    />
  </nav>
</template>

<script>
import { mapState } from "pinia";
import ModalConnectWallet from "./ModalConnectWallet.vue";
import ConnectWalletMixin from "../mixins/connectWallet";
import { WALLET_LIST } from "../utils/constants";
import { authStore } from "@/store/auth.module";

export default {
  name: "RwvHeader",
  components: { ModalConnectWallet },
  mixins: [ConnectWalletMixin],
  data() {
    return {
      isShowMenu: false,
      isShowModalConnectWallet: false,
      isDark: null
    };
  },
  computed: {
    ...mapState(authStore, ["isAuthenticated", "currentUser"])
  },
  created() {
    let theme = localStorage.getItem("theme");
    if (theme === "dark") {
      document.documentElement.classList.add("dark");
    } else {
      document.documentElement.classList.remove("dark");
    }
    this.isDark = theme === "dark";
  },
  mounted() {
    if (
      Object.values(WALLET_LIST).includes(
        localStorage.getItem("connectedWallet")
      )
    ) {
      this.connectWallet(localStorage.getItem("connectedWallet"));
    }
  },
  methods: {
    toggleTheme() {
      this.isDark = !this.isDark;
      localStorage.setItem("theme", this.isDark ? "dark" : "light");
      document.documentElement.classList.toggle("light");
      document.documentElement.classList.toggle("dark");
    },
    handleCloseModalConnectWallet() {
      this.isShowModalConnectWallet = false;
      this.isShowMenu = false;
    },
    async connectWallet(name) {
      switch (name) {
        case WALLET_LIST.METAMASK:
          this.connectMetaMask(this.handleFinishConnectWallet);
          break;
        case WALLET_LIST.WALLET_CONNECT:
          this.connectWalletConnect(this.handleFinishConnectWallet);
          break;
        default:
          break;
      }
    },
    handleFinishConnectWallet() {
      this.isShowMenu = false;
    },
    showModalConnectWallet() {
      this.isShowModalConnectWallet = true;
    },
    formatAccount(account) {
      return account ? `${account.slice(0, 6)}...${account.slice(-4)}` : "";
    }
  }
};
</script>
<style>
.dropdown-wallet {
  position: absolute;
  top: 40px;
  right: 0;
  background: white;
  border-radius: 12px;
  padding: 20px;
}
.account-address {
  position: relative;
  display: flex;
  align-items: center;
  padding: 5px;
  gap: 10px;
}
.ion-arrow-down-b {
  cursor: pointer;
}
</style>
