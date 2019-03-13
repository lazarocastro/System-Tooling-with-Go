Before we can actually utilize Go we're going to need to install it on our workstation. In this lesson, we'll go through the very straightforward approach used to install Go.

#### Documentation For This Video

+ [Official Go Installation Documentation](https://golang.org/doc/install)
+ [Go Package Archives](https://golang.org/dl/)
+ [Go's "How to Write Go Code" Document](https://golang.org/doc/code.html)
#### Installing Go on Unix Systems

One of the big themes that we'll find as we work with Go is that Go tools can be distributed as static binaries and Go itself works the same way.

Depending on the system that you're using, you need to find the corresponding package from the [Go downloads page](https://golang.org/dl/) and unpackage its contents into the `/usr/local` directory on your machine.

For this course, we'll be using a CentOS 7 cloud server so we'll need the `linux-amd64` version of the package.

_Note:_ We'll be using Go 1.11.2 for this course.

```
[workstation] ~ $ cd /tmp
[workstation] /tmp $ curl -O https://dl.google.com/go/go1.11.2.linux-amd64.tar.gz
[workstation] /tmp $ tar -xzvf go1.11.2.linux-amd64.tar.gz
...
[workstation] /tmp $ sudo mv go /usr/local/
[workstation] /tmp $ cd ~
```

By unpacking this archive into the `/usr/local/` directory we now have a `/usr/local/go` directory that will hold of the Go's internal parts.

#### Adding Go Binaries to The $PATH

Before we can actually use the Go tools we'll need to adjust our `$PATH` to include the binaries distributed as part of Go. We need to at the `/usr/local/go/bin` directory to our path by modifying our `~/.barshrc` file:

_~/.bashrc_

```
# previous lines omitted
export PATH=$PATH:/usr/local/go/bin
```
Now we're able to run the `go` tool after reloading our `.bashrc` file:

```
[workstation] ~ $ exec $SHELL
[workstation] ~ $ go version
go version go1.11.2 linux/amd64
```
#### Setting Up the `$GOPATH`

Go uses a very specific folder structure and environment variables to know where to find Go code. For the code that we'll be writing, we need to put them into a path that we store as the `$GOPATH` environment variable. We'll set this directory structure up at `$HOME/go` and set the `$GOPATH` within our `.bashrc`.

Let's create the directory structure first:

```
[workstation] ~ $ mkdir -p $HOME/go/{bin,src}
```

Each of these directories stores something specific to Go:

+ `$GOPATH/src` - contains Go source files
+ `$GOPATH/bin` - contains executables

Next, let's set the environment variable in our `.bashrc`:

_~/.bashrc_

```
# previous lines omitted
export GOPATH=$HOME/go
export PATH=$PATH:/usr/local/go/bin:$GOPATH/bin
```
#### Setting Up Other Tools

We don't need very many other tools for our development, but there are some that we'll need:

+ Git

Let's install these now (on CentOS):

```
[workstation] ~ $ sudo yum install -y git
```
#### Customizing Our Text Editor

Go is a compiled language that has some very powerful tools built for developing with it. Depending on the text editor that you're using
you'll probably want to start by installing some tools to make development easier. Here are some of the popular ones:

+ [VSCode Go](https://github.com/Microsoft/vscode-go)
+ [vim-go](https://github.com/fatih/vim-go)
+ [GoLand](https://www.jetbrains.com/go/) (IDE by JetBrains)

For this course, the lectures will be using Vim, but feel free to follow along with whatever tool you'd like. Here are the steps that I'm taking to set up my development environment. The [vim-go](https://github.com/fatih/vim-go) package requires a newer version of Vim than is available on CentOS 7 systems so the first thing to do upgrade to Vim 8. This will require us to install some more development tools and remove the current installation of Vim.

```
[workstation] $ sudo yum autoremove -y vim
...
[workstation] $ sudo yum groupinstall -y "Development Tools"
...
[workstation] $ sudo yum install -y python36
...
[workstation] $ git clone https://github.com/vim/vim.git
[workstation] $ cd vim/src
[workstation] src $ make -j8
...
[workstation] src $ sudo make install
...
[workstation] src $ sudo cp vim /usr/bin/
[workstation] src $ cd ~
```

Next, we'll install the [vim-plug](https://github.com/junegunn/vim-plug) package manager:

```
[workstation] $ curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

From here, I'll modify my `~/.vimrc` file to use the package manager to install [vim-go](https://github.com/fatih/vim-go).

_~/.vimrc_

```
call plug#begin('~/.vim/plugged')
Plug 'fatih/vim-go'
call plug#end()
filetype off
filetype plugin indent on
set number
set noswapfile
set noshowmode
set ts=2 sw=2 sts=2 et
set backspace=indent,eol,start
" Map &lt;leader&gt; to comma
let mapleader=","
if has("autocmd")
autocmd FileType go set ts=2 sw=2 sts=2 noet nolist autowrite
endif
```

All that is remaining now is for us to install the [vim-go](https://github.com/fatih/vim-go) package and then have that package install the Go tools that it needs.

```
[workstation] ~ $ vim +PlugInstall +qa
...
[workstation] ~ $ vim +GoInstallBinaries +qa
...
```
Now we're ready to work with Go.
