<template>
  <div class="projects">
    <div class="category-nav">
      <div @click="setCategory('all')" class="category">
        <span v-if="currentCategory === 'all'" class="category-current">Vis alle</span>
        <span v-else>Vis alle</span>
      </div>
      <div v-for="category in $static.categories.edges" :key="category.node.title" @click="setCategory(category.node.title)" class="category">
        <span v-if="currentCategory === category.node.title" class="category-current">{{ category.node.title }}</span>
        <span v-else>{{ category.node.title }}</span>
      </div>
    </div>
    <div class="project-grid">
      <PostItem
        v-for="project in visibleProjects"
        :key="project.node.id"
        :project="project.node"
        :metadata="$static.metadata"
      />
    </div>
  </div>
</template>

<static-query>
{
  metadata {
    sanityOptions {
      projectId
      dataset
    }
  }
  projects: allSanityProject(
    filter: {slug: {current: {ne: null}}, publishedAt: {ne: null}},
    sortBy: "publishedAt", order: DESC
  ) {
    edges {
      node {
        publishedAt
        id
        mainImage {
          asset {
            _id
          }
          alt
        }
        title
        _rawExcerpt
        slug {
          current
        }
        categories {
          _id
          title
        }
      }
    }
  }
  categories: allSanityCategory(
    limit: 12
  ) {
    edges {
      node {
        id
        title
      }
    }
  }
}

</static-query>

<script>
import PostItem from '~/components/PostItem'

export default {
  props: {
    limit: {
      type: Number,
      default: 100
    },
    showMenu: Boolean
  },
  components: {
    PostItem
  },
  data() {
    return {
      currentCategory: 'all'
    }
  },
  computed: {
    visibleProjects() {
      const projects = this.$static.projects.edges;
      if (this.currentCategory !== 'all') {
        const filteredProjects = [];
        for (let i = 0; i < projects.length; i++) {
          if (projects[i].node.categories.some(category => {
              return this.currentCategory === category.title
            })) {
            filteredProjects.push(projects[i])
          }
        }
        return filteredProjects
      }
      return projects
    }
  },
  methods: {
    setCategory(category) {
      this.currentCategory = category
    }
  }
}
</script>

<style lang="scss" scoped>
.project-grid {
  display: grid;
  grid-gap: var(--spacing-m);
  grid-template-columns: repeat(12, 1fr);
  margin-bottom: 6rem;
}

.category-nav {
  margin: -1rem 0 var(--spacing-m);

  .category {
    display: inline-block;
    margin-right: 1rem;
    font-family: var(--sans-serif);
    font-size: .9rem;
    transition: all .1s ease-in-out;
    &:hover {
      cursor: pointer;
      text-decoration: underline;
    }
    &-current {
      text-decoration: underline;
    }
  }
}
@media (max-width: 1000px) {
  .project-grid {
    grid-template-columns: repeat(8, 1fr);
  }
}
@media (max-width: 800px) {
  .project-grid {
    grid-template-columns: repeat(4, 1fr);
  }
}
@media (max-width: 800px) {
  .category-nav {
    margin: -2.6rem 0 1rem;
    line-height: 1.4;
    .category {
      margin-right: .8rem;
    }
  }
}
@media (max-width: 600px) {
  .category-nav {
    margin: -2.6rem 0 1rem;
    line-height: 1.4;
    .category {
      font-size: 12px;
      margin-right: .8rem;
    }
  }
}
</style>
