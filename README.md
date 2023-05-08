# nerdps1
Nerd prompt for bash/ksh/zsh

## activate the prompt

Install Nerd font on your system/console (Windows console / Windows terminal / putty / git-bash / CmdEr / iTerm2 / Terminator...):  
[Consolas NF](https://github.com/wclr/my-nerd-fonts/raw/master/Consolas%20NF/Consolas%20Nerd%20Font%20Complete%20Mono%20Windows%20Compatible.ttf)  
[Nerd Fonts](https://www.nerdfonts.com/)

on Unix, copy to `~/.fonts` and run `fc-cache -fv` then relaunch your terminal and set the font

Then activate the nerdps1 prompt:
```shell
$ . <(curl -s https://raw.githubusercontent.com/joknarf/nerdps1/main/nerdps1)
or
$ . <(curl -sL https://shorturl.at/cjtzE)
```

<img width="804" alt="image" src="https://user-images.githubusercontent.com/10117818/236626851-eb236c7d-0756-48c6-b2f2-cb42de60b398.png">

## persistent prompt across sudo

Using functions psudo you can login to other users keeping your nerdps1 prompt, and even add your environment file to source after user profile.
```shell
$ psudo user [myenvfile]
```
* psudo uses `sudo -u user` command
The login shell will be the user shell (must be one of bash/ksh/zsh)

![image](https://user-images.githubusercontent.com/10117818/236661556-becd0184-4cb1-4b14-ab6c-5fc5c2f16f2e.png)

## persistent prompt across ssh connection

Using functions pssh and psshu you can connect to remote servers with your nerdps1 prompt, and even add your local environment file to source after user profile.
```shell
$ pssh user@remote [myenvfile]
$ psshu user@remote [myenvfile]
```
* pssh will use local nerdps1 to make a copy to remote, shell is remote user shell
* psshu will use `$ps1_url` to download nerdps1. The remote shell is bash (not necessarily the remote user shell)

![image](https://user-images.githubusercontent.com/10117818/236662496-00aafc19-a253-4a2d-a356-df900b28324c.png)