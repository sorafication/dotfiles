# zsh_unplugged

> 🤔 perhaps you don't need a Zsh plugin manager after all...

TLDR; You don't need a big bloated plugin manager for your Zsh plugins. A simple
~20 line function may be all you need.

Click here to [skip to the code](#jigsaw-the-humble-plugin-load-function).

## :electric_plug: Zsh Plugin Managers

### :newspaper_roll: Current state

There are an embarrassingly large number of Zsh plugin managers out there. Many of them
are abandonware, are no longer actively developed, are brand new without many users, or
don't have much reason to even exist other than as a novelty.

Here's a list of many (but certainly not all) of them from [awesome-zsh-plugins]:

| Zsh Plugin Manager | Performance        | Current state                                   |
|--------------------|--------------------|-------------------------------------------------|
| [antibody]         | :rabbit2: fast     | :imp: Maintenance mode, no new features         |
| [antigen]          | :turtle: slow      | :imp: Maintenance mode, no new features         |
| [antidote]         | :rabbit2: fast     | :white_check_mark: Active                       |
| [sheldon]          | :question: unknown | :white_check_mark: Active                       |
| [zcomet]           | :rabbit2: fast     | :white_check_mark: Active                       |
| [zgem]             | :question: unknown | :skull_and_crossbones: Abandonware              |
| [zgen]             | :rabbit2: fast     | :skull_and_crossbones: Abandonware              |
| [zgenom]           | :rabbit2: fast     | :white_check_mark: Active                       |
| [zinit-continuum]  | :rabbit2: fast     | :white_check_mark: Active [\*][#1]              |
| [zinit]            | :rabbit2: fast     | :cursing_face: Author deleted project           |
| [zit]              | :question: unknown | :imp: Few/no recent commits                     |
| [znap]             | :rabbit2: fast     | :white_check_mark: Active                       |
| [zplug]            | :turtle: slow      | :skull_and_crossbones: Abandonware              |
| [zplugin][zinit]   | :rabbit2: fast     | :cursing_face: Renamed to zinit, author deleted |
| [zpm]              | :rabbit2: fast     | :white_check_mark: Active                       |
| [zr]               | :question: unknown | :imp: Few/no recent commits                     |

_Full disclosure, I'm the author of one of these - [antidote] (formerly called [pz])._

There's new ones popping up all the time too:

| Zsh Plugin Manager | Performance        | Current state        |
|--------------------|--------------------|----------------------|
| [mzpm]             | :question: unknown | :hatching_chick: New |
| [tzpm]             | :question: unknown | :hatching_chick: New |
| [uz]               | :question: unknown | :hatching_chick: New |
| [zed]              | :question: unknown | :hatching_chick: New |

### :firecracker: The catalyst

In January 2021, the plugin manager I was using, [antibody], was deprecated.
The author even [went so far as to say](https://github.com/getantibody/antibody/tree/2ca7616ae78754c0ab70790229f5d19be42206e9):

> Most of the other plugin managers catch up on performance, thus keeping this \[antibody] does not make sense anymore.

Prior to that, I used [zgen], which also stopped being actively developed and the
[developer](https://github.com/tarjoilija) seems to have disappeared. (_Shoutout
to @jandamm for carrying on Zgen with [Zgenom](https://github.com/jandamm/zgenom)!_)

In November 2021, a relatively well known and popular Zsh plugin manager, zinit, was
removed from GitHub entirely and without warning. In fact, the author
[deleted almost his entire body of work][zdharma-debacle]. Zinit was really popular
because it was super fast, and the author promoted his projects in multiple venues
for many years. (_Shoutout to the folks running [zdharma-continuum] though -
great work keeping Zinit alive!_)

With all the instability in the Zsh plugin manager space, it got me wondering why I
even bother with a plugin manager at all.

### :bulb: The simple idea

After [antibody] was deprecated, I tried [znap], but it was in early development at the
time and kept breaking, so like many others before me, I decided to write my own - [antidote].

When developing [antidote], my goal was simple - make a plugin manager
that was fast, functional, and easy to understand - which was everything I loved about
[zgen] and [antibody]. While [antidote] is a great project, and what I fully recommend
if you want to use a plugin manager, I kept wondering if I could cut further
down to a single _function_ and see what it would take to not use plugin management
utilities altogether.

Thus was born... **zsh_unplugged**.

This isn't a plugin manager - it's a way to show you how to manage your own plugins
using small, easy to understand snippets of Zsh. All this with the hope that perhaps,
once-and-for-all, we can demystify what plugin managers do, and perhaps for simple
configs do away with the idea that we even need to use bloated Zsh plugin manager
projects and just simply do it ourselves.

You can grab a ~20 line function and you have everything you need to manage your own
plugins from here on out. By way of contrast, I ran a rough line count of zinit's
codebase which comes out to nearly a whopping ~10,000 lines.

```zsh
zinit_tmpdir=$(mktemp -d)
git clone --depth 1 https://github.com/zdharma-continuum/zinit $zinit_tmpdir
wc -l $zinit_tmpdir/**/*.zsh
[[ -d $zinit_tmpdir ]] && rm -rf $zinit_tmpdir
```

Results:
```
      18 /var/folders/z0/w4blz6g14td2lf3b41gmhgd80000gn/T/tmp.w2WtwTZJ/docker/init.zsh
      61 /var/folders/z0/w4blz6g14td2lf3b41gmhgd80000gn/T/tmp.w2WtwTZJ/docker/utils.zsh
     186 /var/folders/z0/w4blz6g14td2lf3b41gmhgd80000gn/T/tmp.w2WtwTZJ/share/git-process-output.zsh
      51 /var/folders/z0/w4blz6g14td2lf3b41gmhgd80000gn/T/tmp.w2WtwTZJ/share/rpm2cpio.zsh
      19 /var/folders/z0/w4blz6g14td2lf3b41gmhgd80000gn/T/tmp.w2WtwTZJ/share/single-line.zsh
      23 /var/folders/z0/w4blz6g14td2lf3b41gmhgd80000gn/T/tmp.w2WtwTZJ/tests/setup.zsh
      12 /var/folders/z0/w4blz6g14td2lf3b41gmhgd80000gn/T/tmp.w2WtwTZJ/tests/teardown.zsh
     147 /var/folders/z0/w4blz6g14td2lf3b41gmhgd80000gn/T/tmp.w2WtwTZJ/zinit-additional.zsh
    3486 /var/folders/z0/w4blz6g14td2lf3b41gmhgd80000gn/T/tmp.w2WtwTZJ/zinit-autoload.zsh
    2389 /var/folders/z0/w4blz6g14td2lf3b41gmhgd80000gn/T/tmp.w2WtwTZJ/zinit-install.zsh
     404 /var/folders/z0/w4blz6g14td2lf3b41gmhgd80000gn/T/tmp.w2WtwTZJ/zinit-side.zsh
    3139 /var/folders/z0/w4blz6g14td2lf3b41gmhgd80000gn/T/tmp.w2WtwTZJ/zinit.zsh
    9935 total
```

\**Note: SLOC is not intended as an indicator of much here beyond a rough comparison of effort, maintainability, and complexity*

## :tada: The code

### :gear: The bare metal way

If you don't want to use anything resembling a plugin manager at all, you could simply
clone and source plugins yourself manually:

```zsh
ZPLUGINDIR=$HOME/.zsh/plugins

if [[ ! -d $ZPLUGINDIR/zsh-autosuggestions ]]; then
  git clone https://github.com/zsh-users/zsh-autosuggestions $ZPLUGINDIR/zsh-autosuggestions
fi
if [[ ! -d $ZPLUGINDIR/zsh-history-substring-search ]]; then
  git clone https://github.com/zsh-users/zsh-history-substring-search $ZPLUGINDIR/zsh-history-substring-search
fi
if [[ ! -d $ZPLUGINDIR/z ]]; then
  git clone https://github.com/rupa/z $ZPLUGINDIR/z
fi

source $ZPLUGINDIR/zsh-autosuggestions/zsh-autosuggestions.plugin.zsh
source $ZPLUGINDIR/zsh-history-substring-search/zsh-history-substring-search.plugin.zsh
source $ZPLUGINDIR/z/z.sh
```

This can get pretty cumbersome and tricky to maintain. You need to figure out each
plugin's init file, and sometimes adding a plugin to your `fpath` is required. While
this method works, there's another way...

### :jigsaw: The humble `plugin-load` function

If we go one level of abstraction higher than manual `git clone` calls, we can use a
simple function wrapper as the basis for everything you need to manage your own Zsh
plugins:

```zsh
# clone a plugin, identify its init file, source it, and add it to your fpath
function plugin-load {
  local repo plugin_name plugin_dir initfile initfiles
  ZPLUGINDIR=${ZPLUGINDIR:-${ZDOTDIR:-$HOME/.config/zsh}/plugins}
  for repo in $@; do
    plugin_name=${repo:t}
    plugin_dir=$ZPLUGINDIR/$plugin_name
    initfile=$plugin_dir/$plugin_name.plugin.zsh
    if [[ ! -d $plugin_dir ]]; then
      echo "Cloning $repo"
      git clone -q --depth 1 --recursive --shallow-submodules https://github.com/$repo $plugin_dir
    fi
    if [[ ! -e $initfile ]]; then
      initfiles=($plugin_dir/*.plugin.{z,}sh(N) $plugin_dir/*.{z,}sh{-theme,}(N))
      [[ ${#initfiles[@]} -gt 0 ]] || { echo >&2 "Plugin has no init file '$repo'." && continue }
      ln -sf "${initfiles[1]}" "$initfile"
    fi
    fpath+=$plugin_dir
    (( $+functions[zsh-defer] )) && zsh-defer . $initfile || . $initfile
  done
}
```

That's it. ~20 lines of code and you have a simple, robust Zsh plugin management
alternative that is likely as fast as most everything else out there.

What this does is simply clones a Zsh plugin's git repository, and then examines that
repo for an appropriate .zsh file to use as an init script. We then find and symlink the
plugin's init file if necessary, which allows us to get the performance advantage
of static sourcing rather than searching for which plugin files to load every time we
open a new terminal.

Then, the plugin is sourced and added to `fpath`.

You can even get turbocharged-hypersonic-load-speed-magic :rocket: if you really need
every last bit of performance.
[See how here](#question-how-do-i-load-my-plugins-with-hypersonic-speed-rocket).

### :question: How do you use this in your own Zsh config?

You are free to grab the `plugin-load` function above and put it directly in your
.zshrc, maintain it yourself, and never rely on anyone else's plugin manager again. Or,
this repo makes the plugin-load function available as a plugin itself if you prefer.
Here's an example .zshrc:

```zsh
# where do you want to store your plugins?
ZPLUGINDIR=${ZPLUGINDIR:-${ZDOTDIR:-$HOME/.config/zsh}/plugins}

# get zsh_unplugged and store it with your other plugins
if [[ ! -d $ZPLUGINDIR/zsh_unplugged ]]; then
  git clone --quiet https://github.com/mattmc3/zsh_unplugged $ZPLUGINDIR/zsh_unplugged
fi
source $ZPLUGINDIR/zsh_unplugged/zsh_unplugged.plugin.zsh

# make list of the Zsh plugins you use
repos=(
  # plugins that you want loaded first
  sindresorhus/pure

  # other plugins
  zsh-users/zsh-completions
  rupa/z
  # ...

  # plugins you want loaded last
  zsh-users/zsh-syntax-highlighting
  zsh-users/zsh-history-substring-search
  zsh-users/zsh-autosuggestions
)

# now load your plugins
plugin-load $repos
```

### :question: How do I update my plugins?

Updating your plugins is as simple as deleting the $ZPLUGINDIR and reloading Zsh.

```zsh
ZPLUGINDIR=~/.config/zsh/plugins
rm -rfi $ZPLUGINDIR
zsh
```

If you are comfortable with `git` commands and prefer to not rebuild everything, you
can run `git pull` yourself, or even use a simple `plugin-update` function:

```zsh
function plugin-update {
  ZPLUGINDIR=${ZPLUGINDIR:-$HOME/.config/zsh/plugins}
  for d in $ZPLUGINDIR/*/.git(/); do
    echo "Updating ${d:h:t}..."
    command git -C "${d:h}" pull --ff --recurse-submodules --depth 1 --rebase --autostash
  done
}
```

### :question: How do I list my plugins?

You can see what plugins you have installed with a simple `ls` command:

```zsh
ls $ZPLUGINDIR
```

If you need something fancier and would like to see the git origin of your plugins, you
could run this command:

```zsh
for d in $ZPLUGINDIR/*/.git; do
  git -C "${d:h}" remote get-url origin
done
```

### :question: How do I remove a plugin?

You can just remove it from your `plugins` list in your .zshrc. To delete it
altogether, feel free to run `rm`:

```zsh
# remove the fast-syntax-highlighting plugin
rm -rfi $ZPLUGINDIR/fast-syntax-highlighting
```

### :question: How do I load my plugins with hypersonic speed :rocket:?

You can get turbocharged-hypersonic-load-speed-magic if you choose to use the
[romkatv/zsh-defer](https://github.com/romkatv/zsh-defer) plugin. Essentially, if you
add `romkatv/zsh-defer` to your plugins list, everything you load afterwards will use
zsh-defer, meaning you'll get speeds similar to zinit's [turbo mode](https://github.com/zdharma-continuum/zinit#turbo-and-lucid).

Notably, if you like the [zsh-abbr] plugin for fish-like abbreviations in Zsh,
using zsh-defer [will boost performance greatly](https://github.com/olets/zsh-abbr/issues/52).

:warning: Warning - the author of zsh-defer does not recommend using the plugin this way,
so be careful and selective about which plugins you load with zsh-defer. If you get weird
behavior from a plugin, then load it before zsh-defer. In my extensive testing, the biggest
benefit came only from especially sluggish plugins like [zsh-abbr].

### :question: What if I want my plugins to be even faster?

If you are an experienced Zsh user, you may know about [zcompile], which takes your
Zsh scripts and potentially speeds them up by compiling them to byte code. If you feel
confident you know what you're doing and want to eek every last bit of performance out
of your Zsh, you can use this function

```zsh
function plugin-compile {
  ZPLUGINDIR=${ZPLUGINDIR:-$HOME/.config/zsh/plugins}
  autoload -U zrecompile
  local f
  for f in $ZPLUGINDIR/**/*.zsh{,-theme}(N); do
    zrecompile -pq "$f"
  done
}
```

### :question: How can I use this with Zsh frameworks like Oh-My-Zsh or Prezto?

[Oh-My-Zsh][ohmyzsh] and [Prezto][prezto] have their own built-in methods for loading
plugins, they just don't come with a way to clone them. You don't need the zsh_unplugged
script if you are using those frameworks. However, you also don't need a separate plugin
manager utility. Here's how you handle cloning yourself and go plugin-manager-free with
Zsh frameworks:

#### Oh-My-Zsh

If you are using [Oh-My-Zsh][ohmyzsh], the way to go without a plugin manager would be
to utilize the `$ZSH_CUSTOM` path.

_Note that this assumes your init file is called {plugin_name}.plugin.zsh which may not
be true._

```zsh
# .zshrc
# don't call this list 'plugins' since omz uses that
repos=(
  marlonrichert/zsh-hist
  zsh-users/zsh-syntax-highlighting
  zsh-users/zsh-autosuggestions
)
for repo in $repos; do
  if [[ ! -d $ZSH_CUSTOM/${repo:t} ]]; then
    git clone https://github.com/${repo} $ZSH_CUSTOM/plugins/${repo:t}
  fi
done
unset repo{s,}

# add your external plugins to your OMZ plugins list
plugins=(
  ...
  zsh-hist
  zsh-autosuggestions
  ...
  zsh-syntax-highlighting
)
```

#### Prezto

If you are using [Prezto][prezto], the way to go without a plugin manager would be to
utilize the `$ZPREZTODIR/contrib` path.

_Note that this assumes your init file is called {plugin_name}.plugin.zsh which may not
be true._

```zsh
# .zshrc
contribs=(
  rupa/z
  marlonrichert/zsh-hist
  mattmc3/zman
)
for contrib in $contribs; do
  if [[ ! -d $ZPREZTODIR/contrib/${contrib:t} ]]; then
    git clone https://github.com/${contrib} $ZPREZTODIR/contrib/${contrib:t}
  fi
done
unset contrib{,s}

# add the contribs to your Prezto modules list in your `.zpreztorc`
zstyle ':prezto:load' pmodule \
  ... \
  z \
  zsh-hist \
  ... \
  zman
```

[zinit-docs-reddit]: https://www.reddit.com/r/zsh/comments/mur6eu/anyone_interested_in_zinit_documentation/
[awesome-zsh-plugins]: https://github.com/unixorn/awesome-zsh-plugins
[zdharma-debacle]: https://www.reddit.com/r/zsh/comments/qinb6j/httpsgithubcomzdharma_has_suddenly_disappeared_i/
[zdharma-continuum]: https://github.com/zdharma-continuum
[zcompile]: https://github.com/antonio/zsh-config/blob/master/help/zcompile
[antibody]: https://github.com/getantibody/antibody
[antigen]: https://github.com/zsh-users/antigen
[mzpm]: https://github.com/xylous/mzpm
[antidote]: https://github.com/mattmc3/antidote
[pz]: https://github.com/mattmc3/antidote/tree/pz
[sheldon]: https://github.com/rossmacarthur/sheldon
[tzpm]: https://github.com/notusknot/tzpm
[uz]: https://github.com/maxrodrigo/uz
[zcomet]: https://github.com/agkozak/zcomet
[zed]: https://github.com/MunifTanjim/zed
[zgem]: https://github.com/qoomon/zgem
[zgen]: https://github.com/tarjoilija/zgen
[zgenom]: https://github.com/jandamm/zgenom
[zinit-continuum]: https://github.com/zdharma-continuum/zinit
[zinit]: https://github.com/zdharma/zinit
[zit]: https://github.com/thiagokokada/zit
[znap]: https://github.com/marlonrichert/zsh-snap
[zplug]: https://github.com/zplug/zplug
[zpm]: https://github.com/zpm-zsh/zpm
[zr]: https://github.com/jedahan/zr
[#1]: https://github.com/mattmc3/zsh_unplugged/issues/1
[ohmyzsh]: https://github.com/ohmyzsh/ohmyzsh
[prezto]: https://github.com/sorin-ionescu/prezto
[pure]: https://github.com/sindresorhus/pure
[zsh-abbr]: https://github.com/olets/zsh-abbr
