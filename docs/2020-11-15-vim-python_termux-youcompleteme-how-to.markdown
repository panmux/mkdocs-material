Install these requirements 


```
termux: pkg install python3
termux: pkg install nodejs
termux: pkg install vim-python
termux: pkg install cmake
```

Then install Vim Plug (using curl command for unix), something like:


``` bash
curl -fLo ~/.vim/autoload/plug.vim --create-dirs     https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim

```


Then add these to ~/.vimrc:  ====> see the end of this file


Then clone YouCompleteMe to `~/.vim/plugged`:


```bash
git clone https://github.com/ycm-core/YouCompleteMe.git ~/.vim/plugged
```


Then manually cd to YouCompleteMe and complie it with


```bash
python3 install.py --ts-completer
```

The tail --ts-completer enables JS support, can add another lang. 

If error occur, try below instructions.

**Troubleshooting-steps-for-ycmd-server-SHUT-DOWN**

[https://github.com/ycm-core/YouCompleteMe/wiki/Troubleshooting-steps-for-ycmd-server-SHUT-DOWN](https://github.com/ycm-core/YouCompleteMe/wiki/Troubleshooting-steps-for-ycmd-server-SHUT-DOWN)

+ The best way to validate installation is to try and start ycmd manually using the process that YouCompleteMe will use.

Do this:
```
$ cd /path/to/YouCompleteMe/third_party/ycmd/
$ cat PYTHON_USED_DURING_BUILDING
/some/path/to/python3
$ cp ycmd/default_settings.json .
$ [/some/path/to/python3] ycmd --options_file=default_settings.json

serving on http://localhost:<some port>
```

I met an error related to atomic

Then Google, I found a related discussion here:

[https://gitter.im/Valloric/YouCompleteMe?at=5c8a13b51fae6423ca81e34c](https://gitter.im/Valloric/YouCompleteMe?at=5c8a13b51fae6423ca81e34c)

```
ImportError: dlopen failed: cannot locate symbol "__atomic_fetch_sub_4" referenced by "/data/data/com.termux/files/home/.vim/plugged/YouCompleteMe/third_party/ycmd/ycm_core.so
```

Answer Open


```
Open YouCompleteMe/third_party/ycmd/cpp/ycm/CMakeLists.txt
Find target_link_libraries
Add atomic to the list
Recompile
```
```
@@ -335,6 +335,7 @@ target_link_libraries( ${PROJECT_NAME}
${PYTHON_LIBRARIES}
${LIBCLANG_TARGET}
${EXTRA_LIBS}
**atomic**
)
if( LIBCLANG_TARGET )
```

I delete ~/.cache/pip and recompile with python3 install.py --ts-completer

Then it works.


## Here is my current ~/.vimrc content

```bash

"============~/.vimrc content============

" Enable line numbers, highlighting of current line by default; syntax
" highlighting is enabled by vim-plug

set number
set cursorline

" https://github.com/thoughtbot/dotfiles/blob/master/vimrc


set encoding=utf-8

" Leader
let mapleader = " "

set backspace=2   " Backspace deletes like most programs in insert mode
set nobackup
set nowritebackup
set noswapfile    " http://robots.thoughtbot.com/post/18739402579/global-gitignore#comment-458413287
set history=50
set ruler         " show the cursor position all the time
set showcmd       " display incomplete commands
set incsearch     " do incremental searching
set laststatus=2  " Always display the status line
set autowrite     " Automatically :write before running commands
set modelines=0   " Disable modelines as a security precaution
set nomodeline



" autocmd FileType javascript set omnifunc=javascriptcomplete#CompleteJS
" autocmd FileType php set omnifunc=phpcomplete#CompletePHP


" --------------------------------------------------------------------------------------
" Plugin manager vim-plug https://github.com/junegunn/vim-plug
call plug#begin('~/.vim/plugged')

" Plug 'Shougo/deoplete.nvim', { 'do': ':UpdateRemotePlugins' }

" Plug 'dense-analysis/ale'

" Plug 'Valloric/YouCompleteMe', { 'do': './install.py --ts-completer'}
" --ts-completer is more updated than the tern none
" Should git clone and install manually with python3 install.py --ts-completer

Plug 'ycm-core/YouCompleteMe', { 'do': './install.py --ts-completer' }

" Plug 'ycm-core/YouCompleteMe'
" Js prettier
" post install (yarn install | npm install) then load plugin only for editing supported files
Plug 'prettier/vim-prettier', {
  \ 'do': 'npm install',
  \ 'for': ['javascript', 'typescript', 'css', 'less', 'scss', 'json', 'graphql', 'markdown', 'vue', 'yaml', 'html'] }

call plug#end()

" let g:deoplete#enable_at_startup = 1






"============Pretier configure ============



" Max line length that prettier will wrap on: a number or 'auto' (use
" textwidth).
" default: 'auto'
let g:prettier#config#print_width = 'auto'

" number of spaces per indentation level: a number or 'auto' (use
" softtabstop)
" default: 'auto'
let g:prettier#config#tab_width = 2

" use tabs instead of spaces: true, false, or auto (use the expandtab setting).
" default: 'auto'
let g:prettier#config#use_tabs = 'false'


" flow|babylon|typescript|css|less|scss|json|graphql|markdown or empty string
" (let prettier choose).
" default: ''
let g:prettier#config#parser = ''

" cli-override|file-override|prefer-file
" default: 'file-override'
let g:prettier#config#config_precedence = 'file-override'

" always|never|preserve
" default: 'preserve'
let g:prettier#config#prose_wrap = 'preserve'

" css|strict|ignore
" default: 'css'
let g:prettier#config#html_whitespace_sensitivity = 'css'

" false|true
" default: 'false'
let g:prettier#config#require_pragma = 'false'

" Define the flavor of line endings
" lf|crlf|cr|all
" defaut: 'lf'
let g:prettier#config#end_of_line = get(g:, 'prettier#config#end_of_line', 'lf')
" No semi

let g:prettier#config#semi = 'false'

" Allow auto formatting for files without "@format" or "@prettier" tag

" let g:prettier#autoformat_require_pragma = 0


"https://www.dailysmarty.com/posts/how-to-setup-prettier-with-vim

let g:prettier#autoformat = 0
autocmd BufWritePre *.js,*.jsx,*.mjs,*.ts,*.tsx,*.css,*.less,*.scss,*.json,*.graphql,*.md,*.vue PrettierAsync




```
