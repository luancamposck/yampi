<template>
  <div class="load-more-footer" v-if="pageCount > 1">
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
      Sem mais produtos
    </div>
  </div>
</template>

<script>
export default {
  name: "LoadMoreAppend",

  props: {
    pageCount: { type: Number, default: 1 },
  },

  data: () => ({
    isLoading: false,
    currentPageLocal: 1,
  }),

  computed: {
    canLoadMore() {
      return this.currentPageLocal < this.pageCount
    },
  },

  mounted() {
    // Pega a página atual pela URL (se não tiver, é 1)
    const url = new URL(window.location.href)
    const page = Number(url.searchParams.get("page") || 1)
    this.currentPageLocal = Number.isFinite(page) ? page : 1
  },

  methods: {
      activateImages(nodes) {
      for (const node of nodes) {
        // pega <img> lazy e também custom-image se for o caso
        const imgs = node.querySelectorAll("img[data-src]")
        imgs.forEach((img) => {
          const real = img.getAttribute("data-src")
          if (real && img.getAttribute("src") !== real) {
            img.setAttribute("src", real)
            img.classList.remove("-loading")
          }
        })
      }
    },
    buildFetchUrl(nextPage) {
      const url = new URL(window.location.href)
      url.searchParams.set("page", String(nextPage))

      // tenta pedir só resultados (se o backend reconhecer)
      url.searchParams.set("resultsOnly", "true")

      return url.toString()
    },

    extractProductNodesFromHtml(html) {
      const tmp = document.createElement("div")
      tmp.innerHTML = html

      const list = tmp.querySelector(".products-list")
      if (!list) return []

      // pega só os cards (remove os clears)
      return Array.from(list.querySelectorAll(".box-product:not(.-clear)"))
    },

    appendToCurrentList(nodes) {
      const targetList = document.querySelector(".products-list")
      if (!targetList) return

      // remove clears atuais
      targetList.querySelectorAll(".box-product.-clear").forEach(el => el.remove())

      // se existir mosaic columns (mobile mosaic), anexa alternando
      const cols = targetList.querySelectorAll(".mosaic-column")
      if (cols.length === 2) {
        const allExisting = Array.from(targetList.querySelectorAll(".box-product[order]"))
        const maxOrder = allExisting.reduce((acc, el) => {
          const v = Number(el.getAttribute("order"))
          return Number.isFinite(v) ? Math.max(acc, v) : acc
        }, -1)

        let order = maxOrder + 1
        for (const node of nodes) {
          node.setAttribute("order", String(order))
          const col = order % 2 === 0 ? cols[0] : cols[1]
          col.appendChild(node)
          order++
        }
      } else {
        // grid normal: append direto
        for (const node of nodes) targetList.appendChild(node)
      }

      // recoloca clears
      for (let i = 0; i < 3; i++) {
        const clear = document.createElement("div")
        clear.className = "box-product -clear"
        targetList.appendChild(clear)
      }
    },

    async loadMore() {
      if (!this.canLoadMore || this.isLoading) return

      this.isLoading = true
      const nextPage = this.currentPageLocal + 1

      try {
        const res = await fetch(this.buildFetchUrl(nextPage), {
          headers: { "X-Requested-With": "XMLHttpRequest" },
        })
        const html = await res.text()

        const nodes = this.extractProductNodesFromHtml(html)
        if (!nodes.length) {
          // se não veio nada, para por aqui
          this.currentPageLocal = this.pageCount
          return
        }

        this.appendToCurrentList(nodes)
        this.activateImages(nodes)
        this.currentPageLocal = nextPage
      } catch (err) {
        console.error("[LoadMoreAppend] erro ao carregar mais:", err)
      } finally {
        this.isLoading = false
      }
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
