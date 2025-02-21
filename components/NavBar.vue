<template>
  <div class="header">
    <div class="header-logo">
      <img :src="require('~/assets/images/logo.png')" alt="" />
    </div>

    <template v-for="item in navList">
      <b-dropdown
        v-if="item.hasChild"
        :key="item.name"
        :text="item.name === 'Locales' ? locale : $t(`header.${item.name}`)"
        right
      >
        <template v-for="child in item.children">
          <b-dropdown-item :key="child" href="#" @click="jump(child)">
            {{ $t(`header.${child}`) }}</b-dropdown-item
          >
        </template>
      </b-dropdown>
      <div class="header-button"
        v-else
        @click="jump(item.name)"
      >{{ $t(`header.${item.name}`) }}</div>
    </template>

  </div>
</template>

<script>
import VueScrollTo from "vue-scrollto"
import jumpTo from "../utils"

export default {
  data() {
    return {
      activeNav: "home",
      navList: [
        {
          name: "Grants",
          hasChild: false,
        },
        {
          name: "Technology",
          hasChild: false,
        },
        {
          name: "Projects",
          hasChild: true,
          children: ["CrustNetwork"],
        },

        // {
        //   name: "Locales",
        //   hasChild: true,
        //   children: ["En", "Zh"],
        // },
      ],
    }
  },
  computed: {
    routerName() {
      let name = this.$route.name
      if (name === "index" || name === "/") {
        name = "home"
      }
      return name
    },
    locale() {
      return this.$store.state.locale === "zh" ? "中文" : "En"
    },
  },
  watch: {
    routerName() {
      this.activeNav = this.routerName
    },
  },
  methods: {
    jump(name) {
      switch (name) {
        case "Zh":
          this.setLocale("zh")
          break
        case "En":
          this.setLocale("en")
          break
        case "Grants":
          VueScrollTo.scrollTo(document.querySelector(".description"))
          break
        case "Technology":
          VueScrollTo.scrollTo(document.querySelector(".application"))
          break
        default:
          jumpTo(name)
      }
    },
    setLocale(name) {
      this.$i18n.locale = name
      // 将当前语言保存到cookie 中，
      document.cookie = "locale=" + name
      return this.$store.commit("SET_LANG", name)
    },
  },
}
</script>

<style lang="scss" scoped>
@media (min-width: 600px) {
  .header {
    position: absolute;
    display: flex;
    align-items: center;
    width: 100%;
    min-width: 1440px;
    min-height: 110px;
    padding: 0 55px 0 85px;

    .header-logo {
      flex-grow: 1;
    };

    .header-button {
      font-size: 24px;
      font-weight: bold;
      color: white;
      line-height: 60px;
      padding: 0 35px 0 35px;
      border-radius: 30px;
      outline: none;
      cursor: pointer;
      margin-left: 20px;
      &:hover {
        background: #2dacd1;
        color: white;
      }
      &:focus {
        color: white;
      }
    }

    /deep/ .b-dropdown {
      button {
        @extend .header-button;
        background-color: transparent;
        border: none;
        outline: none;

        &:hover {
          transition-duration: 0s;
        }

        &:focus {
          outline: none;
        }
      }
    }
    /deep/ .dropdown-toggle::after {
      content: none;
    }
  }
}

@media (max-width: 600px) {
  .header {
    position: absolute;
    display: flex;
    align-items: center;
    width: 100%;
    min-height: 110px;
    padding: 0 1.25rem;

    .header-logo {
      flex-grow: 1;
      img {
        width: 1.875rem;
        height: 2.25rem;
      }
    };

    .header-button {
      font-size: 0.875rem;
      color: white;
      line-height: 1.5rem;
      padding: 0 0.5rem;
      border-radius: 1rem;
      outline: none;
      cursor: pointer;
      margin-left: 0.5rem;

      &:hover {
        background: #2dacd1;
        color: white;
      }
      &:focus {
        color: white;
      }
    }

    /deep/ .b-dropdown {
      button {
        font-size: 0.875rem;
        color: white;
        line-height: 1.5rem;
        padding: 0 0.5rem;
        border-radius: 1rem;
        outline: none;
        cursor: pointer;
        margin-left: 0.5rem;
        &:hover {
          background: #2dacd1;
          color: white;
          transition-duration: 0s;
        }
        &:focus {
          color: white;
          outline: none;
        }

        background-color: transparent;
        border: none;

      }
    }

    /deep/ .dropdown-toggle::after {
      content: none;
    }

    /deep/ .dropdown-item {
      font-size: 0.75rem;
      padding: 0.1rem 0.5rem;
    }

    /deep/ .dropdown-menu {
      min-width: 0;
    }
  }
}
</style>
