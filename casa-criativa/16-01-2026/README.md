*16/01/2026*


## product-highlights
> Foi pedido um carrosel dos highlight na página de produto
> quando a loja for acessada por um dispositivo mobile.
> Adotei 700px como divisor de mobile/desktop, assim como a YAMPI já usa

Twig: `sections/product/product_content.twig`
Vue: `components/product/HighlightsCarousel.vue`
SASS: `assets/styles/sections/highlight.scss`

## quantity-limit
> Demanda para que o QuantitySelector.vue fosse limitado a quantidade em estoque e
> aparecesse uma mensagem "Atingiu limite máximo disponível"

Foram necessário apenas dois vue nessa demanda:

Vue: `components/generic/QuantitySelector.vue`
Vue: `components/product/ProductCustomization.vue`
