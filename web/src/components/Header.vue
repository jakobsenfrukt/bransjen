<template>
  <header class="site-header">
    <g-link to="/" class="logo">
      <img v-if="$static.settings.logo" :src="$static.settings.logo.asset.url" alt="Bransjens logo" />
      <h1 v-else>{{$static.settings.title}}</h1>
    </g-link>
    <nav>
      <g-link to="/om">Om Bransjen</g-link>
      <g-link to="/prosjekter">Prosjekter</g-link>
      <g-link to="/div">Diverse</g-link>
      <a class="smiley" href="/" @click="refreshIndexGrid()">
        <img class="sad" src="/graphics/smiley-no.svg" alt="Smiley som ser trist men litt psycho ut" />
        <img class="happy" src="/graphics/smiley.svg" alt="Smiley som ser glad men litt psycho ut" />
      </a>
    </nav>
  </header>
</template>

<static-query>
query {
  metadata {
    sanityOptions{
      projectId
      dataset
    }
  }
  settings: sanitySiteSettings(id: "siteSettings") {
    logo {
      asset {
        url
      }
    }
    title
  }
}
</static-query>

<script>
export default {
  methods: {
    refreshIndexGrid() {
      this.$store.commit('increment')
    }
  }
}
</script>

<style lang="scss" scoped>
.site-header {
  padding: 1.5rem 1.5rem 0;
  display: flex;

  .logo {
    width: 12rem;
  }

  h1 {
    font-size: 2.4rem;
  }

  a {
    text-decoration: none;
    transition: all .1s ease-in-out;
  }

  nav {
    margin: 0 0 0 auto;
    font-family: var(--sans-serif);
    font-size: 1.3rem;
    a {
      display: inline-block;
      margin-right: 2rem;
      &:hover, &.active {
        color: inherit;
        text-decoration: underline;
      }
    }
    .smiley {
      display: inline-block;
      vertical-align: middle;
      width: 1.5em;
      height: 1.5em;
      position: relative;
      margin-right: 0;
    }
  }
}

.sad, .happy {
  position: absolute;
  width: 1.5em;
  height: 1.5em;
}

.sad {
  display: none;
}

.smiley:hover .sad {
  display: block;
}

.smiley:hover .happy {
  display: none;
}

@media (max-width: 800px) {
  .site-header {
    padding: 1rem;
    padding: 2rem 1rem 1rem;
    flex-wrap: wrap;
    .logo {
      width: 100%;
      img {
        width: 50%;
        max-width: 12rem;
      }
    }
    nav {
      width: 100%;
      max-width: 30rem;
      display: flex;
      justify-content: space-between;
      margin: 1rem 0 0 0;
      a {
        margin: 0 0 .6rem 0;
        line-height: 1.2;
        display: inline-block;
        font-weight: bold;
      }
    }
  }
  .smiley {
    display: none !important;
  }
}

@media (max-width: 600px) {
  .site-header {
    padding: 1rem;
    padding: 2rem 1rem 1rem;
    flex-wrap: wrap;

    nav {
      font-size: 14px;
      max-width: 20rem;
      a {
        margin: 0 0 .6rem 0;
        line-height: 1.2;
        display: inline-block;
        font-weight: bold;
      }
    }
  }
}
</style>
