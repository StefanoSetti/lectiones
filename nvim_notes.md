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


## [4] Plugins & Co.

In this case the **plug-in manager** in use is `packer.nvim`, enterly written in lua.
Packer brings some commands with it:

- `:PackerStatus`: Shows the installed plugins.
- `:PackerUpdate`: Updates plugins.
- `:PackerSync`: Updates & compile a file used by packer to speedup install.


### plugins.lua 
In plugins file is located in `/home/vnmRD/.config/nvim/lua/vnmrd/plugins.lua`.

In the file it is frequent to implement **protected calls** (`pcall(<command>)`), those calls are essentially try-catch.
With the keyword `use` it is possible do "import" the actual plugins for nvim, the semantic to call it is the following:

```lua
use "<user>/<repo>" -- Usually a in-line comment that describes the plugin is added. 
```

For any plugin is possible to import it via a table:

```lua
use {
    '<user>/<repo>',
    <option> = <value>,
    ...
    <option> = <value>,
}
```
So it is possible to specify multiple options for that specific plugin (read documentation for further findings).

The recommended plugins are (usually dependencies for many other plugins):

- `packer.nvim`: handles the packer updates.
- `popup.nvim`: Popup API implementation.
- `plenary.nvim`: useful and common lua functions.

### Data directory

Usually this directory is situated at `~/.local/share/nvim/`.
This directory is where all the plugings save their data.
The plugins are saved in `~/.local/share/nvim/site`.
In `site` are situated two directories:

- start: plugins that run on start-up.
- opt: plugins that has the `opt` flag set as `true`. 
       Those are only loaded when "binded" commands (defined in the `plugins.lua`) are executed. 

If a plugin is not loaded during startup, it measn that it will be loaded in a **lazy loading** fashion.
Note that even is it might sounds good to load plugins after startup, it might worsen the perfrmances.

### Plugin directory

Additionally a new directory has been created: `/home/vnmRD/.config/nvim/plugin`.
It contains `packer_compiled.lua`, this file is used by packer, nothing important for the user, it speedup plugins loading.
When packer has something wrong, it might be suggested to delete the file.
Gitignore this file.

### Lazy loading

A lazy loaded plugin is a plugin that is loaded only when one of the specified command in the use expression is executed.
It myght be triggered by events too, but it bught be tricky to use it properly.

### Vim Events

It is possible to read about events and the types ov events available by executing the command `:help autocmd`.


## [5] Colorscheme

To change colorscheme there are multiple ways to do that.
The most immediate way is to use the command `:colorscheme <scheme>`, the scheme can be chosen in the menu via `<tab>` key, note that the new scheme won't persist.
To use more colorful schemas, in the `option.lua` it is necessary to add the `termguicolors = true` option.
To have a persistent colorscheme it is necessary to insert in the `init.lua` file the command `vim.cmd "colorscheme <colorscheme>"`.

It is possible to add colorschemes as plugin, just add in in `plugins.lua`. The video presents plenty custom colorschemes.
In case the colorsheme requested is not present, the nvim config will break, so to solve this it is possible to use a file (`colorscheme.lua`) and not a straight forward command.
In this file as a bakcup situation, the default schema is used, all thanks to a `pcall`.


## [6] Completition


