Author: SyerexX Date: 18/10/2025 Category: Forensics
This challenge belongs to the Forensics category at an easy level; players are asked to find a flag hidden inside a disk image file.

After exploring and visiting https://play.picoctf.org/ I began to see the first challenges:
Steps I took before solving the challenge:
 * Downloaded VMWare and used a Linux VM (ParrotOS or Kali Linux).
 * Used the MATE Terminal on the VM to start solving the challenge.
To begin the journey I chose the Easy challenge called DISKO 1 by Darkraicg492:

### 1. File analysis:
 * The challenge gave me a disk image file to download named disko-1.dd.gz.
 * I immediately noticed this is a disk image file (.dd) compressed with gzip (.gz). To decompress, I opened a terminal and navigated to the folder containing the file with cd Downloads.
 * You can use the ls command to check whether the file you need is in that directory.

### 2. Decompression:
 * Use the gunzip command to decompress: for example gunzip disko-1.dd.gz. When complete, the machine will return the decompressed file, e.g. disko-1.dd (the .gz extension will be gone). You can use ls to verify.

### 3. Solving the challenge:
 * At this point I looked at the hint the author provided on PicoCTF: “Maybe Strings could help? If only there was a way to do that?” SO THAT MADE IT CLEAR.
 * I immediately used the strings command in the terminal specifically:
'strings disko-1.dd | grep pico'
Explanation of this command line:
 * strings: Use strings to scan the entire disko-1.dd file and extract all readable text strings.
 * grep: Use grep to filter and display only the lines containing the flag’s distinctive keyword pico.
 * | : Use the pipe | to pass all output from strings as input to grep.
The result returned many lines but the most notable was: picoCTF{1t5_ju5t_4_5tr1n9_c63b02ef} — this is the flag required by the challenge!

### 4. What I learned:
 * This challenge taught me more clearly how to use basic Linux commands like cd, ls, gunzip, strings, grep, and the pipe |.
 * By using strings and grep, we can quickly analyze a large binary file and find the needed information without complicated procedures.
