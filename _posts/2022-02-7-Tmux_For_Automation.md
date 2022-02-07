# Tmux For Automation

## Jai Shree Ram🚩
## Hello Everyone its me BoyFromFuture and in this blog I am going to show how can we use tmux for automation. But first why tmux?
- Why Tmux For Automation?
	- Tmux Provides detachable sessions
	- We can use tmux as a threading for our bash script which can save time
### First , We need to create detachable session in tmux so we can automate stuff in it
```bash
tmux new -s <session name> -d
```
For eg:- 
```bash
tmux new -s BOT -d
```
### You have to give your session a name for the ease of later commands
### We Can also list our detached session using
```bash
tmux ls
```
### When our detachable session is ready we can rename the window for ease with
```bash
tmux rename-window -t "<session name>.0" '<window name>'
```
For eg:- 
```bash
tmux rename-window -t "BOT.0" 'nmap scan'
```
### You have to provide the session name you used upper to create a detachable session and also give a unique name to the window 
### After renaming the session we can execute any command such as nmap , dirsearch , nikto etc.
```bash
tmux send-keys -t "${sess}.0" C-z '<your command here>' Enter
```
For eg:- 
```bash
tmux send-keys -t "BOT" C-z 'nmap -v -p- 192.168.1.1' Enter
```
### Now come the good part we can execute other command while our nmap command is running we need to first create a **new window**. We can use the command
```bash
tmux new-window -t "<session name>" -n '<new window name>'
```
For eg:- 
```bash
tmux new-window -t "BOT" -n 'FFUF'
```
### Now after creating new window the current selected window in tmux detach session is the new we created .We can execute our **ffuf** command meanwhile our **nmap** scap is running
```bash
tmux send-keys -t "<session name>" C-z '<command to run on new window>' Enter
```
For eg:- 
```bash
tmux send-keys -t "BOT" C-z 'ffuf -u http://192.168.1.1/FUZZ -w wordlist.txt -c' Enter
```
### We can repeat the upper proccess again and again for automation
### Finally Now all things are done we need to enter into our attached session. We can use the following command to **attach** our detached session
```bash
tmux attach -t "<session name>"
```
For eg:- 
```bash
tmux attach -t "BOT"
```
### and we are in our session and all the required command is executing in the session 
### I am also adding a sample bash script for tmux 
```bash
#!/bin/bash
# Author: BoyFromFuture
read -p "Specify Session name:-" sess
tmux new -s ${sess} -d
tmux rename-window -t "${sess}.0" 'Release Files'
tmux send-keys -t "${sess}" C-z 'cat /etc/*release' Enter
tmux new-window -t "${sess}" -n 'Passwd File'
tmux send-keys -t "${sess}.0" C-z 'cat /etc/passwd' Enter
tmux new-window -t "${sess}" -n 'Shadow File'
tmux send-keys -t "${sess}" C-z 'cat /etc/hosts' Enter
tmux -u attach -t "${sess}"
```
### If you have some issues regarding the blog you can ping me on twitter [root_tanishq](https://twitter.com/root_tanishq)
### See you in the next blog Byeeeeeeeeeeeeeeeeeeeeeeeeeee :D