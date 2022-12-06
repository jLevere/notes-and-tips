## Steps to install GEF, the GDB extention on OSU stdlinux

#### install notes:

GEF is a gdb extention that uses python to preform aditional user interface opetions and is generally easier to look at than GDB in my opinion.  I think it is also less glitchy than tui mode in gdb.

Modern GEF requires a GDB version above ~8.7 and python3.  stdlinux has neither of these things.  To account for this, we are going to install a newer version of GDB and an old version of GEF to meet in the middle.  If you have intalled python3, then make sure when you run `which python` in the commandline that it returns `/usr/bin/python`



1. Clone GEF-Legacy

    Everything we are going to do should be done in your user home directory.  If you dont know where that is, just run `cd ~` to return to home.

    ```bash
    git clone `https://github.com/hugsy/gef-legacy.git
    ```

2. Prepare .gdbinit

    We need to setup plugins in gdb.  .gdbinit is called each time it starts, just like .bashrc.

    ```bash
    echo source ~/gef-legacy/gef.py >> .gdbinit
    ```

3. Install newer GDB

    We are going to use the OSU-ETS tool `subscribe` to fetch a prebuilt version of gdb for stdlinux.  There is a version compiled with python3, but I have not been able to deconflict some of the python versions effectivly.  So we are going to just use a python2.7 version.

    ```bash
    subscribe
    ```
    Select `GDB-9.2` and hit <kbd>q</kbd> to install it

4. Configure GEF

    Everything should be installed at this point. You can start using GEF now by just callling `gdb` like normal.  I am going to customize a few things to make it easier to use on stdlinux.

    ```bash
    gdb
    gef config context.layout "legend regs stack args source -threads -trace -code -extra memory"
    gef save
    ```
    Context.layout will remove the panes with a `-` infront of them. I find that with all the panes it is dificult to fit everything on my screen.

    This will create a `.gef.rc` file in your home and populate it with your current config. GEF uses this to save your settings between runs and also allows you to look through them more easily.


5. Start GEF

    GEF should start automatically when you run GDB normally.  If you want to run GDB without GEF, you can use `gdb -nx` to disable plugins.