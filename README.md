## Hydra documentation and blog site

source code for documentation running at https://hydra-synth.github.io/hydra-docs-v2/

### To run locally:

1. install hugo: https://gohugo.io/installation/
2. clone this repo:
```git clone https://github.com/hydra-synth/hydra-docs-v2.git```
3. clone submodules:
```git submodule update --init --recursive```
4. run locally:
```hugo serve```

### To edit:
1. Create a branch of this repo
2. Update markdown files contained in the `/content/docs`` folder
3. Commit your changes
4. Submit a pull request



### Content that is not yet included
- Information about alternative editors such as flok, atom/pulsar.
- Better documentation of libraries built on hydra such as hydra-ts and hydra-element
https://github.com/jdomizz/hydra-element

Feel free to submit pull requests!

### Could be nice to add
- use data templates and shortcodes to load airtable info for generating list of resources from hydra garden
https://gohugo.io/templates/data-templates/
- use data tempates and shortcodes to generate markdown from hydra-functions repo