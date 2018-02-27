# Quick Start

### Installation
Install the component through npm or yarn


```shell
npm install -s vue-qr-reader

yarn add vue-qr-reader
```

### Usage
> As this is a web component, it is only necessary to include the js and then place the component in the markup in order to be utilized.

```js
import 'vue-qr-reader';
```

or

```html
    <script src="//unpkg.com/pulilab/vue-qr-reader"></script>
```

```html
<vue-qr-reader />
```


Once a QR code is scanned the `CustomEvent`: `code-scanned` is emitted. One can listen to this event as he normally would, for example in vue:

!> The event emitted is a `CustomEvent` and the payload is contained as the first element of the array referenced by the key `detail`

```html
<template>
    <vue-qr-reader v-on:code-scanned="codeArrived">
</template>

<script>
    export default {
        methods: {
            codeArrived (event) {
                console.log(event.detail[0]);
            }
        }
    };
</script>
```

!> The event is emitted once if the prop `stop-on-scanned` is set to `true` (default), if the prop is set to `false` the event is emitted continuously while the QR is being detected.
