---
bookCollapseSection: true
weight: 7
title: "developing editor"
---

# Developing editor

To run locally, you must have nodejs and npm installed. Install node and npm from: https://nodejs.org/en/.

First, clone the repository

    git clone git@github.com:hydra-synth/hydra.git

enter the directory of the hydra source code:

    cd hydra

## Current main branch

The current `main` branch uses browserify to bundle the script. While new features should be implemented in dev branch, if there is a hot fix needed in the current `main` branch, please follow this guide.

Once you have node and npm installed, you can install yarn globally by running the following from the command line:

    npm install --global yarn

install dependencies:

    yarn install

bundle JavaScript with browserify:

    yarn build

run server

    yarn serve

go to <http://localhost:8000> in the browser. Congratulations! You built hydra-editor on your computer!

### Where do these commands come from?

Yarn commands are defined in `package.json`.

### Development

Make your new branch

    git checkout -b my-awesome-feature

Edit the code. If you want to see changes in real time, you can use the watch script. After running `yarn serve`, open another terminal and run

    yarn watch

Then every time you save code, it will automatically re-bundle the code.

### Serving on your own server

(stub)

### Commit, push and pull request

(stub)

## dev branch

New features should be implemented in `dev` branch. [After entering the directory](#developing-editor), checkout the branch

    git checkout -b dev origin/dev

install dependencies:

    npm install

run dev environment

    npm run dev

Since we use vite in `dev` branch, we don't need to bundle the code during development (vite takes care of bundling and serving while you code). When you want to publish the code, build the bundle:

    npm run build


### Connecting to server from dev/ local editor environment
This repo only contains hydra editor frontend. You can connect to a backend server (https://github.com/hydra-synth/hydra-server) for signaling and gallery functionality. To do this, set up hydra-server from above. Then create a `.env` file in the root of the `hydra` directory. Add the url of your server as a line in the .env file as:
```
VITE_SERVER_URL=http://localhost:8000
```
(replace http://localhost:8000 with the url of your server)

