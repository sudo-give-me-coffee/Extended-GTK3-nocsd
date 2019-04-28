# Extended-GTK3-nocsd

`extgtk3-nocsd` is a wrappper for [gtk3-nocsd module](https://github.com/PCMan/gtk3-nocsd) used to disable the client side decoration of Gtk+ 3 per application 

## Introduction:

Since Gtk+ 3.10, its developers added a so-called header bar or custom title bar. With this and the client-side decoration, the original title bar and window border provided by the window manager are disabled by Gtk+. The worst part, is that it breaks down some window managers or composers. See a example:

![](https://github.com/sudo-give-me-coffee/Extended-GTK3-nocsd/raw/master/screenshots/no-hack.png)

Unfortunately, the Gtk+ developers decided to be against the existing standards and provide "no option" to turn it off. Thanks to PCMan's gtk3-nocsd, we still have a way to (partially) turn it off. Window manager (title bar and window border) can be re-enabled. But isn't seamless as you can see:

![](https://github.com/sudo-give-me-coffee/Extended-GTK3-nocsd/raw/master/screenshots/gtk3-nocsd-hack.png)

> Note the duplication of application title and window icon

The `extgtk3-nocsd` fix it by overriding GTK 3 config dir, so the result looks more naturally:

![](https://github.com/sudo-give-me-coffee/Extended-GTK3-nocsd/blob/master/screenshots/extended-gtk3-nocsd-hack.png)

## How to use:

* [Install gtk3-nocsd](https://github.com/PCMan/gtk3-nocsd)  (usually by installing `libgtk3-nocsd`package)
* Download the wrapper: `wget https://git.io/fjGRR -O extgtk3-nocsd`
* Turn executable: `chmod +x extgtk3-nocsd`
* Move to /usr/bin: `sudo mv extgtk3-nocsd /usr/bin/`
* Use the command `extgtk3-nocsd application`for example `extgtk3-nocsd evince`

## To have all Gtk+ 3 apps (of current user) use this:

**Note: If you installed [gtk3-nocsd](https://github.com/PCMan/gtk3-nocsd)  from your package manager you don't need do these steps, only run:**

`extgtk3-nocsd`

<hr>

* [Install gtk3-nocsd](https://github.com/PCMan/gtk3-nocsd)  (usually by installing `libgtk3-nocsd`package)
* Run script without parameter:`extgtk3-nocsd`
> You will see some errors, ignore them
* Do these steps:



Export some environment variables in your ~/.bashrc:

```
GTK3_NOCSD=$(ldconfig -p | grep libgtk3-nocsd.so | awk '{print $4}')
export GTK_CSD=0
export LD_PRELOAD="${GTK3_NOCSD}${LD_PRELOAD:+:$LD_PRELOAD}"
```

On Arch Linux, you should use ~/.xsession instead of ~/.bashrc for the CSDs to be disabled properly.

On Debian-based systems with graphical login, instead modify (or create) ~/.xsessionrc and add the code:

```
  if [ -n "$STARTUP" ]; then
    BASESTARTUP=${STARTUP%% *}
    BASESTARTUP=${BASESTARTUP##*/}
    if [ "$BASESTARTUP" = x-session-manager ]; then
      BASESTARTUP=$(basename $(readlink /etc/alternatives/x-session-manager))
    fi
    if [ x"$BASESTARTUP" = x"${BASESTARTUP#gnome-session}" ] ; then
      GTK3_NOCSD=$(ldconfig -p | grep libgtk3-nocsd.so | awk '{print $4}')
      export GTK_CSD=0
      STARTUP="env LD_PRELOAD=${GTK3_NOCSD}${LD_PRELOAD:+:$LD_PRELOAD} $STARTUP"
    fi
  fi
  ```

Re-login to make the environment variables take effect.
