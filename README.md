# Vue.JS

## Getting Started

### Instalar dependências
```
yarn install
```

### Iniciar servidor web
```
yarn serve
```

### Realizar build para produção
```
yarn build
```

### Executar o linter nos arquivos
```
yarn lint
```

## Single File Components (SFC)

Todo arquivo `.vue` é um componente composto por **HTML, CSS e JS** em um mesmo arquivo.

```html
<template>
<!-- Tags HTML -->
</template>

<script>
// Scripts JS
export default {

}
</script>

<style>
/* Estilos CSS */
.header {
    background-color: #000;
    color: #fff;
}
</style>
```

## Diretivas

### v-show
Aplica `style="display: none"` no elemento, mas ainda mantém ele na DOM

```html
<div v-show="true">Componente está sendo exibido</div>
<div v-show="false">Componente não está sendo exibido</div>
```

### v-if, v-else-if, v-else
Declarações condicionais, quando falso, o elemento na DOM não é renderizado

```html
<div v-if="userType === 'Admin'">Usuário Admin</div>
<div v-else-if="userType === 'Gestor'">Usuário Gestor</div>
<div v-else>Usuário Normal</div>
```

### v-for
Instrução de loop, similar ao `.map` em **React**. Todo `v-for` precisa de uma `key` única

```html
<div v-for="user in users" :key="user.id">
    {{ user.firstName }}
    {{ user.lastName }}
</div>
```

### v-bind
Permite que o atributo passado para a tag seja dinâmico

```html
<img v-bind:src="imgSrc">
<!-- OU... -->
<img :src="imgSrc">
```

### v-model
Permite com que os dados sejam manipulados de forma bidirecional
- JS para HTML 
- HTML para JS

```html
<input v-model="name" type="text">
{{ name }}
```

> O input irá alterar o valor de `name`

## Eventos
Eventos são disparados quando determina ação é executada em um componente

```html
<template>
    <button @click="onClick">Clicar</button>
</template>

<script>
export default {
    methods: {
        onClick($event) {
            console.log($event)
        }
    }
}
</script>
```

### Modificadores de eventos
modificam eventos para determinadas ações, alguns modificadores são, ver mais [Event Handling](https://vuejs.org/guide/essentials/event-handling.html#event-modifiers)
- `.stop`
- `.prevent`
- `.self`
- `.capture`
- `.once`
- `.passive`

```html
<template>
    <!-- Irá executar onClick apenas uma vez  -->
    <button @click.once="onClick">Clicar</button>
</template>

<script>
export default {
    methods: {
        onClick($event) {
            console.log($event)
        }
    }
}
</script>
```

## Propriedades computadas
São métodos que o VueJS irá cachear e serão recomputados quando alguns dos atributos internos deles forem alterados. Esses métodos tem acesso ao objeto `data` e pode manipular esses dados.

```html
<template>
    <p>{{ fullName }}</p>
</template>

<script>
export default {
    data(): {
        return {
            user: {
                firstName: 'Daniel',
                lastName: 'Favero'
            }
        }
    }
    computed: {
        fullName() {
            return `${this.user.firstName} ${this.user.lastName}`
        },
    }
}
</script>
```

## Watchers
Executam quando determina valor é alterado.

```html
<template>
    <input type="text" v-model="name">
</template>

<script>
export default {
    data(): {
        return {
            name: 'Daniel Favero'
        }
    }
    watch: {
        // Irá executar quando o atributo name mudar (o nome do método deve ser o mesmo do atributo)
        name(newValue, oldValue) {
            console.log(newValue, oldValue)
        }
    }
}
</script>
```

Para atualizar valores dentro de objetos, o método em watch precisa ser alterado
<template>
    <input type="text" v-model="user.firstName">
    <input type="text" v-model="user.lastName">
</template>

<script>
export default {
    data(): {
        return {
            user: {
                firstName: 'Daniel'
                lastName: 'Favero'
            }
        }
    }
    watch: {
        user: {
            handler() {
                console.log("Usuário alterado")
            },
            // Permite que o watch observe os atributos do objeto
            deep: true
        }
    }
}
</script>

## Ciclo de vida de um componente
Um componente em `vue` tem um ciclo de vida divido em 4 etapas

- Criação
    - Preparação do componente
    - Não tem acesso a DOM da página
- Montagem
    - Inicializar um lib externa
    - Tem acesso a DOM da página
- Atualização
    - Usado para fazer debug e atualizações
- Desmontagem
    - Remover tudo que for necessário para liberar memória do sistema

É possível interagir com a aplicação em cada etapa do ciclo de vida com os seguintes métodos:

- `beforeCreated()`
- `created()`
- `beforeMounted()`
- `mounted()`
- `beforeUpdated()`
- `updated()`
- `beforeUnmounted()`
- `unmounted()`

## Slots
Similar ao `children` do **React**

`TheHeader.vue`
```html
<template>
    <header class="header">
        <slot></slot>
    </header>
</template>
<!-- ... -->
```

`App.vue`
```html
<template>
  <TheHeader>Hello World</TheHeader>
</template>
<!-- ... -->
```

O texto 'Hello World' é inserido dentro do `<slot>`. É possível criar mais de um slot por componente com a diretiva `v-slot`

`TheHeader.vue`
```html
<template>
    <header class="header">
        <slot name="title" />
        <slot name="description" />
        <slot />
    </header>
</template>
<!-- ... -->
```

`App.vue`
```html
<template>
  <TheHeader>
    <template v-slot:title>
        <h1>Home</h1>
    </template>

    <template v-slot:description>
        <p>Lorem Ipsum Dolor Sit Amet</p>
    </template>

    Estou sendo inserido no último slot :)
  </TheHeader>
</template>
<!-- ... -->
```

É possível acessar os slots do componente através de `this.$slots`

## Trabalhando com CSS

### 1º Forma (Global)
Criando um arquivo `.css` global na aplicação

`src/assets/css/styles.css`
```css
.card {
    background: bisque;
    padding: 10px;
    border-radius: 10px;
}
```

`src/main.js`
```js
import { createApp } from 'vue'
import App from './App.vue'
import '@/assets/css/styles.css'

createApp(App).mount('#app')
```

> Dessa forma, assim que a aplicação carregar, esse arquivo será carregado

### 2º Forma (Scoped)
Os estilos CSS ficam restritos ao escopo do componente

```html
<template>
    <div class="card">Lorem Ipsum Dolor Sit Amet</div>
</template>

<!-- O scoped previne que os estilos desse componente sejam utilizados em outros arquivos -->
<style scoped>
.card {
    background: bisque;
    padding: 10px;
    border-radius: 10px;
}
</style>
```

## Enviando dados do pai para o filho (Props)
Similar ao **React**.

`src/components/BaseAlert.vue`
```html
<template>
    <div :class="baseClass">
        Lorem Ipsum Dolor Sit Amet
    </div>
</template>

<script>
export default {
    props: ['variant'],
    // Quando a prop 'variant' mudar o método baseClass será recomputado
    computed: {
        baseClass() {
            return ['alert', this.variant ? `alert-${this.variant}` : '']
        }
    }
}
</script>

<style>
.alert {
    padding: 5px;
    border-radius: 6px;
    color: gray;
    background: #ddd;
}
.alert-success {
    color: white;
    background: #42b983;
}
</style>
```

`src/components/App.vue`
```html
<template>
    <BaseAlert variant="success" />
</template>
```

## Enviando dados do filho para o pai (Emit)

`src/components/BaseAlert.vue`
```html
<template>
    <div :class="baseClass">
        Lorem Ipsum Dolor Sit Amet
    </div>

    <button @click="onClick">Fechar</button>
</template>

<script>
export default {
    props: ['variant'],
    computed: {
        baseClass() {
            return ['alert', this.variant ? `alert-${this.variant}` : '']
        }
    },
    methos: {
        onClick() {
            // Emitir um evento chamado close para o pai
            this.$emit('close')
        }
    }
}
</script>

<style>
.alert {
    padding: 5px;
    border-radius: 6px;
    color: gray;
    background: #ddd;
}
.alert-success {
    color: white;
    background: #42b983;
}
</style>
```

`src/components/App.vue`
```html
<template>
    <BaseAlert 
        v-if="showAlert"
        :variant="variant"
        @close="onClose()"
    />
</template>

<script>
export default {
    data() {
        return {
            showAlert: true,
            variant: 'success',
        }
    },
    methos: {
        onClose() {
            this.showAlert = false;
            console.log('Alert Fechado')
        }
    }
}
</script>
```
