Open Rust document in Vim
=========================

[![This project is considered stable](https://img.shields.io/badge/status-stable-success.svg)](https://benknoble.github.io/status/stable/)

[This project is a fork of
rhysd/rust-doc.vim](https://github.com/rhysd/rust-doc.vim) that exists mostly to
solve [this issue from 2019](https://github.com/rhysd/rust-doc.vim/issues/4) and
to provide better defaults (i.e., no remapping `K` when `keywordprg` will do).

I haven't had time to copy-edit all the docs, re-format all the code, etc., so
it remains mostly in its original state, with my few fixes.

`cargo` has a nice feature to generate document.  But it doesn't have a feature to search it yet.
`rustup` can search documentation, but you have to know exactly what you're
looking for.
This plugin searches documents (standard libraries, libraries your crate depends on, and so on) from Vim.

- Open a document for the word under cursor `std`

![screenshot](https://raw.githubusercontent.com/rhysd/screenshots/master/rust-doc.vim/rust-doc.vim.gif)

- Search documents with [unit.vim](https://github.com/Shougo/unite.vim) interface

![unite screenshot](https://raw.githubusercontent.com/rhysd/screenshots/master/rust-doc.vim/unite-rust-doc.gif)

## Usage

- __`:RustDoc` command__

```vim
:RustDoc {crate or module name} [{identifier name}]
```

For example, assume that your crate uses [rand](http://doc.rust-lang.org/rand/rand/index.html).

1. `:RustDoc rand` opens the document for crate `rand`.
2. `:RustDoc rand::distributions` opens the document for module `rand::distributions`.
3. `:RustDoc rand Rng` opens the document for trait `rand::Rng`.

All arguments of the command can be completed.  Please enter `<Tab>` to complete them.

Of course you can search standard libraries.

```vim
:RustDoc std::vec
```

And `:RustDocFuzzy` and `:RustDocModule` are also available.
Please see [document for them](https://github.com/rhysd/rust-doc/blob/master/doc/rust-doc.txt) for more detail.

Set the local `keywordprg` to one of these commands in your rust filetype-plugin
to benefit from `K` for documentation:

```vim
" .../ftplugin/rust.vim
setlocal keywordprg=:RustDocFuzzy

```

- __[unite.vim](https://github.com/Shougo/unite.vim) source__

```vim
:Unite rust/doc
```

You can select from all document candidates with unite.vim interface.

```vim
" .../ftplugin/rust.vim
nnoremap <buffer> K :Unite rust/doc:cursor -no-empty -immediately<CR>
```

It may takes some time to prepare candidates.  If you don't want to wait and it is enough for you to search only module names,
please specify `modules` to the argument of `rust/doc` source as below.

```vim
:Unite rust/doc:modules
```

- __[denite.nvim](https://github.com/Shougo/denite.nvim) source__

```vim
:Denite rust/doc
```

You can select from all document candidates with denite.nvim interface.
And if you want to use denite.nvim for `K` mapping, you can setup in your vimrc as follows.

```vim
" .../ftplugin/rust.vim
nnoremap <buffer> K :<C-u>DeniteCursorWord rust/doc<CR>
```

It may takes some time to prepare candidates.  If you don't want to wait and it is enough for you to search only module names,
please specify `modules` to the argument of `rust/doc` source as below.

```vim
:Denite rust/doc:modules
```

## Installation

### 1. Install This Plugin

If you use some plugin manager, please follow the instruction of them.
For example, you can install this plugin with [neobundle.vim](https://github.com/Shougo/neobundle.vim) as below.

```vim
NeoBundle 'benknoble/rust-doc'
```

If you use no plugin manager, copy all directories and files in `autoload`, `plugin`, `ftplugin`
and `doc` directories to your `~/.vim` directory's corresponding directories.

Or you can also use Vim's standard package management. Please read `:help packages`.

### 2. (Optional) Setup Standard Library Documents

rust-doc searches documents downloaded by [rustup](https://github.com/rust-lang-nursery/rustup.rs).
If there is a toolchain installed by rustup, rust-doc tries to use documentations in it.

If you want to see your own standard library documents, you must set `g:rust_doc#downloaded_rust_doc_dir`.
The variable should be string type and contain the path to rust documents bundled in downloaded rust tar.

__e.g.__:

```sh
$ wget https://static.rust-lang.org/dist/rust-1.0.0-i686-unknown-linux-gnu.tar.gz
$ tar xf rust-1.0.0-i686-unknown-linux-gnu.tar.gz
$ mv rust-1.0.0-i686-unknown-linux-gnu/rust-doc ~/Documents/
```

Then set the variable as below

```vim
let g:rust_doc#downloaded_rust_doc_dir = '~/Documents/rust-docs'
```


## Customization

- `g:rust_doc#vim_open_cmd`

Vim command to open the path to a document.  rust-doc uses it to open the document if it is not empty.
The command must take a url to the local file.

- `g:rust_doc#open_cmd`

Shell command to open the path to a document.  rust-doc uses it to open the document if it is not empty.
The command must take a url to the local file.

- `g:rust_doc#do_not_ask_for_module_list`

If the value is `1`, rust-doc never ask if it shows the list of modules/identifiers when no document is found.
The default value is `0`.

- `g:rust_doc#downloaded_rust_doc_dir`

As described above, the path to directory of rust standard library documents.


## License

This plugin is distributed under [the MIT License](http://opensource.org/licenses/MIT).

```
Copyright (c) 2020 benknoble
Copyright (c) 2015 rhysd
```

