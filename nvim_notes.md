# NeoVim Notes

This files holdes notes taken to learn about nvim (NeoVim), lua and packer.

The reference for these notes comes from this playlist:
https://www.youtube.com/playlist?list=PLhoH5vyxr6Qq41NFL4GvhFp-WLd5xzIzZ

## [1] Introduction

Just an overwiew of the final outcome of the playlist

## [2] nvim Options

### options.lua

The `options.lua` file is a config file for nvim, the multiple options can be seen via `:help options`.
To setup an option: `vim.opt.<option name> = <value>`.

Additionally it can sotre VimSript too `vim.cmd <VimScript>`.
It's good practice to write VimScripts at the end of the `options.lua` file.

The position of the file is `/home/vnmRD/.config/nvim/lua/vnmrd/options.lua`. 
Note `vnmrd` is a user name space, it can be anything." 

Additionally, the options can be added via **tables**:

```lua
-- Vars in lua are global, to have a local variable use `local`.
local options = {
	<option> = <value>,
	...
	<option> = <value>,
}

for k, v in pairs(options) do
	vim.opt[k] = v
end
```

### init.lua

The `init.lua` file is used to import `lua` files via `require "vnmrd.<file>"`.
Note that there is no such `lua.vnmrd.<file>.lua` path definition, this is implied.

The position of the file is `/home/vnmRD/.config/nvim/init.lua`.

## [3] Custom Keymaps

The `keymaps.lua` files define custom keymaps that overwrites the standard ones. 
To create a new keymap the `keymap` function is used:

```lua
-- `noremap` stands for no recursive remap (TODO: idn why, but is usally used).
-- `silent` is used to have no output. 
local opts = { noemap = true, silent = true } 
local keymap = vim.api.nvim_set_keymap


keymap("<mode>", "<<key to map>>", "<<keybind/command>>", opts)
```

It is important to present the **leader key**, this key is pressed before using custom commands, it is used in the `keymap` function like this: `<leader>`.

The position of the file is `/home/vnmRD/.config/nvim/lua/vnmrd/keymaps.lua`.

### Glossary

- `<cr>`: carriage return, the enter.
- A: alt key.
- buffer: it is a memory "chunk" that stores another opended file (like tabs, but tabs in nvim is something else).


