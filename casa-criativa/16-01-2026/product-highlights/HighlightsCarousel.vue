<!-- ATENÇÃO: O componente a seguir contém código novo. NÃO é um component nativo -->

<template>
  <section class="highlight">
    <div class="container highlights-6">
      <div class="highlight-holder">
        <!-- MOBILE (<= 700): CARROSSEL -->
        <div v-if="useCarousel" class="product-highlight-grid -carousel" v-cloak>
          <component
            :is="getComponent('splide')"
            v-if="ready"
            :options="carouselOptions"
          >
            <!-- 1) Frete grátis -->
            <component :is="getComponent('splide-slide')">
              <div style="align-items: center; display: flex; justify-content: center">
                <svg fill="#FFBD00" width="46" height="46" class="highlight-icon" viewBox="0 0 36 36" xmlns="http://www.w3.org/2000/svg" data-id="svg-loader_5">
                  <path fill-rule="evenodd" clip-rule="evenodd" d="M18 9C19.864 9 21.375 10.511 21.375 12.375V21.375C20.1324 21.375 19.125 22.3824 19.125 23.625C19.125 24.0348 19.2346 24.4191 19.426 24.75H15.449C15.6404 24.4191 15.75 24.0348 15.75 23.625C15.75 23.2152 15.6404 22.8309 15.449 22.5H19.125L19.125 12.375C19.125 11.7537 18.6213 11.25 18 11.25L11.25 11.25C10.6287 11.25 10.125 11.7537 10.125 12.375L10.125 21.375C10.125 21.9963 10.6287 22.5 11.25 22.5H11.551C11.3596 22.8309 11.25 23.2152 11.25 23.625C11.25 24.0348 11.3596 24.4191 11.551 24.75H11.25C9.38604 24.75 7.875 23.239 7.875 21.375V12.375C7.875 10.511 9.38604 9 11.25 9H18Z"></path>
                  <path fill-rule="evenodd" clip-rule="evenodd" d="M13.5 22.5C12.8787 22.5 12.375 23.0037 12.375 23.625C12.375 24.2463 12.8787 24.75 13.5 24.75C14.1213 24.75 14.625 24.2463 14.625 23.625C14.625 23.0037 14.1213 22.5 13.5 22.5ZM10.125 23.625C10.125 21.761 11.636 20.25 13.5 20.25C15.364 20.25 16.875 21.761 16.875 23.625C16.875 25.489 15.364 27 13.5 27C11.636 27 10.125 25.489 10.125 23.625Z"></path>
                  <path fill-rule="evenodd" clip-rule="evenodd" d="M21.375 22.5C20.7537 22.5 20.25 23.0037 20.25 23.625C20.25 24.2463 20.7537 24.75 21.375 24.75C21.9963 24.75 22.5 24.2463 22.5 23.625C22.5 23.0037 21.9963 22.5 21.375 22.5ZM18 23.625C18 21.761 19.511 20.25 21.375 20.25C23.239 20.25 24.75 21.761 24.75 23.625C24.75 25.489 23.239 27 21.375 27C19.511 27 18 25.489 18 23.625Z"></path>
                  <path fill-rule="evenodd" clip-rule="evenodd" d="M24.8865 16.3525C25.5194 16.9854 25.875 17.8439 25.875 18.739L25.875 21.375C25.875 21.9963 25.3713 22.5 24.75 22.5H23.324C23.5154 22.8309 23.625 23.2152 23.625 23.625C23.625 24.0348 23.5154 24.4191 23.324 24.75H24.75C26.614 24.75 28.125 23.239 28.125 21.375V18.739C28.125 17.2471 27.5324 15.8164 26.4775 14.7615L25.0795 13.3635C24.4466 12.7306 23.5881 12.375 22.693 12.375H20.25C19.9516 12.375 19.6655 12.4935 19.4545 12.7045C19.2435 12.9155 19.125 13.2016 19.125 13.5V23.625C19.125 22.3824 20.1324 21.375 21.375 21.375V14.625H22.693C22.9914 14.625 23.2775 14.7435 23.4885 14.9545L24.8865 16.3525Z"></path>
                </svg>

                <span style="color: rgb(34, 34, 34); font-size: 13px; max-width: 177px;">
                  <p><strong>Frete Grátis Sul e Sudeste</strong></p>
                  <p>pedidos acima R$250</p>
                </span>
              </div>
            </component>

            <!-- 2) PIX -->
            <component :is="getComponent('splide-slide')">
              <div style="align-items: center; display: flex; justify-content: center">
                <svg fill="#FFBD00" width="46" height="46" viewBox="0 0 32 32" xmlns="http://www.w3.org/2000/svg" data-id="svg-loader_6">
                  <path fill-rule="evenodd" clip-rule="evenodd" d="M16 6C21.5228 6 26 10.4772 26 16C26 21.5228 21.5228 26 16 26C10.4772 26 6 21.5228 6 16C6 10.4772 10.4772 6 16 6ZM24 16C24 11.5817 20.4183 8 16 8C11.5817 8 8 11.5817 8 16C8 20.4183 11.5817 24 16 24C20.4183 24 24 20.4183 24 16Z"></path>
                  <path fill-rule="evenodd" clip-rule="evenodd" d="M12.7094 13.7094C13.0497 12.6886 14.0051 12 15.0811 12H19V14H15.0811C14.8659 14 14.6749 14.1377 14.6068 14.3419C14.4989 14.6657 14.7399 15 15.0811 15H16.9189C18.6253 15 19.8302 16.6717 19.2906 18.2906C18.9503 19.3114 17.9949 20 16.9189 20H13V18H16.9189C17.1341 18 17.3251 17.8623 17.3932 17.6581C17.5011 17.3343 17.2601 17 16.9189 17H15.0811C13.3747 17 12.1698 15.3283 12.7094 13.7094Z"></path>
                  <path fill-rule="evenodd" clip-rule="evenodd" d="M15 14L15 11H17L17 14H15Z"></path>
                  <path fill-rule="evenodd" clip-rule="evenodd" d="M15 21L15 18H17L17 21H15Z"></path>
                </svg>

                <span style="color: rgb(34, 34, 34); font-size: 13px; max-width: 177px;">
                  <p><strong>3% De Desconto em </strong>pagamento no PIX</p>
                </span>
              </div>
            </component>

            <!-- 3) +90 mil -->
            <component :is="getComponent('splide-slide')">
              <div style="align-items: center; display: flex; justify-content: center">
                <svg fill="#FFBD00" width="46" height="46" class="highlight-icon" viewBox="0 0 32 32" xmlns="http://www.w3.org/2000/svg" data-id="svg-loader_7">
                  <path fill-rule="evenodd" clip-rule="evenodd" d="M11.9996 7.34375L20.9996 12.5337L20.0004 14.2663L11.0004 9.07631L11.9996 7.34375Z"></path>
                  <path fill-rule="evenodd" clip-rule="evenodd" d="M15.4993 16.8756L6.76928 11.8256L7.77073 10.0944L16 14.8548L24.2293 10.0944L25.2307 11.8256L16.5007 16.8756C16.191 17.0548 15.809 17.0548 15.4993 16.8756Z"></path>
                  <path fill-rule="evenodd" clip-rule="evenodd" d="M17 16V25H15V16H17Z"></path>
                  <path fill-rule="evenodd" clip-rule="evenodd" d="M14.5431 5.66543C15.4491 5.16206 16.5509 5.16206 17.4569 5.66543L24.4569 9.55432C25.4093 10.0834 26 11.0873 26 12.1768V19.8232C26 20.9127 25.4093 21.9165 24.4569 22.4457L17.4569 26.3345C16.5509 26.8379 15.4491 26.8379 14.5431 26.3345L7.54307 22.4457C6.59068 21.9165 6 20.9127 6 19.8232V12.1768C6 11.0873 6.59068 10.0834 7.54307 9.55432L14.5431 5.66543ZM16.4856 7.41375C16.1836 7.24596 15.8164 7.24596 15.5144 7.41375L8.51436 11.3026C8.19689 11.479 8 11.8136 8 12.1768V19.8232C8 20.1863 8.19689 20.521 8.51436 20.6973L15.5144 24.5862C15.8164 24.754 16.1836 24.754 16.4856 24.5862L23.4856 20.6973C23.8031 20.521 24 20.1863 24 19.8232V12.1768C24 11.8136 23.8031 11.479 23.4856 11.3026L16.4856 7.41375Z"></path>
                </svg>

                <span style="color: rgb(34, 34, 34); font-size: 13px; max-width: 177px;">
                  <p><strong>+90 mil pedidos</strong></p>
                  <p>entregues no Brasil</p>
                </span>
              </div>
            </component>

            <!-- 4) WhatsApp -->
            <component :is="getComponent('splide-slide')">
              <div style="align-items: center; display: flex; justify-content: center">
                <svg fill="#FFBD00" width="46" height="46" viewBox="0 0 32 32" xmlns="http://www.w3.org/2000/svg" data-id="svg-loader_8">
                  <path fill-rule="evenodd" clip-rule="evenodd" d="M22.7433 22.4627C24.0513 23.5192 26 22.5882 26 20.9069L26 10C26 8.34315 24.6569 7 23 7L13 7C11.3431 7 10 8.34315 10 10V18C10 19.6569 11.3431 21 13 21H20.5789C20.8076 21 21.0293 21.0784 21.2072 21.2221L22.7433 22.4627ZM24 20.9069L22.4639 19.6662C21.9302 19.2351 21.2649 19 20.5789 19H13C12.4477 19 12 18.5523 12 18V10C12 9.44772 12.4477 9 13 9L23 9C23.5523 9 24 9.44771 24 10L24 20.9069Z"></path>
                  <path fill-rule="evenodd" clip-rule="evenodd" d="M6 24.365C6 26.0606 7.97772 26.9869 9.28037 25.9014L10.0839 25.2318C10.2636 25.082 10.4902 25 10.7241 25H17C18.6569 25 20 23.6569 20 22H18C18 22.5523 17.5523 23 17 23H10.7241C10.0223 23 9.34269 23.246 8.80355 23.6953L8 24.365L8 15C8 14.4477 8.44772 14 9 14L9 12C7.34315 12 6 13.3431 6 15L6 24.365Z"></path>
                  <path fill-rule="evenodd" clip-rule="evenodd" d="M14 11H22V13H14V11Z"></path>
                  <path fill-rule="evenodd" clip-rule="evenodd" d="M14 15H22V17H14V15Z"></path>
                </svg>

                <span style="color: rgb(34, 34, 34); font-size: 13px; max-width: 177px;">
                  <p><strong>Faça Seu Pedido</strong></p>
                  <p>via whatsapp</p>
                </span>
              </div>
            </component>

            <!-- 5) 5x sem juros -->
            <component :is="getComponent('splide-slide')">
              <div style="align-items: center; display: flex; justify-content: center">
                <svg fill="#FFBD00" width="46" height="46" viewBox="0 0 32 32" xmlns="http://www.w3.org/2000/svg">
                  <path opacity="0.6" fill-rule="evenodd" clip-rule="evenodd" d="M6 24.365C6 26.0606 7.97772 26.9869 9.28037 25.9014L10.0839 25.2318C10.2636 25.082 10.4902 25 10.7241 25H17C18.6569 25 20 23.6569 20 22H18C18 22.5523 17.5523 23 17 23H10.7241C10.0223 23 9.34269 23.246 8.80355 23.6953L8 24.365L8 15C8 14.4477 8.44772 14 9 14L9 12C7.34315 12 6 13.3431 6 15L6 24.365Z"></path>
                  <path fill-rule="evenodd" clip-rule="evenodd" d="M22.7433 22.4627C24.0513 23.5192 26 22.5882 26 20.9069L26 10C26 8.34315 24.6569 7 23 7L13 7C11.3431 7 10 8.34315 10 10V18C10 19.6569 11.3431 21 13 21H20.5789C20.8076 21 21.0293 21.0784 21.2072 21.2221L22.7433 22.4627ZM24 20.9069L22.4639 19.6662C21.9302 19.2351 21.2649 19 20.5789 19H13C12.4477 19 12 18.5523 12 18V10C12 9.44772 12.4477 9 13 9L23 9C23.5523 9 24 9.44771 24 10L24 20.9069Z"></path>
                  <path d="M14.5 11H18.5V12.2H15.7L15.4 13.8C15.7 13.6 16.1 13.5 16.5 13.5C17.6 13.5 18.5 14.3 18.5 15.5C18.5 16.7 17.6 17.5 16.5 17.5C15.6 17.5 14.9 17 14.6 16.3L15.7 15.9C15.8 16.2 16.1 16.4 16.5 16.4C17 16.4 17.3 16 17.3 15.5C17.3 15 17 14.6 16.5 14.6C16.1 14.6 15.8 14.8 15.6 15.1L14.3 14.8L14.5 11Z"></path>
                  <path d="M19.5 13.5H20.7L21.5 14.7L22.3 13.5H23.5L22.1 15.5L23.5 17.5H22.3L21.5 16.3L20.7 17.5H19.5L20.9 15.5L19.5 13.5Z"></path>
                </svg>

                <span style="color: rgb(34, 34, 34); font-size: 13px; max-width: 177px;">
                  <p><strong>Até 5X sem juros</strong></p>
                  <p>Parcela mínima R$50</p>
                </span>
              </div>
            </component>
          </component>
        </div>

        <!-- DESKTOP (> 700): MARKUP BRUTO ORIGINAL (igual o seu) -->
        <div v-else class="product-highlight-grid">
          <div class="product-highlight-card first-row">
            <div style="align-items: center; display: flex;">
              <svg fill="#FFBD00" width="46" height="46" class="highlight-icon" viewBox="0 0 36 36" xmlns="http://www.w3.org/2000/svg" data-id="svg-loader_5">
                <path fill-rule="evenodd" clip-rule="evenodd" d="M18 9C19.864 9 21.375 10.511 21.375 12.375V21.375C20.1324 21.375 19.125 22.3824 19.125 23.625C19.125 24.0348 19.2346 24.4191 19.426 24.75H15.449C15.6404 24.4191 15.75 24.0348 15.75 23.625C15.75 23.2152 15.6404 22.8309 15.449 22.5H19.125L19.125 12.375C19.125 11.7537 18.6213 11.25 18 11.25L11.25 11.25C10.6287 11.25 10.125 11.7537 10.125 12.375L10.125 21.375C10.125 21.9963 10.6287 22.5 11.25 22.5H11.551C11.3596 22.8309 11.25 23.2152 11.25 23.625C11.25 24.0348 11.3596 24.4191 11.551 24.75H11.25C9.38604 24.75 7.875 23.239 7.875 21.375V12.375C7.875 10.511 9.38604 9 11.25 9H18Z"></path>
                <path fill-rule="evenodd" clip-rule="evenodd" d="M13.5 22.5C12.8787 22.5 12.375 23.0037 12.375 23.625C12.375 24.2463 12.8787 24.75 13.5 24.75C14.1213 24.75 14.625 24.2463 14.625 23.625C14.625 23.0037 14.1213 22.5 13.5 22.5ZM10.125 23.625C10.125 21.761 11.636 20.25 13.5 20.25C15.364 20.25 16.875 21.761 16.875 23.625C16.875 25.489 15.364 27 13.5 27C11.636 27 10.125 25.489 10.125 23.625Z"></path>
                <path fill-rule="evenodd" clip-rule="evenodd" d="M21.375 22.5C20.7537 22.5 20.25 23.0037 20.25 23.625C20.25 24.2463 20.7537 24.75 21.375 24.75C21.9963 24.75 22.5 24.2463 22.5 23.625C22.5 23.0037 21.9963 22.5 21.375 22.5ZM18 23.625C18 21.761 19.511 20.25 21.375 20.25C23.239 20.25 24.75 21.761 24.75 23.625C24.75 25.489 23.239 27 21.375 27C19.511 27 18 25.489 18 23.625Z"></path>
                <path fill-rule="evenodd" clip-rule="evenodd" d="M24.8865 16.3525C25.5194 16.9854 25.875 17.8439 25.875 18.739L25.875 21.375C25.875 21.9963 25.3713 22.5 24.75 22.5H23.324C23.5154 22.8309 23.625 23.2152 23.625 23.625C23.625 24.0348 23.5154 24.4191 23.324 24.75H24.75C26.614 24.75 28.125 23.239 28.125 21.375V18.739C28.125 17.2471 27.5324 15.8164 26.4775 14.7615L25.0795 13.3635C24.4466 12.7306 23.5881 12.375 22.693 12.375H20.25C19.9516 12.375 19.6655 12.4935 19.4545 12.7045C19.2435 12.9155 19.125 13.2016 19.125 13.5V23.625C19.125 22.3824 20.1324 21.375 21.375 21.375V14.625H22.693C22.9914 14.625 23.2775 14.7435 23.4885 14.9545L24.8865 16.3525Z"></path>
              </svg>

              <span style="color: rgb(34, 34, 34); font-size: 13px; max-width: 177px;">
                <p><strong>Frete Grátis Sul e Sudeste</strong></p>
                <p>pedidos acima R$250</p>
              </span>
            </div>
          </div>

          <div class="product-highlight-card first-row">
            <div style="align-items: center; display: flex;">
              <svg fill="#FFBD00" width="46" height="46" viewBox="0 0 32 32" xmlns="http://www.w3.org/2000/svg" data-id="svg-loader_6">
                <path fill-rule="evenodd" clip-rule="evenodd" d="M16 6C21.5228 6 26 10.4772 26 16C26 21.5228 21.5228 26 16 26C10.4772 26 6 21.5228 6 16C6 10.4772 10.4772 6 16 6ZM24 16C24 11.5817 20.4183 8 16 8C11.5817 8 8 11.5817 8 16C8 20.4183 11.5817 24 16 24C20.4183 24 24 20.4183 24 16Z"></path>
                <path fill-rule="evenodd" clip-rule="evenodd" d="M12.7094 13.7094C13.0497 12.6886 14.0051 12 15.0811 12H19V14H15.0811C14.8659 14 14.6749 14.1377 14.6068 14.3419C14.4989 14.6657 14.7399 15 15.0811 15H16.9189C18.6253 15 19.8302 16.6717 19.2906 18.2906C18.9503 19.3114 17.9949 20 16.9189 20H13V18H16.9189C17.1341 18 17.3251 17.8623 17.3932 17.6581C17.5011 17.3343 17.2601 17 16.9189 17H15.0811C13.3747 17 12.1698 15.3283 12.7094 13.7094Z"></path>
                <path fill-rule="evenodd" clip-rule="evenodd" d="M15 14L15 11H17L17 14H15Z"></path>
                <path fill-rule="evenodd" clip-rule="evenodd" d="M15 21L15 18H17L17 21H15Z"></path>
              </svg>

              <span style="color: rgb(34, 34, 34); font-size: 13px; max-width: 177px;">
                <p><strong>3% De Desconto em </strong>pagamento no PIX</p>
              </span>
            </div>
          </div>

          <div class="product-highlight-card second-row">
            <div style="align-items: center; display: flex;">
              <svg fill="#FFBD00" width="46" height="46" class="highlight-icon" viewBox="0 0 32 32" xmlns="http://www.w3.org/2000/svg" data-id="svg-loader_7">
                <path fill-rule="evenodd" clip-rule="evenodd" d="M11.9996 7.34375L20.9996 12.5337L20.0004 14.2663L11.0004 9.07631L11.9996 7.34375Z"></path>
                <path fill-rule="evenodd" clip-rule="evenodd" d="M15.4993 16.8756L6.76928 11.8256L7.77073 10.0944L16 14.8548L24.2293 10.0944L25.2307 11.8256L16.5007 16.8756C16.191 17.0548 15.809 17.0548 15.4993 16.8756Z"></path>
                <path fill-rule="evenodd" clip-rule="evenodd" d="M17 16V25H15V16H17Z"></path>
                <path fill-rule="evenodd" clip-rule="evenodd" d="M14.5431 5.66543C15.4491 5.16206 16.5509 5.16206 17.4569 5.66543L24.4569 9.55432C25.4093 10.0834 26 11.0873 26 12.1768V19.8232C26 20.9127 25.4093 21.9165 24.4569 22.4457L17.4569 26.3345C16.5509 26.8379 15.4491 26.8379 14.5431 26.3345L7.54307 22.4457C6.59068 21.9165 6 20.9127 6 19.8232V12.1768C6 11.0873 6.59068 10.0834 7.54307 9.55432L14.5431 5.66543ZM16.4856 7.41375C16.1836 7.24596 15.8164 7.24596 15.5144 7.41375L8.51436 11.3026C8.19689 11.479 8 11.8136 8 12.1768V19.8232C8 20.1863 8.19689 20.521 8.51436 20.6973L15.5144 24.5862C15.8164 24.754 16.1836 24.754 16.4856 24.5862L23.4856 20.6973C23.8031 20.521 24 20.1863 24 19.8232V12.1768C24 11.8136 23.8031 11.479 23.4856 11.3026L16.4856 7.41375Z"></path>
              </svg>

              <span style="color: rgb(34, 34, 34); font-size: 13px; max-width: 177px;">
                <p>
                  <strong>+90 mil pedidos</strong>
                </p>
                <p>entregues no Brasil</p>
              </span>
            </div>
          </div>

          <div class="product-highlight-card second-row">
            <div style="align-items: center; display: flex;">
              <svg fill="#FFBD00" width="46" height="46" viewBox="0 0 32 32" xmlns="http://www.w3.org/2000/svg" data-id="svg-loader_8">
                <path fill-rule="evenodd" clip-rule="evenodd" d="M22.7433 22.4627C24.0513 23.5192 26 22.5882 26 20.9069L26 10C26 8.34315 24.6569 7 23 7L13 7C11.3431 7 10 8.34315 10 10V18C10 19.6569 11.3431 21 13 21H20.5789C20.8076 21 21.0293 21.0784 21.2072 21.2221L22.7433 22.4627ZM24 20.9069L22.4639 19.6662C21.9302 19.2351 21.2649 19 20.5789 19H13C12.4477 19 12 18.5523 12 18V10C12 9.44772 12.4477 9 13 9L23 9C23.5523 9 24 9.44771 24 10L24 20.9069Z"></path>
                <path fill-rule="evenodd" clip-rule="evenodd" d="M6 24.365C6 26.0606 7.97772 26.9869 9.28037 25.9014L10.0839 25.2318C10.2636 25.082 10.4902 25 10.7241 25H17C18.6569 25 20 23.6569 20 22H18C18 22.5523 17.5523 23 17 23H10.7241C10.0223 23 9.34269 23.246 8.80355 23.6953L8 24.365L8 15C8 14.4477 8.44772 14 9 14L9 12C7.34315 12 6 13.3431 6 15L6 24.365Z"></path>
                <path fill-rule="evenodd" clip-rule="evenodd" d="M14 11H22V13H14V11Z"></path>
                <path fill-rule="evenodd" clip-rule="evenodd" d="M14 15H22V17H14V15Z"></path>
              </svg>

              <span style="color: rgb(34, 34, 34); font-size: 13px; max-width: 177px;">
                <p><strong>Faça Seu Pedido</strong> </p><p>via whatsapp</p>
              </span>
            </div>
          </div>

          <div class="product-highlight-card second-row">
            <div style="align-items: center; display: flex;">
              <svg fill="#FFBD00" width="46" height="46" viewBox="0 0 32 32" xmlns="http://www.w3.org/2000/svg">
                <path opacity="0.6" fill-rule="evenodd" clip-rule="evenodd" d="M6 24.365C6 26.0606 7.97772 26.9869 9.28037 25.9014L10.0839 25.2318C10.2636 25.082 10.4902 25 10.7241 25H17C18.6569 25 20 23.6569 20 22H18C18 22.5523 17.5523 23 17 23H10.7241C10.0223 23 9.34269 23.246 8.80355 23.6953L8 24.365L8 15C8 14.4477 8.44772 14 9 14L9 12C7.34315 12 6 13.3431 6 15L6 24.365Z"></path>

                <path fill-rule="evenodd" clip-rule="evenodd" d="M22.7433 22.4627C24.0513 23.5192 26 22.5882 26 20.9069L26 10C26 8.34315 24.6569 7 23 7L13 7C11.3431 7 10 8.34315 10 10V18C10 19.6569 11.3431 21 13 21H20.5789C20.8076 21 21.0293 21.0784 21.2072 21.2221L22.7433 22.4627ZM24 20.9069L22.4639 19.6662C21.9302 19.2351 21.2649 19 20.5789 19H13C12.4477 19 12 18.5523 12 18V10C12 9.44772 12.4477 9 13 9L23 9C23.5523 9 24 9.44771 24 10L24 20.9069Z"></path>

                <path d="M14.5 11H18.5V12.2H15.7L15.4 13.8C15.7 13.6 16.1 13.5 16.5 13.5C17.6 13.5 18.5 14.3 18.5 15.5C18.5 16.7 17.6 17.5 16.5 17.5C15.6 17.5 14.9 17 14.6 16.3L15.7 15.9C15.8 16.2 16.1 16.4 16.5 16.4C17 16.4 17.3 16 17.3 15.5C17.3 15 17 14.6 16.5 14.6C16.1 14.6 15.8 14.8 15.6 15.1L14.3 14.8L14.5 11Z"></path>

                <path d="M19.5 13.5H20.7L21.5 14.7L22.3 13.5H23.5L22.1 15.5L23.5 17.5H22.3L21.5 16.3L20.7 17.5H19.5L20.9 15.5L19.5 13.5Z"></path>
              </svg>

              <span style="color: rgb(34, 34, 34); font-size: 13px; max-width: 177px;">
                <p><strong>Até 5X sem juros</strong> </p><p>Parcela mínima R$50</p>
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'HighlightsCarousel',

  data: () => ({
    viewportWidth: 9999,
    ready: true,
  }),


    computed: {
      useCarousel() {
        return this.viewportWidth <= 700;
      },

      carouselOptions() {
        return {
          type: 'loop',
          perMove: 1,

          autoplay: true,
          interval: 6000,
          pauseOnHover: false,
          pauseOnFocus: false,

          arrows: false,
          pagination: true,

          gap: '12px',
          padding: { left: '16px', right: '16px' },

          perPage: 1,
          breakpoints: {
            549: { perPage: 1 }, // até 549px
            700: { perPage: 2 }, // 550px até 700px
          },
        };
      }
    },

  watch: {
    useCarousel() {
      this.ready = false;
      this.$nextTick(() => (this.ready = true));
    },
  },

  mounted() {
    if (typeof window === 'undefined') return;
    this.viewportWidth = window.innerWidth;
    window.addEventListener('resize', this.handleResize, { passive: true });
  },

  beforeDestroy() {
    if (typeof window === 'undefined') return;
    window.removeEventListener('resize', this.handleResize);
  },

  methods: {
    handleResize() {
      this.viewportWidth = window.innerWidth;
    },

    getComponent(component) {
      return component;
    },
  },
};
</script>

<style scoped lang="scss">
    section.highlight {
        padding-inline: 16px;
    }
</style>
