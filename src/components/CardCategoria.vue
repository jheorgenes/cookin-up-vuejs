<template>
  <article class="categoria">
    <header class="categoria__cabecalho">
      <!-- A forma de importar imagens da pasta public é usando :src e acessando diretamente o diretório ou arquivo dentro de public -->
      <!-- Vale lembrar também que pode ser usado a interpolação do js para deixar esse src dinâmico, como abaixo -->
      <img :src="`/imagens/icones/categorias_ingredientes/${categoria.imagem}`" alt="" class="categoria__imagem">
      <h2 class="paragrafo-lg categoria__nome">{{ categoria.nome }}</h2>
    </header>
    <ul class="categoria__ingredientes">
      <li v-for="ingrediente in categoria.ingredientes" :key="ingrediente">
        <IngredienteSelecionavel 
          :ingrediente="ingrediente"
          @adicionar-ingrediente="$emit('adicionarIngrediente', $event)" 
          @remover-ingrediente="$emit('removerIngrediente', $event)" 
        />
        <!-- O Componente IngredienteSelecionavel emite um evento chamdao adicionar-ingrediente. 
          Aqui, estamos reemitindo esse mesmo evento pra o componente pai desse CardCategoria -->
      </li>
    </ul>
  </article>
</template>

<script lang="ts">
import type ICategoria from '@/interfaces/ICategoria';
import type { PropType } from 'vue';
import Tag from './Tag.vue';
import IngredienteSelecionavel from './IngredienteSelecionavel.vue';

export default {
    props: {
        categoria: {
            type: Object as PropType<ICategoria>, //PropType permite eu definir tipagem de Interfaces em props.
            required: true
        }
    },
    components: { Tag, IngredienteSelecionavel },
    emits: ['adicionarIngrediente', 'removerIngrediente']
}
</script>

<style scoped>
.categoria {
  width: 19.5rem;
  padding: 1rem;
  border-radius: 1rem;
  background: var(--branco, #FFF);
  box-shadow: 4px 4px 10px 0px rgba(68, 68, 68, 0.05);
  height: 100%;

  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem;
}

.categoria__cabecalho {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
}

.categoria__imagem {
  width: 3.5rem;
}

.categoria__nome {
  text-align: center;
  color: var(--verde-medio, #3D6D4A);
  font-weight: 700;
}

.categoria__ingredientes {
  display: flex;
  justify-content: center;
  gap: 0.5rem;
  flex-wrap: wrap;
}

</style>

