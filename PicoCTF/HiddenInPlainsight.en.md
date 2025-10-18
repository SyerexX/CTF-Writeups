Author: SyerexX Date: 18/10/2025 Category: Forensics
This challenge belongs to the Forensics category at an easy level; players are asked to find a flag hidden inside a `.jpg` image file.

Today I will **NOT** use a virtual machine to solve the challenge (not recommended — CTF files can be corrupted or contain malware, which is risky).
Steps I took before solving the challenge:
* I opened Command Prompt as Administrator, then ran `wsl --install`. After that completes you can use the Microsoft Store to install Ubuntu or Kali Linux and use it as the VM-like terminal.
* Today we’ll solve another Easy challenge called **Hidden in plainsight** by Yahaya Meddy.

### 1. File analysis

* The challenge provided an image file named **img.jpg** to download.
* When you open the file it looks like a normal picture.
* The hint from the author was: “Download the jpg image and read its metadata.” Based on that hint, the first step is to check the image file’s metadata.

### 2. Solving the challenge

* I solved it using an image metadata tool like `exiftool`:

  * To install `exiftool` first run:

    ```
    sudo apt update
    sudo apt install libimage-exiftool-perl -y
    ```
  * Then view the image metadata with:

    ```
    exiftool img.jpg
    ```
  * In the metadata output I found a weird string that looked like Base64:
    `c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9`
* With that Base64 string in hand I decoded the first layer using `echo` and `base64 -d`:

  ```
  echo c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9 | base64 -d
  ```

  This produced: `steghide:cEF6endvcmQ=`

  * Explanation:

    * `echo`: prints the given text to stdout.
    * `|` (pipe): passes the output of the left command as input to the right command.
    * `base64 -d`: decodes the Base64-encoded input to its original data.
* Decode the second layer to find the password:
  ```
  echo cEF6endvcmQ= | base64 -d
  ```
  This yields: `pAzzword` — that is the password we needed.

### 3. Extracting the hidden file
* Use the `steghide` tool with the recovered password to extract the hidden data from the image:
  ```
  steghide extract -sf img.jpg
  ```
* When prompted for the passphrase, enter: `pAzzword`

### 4. Result
* `steghide` produces a new text file, usually `hidden.txt` or `flag.txt`.
* Read the file with:
  ```
  cat flag.txt
  ```
* The final flag is: `picoCTF{h1dd3n_1n_1m4g3_1c55ccd0}`

### 5. What I learned
* This challenge taught me how to install and use specialized tools for a task (for example `exiftool` and `steghide`), reinforced the function of `echo`, and refreshed how to use the pipe `|`.
* It’s also a great example of multi-layer hiding (multi-layer steganography). It required chaining tools and decoding steps — not just knowing one tool, but following multiple clues and decoding stages to reach the final key.
