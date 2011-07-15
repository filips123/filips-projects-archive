---
layout: viceroy
title: Viceroy - Vim Configuration Management
---

Viceroy is a starter kit for the [Vim text editor](http://www.vim.org/). It is a set of vim scripts that help programmers get the most out of vim with common options that advanced vim users would set.

Viceroy is not a crutch that experienced vim users would get annoyed at. It is opinionated, but tries to follow best practices of the community. The goal is to have a solid set a defaults with an easy way to customize.

To install simply run this command (if you trust me, otherwise clone the [git repo](http://github.com/csexton/viceroy)):

    $ curl https://raw.github.com/csexton/viceroy/master/bootstrap.sh -o - | sh

Included are a set of plugin bundles that really unleash the power of Vim. They are not installed by default, but can easily be downloaded and installed by running this command in vim:

    :BundleInstall

Local customization is easy. If you want to change your color scheme, or override any of the settings in Viceroy, edit the .vimrc.local file. This is also the place to specify any additional plugin bundles that you want to install.

    " Customizations go in ~/.vimrc.local
    set nofoldenable
    set guifont=Monaco:h14
    Bundle 'snipMate'
    silent! colorscheme solarized

Since Viceroy uses vundle under the covers, adding additional plugins is easy.  Vundle even provides a slick interactive browser that you can use to install bundles.

    :Bundles

This is supposed to be a community driven project. If you have suggestions or changes please add an [issue](https://github.com/csexton/viceroy/issues), check the [wiki](https://github.com/csexton/viceroy/wiki) or best of all a [pull request](https://github.com/csexton/viceroy/pulls).

