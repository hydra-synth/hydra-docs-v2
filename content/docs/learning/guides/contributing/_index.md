---
bookCollapseSection: true
weight: 7
title: "developing and contributing"
---

# Developing and Contributing

In this guide, you will learn how to "compile" hydra, to test your own version, and to contribute to the original repository. This guide is for those who are familar with JavaScript to edit the code base. Knowledge of command line and front-end tools is preferred but we try to guide you step by step.

## Understanding the structure

Hydra consists of mainly 4 repositories:

* [hydra](https://github.com/hydra-synth/hydra)
* [hydra-synth](https://github.com/hydra-synth/hydra-synth)
* [hydra-server](https://github.com/hydra-synth/hydra-server)
* [l10n](https://github.com/hydra-synth/l10n)

hydra, or hydra-editor, is the webpage that comes with the editor. If you want to make changes in, e.g., the behavior of the editor, this is the right repository. We will look into detail in [developing editor](editor).

hydra-synth is the "engine" that processes your hydra code on the editor and produces GLSL (shader) code. We explain in detail in [developing synth](synth).

hydra-server is a backend program for signaling and storing the gallery. Note that you don't need this for testing the editor. We explain in detail in [developing backend server](server).

l10n is a collection of locale files, i.e., translations for the editor interface. If you want to contribute translations, please refer to this [doc](https://github.com/hydra-synth/hydra-docs/blob/main/contributing_translation.md#hydra-editor).

## All contributors

Thank you to everyone who contributed to the project, not only contributing the code, but including reporting bugs, organizing events, and making tutorials, etc! We would like to acknowledge everyone who contributes to make hydra better. Please submit your information [here](https://github.com/hydra-synth/hydra/issues/265).