<template>
  <ul class="catalog__pagination pagination" v-if="count">
    <li class="pagination__item">
      <a aria-label="Предыдущая страница" 
        href="#" 
        class="pagination__link pagination__link--arrow" 
        :class="{'pagination__link--disabled': page <= 1}"
        @click.prevent="prevPage"
      >
        <svg width="8" height="14" fill="currentColor">
          <use xlink:href="#icon-arrow-left"></use>
        </svg>
      </a>
    </li>

    <li class="pagination__item" v-for="(pageNumber, index) in pagesInPagination" :key="index">
      <a class="pagination__link" 
        :class="{'pagination__link--current': pageNumber === page}" 
        :href="pageNumber === page || pageNumber === '...' ? false : '#'" 
        @click.prevent="paginate($event, pageNumber)"
      >
        {{ pageNumber }}
      </a>
    </li>

    <li class="pagination__item">
      <a aria-label="Следующая страница" 
        href="#" 
        class="pagination__link pagination__link--arrow" 
        :class="{'pagination__link--disabled': page >= pagesAll}" 
        @click.prevent="nextPage"
      >
        <svg width="8" height="14" fill="currentColor">
          <use xlink:href="#icon-arrow-right"></use>
        </svg>
      </a>
    </li>
  </ul>
</template>

<script>
import getPagesInPagination from "../functions/pagination"

export default {
  props: ['page', 'count', 'perPage'],
  computed: {
    pagesAll() {
      return Math.ceil(this.count / this.perPage);
    },
    pagesInPagination() {
      return getPagesInPagination(this.pagesAll, this.page);
    }
  },
  methods: {
    paginate(e, page) {
      if(!e.target.innerText.includes('...')){
        this.$emit('update:page', page);
      }
    },
    prevPage() {
      if(this.page > 1) {
        this.$emit('update:page', this.page - 1);
      }
    },
    nextPage() {
      if(this.page < this.pagesAll) {
        this.$emit('update:page', this.page + 1);
      }
    },
  },
}
</script>