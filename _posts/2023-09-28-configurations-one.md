---
layout: post
author: gashetonwan
---

2023-09-28

## Configurations One

> settings i use [lazy.nvim](https://github.com/LazyVim/LazyVim)

{% highlight lua %}
--------------------------keymap"m"------------------------------------------------------
vim.keymap.set("n", "mw", ":w<cr>", { silent = true })
vim.keymap.set("n", "mq", "<C-w>c", { silent = true })
vim.keymap.set("n", "md", ":bd<cr>", { silent = true })
vim.keymap.set("n", "mm", ":Neotree<cr>", { silent = true })
vim.keymap.set("n", "mt", ":quitall<cr>")
vim.keymap.set("n", "<leader>r", ":wq<cr>", { silent = true })

--------------------------delete---------------------------------------------------
vim.keymap.del("n", "y")
vim.keymap.del("n", "v")
vim.keymap.del({ "n", "i", "t" }, "f")
vim.keymap.del({ "n", "t" }, "F")

---

vim.keymap.set("i", "jk", "<ESC>", { noremap = true, silent = true, desc = "<ESC>" })

--------------------------codeium---------------------------------------------------
vim.keymap.set("i", "<C-t>", function()
return vim.fn["codeium#Accept"]()
end, { expr = true })
vim.keymap.set("i", "<M-;>", function()
return vim.fn["codeium#CycleCompletions"](1)
end, { expr = true })
vim.keymap.set("i", "<M-,>", function()
return vim.fn["codeium#CycleCompletions"](-1)
end, { expr = true })
vim.keymap.set("i", "<M-x>", function()
return vim.fn["codeium#Clear"]()
end, { expr = true })

---

{% endhighlight %}
