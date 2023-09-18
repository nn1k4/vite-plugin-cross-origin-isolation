# vite-plugin-cross-origin-isolation

A vite plugin for enabling cross-origin-isolation for dev server and preview.

## Why

`Cross origin isolation` is required for `SharedArrayBuffer`.

Using this plugin, in local dev server, functions that rely on SAB can be enabled and tested.

## Usage

First install it in your vite project:

`npm i -D @laimi7/vite-plugin-cross-origin-isolation`

Then, in `vite.config.js`:

```
import crossOriginIsolation from '@laimi7/vite-plugin-cross-origin-isolation'

export default {
    plugins: [
      // other plugins...
      crossOriginIsolation()
    ]
}
```

If this doesn't work, try this instead:
```
import { defineConfig } from "vite";

export default defineConfig({
  plugins: [
     {
        name: "configure-response-headers",

        configureServer: (server) => {
          server.middlewares.use((_req, res, next) => {
          res.setHeader("Cross-Origin-Embedder-Policy", "require-corp");
          res.setHeader("Cross-Origin-Opener-Policy", "same-origin");
          next();
          });
        },
        configurePreviewServer(server) {
          server.middlewares.use((_req, res, next) => {
          res.setHeader("Cross-Origin-Opener-Policy", "same-origin");
          res.setHeader("Cross-Origin-Embedder-Policy", "require-corp");
          next();
          });
          }
      },
  ],
});
```
> Copy from: https://github.com/vitejs/vite/issues/3909#issuecomment-934044912

## License
The MIT License (MIT)

Copyright (c) 2021 - present Qichao Lan (chaosprint)