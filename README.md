# 🤯 wtf.nvim

A Neovim plugin to help you work out *what the fudge* that diagnostic means **and** how to fix it!

`wtf.nvim` provides faster and more efficient ways of working with the buffer line's diagnostic messages by redirecting them to your favourite resources straight from Neovim. 

Works with any language that has [LSP](https://microsoft.github.io/language-server-protocol/) support in Neovim.

## ✨ Features

### AI powered diagnostic debugging

Use the power of [ChatGPT](https://openai.com/blog/chatgpt) to provide you with explanations *and* solutions for how to fix diagnostics, custom tailored to the code responsible for them.

https://github.com/piersolenski/wtf.nvim/assets/1285419/9b7ab8b1-2dc4-4a18-8051-68745305198a

### Search the web for answers 

Why spend time copying and pasting, or worse yet, typing out diagnostic messages, when you can open a search for them in Google, Stack Overflow and more, directly from Neovim?

https://github.com/piersolenski/wtf.nvim/assets/1285419/6697d9a5-c81c-4e54-b375-bbe900724077

## 🔩 Installation

In order to use the AI functionality, set the environment variable `OPENAI_API_KEY` to your [openai api key](https://platform.openai.com/account/api-keys) (the search functionality will still work without it).

Install the plugin with your preferred package manager:

```lua
-- Packer
use({
  "piersolenski/wtf.nvim",
    config = function()
      require("wtf").setup()
    end,
    requires = {
      "MunifTanjim/nui.nvim",
    }
})

-- Lazy
{
	"piersolenski/wtf.nvim",
	dependencies = {
		"MunifTanjim/nui.nvim",
	},
	event = "VeryLazy",
  	opts = {},
	keys = {
		{
			"gw",
			mode = { "n" },
			function()
				require("wtf").ai()
			end,
			desc = "Debug diagnostic with AI",
		},
		{
			mode = { "n" },
			"gW",
			function()
				require("wtf").search()
			end,
			desc = "Search diagnostic with Google",
		},
	},
}
```

## ⚙️ Configuration

```lua
{
    -- Default AI popup type
    popup_type = "popup" | "horizontal" | "vertical",
    -- An alternative way to set your OpenAI api key
    openai_api_key = "sk-xxxxxxxxxxxxxx",
    -- ChatGPT Model
    openai_model_id = "gpt-3.5-turbo",
    -- Set your preferred language for the response
    language = "english",
    -- Any additional instructions
    additional_instructions = "Start the reply with 'OH HAI THERE'",
    -- Default search engine, can be overridden by passing an option to WtfSeatch 
    search_engine = "google" | "duck_duck_go" | "stack_overflow" | "github",
}
```


## 🚀 Usage

`wtf.nvim` works by sending the line's diagnostic messages along with contextual information (such as the offending code, file type and severity level) to various sources you can configure.

To use it, whenever you have an hint, warning or error in an LSP enabled environment, invoke one of the commands anywhere on that line in Normal mode:

| Command | Description |
| -- | -- |
| `:Wtf [additional_instructions]` | Sends the current line along with all diagnostic messages to ChatGPT. Additional instructions can also be specified, which might be useful if you want to refine the response further.
| `:WtfSearch [search_engine]` | Uses a search engine (defaults to the one in the setup or Google if not provided) to search for the **first** diagnostic. It will attempt to filter out unrelated strings specific to your local environment, such as file paths, for broader results. 

### Custom status hooks

You can add custom hooks to update your status line or other UI elements, for example, this code updates the status line colour to yellow whilst the request is in progress.

```lua
vim.g["wtf_hooks"] = {
	request_started = function()
		vim.cmd("hi StatusLine ctermbg=NONE ctermfg=yellow")
	end,
  request_finished = vim.schedule_wrap(function()
		vim.cmd("hi StatusLine ctermbg=NONE ctermfg=NONE")
	end)
}
```

### Lualine Status Component

There is a helper function `get_status` so that you can add a status component to [lualine](https://github.com/nvim-lualine/lualine.nvim).

```lua
local wtf = require("wtf")

require('lualine').setup({
    sections = {
        lualine_x = { wtf.get_status },
    }
})
```

## 💡 Inspiration

- [Pretty TypeScript Errors](https://github.com/yoavbls/pretty-ts-errors)
- [backseat.nvim](https://github.com/james1236/backseat.nvim/) 
- [CodeGPT.nvim](https://github.com/dpayne/CodeGPT.nvim) 

