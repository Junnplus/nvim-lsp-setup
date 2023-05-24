# lsp-setup.nvim

A simple wrapper for [nvim-lspconfig](https://github.com/neovim/nvim-lspconfig) and [mason-lspconfig](https://github.com/williamboman/mason-lspconfig.nvim) to easily setup LSP servers.

## Installation

- Neovim >= 0.8 (>= 0.10 or nightly for inlay hints)
- nvim-lspconfig
- mason.nvim & mason-lspconfig.nvim

### [lazy.nvim](https://github.com/folke/lazy.nvim)

```
{
    'junnplus/lsp-setup.nvim',
    dependencies = {
        'neovim/nvim-lspconfig',
        'williamboman/mason.nvim',
        'williamboman/mason-lspconfig.nvim',
    },
}
```

### [packer.nvim](https://github.com/wbthomason/packer.nvim)

```lua
use {
    'junnplus/lsp-setup.nvim',
    requires = {
        'neovim/nvim-lspconfig',
        'williamboman/mason.nvim',
        'williamboman/mason-lspconfig.nvim',
    }
}
```

### [vim-plug](https://github.com/junegunn/vim-plug)

```vim
Plug 'junnplus/lsp-setup.nvim'
Plug 'neovim/nvim-lspconfig'
Plug 'williamboman/mason.nvim'
Plug 'williamboman/mason-lspconfig.nvim'
```


## Usage

```lua
require('lsp-setup').setup({
    servers = {
        pylsp = {}
    }
})
```

You can replace `pylsp` with the LSP server name you need, see [available LSPs](https://github.com/williamboman/mason-lspconfig.nvim#available-lsp-servers).

Also support installing custom versions of LSP servers, for example:

```lua
require('lsp-setup').setup({
    servers = {
        ['rust_analyzer@nightly'] = {}
    }
})
```

### Setup structure

```lua
require('lsp-setup').setup({
    -- Default mappings
    -- gD = 'lua vim.lsp.buf.declaration()',
    -- gd = 'lua vim.lsp.buf.definition()',
    -- gt = 'lua vim.lsp.buf.type_definition()',
    -- gi = 'lua vim.lsp.buf.implementation()',
    -- gr = 'lua vim.lsp.buf.references()',
    -- K = 'lua vim.lsp.buf.hover()',
    -- ['<C-k>'] = 'lua vim.lsp.buf.signature_help()',
    -- ['<space>rn'] = 'lua vim.lsp.buf.rename()',
    -- ['<space>ca'] = 'lua vim.lsp.buf.code_action()',
    -- ['<space>f'] = 'lua vim.lsp.buf.formatting()',
    -- ['<space>e'] = 'lua vim.diagnostic.open_float()',
    -- ['[d'] = 'lua vim.diagnostic.goto_prev()',
    -- [']d'] = 'lua vim.diagnostic.goto_next()',
    default_mappings = true,
    -- Custom mappings, will overwrite the default mappings for the same key
    -- Example mappings for telescope pickers:
    -- gd = 'lua require"telescope.builtin".lsp_definitions()',
    -- gi = 'lua require"telescope.builtin".lsp_implementations()',
    -- gr = 'lua require"telescope.builtin".lsp_references()',
    mappings = {},
    -- Global on_attach
    on_attach = function(client, bufnr)
        -- Support custom the on_attach function for global
        -- Formatting on save as default
        require('lsp-setup.utils').format_on_save(client)
    end,
    -- Global capabilities
    capabilities = vim.lsp.protocol.make_client_capabilities(),
    -- Configuration of LSP servers 
    servers = {
        -- Install LSP servers automatically
        -- LSP server configuration please see: https://github.com/neovim/nvim-lspconfig/blob/master/doc/server_configurations.md
        -- pylsp = {},
        -- rust_analyzer = {
        --     settings = {
        --         ['rust-analyzer'] = {
        --             cargo = {
        --                 loadOutDirsFromCheck = true,
        --             },
        --             procMacro = {
        --                 enable = true,
        --             },
        --         },
        --     },
        -- },
    },
    -- Configuration of LSP inlay hints
    inlay_hints = {
        enabled = false,
        parameter_hints = true,
        type_hints = true,
        highlight = 'Comment',
        priority = 0,
    }
})
```

## Integrations

### [cmp-nvim-lsp](https://github.com/hrsh7th/cmp-nvim-lsp) or [coq_nvim](https://github.com/ms-jpq/coq_nvim)

If installed, will auto advertise capabilities to LSP servers.

### [neodev](https://github.com/folke/neodev.nvim)

```lua
-- Setup lua_ls with neodev
require('neodev').setup()
require('lsp-setup').setup({
    servers = {
        lua_ls = {}
    }
})
```
### [rust-tools.nvim](https://github.com/simrat39/rust-tools.nvim)

Using `require('lsp-setup.rust-tools').setup({})` instead of `require('rust-tools').setup({})`.

```lua
require('lsp-setup').setup({
    servers = {
        rust_analyzer = require('lsp-setup.rust-tools').setup({
            server = {
                settings = {
                    ['rust-analyzer'] = {
                        cargo = {
                            loadOutDirsFromCheck = true,
                        },
                        procMacro = {
                            enable = true,
                        },
                    },
                },
            },
        })
    }
})
```

### [clangd_extensions.nvim](https://github.com/p00f/clangd_extensions.nvim)

Using `require('lsp-setup.clangd_extensions').setup({})` instead of `require('clangd_extensions').setup({})`.

```lua
require('lsp-setup').setup({
    servers = {
        clangd = require('lsp-setup.clangd_extensions').setup({})
    }
})
```

## Contributing

Bug reports and feature requests are welcome! PRs are doubly welcome!

## Acknowledgements

The implementation of inlay hints is based on lvimuser's [lsp-inlayhints.nvim](https://github.com/lvimuser/lsp-inlayhints.nvim).
