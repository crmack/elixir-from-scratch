# elixir-from-scratch
Instructions for creating a new Elixir/Phoenix app from scratch.

We will step through creating a new Elixir/Phoenix project then adding common features/functionality on top.

The steps will represent a real world project that will get deployed in a cloud service. All steps will be documented here while the project code will exist in a separate repo: 

This top level README will cover the prereqs and creating the Phoenix app, then we will move to different README files from there.

## The Project

This is not super important for the purposes of following along, but just to provide some context about what we'll be doing here...

We will be creating a web app to allow users to track their video games. They can catalog games they own or want and keep status on played/finished/completed/abandoned/in progress, along with ratings and reviews. This will be a fairly basic DB.

## Install the Software

This will be written to outline the process of installing on a Linux system. We will be using [Homebrew](https://brew.sh/) to start, which is available on Linux and macOS, so macOS instructions should mostly follow suit. (Full Disclosure: I'll be doing this on a WSL2 Debian instance)

### Homebrew

From the above link, you have options. We will choose the simplest one and install Homebrew with the following command

`/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`

The end of the installation process above will inform you to run the following commands, so do that:

```
(echo; echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"') >> /home/crmack/.bashrc
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
sudo apt-get install build-essential
```

Then, it recommends installing `gcc`, which certaily sounds like fun we wouldn't want to miss out on, go ahead and run:

`brew install gcc`

By running the above command you will have unknowingly verified that the Homebrew isntallation worked. Two birds and whatnot.

### asdf

asdf if a tool version manager. It allows you to define which version of which tools you want to use on a per-project basis. This is very handy, so we will use this to manage some of our tools.

Install asdf with Homebrew using

`brew install asdf`

Confirm this worked by following it up with a `asdf --version` which should give you something like `v0.13.1` if the installation succeeded. 

You will also want to make sure the asdf shims directory is in your path so the packages it installs can be found. Update your rc file of choice (`~/.bashrc` by default) by adding the following PATH update:

`export PATH=~/.asdf/shims:$PATH`

### Odds and Ends

Some other stuff we need to get installed for....reasons.

`brew install wxwidgets`

`brew install unzip`

### Elixir and Erlang

Now we're getting to the good stuff. First up, we need to tell asdf which tools we want it to support.

We will start with Erlang which is definitely not Elixir or Phoenix. Whoops. Run

`asdf plugin add erlang`

Erlang is the base which Elixir is built on. It's important, trust me.

With that installed, we can do something similar for Elixir. See the command below:

`asdf plugin add elixir`

If you run an `asdf list-all elixir` you should get a long list of version numbers, these are the version of Elixir that your asdf setup now supports.

Now it gets a little tricky and personal preference-y. You can now install Elixir at a system level if you want. Or you can so super-asdf and create your version `.tool-versions` file to keep everything project-specific. You do you, but for simplicity and the safe of not making more system-wide installations, we will go with the latter option here.

Find a nice, safe directory, something like `~/projects/` and `cd` into it. Then create an empty asdf file using `touch .tool-versions` Open the file with your editor of choice and add the following lines:

```
erlang 26.1.2
elixir 1.15.7-otp-26
```

Save and exit. We then use the magic of asdf to install these two specific versions we defined in our tools file. From the same directory, run:

`asdf install`

### Troubleshooting Erlang Install

There are some very specific Ubuntu/Erlang compatibility issues that might make this install *extremely* painful. If you are on Ubuntu, see [this link](https://github.com/asdf-vm/asdf-erlang/issues/247) and good luck.

Other Linux envs will require some addition packages to be installed before Erlang's installation will work. If you run `asdf install` you will see a list of warnings about missing dependencies, go ahead and install those with `sudo apt-get install libssl-dev automake autoconf libncurses5-dev` then retry the asdf install and it should work.

If all has gone correctly, you should be able to run `elixir -v` and get a result similar to:

> Erlang/OTP 26 [erts-14.1.1] [source] [64-bit] [smp:6:6] [ds:6:6:10] [async-threads:1] [jit:ns]]
> 
> Elixir 1.15.7 (compiled with Erlang/OTP 26)

### Phoenix

With Elixir installed, we can quickly and easily install Phoenix (Elixir's web framework) using the following

`mix archive.install hex phx_new`


## Creating a Project

The first step for any new Phoenix project is the most wonderful command in all of software development:

`mix phx.new video_game_tracker`

Type `Y` to accept the dependency installation `mix` wants to run for you.

We've done it. We have created a Phoenix application. As you probably noticed, 97% of the work done here *wasn't actually in Elixir* but rather installing all of the stuff we need. That's because Elixir is awesome and it's everything else that is terrible!

Elixir is kind enough to give us some next-steps right there on the command line at the end of our install:

> We are almost there! The following steps are missing:
>
>    $ cd video_game_tracker
>
> Then configure your database in config/dev.exs and run:
>
>    $ mix ecto.create
>
> Start your Phoenix app with:
>
>    $ mix phx.server
>
> You can also run your app inside IEx (Interactive Elixir) as:
>
>   $ iex -S mix phx.server


This...isn't going to work yet unless you have postgres installed and running (which you very well might!)

We will tackle how we want to handle running postgres in the next section.





