# nerdps1
Nerd prompt for bash/ksh/zsh (mksh/ash)

## activate the prompt

For better experience, install Nerd font on your system/console (Windows console / Windows terminal / putty / git-bash / CmdEr / iTerm2 / Terminator / MobaXterm / VScode terminal / Pycharm terminal...):  
[Consolas NF](https://github.com/wclr/my-nerd-fonts/raw/master/Consolas%20NF/Consolas%20Nerd%20Font%20Complete%20Mono%20Windows%20Compatible.ttf)  
[Nerd Fonts](https://www.nerdfonts.com/)

on Unix, copy to `~/.fonts` and run `fc-cache -fv` then relaunch your terminal and set the font

Then activate the nerdps1 prompt:
```shell
$ . <(curl -s https://raw.githubusercontent.com/joknarf/nerdps1/main/nerdps1)
```

<img width="900" alt="image" src="https://github.com/joknarf/nerdps1/assets/10117818/ebc3f680-69b1-45d2-b1ce-b09b09b545f2">

You can get your local copy using:  
`$ curl -sL -o ~/nerdps1 https://raw.githubusercontent.com/joknarf/nerdps1/main/nerdps1`  
then source it in your profile/rcfile:  
`. ~/nerdps1`

Following information displayed:
* exit code if command returns code is not 0
* elapse time during command if command lasts more than 1s (bash / zsh / ksh >2012)
* user@hostname
* current working directory
* git branch if in git directory (colorized according to git status)
* python VIRTUAL_ENV and other variables values with name in `ps1_info` variable
* filesystem usage check of `ps1_fslist` (default "/ /tmp") according to `ps1_fslimits` (default "95 100")
* 1min cpu load (colorized default `ps1_loadlimits` "10 20")
* Available memory (colorized default `ps1_memlimits` "300 100" MB)
* Time
  
## choose your style
set `ps1_style` variable to available styles in your .nerdrc  
You can test using `ps1_style` function:

![image](https://github.com/joknarf/nerdps1/assets/10117818/f8d32297-73f4-4827-9802-b635c9d9a481)


## persistent prompt across sudo

Using functions `psudo` you can login to other users keeping your nerdps1 prompt, and even add your environment file to source after user profile is loaded.  
* `psudo` is reproducing full login of user according to its shell (/etc/profile .profile ...) and adds the nerdps1 and your optional custom env file
```shell
$ psudo user [myenvfile]
```
* psudo uses `sudo -u user` command  
The login shell will be the user shell (must be one of bash/ksh/zsh)


![image](https://user-images.githubusercontent.com/10117818/236661556-becd0184-4cb1-4b14-ab6c-5fc5c2f16f2e.png)

`psudo` can be used multiple times to forward the prompt/environment to users (psudo user1 followed by psudo user2...)

* psudo options:
  * `-b` : use bash shell login instead of sudo user shell
  * `-g` : don't load global profile (/etc/profile)
  * `-u` : don't load user profile  

## persistent prompt across ssh connection

Using functions pssh and psshu you can connect to remote servers with your nerdps1 prompt, and even add your local environment file to source after user profile.  
* `pssh/psshu` is reproducing full login of user according to its shell (/etc/profile .profile ...) and adds the nerdps1 and your optional custom env file

```shell
$ pssh [user@]remote [myenvfile] [ssh options]
$ psshu [user@]remote [myenvfile] [ssh options]
```
* pssh will use local nerdps1 to make a copy to remote.
* psshu will use `$ps1_url` to download nerdps1.  
Invocated shell is remote user shell (bash/ksh/zsh)

`pssh\psshu` can be used multiple times to forward prompt/environment (can be mixed with `psudo` too)

* pssh/psshu options:
  * `-b` : use bash shell login instead of remote user shell
  * `-g` : don't load global profile (/etc/profile)
  * `-u` : don't load user profile  

![image](https://user-images.githubusercontent.com/10117818/236662496-00aafc19-a253-4a2d-a356-df900b28324c.png)

* Overriding user login profile at ssh connexion with nerdps1 download
  
`$ ssh -t <remote> '. <(curl -s https://raw.githubusercontent.com/joknarf/nerdps1/main/nerdps1) login'`

or in .ssh/config:
```
    RequestTTY force
    RemoteCommand . <(curl -s https://raw.githubusercontent.com/joknarf/nerdps1/main/nerdps1) login
```

## using .nerdrc as your custom env file

Instead of passing custom env file, you can create a `~/.nerdrc` env file that will be automatically sourced after user profile and forwarded by psudo/pssh/psshu.  
You can put all ps1_ variables to override nerdps1 defaults, and all functions/path/env settings you want to have everywhere !  

## /tmp full proof

nerdps1 will use /var/tmp if not enough space in /tmp. (ssh connections can occur even if /tmp full on remote)

## Font rendering

If your terminal does not manage correctly nerd font symbols, you may switch to more commonly supported powerline font symbols, or even disable the segment separator symbols.  
You can use : `ps1_display` function/var to switch prompt display symbol characters:
```
$ ps1_display -h
usage: ps1_display <option>
    <option>: nerdicons, nerd, powerline, nofont, ascii
```

## Customizing prompt

You can add informations on the prompt using ps1_info variable:  
* `ps1_info="MYVAR MYVAR2..."` : will display content of variables
* `ps1_info="(myfunc) (myfunc2)"` : will display output of functions myfunc myfunc2

You can add custom colorized segment defining `ps1_addon()` function:
* `ps1_addon() { pgrep rsyslogd >/dev/null || echo 'red:syslog'; }`
output format of function:
  `<bgcolor>[/<fgcolor>/<sepcolor>]:<message>[|message]`
empty output discards the segment.

Changing prompt powerline, ps1_powerline variable represents the prompt:  
*  segment setting : `symbol/bgcolor/fgcolor/sepcolor:function`
    * function called is `ps1_function` (ps1_ prefixed)
    * colors : `black red green yellow blue magenta cyan white`, prefix `l` for light color
    * symbols : `< > ( )`
    * when color is set to auto, the function output must be `<color>:<text>` else only `<text>`
* right alignment separator : `|`
* `ps1_powerline="(/auto:exit_status (/blue:userhost )/auto:git_branch )/lblack:cwd > | (/lblue/black/blue:info (/auto:freemem (/blue:time )"`
![image](https://github.com/joknarf/nerdps1/assets/10117818/54fa6b66-03b5-4b04-8920-a461efcd9ca4)
* `ps1_powerline="auto:exit_status blue:userhost >/auto:git_branch >/lblack:cwd > | </lblue/black/blue:info </auto:freemem </blue:time"`
![image](https://github.com/joknarf/nerdps1/assets/10117818/f5d3bc4e-cf65-4889-af5b-817445575ef7)

## color theme example
used terminal colors in example:
```json
        {
            "name": "NerdPS1",
            "background": "#000000",
            "foreground": "#D3D7CF",
            "black": "#000000",
            "blue": "#2760AA",
            "cyan": "#06989A",
            "green": "#088A5B",
            "purple": "#66458d",
            "red": "#BA1611",
            "white": "#D3D7CF",
            "yellow": "#CF8700",
            "brightBlack": "#243C4F",
            "brightBlue": "#729FCF",
            "brightCyan": "#34E2E2",
            "brightGreen": "#59c566",
            "brightPurple": "#AD7FA8",
            "brightRed": "#EF2929",
            "brightWhite": "#EEEEEC",
            "brightYellow": "#FCE94F"
        }
```
