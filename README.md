# chrome-pwa-app-test

[CRBUG#781313](https://bugs.chromium.org/p/chromium/issues/detail?id=781313)

This is a test application to demonstrate problems with offline behavior when loading a progressive web app (PWA) inside of a Chrome App.

The Chrome App is a single HTML page that loads a PWA in a `Webview`:
```
  <body>
    <main>
      <webview src="https://s3.amazonaws.com/displaykit-assets-development/testing/swtest2/index.html" partition="persist:displaykit">
    </main>
  </body>
```

The [PWA source code](pwa_webapp_code) is contained in this repository, and it can be loaded from this remote URL: https://s3.amazonaws.com/displaykit-assets-development/testing/swtest2/index.html

## Expected behavior

When the Chrome App launches for the first time, it should load the remote URL into the webview. Once loaded, the webapp's service worker should cache all its resources. Once loaded, the application should now serve all its resources from cache and not need to make external network requests.

You can launch the Chrome App on Chrome OS in kiosk mode and it will cache its resources. Rebooting the app **with the network on** runs as expected and the resources are served from cache. 

## Observed Behavior

If you turn off the network interface on the Chrome OS machine, unplug the ethernet cable (if it has a wired connection), or turn the power off on the wifi access point, the PWA will **sometimes** fail to load, resulting in a "Dinosaur" screen with a `ERR_INTERNET_DISCONNECTED` message.

*It's important to note that this offline behavior is not consistent: Sometimes it will load properly, sometimes it will not.*


