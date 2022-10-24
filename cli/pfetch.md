``pfetch`` is a leightweight system infomormation tool for systems supporting POSIX shell. Unlike ``neofetch``, it fits into a singular file with the size of around 60kB.
The logos used by it, are provided by ``ufetch``.

## Installation
There are multiple ways of installation. The most leightweight, and the most easy are listed here. 

### Bin-directory
```sh
wget https://raw.githubusercontent.com/dylanaraps/pfetch/master/pfetch
chmod +x pfetch
mv pfetch /usr/local/bin
```

### Alias
```sh
alias pfetch wget https://raw.githubusercontent.com/dylanaraps/pfetch/master/pfetch -q; chmod +x ./pfetch ; PF_ASCII="linux" PF_COL1=9 PF_COL2=9 PF_COL3=7 ./pfetch ; rm ./pfetch
```

## Configuration
One can configure 14 different settings, each being set via environment variables.

### Colors
Colors can be changes with ``PF_COL1``, ``PF_COL2`` and ``PF_COL3``. These can have the values from ``1`` to ``9``, specifying a color.

```sh
PF_COL1=9 PF_COL2=9 PF_COL3=7 pfetch
```

### Logos
The logo shown can be changed with the ``PF_ASCII`` property. Its possible values are listed [here](https://gitlab.com/jschx/ufetch)  

```sh
PF_ASCII="linux" pfetch
```

### Information
Everything that's shown can be toggled on or off via the ``PF_INFO`` variable.

```shell
# Show everything
PF_INFO="ascii title os host kernel uptime pkgs memory" pfetch

# Only The os and ascii
PF_INFO="ascii of" pfetch
```

### Rest
The other properties make minor differences, and are therefor listed here.

```shell
# Which user to display.
USER=""

# Which hostname to display.
HOSTNAME=""

# Which editor to display.
EDITOR=""

# Which shell to display.
SHELL=""

# Which desktop environment to display.
XDG_CURRENT_DESKTOP=""
```