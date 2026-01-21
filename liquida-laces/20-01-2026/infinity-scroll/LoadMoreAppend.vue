<template>
  <div class="load-more-results">
    <!-- Lista Vue (a “verdadeira” depois que hidratou) -->
    <products-list
      v-if="hydrated"
      :products="accumulated"
      :is-mobile="isMobile"
      :products-per-line="productsPerLine"
      :loading="isLoading"
    >
      <template slot-scope="data">
        <slot :product="data.product" :loading="data.loading" />
      </template>
    </products-list>

    <div v-if="hydrated && accumulated.length" class="load-more-footer">
      <button
        v-if="canLoadMore"
        class="btn-load-more"
        type="button"
        :disabled="isLoading"
        @click="loadMore"
      >
        <span v-if="isLoading">Carregando...</span>
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
  name: "LoadMoreAppend",

  props: {
    pageCount: { type: Number, default: 1 },
    isMobile: { type: Boolean, default: false },
    productsPerLine: { type: Number, default: 2 },
  },

  data: () => ({
    hydrated: false,
    isLoading: false,
    currentPageLocal: 1,
    accumulated: [],
    seenIds: new Set(),

    takeoverTries: 0,
    takeoverDone: false,
  }),

  computed: {
    canLoadMore() {
      return this.currentPageLocal < this.pageCount
    },
  },

  mounted() {
    // página atual pela URL
    const url = new URL(window.location.href)
    const page = Number(url.searchParams.get("page") || 1)
    this.currentPageLocal = Number.isFinite(page) ? page : 1

    // carrega a página atual e “toma conta” do SSR
    this.loadPage(this.currentPageLocal)
  },

  methods: {
      safeTakeoverSSR() {
          if (this.takeoverDone) return

          // procura cards Vue renderizados (box-product vem do teu box-product.twig)
          const vueCards = this.$el.querySelectorAll(".box-product:not(.-clear)").length

          if (vueCards > 0) {
            const ssr = document.getElementById("ssr-products-wrapper")
            if (ssr) ssr.style.display = "none"

            this.takeoverDone = true
            return
          }

          // ainda não renderizou: tenta de novo algumas vezes
          if (this.takeoverTries < 10) {
            this.takeoverTries += 1
            setTimeout(() => this.safeTakeoverSSR(), 80)
          }
        },
    buildFetchUrl(page) {
      const url = new URL(window.location.href)

      // IMPORTANTE: no preview, buscamos dados como produção
      url.searchParams.delete("preview")

      url.searchParams.set("page", String(page))

      // IMPORTANTE: força resposta “resultsOnly” (normalmente vem com :product)
      url.searchParams.set("resultsOnly", "true")

      return url.toString()
    },
    decodeHtmlEntities(str) {
      const t = document.createElement("textarea")
      t.innerHTML = str
      return t.value
    },

    parseProductsFromHtml(html) {
      const tmp = document.createElement("div")
      tmp.innerHTML = html

      const cards = Array.from(tmp.querySelectorAll(".products-list .box-product:not(.-clear)"))
      const products = []

      for (const card of cards) {
        // tenta pegar o product pelo buy-button (melhor fonte)
        const buy = card.querySelector("buy-button[\\:product]")
        const avg = card.querySelector("average-rating[\\:product]")
        const raw = (buy && buy.getAttribute(":product")) || (avg && avg.getAttribute(":product"))

        if (!raw) continue

        try {
          const decoded = this.decodeHtmlEntities(raw)
          const obj = JSON.parse(decoded)
          if (obj && obj.id) products.push(obj)
        } catch (e) {
          // se falhar, só ignora esse card
        }
      }

      return products
    },

    async loadPage(page) {
      if (this.isLoading) return
      this.isLoading = true

      try {
        const res = await fetch(this.buildFetchUrl(page), {
          headers: { "X-Requested-With": "XMLHttpRequest" },
        })
        const html = await res.text()

        const incoming = this.parseProductsFromHtml(html)

        // dedupe por ID
        for (const p of incoming) {
          if (!this.seenIds.has(p.id)) {
            this.seenIds.add(p.id)
            this.accumulated.push(p)
          }
        }

        // primeira hidratação: remove SSR pra não duplicar
        if (!this.hydrated && this.accumulated.length > 0) {
          this.hydrated = true

          this.$nextTick(() => {
            this.safeTakeoverSSR()
          })
        }
      } finally {
        this.isLoading = false
      }
    },

    loadMore() {
      if (!this.canLoadMore) return
      this.currentPageLocal += 1
      this.loadPage(this.currentPageLocal)
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
