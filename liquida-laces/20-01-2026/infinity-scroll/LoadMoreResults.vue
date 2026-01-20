<!-- Componente NOVO -->

<template>
  <div class="load-more-results">
    <products-list
      :products="productsToRender"
      :loading="loading"
      :is-mobile="isMobile"
      :products-per-line="productsPerLine"
    >
      <template slot-scope="data">
        <slot :product="data.product" :loading="data.loading" />
      </template>
    </products-list>

    <div v-if="hasAnyProducts" class="load-more-footer">
      <button
        v-if="canLoadMore"
        class="btn-load-more"
        type="button"
        :disabled="loading || isLoadingMore"
        @click="loadMore"
      >
        <span v-if="loading || isLoadingMore">Carregando...</span>
        <span v-else>Carregar mais</span>
      </button>

      <div v-else class="end-results">
        Você chegou ao fim 🙂
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "LoadMoreResults",

  props: {
    searchData: { type: Array, default: () => [] },
    paginate: { type: Object, required: true },
    loading: { type: Boolean, default: false },
    queryParams: { type: Object, required: true },
    updateCurrentPage: { type: Function, required: true },

    isMobile: { type: Boolean, default: false },
    productsPerLine: { type: Number, default: 2 },
  },

  data: () => ({
    accumulated: [],
    lastSignature: "",
    lastPageLoaded: 0,
    isLoadingMore: false,

    restoreScrollY: null,
    shouldRestoreScroll: false,
  }),

  computed: {
    signature() {
      const qp = this.queryParams || {}
      const { page, ...rest } = qp
      return JSON.stringify(rest)
    },

    currentPage() {
      return Number(this.paginate?.currentPage || 1)
    },

    pageCount() {
      return Number(this.paginate?.pageCount || 1)
    },

    canLoadMore() {
      return this.currentPage < this.pageCount
    },

    hasAnyProducts() {
      return (this.accumulated?.length || 0) > 0 || (this.searchData?.length || 0) > 0
    },

    productsToRender() {
      // Enquanto ainda não comitamos, mostramos searchData.
      // Depois que acumulado existir, mostramos o acumulado.
      return (this.accumulated?.length || 0) > 0 ? this.accumulated : this.searchData
    },
  },

  watch: {
    signature: {
      immediate: true,
      handler(sig) {
        if (this.lastSignature && sig !== this.lastSignature) {
          this.accumulated = []
          this.lastPageLoaded = 0
        }
        this.lastSignature = sig
      },
    },

    // Assim que searchData chegar/atualizar, tentamos commitar
    searchData: {
      immediate: true,
      handler() {
        this.commitCurrentPage()
      },
    },

    // quando o fetch termina, commit + restaura scroll
    loading(val) {
      if (val === false) {
        this.commitCurrentPage()
        this.isLoadingMore = false

        if (this.shouldRestoreScroll && typeof this.restoreScrollY === "number") {
          this.$nextTick(() => {
            window.scrollTo({ top: this.restoreScrollY, behavior: "auto" })
            setTimeout(() => window.scrollTo({ top: this.restoreScrollY, behavior: "auto" }), 50)
            setTimeout(() => window.scrollTo({ top: this.restoreScrollY, behavior: "auto" }), 250)
          })
        }

        this.shouldRestoreScroll = false
        this.restoreScrollY = null
      }
    },
  },

  methods: {
    commitCurrentPage() {
      const current = this.currentPage
      if (current <= this.lastPageLoaded) return

      const incoming = Array.isArray(this.searchData) ? this.searchData : []

      // 🔥 FIX: se ainda não chegou nada (async), não marca como carregado
      if (incoming.length === 0) return

      if (current === 1 && this.accumulated.length === 0) {
        this.accumulated = [...incoming]
      } else {
        const seen = new Set(this.accumulated.map(p => p?.id))
        for (const p of incoming) {
          if (!seen.has(p?.id)) this.accumulated.push(p)
        }
      }

      this.lastPageLoaded = current
    },

    loadMore() {
      if (!this.canLoadMore) return
      if (this.loading || this.isLoadingMore) return

      this.isLoadingMore = true
      this.restoreScrollY = window.scrollY
      this.shouldRestoreScroll = true

      const next = this.currentPage + 1
      this.updateCurrentPage(next)
    },
  },
}
</script>

<style scoped>
.load-more-footer {
  display: flex;
  justify-content: center;
  margin: 24px 0;
}

.btn-load-more {
  padding: 12px 18px;
  border-radius: 10px;
  border: 1px solid rgba(0,0,0,.15);
  background: #fff;
  font-weight: 600;
  cursor: pointer;
}

.btn-load-more:disabled {
  opacity: .6;
  cursor: not-allowed;
}

.end-results {
  opacity: .75;
}
</style>
