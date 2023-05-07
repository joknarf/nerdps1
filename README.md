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

Using functions psudo or psudosu you can login to other users keeping your nerdps1 prompt, and even add your environment file to source after user profile.
```shell
$ psudo user [myenvfile]
$ psudosu user [myenvfile]
```
* psudo uses sudo -u user command
* psudosu uses sudo su - user command
The login shell will be the user shell (must be bash/ksh/zsh)

## persistent prompt across ssh connection

Using functions pssh and psshu you can connect to remote servers with your nerdps1 prompt, and even add your local environment file to source after user profile.
```shell
$ pssh user@remote [myenvfile]
$ psshu user@remote [myenvfile]
```
* pssh will use local nerdps1 to make a copy to remote
* psshu will use `$ps1_url` to download nerdps1
