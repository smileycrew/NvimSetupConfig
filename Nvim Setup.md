#### Initial Project Setup

* Make a new directory `mkdir` in $HOME/ of your terminal and name it `nvim`

*For me if was in the $HOME/.config of my MacBook 14 Pro - $HOME/.config/nvim

* Create a file with `%` and name it `init.lua`.

* Type a placeholder for now `print("hello)`, exit insert mode `esc`, and save `:w`.

* Create a new directory with `d` and name it `lua` and cd.

* Create another directory `d` and call it whatever you want, for me I named it `smiley` and cd.

* Create a new file `%` and name it `init.lua` and cd.

* Type a placeholder that you can delete later if you want. For me it was a welcome message. `print("Hello smiley")`.

* Navigate to the newtwr menu `:Ex` and cd `../` back to the first init.lua file.

* Replace `print("hello")` with the directory that was created, for me it was the `require("smiley")` directory.

* Save `:w`, quit `:q` and restart `nvim .`

#### Netwr remap 

* Cd to `/nvim/lua/smiley` and create a file `%` and name it `remap.lua`.

* Add the following remaps:
```lua
-- Set single space as the leader key
vim.g.mapleader = " "
-- In normal mode using <leader>pv will execute the command :Ex and open netwrp
vim.keymap.set("n", "<leader>pv", vim.cmd.Ex)
```

* Source it with `:so` and test it out with `<leader>pv` 

* Cd into `/nvim/lua/smiley/init.lua` and include the new remaps `require("smiley.remap")`   

#### Plugin manager

* Install packer with:
```bash
git clone --depth 1 https://github.com/wbthomason/packer.nvim\
        ~/.local/share/nvim/site/pack/packer/start/packer.nvim

        ```

* Restart nvim and cd into `/nvim/lua/smiley` and create a new file `%` and name it `packer.lua` and include the following:
```lua
-- This file can be loaded by calling `lua require('plugins')` from your init.vim

-- Only required if you have packer configured as `opt`
vim.cmd [[packadd packer.nvim]]

return require('packer').startup(function(use)
  -- Packer can manage itself
  use 'wbthomason/packer.nvim'
end)
```

* Restart nvim and cd back into `/nvim/lua/smiley/packer.lua` and source it `:so` and sync it with `:PackerSync`

#### Fuzzy finder
* In the packer.lua startup function include the following:
```lua
-- Use the telescope.nvim plugin for fuzzy finding and more
use {
    -- plugin repository on github and the version (tag)
    'nvim-telescope/telescope.nvim', tag = '0.1.8',
        -- alternatively you can specify a branch
        -- or                            , branch = '0.1.x',
        -- dependency required by telescop.nvim
        requires = { {'nvim-lua/plenary.nvim'} }
}
```

*Feel free to format with `=ap` while in normal mode.

* Source `:so` and sync `:PackerSync`

* Cd to /nvim and create a new directory `d` and name it `after` and cd

* Create another directory `d` within and name it `plugin` and cd to create a file `%` and call it `telescope.lua`

* In the `telescope.lua` file include the following:
```lua
-- Import the built-in functions from telescope.nvim
local builtin = require('telescope.builtin')

-- In normal mode <leader>pf (project finder) will display the project finder menu.
vim.keymap.set('n', '<leader>pf', builtin.find_files, {})
-- In normal mode <C-p> will display Git files in the project finder menu.
vim.keymap.set('n', '<C-p>', builtin.git_files, {})
-- In normal mode <leader>ps (project search) will take a string input to search.
vim.keymap.set('n', '<leader>ps', function()
    -- Telescope's built in grep functionality will take the user's input and use it as the search term.
	builtin.grep_string({ search = vim.fn.input("Grep > ") })
end)
```

* Install Ripgrep using the terminal `brew install ripgrep`.

* Source it `:so` and test it out with `<leader>pf` to open the project finder menu.
* `<leader>ps` and type the name of a file you want to find in the grep input.
* If you are in a git repository use `<C-p>` to open the git project finder menu. 

#### Color Scheme

* Cd into packer.lua and include the following in the startup function:
```lua
-- Use the treesitter plugin from github
use({
	'rose-pine/neovim',
    -- Given the alias 'rose-pine'
	as = 'rose-pine'
    -- Configuration will be a function
	config = function()
        -- Nvim command will set colorscheme to rose pine
		vim.cmd('colorscheme rose-pine')
	end
})
```

* Source it `:so` and sync `:PackerSync`

#### Treesitter

* In the packer.lua file include the following in the startup function: 
```lua
-- Use the treesitter plugin from github and run TSUpdate after plugin is installed to install parsers.
use('nvim-treesitter/nvim-treesitter', { run = ':TSUpdate'})
```

* Source it `:so` and sync `:PackerSync`

* Cd into `nvim/after/plugin` and create a file `%` and name it `treesitter.lua`  

* Add the following to treesitter.lua
```lua
require'nvim-treesitter.configs'.setup {
  -- A list of parser names, or "all" (the listed parsers MUST always be installed)
  ensure_installed = { "help", "javascript", "typescript", "c", "lua", "vim", "vimdoc", "query", "markdown", "markdown_inline" },

  -- Install parsers synchronously (only applied to `ensure_installed`)
  sync_install = false,

  -- Automatically install missing parsers when entering buffer
  -- Recommendation: set to false if you don't have `tree-sitter` CLI installed locally
  auto_install = true,

  highlight = {
    enable = true,

    -- Setting this to true will run `:h syntax` and tree-sitter at the same time.
    -- Set this to `true` if you depend on 'syntax' being enabled (like for indentation).
    -- Using this option may slow down your editor, and you may see some duplicate highlights.
    -- Instead of true it can also be a list of languages
    additional_vim_regex_highlighting = false,
  },
}
```

* Source it `:so`, write `:w` and quit `:q`.

* Cd back into `treesitter.lua` and verify the parsers.

* In the packer.lua file include the following in the startup function:
```lua
-- Use the treesitter playground from github
use('nvim-treesitter/playground')
```

* Source it `:so`, sync `:PackerSync`, write `:w` and restart `:q`.

* Verify that it was installed with `:TSPlaygroundToggle`.

* In the packer.lua file include the following in the startup function:
```lua
-- Use harpoon by primeagen from github
`use('ThePrimeagen/harpoon')`
```

* Source it `:so` and sync `:PackerSync`

* In the `nvim/after/plugin` directory create a file `%` and name it `harpoon.lua`.

* Add the following:
```lua
-- Initiate mark variable by importing the mark module from the harpoon plugin
local mark = require("harpoon.mark")
-- Initiate ui variable by importing the ui module from the harpoon plugin
local ui = require("harpoon.ui")

-- In normal mode <leader>a (add) will add the current file to Harpoon's marked files (quick select)
vim.keymap.set("n", "<leader>a", mark.add_file)
-- In normal mode <C-e> will toggle Harpoon's quick menu
vim.keymap.set("n", "<C-e>", ui.toggle_quick_menu)

-- <C-h>, <C-t>, <C-n>, <C-s> quickly toggles between the marked files.
vim.keymap.set("n", "<C-h>", function() ui.nav_file(1) end)
vim.keymap.set("n", "<C-t>", function() ui.nav_file(2) end)
vim.keymap.set("n", "<C-n>", function() ui.nav_file(3) end)
vim.keymap.set("n", "<C-s>", function() ui.nav_file(4) end)
```

* Source it `:so` and sync `:PackerSync`

* Test it out by add a file `<leader>a` and verifying in the menu `<C-e>`.

#### Undotree
* In the packer.lua file include the following in the startup function:
```lua
-- Use undotree from github
use('mbbill/undotree')
```

* Source it `:so` and sync `:PackerSync`

* Cd to `/nvim/lua/smiley` and create a new file `%` and name it `undotree.lua` and add the following:

```lua
-- In normal mode <leader>u will toggle the undotree window
vim.keymap.set("n", "<leader>u", vim.cmd.UndotreeToggle)
```

* Source it `:so` and verify with `<leader>u`

#### Fugitive
in the packer.lua file add the following before the function ends:
`use('tpope/vim-fugitive')`

write `:w` and  `:so` source it `:PackerSync` to apply

cd to harpoon.lua and create file `%` name it `fugitive.lua`

add the following:
```
vim.keymap.set("n", "<leader>gs", vim.cmd.Git)
```

write `:w` execute config changes `:so`

restart vim `:q`

cd into a git repo and test out with ` gs` leader gs

cd back into packer.lua and add the following before the function ends:
```
use {
	'VonHeikemen/lsp-zero.nvim',
	requires = {
		-- LSP Support
		{ 'neovim/nvim-lspconfig' },
		{ 'williamboman/mason.nvim' },
		{ 'williamboman/mason-lspconfig.nvim' },

		-- Autocomplettion
		{ 'hrsh7th/nvim-cmp' },
		{ 'hrsh7th/cmp-buffer' },
		{ 'hrsh7th/cmp-path' },
		{ 'saadparwaiz1/cmp_luasnip' },
		{ 'hrsh7th/cmp-nvim-lsp' },
		{ 'hrsh7th/cmp-nvim-lua' },
		
		-- Snippets
		{ 'L3MON4D3/LuaSnip' },
		{ 'rafamadriz/friendly-snippets' },
	}
}
```

format `=ap` write `:w` source `:so` sync `:PackerSync`

in after/plugins mkr new file `%` named `lsp.lua`

in the file add the following:

```
local lsp = require('lsp-zero')

lsp.preset('recommended')

lsp.ensure_installed({
	'tsserver',
	'eslint',
	'sumneko_lua',
	'rust_analyzer'
})

local cmp = require('cmp')
local cmp_select = { behavior = cmp.SelectedBehavior.Select }
local cmp_mappings = lsp.defaults.cmp_mappings({
	['<C-p>'] = cmp.mapping.select_prev_item(cmp_select),
	['<C-n>'] = cmp.mapping.select_next_item(cmp_select),
	['<C-y>'] = cmp.mapping.confirm({ select = true }),
	['<C-Space>'] = cmp.mapping.complete(),
})

lsp.set_preferences({
sign_icons = { }
})

lsp.setup_nvim_cmp({
mappings = cmp_mappings
})

lsp.on_attach(function(client, bufnr)
	print("help")

    local opts = { buffer = bufnr, remap = false }

	vim.keymap.set("n", "gd", function vim.lsp.buf.definition() end, opts)
	vim.keymap.set("n", "K", function vim.lsp.buf.hover() end, opts)
	vim.keymap.set("n", "<leader>vws", function vim.lsp.buf.workspace_symbol() end, opts)
	vim.keymap.set("n", "<leader>vd", function vim.diagnostic.open_float() end, opts)
	vim.keymap.set("n", "[d", function vim.diagnostic.goto_next() end, opts)
	vim.keymap.set("n", "]d", function vim.diagnostic.goto_prev() end, opts)
	vim.keymap.set("n", "<leader>vca", function vim.lsp.buf.code_action() end, opts)
	vim.keymap.set("n", "<leader>vrr", function vim.lsp.buf.references() end, opts)
	vim.keymap.set("n", "<leader>vrn", function vim.lsp.buf.rename() end, opts)
	vim.keymap.set("n", "<C-h>", function vim.lsp.buf.signature_help() end, opts)
	
end)

lsp.setup()
```

source `:so`

test out to get autocomplete on any code

# Editor Settings

in .config/nvim/init.lua

remove hello statement

cd back and into init.lua inside of the "smiley" directory

remove hello statement and add `require("smiley.set")` under the .remap

in "smiley" dir create file `%` `set.lua`

add the following to  /set.lua
```
vim.opt.guicursor = ""

vim.opt.nu = true
vim.opt.relativenumber = true

vim.opt.tabstop = 4
vim.opt.softtabstop = 4
vim.opt.shiftwidth = 4
vim.opt.expandtab = 4

vim.opt.smartindent = true

vim.opt.wrap = false

vim.opt.swapfile = false
vim.opt.backup = false
vim.opt.undodir = os.getenv(""HOME) .. "/.vim/undodir"
vim.opt.undofile = true

vim.opt.hlsearch = false
vim.opt.incsearch = true

vim.opt.termguicolors = true

vim.opt.scrolloff = 8
vim.opt.signcolumn = "yes"
vim.opt.isfname:append("@-@")

vim.opt.updatetime = 50

vim.opt.colorcolumn = "80"

vim.g.mapleader = " "
```

in the remap.lua file add the following:
```
vim.keymap.set("v", "J", ":m '>+1<CR>gv=gv")
vim.keymap.set("v", "K", ":m '<-2<CR>gv=gv")

vim.keymap.set("n", "J", "mzJ`z")
vim.keymap.set("n", "<C-d>", "<C-d>zz")
vim.keymap.set("n", "<C-u>", "<C-u>zz")
vim.keymap.set("n", "n", "nzzzv")
vim.keymap.set("n", "N", "Nzzzv")
vim.keymap.set("n", "<leader>zig", "<cmd>LspRestart<cr>")

vim.keymap.set("n", "<leader>vwm", function()
    require("vim-with-me").StartVimWithMe()
end)
vim.keymap.set("n", "<leader>svwm", function()
    require("vim-with-me").StopVimWithMe()
end)

-- greatest remap ever
vim.keymap.set("x", "<leader>p", [["_dP]])

-- next greatest remap ever : asbjornHaland
vim.keymap.set({"n", "v"}, "<leader>y", [["+y]])
vim.keymap.set("n", "<leader>Y", [["+Y]])

vim.keymap.set({"n", "v"}, "<leader>d", [["_d]])

-- This is going to get me cancelled
vim.keymap.set("i", "<C-c>", "<Esc>")

vim.keymap.set("n", "Q", "<nop>")
vim.keymap.set("n", "<C-f>", "<cmd>silent !tmux neww tmux-sessionizer<CR>")
vim.keymap.set("n", "<leader>f", vim.lsp.buf.format)

vim.keymap.set("n", "<C-k>", "<cmd>cnext<CR>zz")
vim.keymap.set("n", "<C-j>", "<cmd>cprev<CR>zz")
vim.keymap.set("n", "<leader>k", "<cmd>lnext<CR>zz")
vim.keymap.set("n", "<leader>j", "<cmd>lprev<CR>zz")

vim.keymap.set("n", "<leader>s", [[:%s/\<<C-r><C-w>\>/<C-r><C-w>/gI<Left><Left><Left>]])
vim.keymap.set("n", "<leader>x", "<cmd>!chmod +x %<CR>", { silent = true })

vim.keymap.set(
    "n",
    "<leader>ee",
    "oif err != nil {<CR>}<Esc>Oreturn err<Esc>"
)

vim.keymap.set("n", "<leader>vpp", "<cmd>e ~/.dotfiles/nvim/.config/nvim/lua/theprimeagen/packer.lua<CR>");
vim.keymap.set("n", "<leader>mr", "<cmd>CellularAutomaton make_it_rain<CR>");

vim.keymap.set("n", "<leader><leader>", function()
    vim.cmd("so")
end)
```

### Rinse and repeat

1. packer.lua - plugin manager
2. run `:so` and `:PackerSync`
3. create file.lua for the added configuration

# Vim Commands

### NETRW

`R` rename folder or file
### Normal mode

`h` moves left
`j` moves down
`k` moves up
`l` moves right

`0` moves to the beginning of the line
`$` moves to the end of the line
`^` moves to the first non-blank character of the line

`gg` moves to the beginning of the file
`G` moves to the end of the file

`<number>j` or `<number>k` moves vertical relative to cursor
`<number>h` or `<number>l` moves horizontal relative to cursor

`w` moves to the start of the next word
`e` moves to the end of the next word
`b` moves to the start of the previous word
`ge` moves to the end of the previous word

`<number>w` or `<number>e` moves right using the specified number
`<number>b` or `<number>ge` moves left using the specified number

`%` jumps to the matching pair (e.g., parenthesis, brackets, etc)

`H` moves to the top of the screen
`M` moves to the middle of the screen
`L` moves to the bottom of the screen

`<C-u>` scrolls up half of a page
`<C-d>` scrolls down half of a page

`*` searches forward for the word under the cursor
`#` searches backward for the word under the cursor

`>>` indent the current line
`<<` un-indent the current line
`==` auto indent the current line

`ciw` change inside the current word
`caw` change around the current word
`ci(` or `ci"` or `ci{` or `ci[` change inside the nearest specified tag
`ca(` or `ca"` or `ca{` or `ca[` change around the nearest specified tag

`J` joins current line with next line wthout losing cursor position

`n` search next and center the line
`N` search previous and center the line



### Visual Mode

`J` moves selected line down into the next line
`K` moves the selected line up into the next line

`<leader>y` yanks to the system clipboard
`<leader>Y` yanks the entire line to the system clipboard

`<leader>f` format code with LSP

`<leader>s` search and replace word under the cursor

`<leader><leader>` source the current file

`<leader>pv` netrw menu
`<leader>pf` project file search
`<C-p>` Git file search
`G` go to last line in a file
`gg` go to the first line in a file
`<leader>a` add file to harpoon
`<C-e>` harpoon menu
`<leader>u` undotree
`<leader>gs` Git
`Shift O` to insert line above
`a` append to cursor position
# COME BACK TO FIX THIS NEXT KEYMAP

`<leader>ps` text search
`h` move left
`k` move up
`l`move right
`j` move down
`=ap` indent `=` a paragraph `ap`
`<leader>a` add to harpoon
h preset 1
t preset 2
n preset 3
s preset 4
`dd` delete entire line
`0` cursor to beginning of line
`^` cursor to first character in the line
`D` delete directory confirmation


`:` enters command mode
	`q` quit - close buffer or current window
	`so` source - executes configuration changes
	`w` write - save
	`wq` write and quit
`%` creates new file
`d` creates new directory

LSP Commands

ctrl + p select prev item
ctrl + n select next item
ctrl + y confirm
ctrl + space select prev item
ctrl + c cancel insert mode
ctrl + o normal mode

REMAPS ON CURRENT BUFFER

    `gd` definition()
    `K` hover()
`<leader>vws` workspace_symbol()
    `<leader>vd` open_float
    `[d` goto_next()
    `]d` goto_prev()
    `<leader>vca` code_action()
    `<leader>vrr` references()
    `<leader>vrn` rename()
`<C-h>` signature_help()
    `:h<leader>rtp` netwr menu
    `:so` sources or reloads your configuration files
    `:PackerSync` syncs plugins managed by packer.lua
