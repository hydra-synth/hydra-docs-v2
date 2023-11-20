---
bookCollapseSection: true
weight: 7
title: "developing hydra-synth"
---

# Developing synth

Clone the repository

    git@github.com:hydra-synth/hydra-synth.git

enter the folder

    cd hydra-synth

install the dependencies

    npm install

build

    npm run build

The bundled code is in `dist/hydra-synth.js`.

## Trying on the browser

This repository does not come with the editor. However, you can use the simple example `dist/index.html`. To do so, install `http-server`

    npm install --global http-server

and serve `dist` folder

    http-server dist

go to <http://localhost:8000> in the browser. You can either edit the hydra code in `index.html` to try hydra functions, or open the developer console and type hydra code (e.g., `osc().out()`) and it will update the canvas. The former is useful for testing more complex examples including non-global mode, and the later is useful for quick testing.

For testing the integration with hydra-editor (of if you want to host your own hydra version on your server), please follow [editor guide](editor) to host your own editor. Then, edit `package.json` in **hydra** (not hydra-synth) to use the local version of hydra-synth (assuming you have hydra and hydra-synth folders in the same folder)

    "hydra-synth": "file:../hydra-synth",

Then in hydra, update the package

    npm update hydra-synth

Now the editor is using your version of hydra-synth.
