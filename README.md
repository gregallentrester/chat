
==__Script name:  bld__==
Perform a Maven build of the entire project (Server, Client, Proxy).
````
#!/bin/bash -p

clear
export PROMPT_COMMAND='echo -ne "\033]0;  bld  \007"'

args=("$@")
ELEMENTS=${#args[@]}

echo
echo
echo  Incoming ARG Count:  ${#args[@]}
echo
echo $@
echo
echo    ...

mvn clean install -B
````
&NewLine;
&NewLine;

==__Script name:  run__==
Launches the Server.
````
#!/bin/bash -p

clear
export PROMPT_COMMAND='echo -ne "\033]0;  run (server)  \007"'

java -jar ./target/chat.jar

````
&NewLine;
&NewLine;

==__Script name:  ask__==
Launches a Client w/ Proxy).
````
#!/bin/bash -p

cd ./target/classes

java net.greg.examples.chat.client.Client

cd -
````

&NewLine;
&NewLine;
==__Script name:  BASHRC.txt__==
Some nice ßash utilities.  Tested to work (typos can mess things up).
SO, Paste-and-test whatever looks useful.
````
#!/bin/bash -p

export PS1="\n\w "

# Catalina OS defaults to the zsh shell
export BASH_SILENCE_DEPRECATION_WARNING=1

function caption {
  export LBL=$1
  export PROMPT_COMMAND='echo -ne "\033]0;   $LBL   \007"'
}

function ask() {
  . ask
}
function ok() {
  . ok
}
function bld() {
  . bld
}
function run() {
  . run
}

kbyport() {
  PID=$(lsof -ti tcp:$1)

  echo;echo "killing " $PID

  if [[ $PID ]]; then
    kill -9 $PID
  fi
}

pid() {
  caption "pid | grep PIDs for an image $1"

  export token=$1
  export incominglen=$(echo "$1" | awk '{print length}')

  split=$((incominglen-1))

  first=${token:0:split}
  last=${token:split:1}

  composite=$first[$last]

  ps auxx | grep $composite
}

alias edt='rm ~/.bashrc.sw*; vi ~/.bashrc; cd -'
alias cls='clear'

alias hh='history'
alias nohistory='cat /dev/null > ~/.bash_history && history -c; rm ~/.bash_history '

function chrome() {
  open -a "/Applications/Google Chrome.app"  $@
}

lc() {
  ls -l | awk '{print $9}'
}

alias ll='ls -Aol'
alias lt='ls -Aolt'
alias llr='ls -AolR'
alias lll='ls -Aol|less'

alias cp='cp -R'

alias rm='rm -fr '
alias mv='mv -f'

alias desk='cd ~/Desktop;echo;echo;ls -Aol;echo;echo;pwd '
alias dsk='cd ~/Desktop;echo;echo;ls -Aol; echo;echo;pwd '
alias downloads='cd ~/Downloads;echo;echo;ls -Aol;echo;echo;pwd '

alias stage='clear; cd ~/stage; ls -Aol; echo; echo; pwd '
alias bb='clear; bash; clear'

alias ww='whoami'

# "$@" - expands to all parameters
# "$0" - expands to script name
ddemoshell() {

  if [ "$(id -u)" != "0" ]; then
    exec sudo "$0" "$@"
  fi
}

function checkfan() {
  SD=~/.spindump.txt;sudo rm $SD;sudo spindump 1 1 -file "$SD" ;grep "Fan speed" $SD
}

export IP="$(ipconfig getifaddr en0)"

foldersize() {
  clear

  ls -Aoli

  echo; echo

  du -sh *

  echo; pwd
  echo;echo '   ... '
  caption "$IP foldersize"
}

listopenports() {
  clear
  echo  netstat:
  echo; echo

  netstat -atp tcp | grep -i "listen"

  echo; echo
  echo   lsof:
  echo; echo

  lsof -i -P | grep -i "listen"
}

list() {
  clear; echo

  compgen -A function

  echo

  alias

  echo '          ... ... ... '; echo
  caption "$IP list "
}

cullany() {
  ps auxx | grep "$1" > "$1"

  while IFS=' ' read -r line || [[ -n "$line" ]]; do
    export PID=2
    PID="$(echo $line | cut -d " " -f $PID)"

    sudo kill -9 "${PID}"

  done < "$1"

  rm $1
}

tree() {

  SEDMAGIC='s;[^/]*/;|__;g;s;_|; |;g'

  if [ "$#" -gt 0 ] ; then
    dirlist="$@"
  else
    dirlist="."
  fi

  for x in $dirlist; do
    find "$x" -print | sed -e "$SEDMAGIC"
   done
}

dtree() {
  SEDMAGIC='s;[^/]*/;|__;g;s;_|; |;g'

  if [ "$#" -gt 0 ] ; then
    dirlist="$@"
  else
    dirlist="."
  fi

  for x in $dirlist; do
    find "$x" -type d -print | sed -e "$SEDMAGIC"
   done
}

echo $SHLVL

# http://linux.die.net/man/1/rsync
# http://www.tecmint.com/rsync-local-remote-file-synchronization-commands/

# executing - new shell process
# sourcing r- current shell process

# iTerm Arrangements can be saved:   Command + Shift + S
# http://chris-schmitz.com/develop-faster-with-iterm-profiles-and-window-arrangements/
# http://blog.andrewray.me/how-to-create-custom-iterm2-window-arrangments/

if [ -f /usr/local/etc/bash_completion ]; then
. /usr/local/etc/bash_completion
fi

 if [ -f $(brew --prefix)/etc/bash_completion ]; then
 . $(brew --prefix)/etc/bash_completion
 fi

# done

````
&NewLine;
&NewLine;




