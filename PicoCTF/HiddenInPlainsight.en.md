Author: SyerexX Date: 18/10/2025 Category: Forensics
This challenge belongs to the Forensics category at an easy level; it requires players to search for a flag hidden inside a `.jpg` image file.
Today I will **NOT** use a virtual machine to solve the challenge anymore (not recommended because solving CTF challenges essentially downloads corrupted files or viruses to the machine — this is quite dangerous).
Steps I performed before solving the challenge:
* I opened Command Prompt with Admin rights then entered the command `wsl --install`. When the command finishes, you can use the Microsoft Store to install Ubuntu or Kali Linux and use it as the VM-like terminal as usual!
* Today we will solve another Easy challenge named **Hidden in plainsight** by Yahaya Meddy.

### 1. File analysis:
* The challenge gave me an image file to download named **img.jpg**.
* When you open the file it is just a normal picture like any other.
* In the hint the author provided: “Download the jpg image and read its metadata” — based on this hint, the first step is to check the image file’s metadata.

### 2. Solving the challenge:
* I solved it by using an image metadata viewer tool like `exiftool`:
  * To view metadata with `exiftool` first install it with two commands:
    ```
    sudo apt update
    sudo apt install libimage-exiftool-perl -y
    ```
  * After installation, use:
    ```
    exiftool img.jpg
    ```
  * In the metadata output I found a strange string in Base64 format:
    `c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9`
* After obtaining the encoded string, I used `echo` to decode the first layer:
  ```
  echo c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9 | base64 -d
  ```
  The result is: `steghide:cEF6endvcmQ=`
  * Explanation:
    * `echo`: prints (or repeats) whatever you give it.
    * `|` (pipe): passes the result of the previous command as input to the next command.
    * `base64 -d`: decodes Base64, i.e., “take this Base64-encoded text and translate it back into the original data.”
* Decode the second layer to find the password:
  ```
  echo cEF6endvcmQ= | base64 -d
  ```
  Result: `pAzzword` — this is the password we needed to find.

### 3. Extract the hidden file:
* Use the `steghide` tool with the found password to extract the hidden data from the original image:
  ```
  steghide extract -sf img.jpg
  ```
* When asked “Enter passphrase:” enter the password: `pAzzword`

### 4. Result:
* `steghide` creates a new text file, usually `hidden.txt` or `flag.txt`.
* Use the `cat` command to read the file contents and find the flag:
  ```
  cat flag.txt
  ```
* The final result is: `picoCTF{h1dd3n_1n_1m4g3_1c55ccd0}`

### 5. What I learned:
* This challenge helped me learn how to download and use specialized tools for a specific purpose (for example `exiftool` and `steghide`), understand the function of `echo`, and review the use of the pipe `|`.
* It is also a good example of multi-layer hiding (multi-layer): it not only requires knowing one tool but also chaining clues and decoding through multiple steps to find the final key.
