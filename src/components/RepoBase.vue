<script setup>
import PrevIcon from './icons/prevIcon.vue'
import NextIcon from './icons/nextIcon.vue'
import SkeletonBase from './SkeletonBase.vue'
</script>

<script>
export default {
  data() {
    return {
      repoData: [],
      page: 1,
      view: '',
      currentPage: 1,
      loading: false,
      perPage: 6,
      skeleton: [...new Array(6)]
    }
  },
  methods: {
    fetchData: function () {
      this.loading = true
      fetch(`https://api.github.com/users/gekwealor/repos`, {
        headers: {
          Accept: 'application/json'
        }
      })
        .then((res) => res.json())
        .then((data) => {
          this.repoData = this.repoData.concat(data)
          this.loading = false
        })
    },
    prevPage() {
      if (this.currentPage === 1) {
        return
      } else {
        this.currentPage--
      }
    },
    nextPage() {
      this.currentPage++
    }
  },
  mounted() {
    this.fetchData()
  },
  computed: {
    showMore: function () {
      let start = (this.currentPage - 1) * this.perPage
      let end = start + this.perPage
      this.loading = false
      return this.repoData.slice(start, end)
    },

    lastPage: function () {
      let length = this.repoData.length
      return length / this.perPage
    }
  }
}
</script>

<template>
  <div>
    <div class="repo-container">
      <div class="repo-container" v-if="loading">
        <SkeletonBase v-for="n in skeleton" :key="n">{{ skeleton }}</SkeletonBase>
      </div>
      <div v-else v-for="repo in showMore" class="repo-card" :key="repo.id">
        <router-link :to="`/details/${repo.name}`"
          ><h2 class="repo-name">{{ repo.name }}</h2></router-link
        >
        <p class="language">Language: {{ repo.language }}</p>
        <p class="date">Start date & time: {{ repo.created_at }}</p>
        <p class="visibility">Visibility: {{ repo.visibility }}</p>
      </div>
    </div>
    <div class="pagination">
      <button class="view-more" :class="currentPage === 1 ? 'disabled' : ''" @click="prevPage">
        <PrevIcon />
      </button>
      <p class="current-page">{{ currentPage }}</p>
      <button
        class="view-more"
        :class="currentPage === lastPage ? 'disabled' : ''"
        @click="nextPage"
      >
      
        <NextIcon class="next" />
      </button>
    </div>
  </div>
</template>

<style>
.repo-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  grid-gap: 30px;
  width: 90%;
  margin: 0 auto;
  margin-bottom: 5rem;
}

.next{
    color:#098f02;
}
.repo-card,
.repodetail-card {
  border: 1px solid #bdbd00;
  padding: 13px;
  border-radius: 5px;
}

.repo-card,
.repodetail-card:hover {
  box-shadow: 0 0 10px #bdbd00;
    transition: 0.3s;
}

.repo-card,
.repodetail-card:active {
  box-shadow: 0 0 10px #bdbd00;
    transition: 0.3s;
}

.repo-name {
  color: #bdbd00;
  font-size: 2rem;
  word-break: break-word;
}

p {
  padding: 5px 0;
}

.pagination {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1rem;
  margin: 3rem 0;
}

button {
  background: none;
  border: none;
  cursor: pointer;
}

.view-more svg {
  width: 2rem;
}

.disabled {
  cursor: not-allowed;
  opacity: 0.5;
}
</style>