# Vue component

> For VueJs project is possible to import the component as a library.

## Usage 


```html
<template>
    <div>
        <vue-qr-reader v-on:code-scanned="codeArrived" />
    </div>
</template>

<script>
    import VueQrReader from 'vue-qr-reader/dist/lib/vue-qr-reader.umd.js';
    export default {
        components: {
            VueQrReader
        },
        methods: {
            codeArrived (code) {
                console.log(code);
            }
        }
    }
</script>
```

!> The biggest difference between the Webcomponent and the library version is that the event `code-scanned` return the code directly and not in the form of `event.detail[0]`



## Nuxt

Since the component use the camera API it is not compatible with `SSR` to avoid `vue-qr-reader` to be processed during the server rendering the suggestion is to use the `vue-no-ssr` component.

#### no-ssr installation

```bash 
yarn add vue-no-ssr
ir
npm install vue-no-ssr
```

#### Usage

```html
<template>
    <no-ssr>
      <vue-qr-reader />
    </no-ssr>
</template>

<script>
    import NoSSR from 'vue-no-ssr'
    import VueQrReader from 'vue-qr-reader/dist/lib/vue-qr-reader.umd.js';

    export default {
        components: {
            'no-ssr': NoSSR,
            VueQrReader
        }
    }
</script>
```