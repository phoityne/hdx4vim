
# Haskell Debugger Extension for vim
This is an experimental.

## System Design
![101_deploy.png](https://raw.githubusercontent.com/phoityne/hdx4vim/master/docs/design/101_deploy.png)

## Movie
![01_sample_debug.gif](https://raw.githubusercontent.com/phoityne/hdx4vim/master/docs/01_sample_debug.gif)


# Setup

## 1. Install vim-8.1
```
# yum install python-devel python3 python3-devel
#
# git clone https://github.com/vim/vim.git
#
# ./configure   --disable-selinux   --enable-cscope \
  --enable-fontset   --enable-gpm   --enable-multibyte  \
  --enable-rubyinterp   --enable-xim \
  --enable-python3interp --enable-pythoninterp
#
# make
# make install
#
```

## 2. Install vimspector
@see : https://github.com/puremourning/vimspector
```
$ cd
$ mkdir -p .vim/pack
$ cd .vim/pack
$ curl -L https://github.com/puremourning/vimspector/releases/download/769/linux-8d025c475d80f9026f38e05c5d4b8a3822483a52.tar.gz | tar zxvf -
$
```

## 3. Create a stack project
```
$ stack new sample rio
$
$ cd sample
$ stack test
$
```

## 4. Install haskell-debugger

 Install [haskell-dap](https://hackage.haskell.org/package/haskell-dap), [ghci-dap](https://hackage.haskell.org/package/ghci-dap), [haskell-debug-adapter](https://hackage.haskell.org/package/haskell-debug-adapter) at once.

```
$ stack install haskell-dap ghci-dap haskell-debug-adapter
  !! current version not work.
     get new one from github.
$
```

## 5. Prepare .vimspector.json
```
$ cd sample
$ cat .vimspector.json
{
  "adapters": {
    "hda": {
      "name": "haskell-debug-adapter",
      "command": [
        "haskell-debug-adapter",
        "--hackage-version=0.0.31.0"
      ]
    }
  },
  "configurations": {
    "haskell-debug-adapter: Launch": {
      "adapter": "hda",
      "configuration": {
        "type": "ghc",
        "request": "launch",
        "name": "haskell-debug-adapter",
        "internalConsoleOptions": "openOnSessionStart",
        "workspace": "${workspaceRoot}",
        "startup": "${workspaceRoot}/test/Spec.hs",
        "startupFunc": "",
        "startupArgs": "",
        "stopOnEntry": true,
        "mainArgs": "",
        "ghciPrompt": "H>>= ",
        "ghciInitialPrompt": "Prelude>",
        "ghciCmd": "stack ghci --test --no-load --no-build --main-is TARGET --ghci-options -fprint-evld-with-show",
        "ghciEnv": {},
        "logFile": "${workspaceRoot}/.vscode/phoityne.log",
        "logLevel": "DEBUG",
        "forceInspect": false
      }
    }
  }
}
$
```

## 6. Modify a bit
```
$ pwd
/home/phoityne/.vim/pack/vimspector/opt/vimspector/python3/vimspector
$ diff stack_trace.py.dist stack_trace.py
177c177
<     if source.get( 'sourceReference', 0 ) > 0:
---
>     if (source.get( 'sourceReference', 0 ) is not None) and (source.get( 'sourceReference', 0 ) > 0):
$
```

## 7. Run vim
```
$ vi test/UtilSpec.hs

:packadd vimspector
:call vimspector#ToggleBreakpoint()
:call vimspector#Launch()

```
