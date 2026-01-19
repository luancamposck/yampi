<template>
  <div class="product-customizations">
    <template v-if="firstValidSku">
      <!-- <SelectSku
        v-if="!validProduct.simple"
        ref="selectSku"
        :variations-style="variationsStyle"
        @update="setSelectedSku($event)"
      /> -->

      <SelectSku
        v-if="!validProduct.simple"
        ref="selectSku"
        :variations-style="variationsStyle"
        @update="setSelectedSku($event)"
        @unit-length-changed="updateVariantBaseUnit"
      />

      <SkuCustomizations
        v-if="selectedSku"
        ref="skuCustomizations"
        :sku="selectedSku"
        @change="setCustomizations($event)"
      />
    </template>

    <div class="main-product-buy-button-holder flex">
      <QuantitySelector
        v-if="showQuantitySelector"
        v-model="quantity"
        :min="1"
        :max="maxQuantity"
        :disabled="!canAddToCart"
        @interaction="quantityTouched = true"
      />

      <LoaderButton
        class="btn btn-primary"
        :title="buyButtonText"
        :sending="loading"
        :disabled="!canAddToCart"
        :listen-position="true"
        @click="addToCart()"
      />

      <FloatingButton
        v-if="showMobileFloatingButton"
        :quantity="quantity"
        :loading-button="loading"
        :disabled="!canAddToCart"
        @click="addToCart('floating-button')"
      />
    </div>

    <!-- Ínicio código novo: aviso visual quando a quantidade encosta no estoque máximo -->
    <div v-if="showLimitReached" class="quantity-limit-holder">
      <span class="quantity-limit-reached">
        *Limite do produto atingido
      </span>
    </div>
    <!-- Fim código novo -->

    <div v-if="showQuantityShowcase" class="quantity-showcase">
      Você está comprando
      <span style="color: #f6ba02; font-weight: 800; margin-left: 2px;">
        {{ formattedQuantityInMeters }}
      </span>
    </div>

    <Cashback
      v-if="hasCashbackValid"
      class="mt-21 mb-21"
      :percent-amount="validCashback.percent_amount"
    />

    <div v-if="!firstValidSku" class="main-product-unavailable alert -yellow">
      Produto indisponível.
    </div>

    <InventoryCountdown v-if="firstValidSku && showInventoryCountdown" />

    <Zipcode
      v-if="showShippingForm"
      :quantity="quantity"
      :disabled="!firstValidSku"
    />
  </div>
</template>

<script>
import { mapActions } from '~/vuex'
import _ from '~/lodash'
import productMixin from '@/mixins/product'
import cashbackMixin from '@/mixins/cashback'
import trackingByApi from '@/mixins/tracking/api'

export default {
  name: 'ProductCustomizations',

  mixins: [
    productMixin,
    cashbackMixin,
    trackingByApi,
  ],

  props: {
    buyButtonText: {
      type: String,
      default: 'Comprar',
    },

    showQuantitySelector: {
      type: Boolean,
      default: false,
    },

    showInventoryCountdown: {
      type: Boolean,
      default: false,
    },

    showShippingForm: {
      type: Boolean,
      default: false,
    },

    showModalAfterPurchase: {
      type: Boolean,
      default: false,
    },

    showMobileFloatingButton: {
      type: Boolean,
      default: false,
    },

    cartType: {
      type: String,
      default: 'suspended',
    },

    variationsStyle: {
      type: String,
      default: 'list',
    },

    cashbacks: {
      type: Array,
      default: () => [],
    },
  },

  data: () => ({
    quantity: 1,
    loading: false,
    customizationValues: {},
    customizationHasErrors: false,

    // Ínicio código novo: guarda o tamanho base (em metros) da unidade vindo da variação selecionada
    // ex: 0.5 (50cm), 0.55 (55cm)
    variantBaseUnitInMeters: null,
    // Fim código novo

    // Ínicio código novo: controla se o usuário já interagiu com o seletor (pra não mostrar aviso “do nada”)
    quantityTouched: false,
    // Fim código novo
  }),

  // Ínicio código novo
  watch: {
    /**
     * Dispara um evento global sempre que a quantidade muda (útil para componentes de preço/parcelas reagirem)
     */
    quantity: {
      immediate: true,
      handler(newQuantity) {
        this.$root.$emit('product-quantity-changed', newQuantity)
      },
    },

    /**
     * Mantém a quantity sempre válida quando o estoque muda (troca de SKU/variação, atualização de dados, etc.)
     * - clampa para [1..estoque]
     * - se estoque <= 0, mantém quantity em 1 (mas canAddToCart bloqueia compra)
     * - reseta o estado “touched” para o aviso reaparecer só após nova interação
     */
    stockAvailable: {
      immediate: true,
      handler(stock) {
        this.quantityTouched = false

        if (typeof stock !== 'number') return

        if (stock <= 0) {
          this.quantity = 1
          return
        }

        if (this.quantity < 1) this.quantity = 1
        if (this.quantity > stock) this.quantity = stock
      },
    },
  },
  // Fim código novo

  computed: {
    // --- INÍCIO código novo ---

    /**
     * SKU efetivo para regras de estoque/compra:
     * - usa a variação selecionada quando existir
     * - senão cai no primeiro SKU válido
     */
    effectiveSku() {
      return this.selectedSku || this.firstValidSku || null
    },

    /**
     * Estoque disponível do SKU efetivo (número) ou null se não existir
     */
    stockAvailable() {
      const stock = _.get(this.effectiveSku, 'total_in_stock', null)
      return typeof stock === 'number' ? stock : null
    },

    /**
     * Limite máximo para o QuantitySelector:
     * - quando houver estoque > 0, limita pelo estoque
     * - caso contrário, não limita (false)
     */
    maxQuantity() {
      if (typeof this.stockAvailable === 'number' && this.stockAvailable > 0) {
        return this.stockAvailable
      }
      return false
    },

    /**
     * Mostra aviso quando o usuário já mexeu e a quantidade encostou no máximo permitido
     */
    showLimitReached() {
      return (
        this.quantityTouched &&
        this.maxQuantity !== false &&
        typeof this.maxQuantity === 'number' &&
        this.quantity >= this.maxQuantity &&
        this.canAddToCart
      )
    },

    /**
     * Retorna o tamanho base em metros de cada unidade:
     * - 50cm  -> 0.5m
     * - 100cm -> 1m
     * Se não achar nada, retorna null.
     */
    baseUnitInMeters() {
      // 1ª prioridade: valor vindo da VARIANTE selecionada
      if (this.variantBaseUnitInMeters) {
        return this.variantBaseUnitInMeters
      }

      // 2ª prioridade: título do produto (fallback)
      const name = _.get(this.validProduct, 'name', '').toLowerCase()

      if (name.includes('55cm')) return 0.55
      if (name.includes('50cm')) return 0.5
      if (name.includes('100cm')) return 1

      return null
    },

    /**
     * Deve ou não mostrar a div de "Você está comprando X m"
     */
    showQuantityShowcase() {
      return this.baseUnitInMeters !== null
    },

    /**
     * Quantidade total em metros (ex: 3 unidades de 0,5m = 1,5m)
     */
    quantityInMeters() {
      if (!this.baseUnitInMeters) return 0
      return this.quantity * this.baseUnitInMeters
    },

    /**
     * Quantidade formatada em metros para exibição (2 casas, vírgula)
     */
    formattedQuantityInMeters() {
      if (!this.baseUnitInMeters) return ''
      const total = this.quantityInMeters
      const formatted = total.toFixed(2).replace('.', ',')
      return `${formatted}m`
    },

    // --- FIM código novo ---

    canAddToCart() {
      // sem SKU válido, sem compra
      if (!this.effectiveSku) return false

      // bloqueio do próprio SKU
      if (_.get(this.effectiveSku, 'blocked_sale', false)) return false

      // estoque zerado => desabilita compra
      if (typeof this.stockAvailable === 'number' && this.stockAvailable <= 0) return false

      return true
    },

    customizations() {
      return _.get(this.selectedSku, 'customizations.data', [])
    },

    areCustomizationsValid() {
      if (!this.selectedSku || this.customizations.length === 0) {
        return true
      }

      if (this.selectedSku.allow_sell_without_customization) {
        return this.customizations
          .every(customization => _.isEmpty(this.customizationValues[customization.id]))
      }

      return this.customizations.every(customization => {
        if (!customization.required) return true
        return !_.isEmpty(this.customizationValues[customization.id])
      })
    },

    validCashback() {
      const { price_sale: salePrice, price_discount: discountPrice } = this.validSku

      let productPrice = salePrice
      if (discountPrice > 0) productPrice = discountPrice

      return this.getValidCashback(this.cashbacks, productPrice)
    },

    hasCashbackValid() {
      if (_.isEmpty(this.validCashback)) return false
      return this.validCashback.percent_amount > 0
    },
  },

  mounted() {
    this.bootSelectedSku()
    this.viewItem()
  },

  methods: {
    ...mapActions('cart', ['addProductsToCart']),
    ...mapActions('product', ['trackViewItem']),

    // Ínicio Código novo: recebe do SelectSku o tamanho da unidade (em metros) da variação selecionada
    updateVariantBaseUnit(lengthInMeters) {
      // pode vir 0.5, 0.55 ou null
      this.variantBaseUnitInMeters = lengthInMeters
    },
    // Fim Código novo

    viewItem() {
      this.trackViewItem({
        skus: this.firstValidSku,
        products: this.validProduct,
        value: this.firstValidSku.prices.data.price * this.quantity,
        quantities: this.quantity,
      })
    },

    bootSelectedSku() {
      if (this.validProduct.has_variations) return
      this.setSelectedSku(this.firstValidSku)
    },

    setCustomizations(payload) {
      this.customizationValues = payload
    },

    async addToCart(locationTrack = 'main-product-buy-button') {
      if (!this.selectedSku) {
        this.$refs.selectSku.verifySelect()
        return
      }

      if (!this.$refs.skuCustomizations.checkValues(this.$refs.skuCustomizations.values)) {
        return
      }

      this.loading = true

      const customization = {}
      const validValues = _.omitBy(this.customizationValues, _.isEmpty)

      // se todas customizações não são required, estão vazias e o sku não pode vender sem customização,
      // envia o objeto com valores vazios
      if (
        this.customizations.every(_customization => !_customization.required)
        && _.isEmpty(validValues)
        && !this.selectedSku.allow_sell_without_customization
      ) {
        customization[this.selectedSku.id] = this.customizationValues
      }

      if (!_.isEmpty(validValues)) {
        customization[this.selectedSku.id] = validValues
      }

      let has_recomm = false
      const { recomm_id } = window.Yampi.mago_config
      const item_metadata = []

      if (recomm_id) {
        has_recomm = true
        item_metadata.push({ recomm_id })
      }

      await this.addProductsToCart({
        skus: this.selectedSku,
        quantities: this.quantity,
        products: this.validProduct,
        value: this.selectedSku.prices.data.price * this.quantity,
        showModal: this.showModalAfterPurchase,
        cartType: this.cartType,
        extras: { has_recomm, customization, item_metadata },
      })

      const themeParams = window.themeConfig.theme.params

      this.handleTrackApi('purchase-intended', {
        location: locationTrack,
        quick_buy_button_enabled: themeParams.show_add_to_cart_button,
        product_quantity_updated: this.quantity,
        items: this.validProduct.name,
        amount: this.quantity * this.selectedSku.prices.data.price,
      })

      this.loading = false
    },
  },
}
</script>

<style scoped>
  /* Ínicio código novo: estilos do aviso de limite atingido */
  div.quantity-showcase {
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    border: 2px solid #f6ba02;
    border-radius: 10px;
    padding: 8px;
    font-size: 14px;
    font-weight: 600;
    color: #000;
  }

  div.quantity-showcase span {
    color: #000 !important;
  }

  .quantity-limit-holder {
    width: 100%;
    margin-top: 8px;
    margin-bottom: 8px;
    display: flex;
    justify-content: center
  }

  .quantity-limit-reached {
    display: inline-block;
    font-size: 16px;
    font-weight: 700;
    opacity: 0.95;
    color: #dd727a;
  }
  /* Fim código novo */
</style>
