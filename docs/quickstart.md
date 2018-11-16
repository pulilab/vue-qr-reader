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

### Exceptions

!> Once an error is occured the `CustomEvent`: `error-captured` is emitted, you can implement your error handler to listen to this event, for example in vue:

```html
<template>
    <vue-qr-reader v-on:code-scanned="codeArrived" v-on:error-captured="errorCaptured">
</template>

<script>
    export default {
        methods: {
            codeArrived (event) {
                console.log(event.detail[0]);
            },
            errorCaptured(error) {
                switch (error.name) {
                    case 'NotAllowedError':
                    this.errorMessage = 'Camera permission denied.'
                    break;
                    case 'NotFoundError':
                    this.errorMessage = 'There is no connected camera.'
                    break;
                    case 'NotSupportedError':
                    this.errorMessage = 'Seems like this page is served in non-secure context.'
                    break;
                    case 'NotReadableError':
                    this.errorMessage = 'Couldn\'t access your camera. Is it already in use?'
                    break;
                    case 'OverconstrainedError':
                    this.errorMessage = 'Constraints don\'t match any installed camera.'
                    break;
                    default:
                    this.errorMessage = 'UNKNOWN ERROR: ' + error.message
                }
                console.error(this.errorMessage);
            }
        }
    };
</script>
```

!> For more detail of possible errors returned, you may refer to https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia#Exceptions
