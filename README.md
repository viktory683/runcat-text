<img src="runcat.gif" width="150" align="left" />

# runcat-text

Is a runcat port for Linux to place it somewhere of your bar


(Another useless cat here..)

> :warning: This was written mostly for waybar and has not yet been tested on others.

# requirements

- `python`
- `asyncio`
- `pyjson5` (recommended)

# table of contents

- [runcat-text](#runcat-text)
- [requirements](#requirements)
- [table of contents](#table-of-contents)
- [install/build](#installbuild)
  - [example](#example)
  - [font installation](#font-installation)
- [start](#start)
- [configuration](#configuration)
  - [UI settings](#ui-settings)
  - [CPU settings](#cpu-settings)
    - [states](#states)
- [styling](#styling)
- [todo](#todo)
- [resources](#resources)
- [motivation](#motivation)
- [plans](#plans)

# install/build

- place [main.py](main.py) and [config.json](config.json) files somewhere in your system
- install all necessary from [requirements.txt](requirements)
    > highly recommended to create virtual environment

## example

```bash
git clone ...
mkdir ~/.config/waybar/modules
cp -r runcat-text ~/.config/waybar/modules/
cd ~/.config/waybar/modules/runcat-text
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## font installation

```bash
cp -r runcat.ttf ~/.local/share/fonts
fc-cache -f
```

# start

`~/.config/waybar/config`
```jsonc
    ...,
    "modules-right": [
        ...,
        "custom/runcat",
        ...,
    ],
    ...,
    "custom/runcat": {
        "exec": "~/.config/waybar/modules/runcat-text/main.py",
        "return-type": "json"
    },
    ...,
```

# configuration

Here is some info about waybar's config options and some that personal for the module. If you are searching for more check your bar's wiki.

`~/.config/waybar/config`

| option | typeof | default | description |
|--------|-------|---------|-------------|
| `exec` | string | | path to python script |
| `return-type` | string |  | `json` if you want to use waybar's CSS-classes or tooltips.<br>Nothing or anything if you want to use with polybar |
| `restart-interval` | int | | The restart interval (in seconds).<br>Can't be used with the interval option, so only with continuous scripts.<br>Once the script exit, it'll be re-executed after the restart-interval. |
| `format` | string | `{}` | The format, how information should be displayed. On `{}` data gets inserted.<br> |

`module/config.json`
| option | typeof | default | description |
|-|-|-|-|
| `icons` | `string`<br>`array[any]` | | Icons to use for animation.<br> I've converted original svg's to [ttf-font](runcat.ttf) so you don't need to (check [above](#font-installation)) |
| `return-type` | string | | json if you want to use waybar's CSS-classes or tooltips.<br>Nothing or anything if you want to use with polybar |
| `tooltip-format` | string | | Custom format for your tooltip<br>Valid values are: `percentage` |
| `ui` | object | | Some UI setting<br>Check [below](#ui-settings) |
| `cpu` | object | | Some CPU setting<br>Check [below](#cpu-settings) |

## UI settings
| option | typeof | default | description |
|-|-|-|-|
| `fps_l` | int | `6` | Minimal FPS |
| `fps_h` | int | `90` | Maximum FPS |

## CPU settings
| option | typeof | default | description |
|-|-|-|-|
| `interval` | float | `1` | CPU info update time |
| `stat-file` | string | `/proc/stat` | Path to `proc` stats file.<br>dunno if it need somebody |
| `states` | object |  | State-classes used for styling<br>[check](#states) |

### states

You can define here your custom classes to use it with waybar.

Format like `{"class-name": lower-percent}`

For example
```jsonc
    "cpu": {
        "states": {
            "high": 90,
            "medium": 40,
            "low": 10
        },
        ...,
    },
    ...
```

With states like that your module will gets classes like
| percent | class |
|-|-|
| 1 | default|
| 10 | low |
| 20 | low |
| 40 | medium |
| 89 | medium |
| 90 | high |
| 100 | high |

# styling

```CSS
#custom-runcat {
    font-family: 'runcat';
    font-size: 18px;
}

#custom-runcat.low {
    font-family: 'runcat';
    color: green;
}

#custom-runcat.medium {
    font-family: 'runcat';
    color: blue;
}

#custom-runcat.high {
    font-family: 'runcat';
    color: red;
}

```

# todo

- [ ] [MORE CATS!!!](https://kyome.io/runcat/index.html?lang=en)
- [ ] test with other bars
- [ ] check font for line-height or something
- [ ] refactor

# resources

The default resources are from internet.

- <img src="runcat.gif" width="36" /> from [win0err/gnome-runcat](https://github.com/win0err/gnome-runcat)
- [icomoon](https://icomoon.io) for converting SVG-icons to font
- original repo for tray. Great work, thanks a lot

# motivation

Cute little kitty running anywhere in the bar and not just in the tray

Rewrite C-code to python to easily modify and distribute

# plans

- refactor and reformat
- maybe rewrite in something like C but more user-friendly (Rust?)
