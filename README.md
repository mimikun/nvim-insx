# nvim-minx

Manage insert mode mapping.

<video src="https://user-images.githubusercontent.com/629908/211611400-4912f939-386c-4ec4-b63d-f79aa543e4e1.mov" width="100%"></video>

### Warning

- This plugin does not support `dot-repeat`.
  - The macro have the highest priority.
- This plugin is *usually* works as expected.
  - Does not aim to support to always work as expected.

### Usage

This plugin does not provide any default mappings.

You should define mapping by yourself like this.

```lua
do
  local minx = require('minx')
  local esc = minx.helper.regex.esc
  for open, close in pairs({
    ['('] = ')',
    ['['] = ']',
    ['{'] = '}',
  }) do
    minx.add(close, require('minx.recipe.fast_wrap')({
      close = close
    }))
    minx.add(open, require('minx.recipe.auto_pair')({
      open = open,
      close = close,
    }))
    minx.add('<Space>', require('minx.recipe.pair_spacing').increase_pair_spacing({
      open_pat = esc(open),
      close_pat = esc(close),
    }))
    minx.add('<BS>', require('minx.recipe.pair_spacing').decrease_pair_spacing({
      open_pat = esc(open),
      close_pat = esc(close),
    }))
    minx.add('<BS>', require('minx.recipe.delete_pair')({
      open_pat = esc(open),
      close_pat = esc(close),
    }))
    minx.add('<CR>', require('minx.recipe.fast_break')({
      open_pat = esc(open),
      close_pat = esc(close),
    }))
  end
  minx.add('<BS>', require('minx.recipe.delete_pair')({
    open_pat = minx.helper.search.Tag.Open,
    close_pat = minx.helper.search.Tag.Close,
  }))
  minx.add('<CR>', require('minx.recipe.fast_break')({
    open_pat = minx.helper.search.Tag.Open,
    close_pat = minx.helper.search.Tag.Close,
  }))
end
```

### Status

early development.