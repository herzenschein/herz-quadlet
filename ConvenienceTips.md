This is just a file for me to make things easier when updating this repository.

These might be useful to you, too.

## Making large edits with Crudini

[Crudini](https://github.com/pixelb/crudini) is a command line tool that allows to edit ini files programmatically.

Here's a [fish](https://fishshell.com/) oneliner to update only containers that should start on boot:

```fish
for file in (find . -name "*.container" -not -name "*generate.container" -not -name "*build.container" -not -name "*init.container"); crudini --ini-options=nospace --set $file "Install" "WantedBy" "default.target"; end
```

I used this for commit 66dfab61a0235991cbeae905931aef9ed952f7d1.

The above adds:

```ini
[Install]
WantedBy=default.target
```

To all containers that do not have "generate", "build", or "init" in their name (in other words, containers that are supposed to start on boot).

If the entry already exists, it will simply change the existing value.
