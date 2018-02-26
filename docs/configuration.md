# Configuration

## Props

!> At the moment of writing this docs default values for `Boolean` props are ignored so they all need to be specified on the instance

### useBackCamera
- Type: `Boolean`
- Default: `true`

Define if the component should try to use the back camera or not, note that in most of the cases the back camera is not present on computers and in some tablets.

```html
<vue-qr-reader use-back-camera="false"></vue-qr-reader>
```

### stopOnScanned
- Type: `Boolean`
- Default: `true`

Define if the component should stop scanning for codes after the first match, this will stop the video grabbing and the qr scanning process. If is set to `false` the component will continuously scan.

```html
<vue-qr-reader stop-on-scanned="false"></vue-qr-reader>
```

### drawOnFound
- Type: `Boolean`
- Default: `true`

Define if the component should draw a 'box' around the found QR, this uses the edges returned by `jsQR` to draw the shape, color and thickness of the lines are configurable with the props: `lineColor` and `lineWidth`

```html
<vue-qr-reader draw-on-found="false"></vue-qr-reader>
```

### lineColor
- Type: `String`
- Default: `#FF3B58`

Define the color drawn around the QR code, takes an HEX string representing the color

```html
<vue-qr-reader line-color="#FFFFFF"></vue-qr-reader>
```

### lineWidth
- Type: `Number`
- Default: `2`

Define the thickness of the lines drawn around the code.

```html
<vue-qr-reader line-width="4"></vue-qr-reader>
```

### videoWidth
- Type: `Number`
- Default: `320`

Define the width of the canvas where the user can see the output of the video

```html
<vue-qr-reader vide-width="640"></vue-qr-reader>
```

!> Note this prop is ignored the if the prop `responsive` is set to true

### videoHeight
- Type: `Number`
- Default: `240`

Define the height of the canvas where the user can see the output of the video

```html
<vue-qr-reader vide-height="480"></vue-qr-reader>
```

!> Note this prop is ignored the if the prop `responsive` is set to true


### responsive
- Type: `Boolean`
- Default: `false`

Define if the component should try to set the video width as big as is container and the height as as 0.75 factor of it, this can result in a laggy video or bad quality depending on the camera resolution

```html
<vue-qr-reader :responsive="true"></vue-qr-reader>
```
