# Vue Camera

Customizable camera library for Vue 3, within asking permission to user.

## Features

- Asking Permission to user
- Flip camera rear/front
- Result blob/b64
- Mirror Camera

## Installation

```
npm i @reiyanyan/vue-camera
```

## Usage

To use it in your Vue app

First, import in your custom component for proxy camera

```
import VxCamera from "@reiyanyan/vue-camera"
```

Second, create ref for canvas and video:

- video for camera places
- canvas for result after camera taken

```
const videoRef = ref<HTMLVideoElement>();
const canvasRef = ref<HTMLCanvasElement>();
```

Third, create variable to memoize your camera instance

```
const cameraRef = ref();
```

Fourth, create an function for starting camera

example:

```
const permissionDenied = ref(false);
const isCameraOpen = ref(false);

async function startCamera () {

    const checkPermission = setInterval(() => {
        if (isCameraOpen.value) {
        clearInterval(checkPermission);
        } else {
        permissionDenied.value = true;
        clearInterval(checkPermission);
        }
    }, 1000);

    cameraRef.value = await new VxCamera(videoRef.value!, canvasRef.value!)
            .setConstraint({
                video: {
                    facingMode: "environment",
                    height: {
                    min: 480,
                    max: 480,
                    ideal: 480,
                    },
                    width: {
                    min: 480,
                    max: 480,
                    ideal: 480,
                    },
                },
                audio: false,
            })
            .requestPermission()
            .then((camera) => camera)
            .catch((err) => {
                console.error(err);
            });

    cameraRef.value?.start().finally(() => (isCameraOpen.value = true));
}
```

- reference of constraint [MDN MediaTrackConstraints](https://developer.mozilla.org/en-US/docs/Web/API/MediaTrackConstraints)

Fifth, taking a picture and save as blob/b64

```
const result = await cameraRef.value?.snapAsBlob().then((data: any) => data)
// or
const result = await cameraRef.value?.snapAsBase64().then((data: any) => data)

snapAsBase64 or snapAsBlob
```

That was example using the library to your project.

## Contributing

Contributions are always welcome! Feel free to fork the [repository](https://github.com/reiyanyan/vue-camera) and submit pull requests.

## Issues

If you find any bug or have a feature request, please file an issue on [Github](https://github.com/reiyanyan/vue-camera).

---

Happy Developing!
