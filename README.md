## Hydra documentation and blog site

source code for documentation running at https://hydra-synth.github.io/hydra-docs-v2/

### To run locally:

1. install hugo:
2. clone this repo:
```git clone https://github.com/hydra-synth/hydra-docs-v2.git```
3. clone submodules:
```git submodule update --init --recursive```
4. run locally:
```hugo serve```
5. (optional) set baseURL in `config.toml` or using `--baseURL` to your hosting URL (e.g., `https://myhydrawebsite.com/blog`)
6. (optional) build:
```hugo```



### TO DO:
- use data templates and shortcodes to load airtable info for generating list of resources from hydra garden
https://gohugo.io/templates/data-templates/
- use data tempates and shortcodes to generate markdown from hydra-functions