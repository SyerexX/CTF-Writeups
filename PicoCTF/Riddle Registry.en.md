### **Riddle Registry by Prince Niyonshuti N.**

* **Author:** SyerexX
* **Date:** 19/10/2025
* **Category:** Forensics

After conquering the previous challenges, I continued with another "Easy" one in the Forensics category.
To my surprise, I found out that the author of this challenge was **Prince Niyonshuti N.**, the same person who created the rather tricky “M1n10n'5_53cr37.”
That immediately got me excited to see what kind of “trap” this challenge might contain.

---

### **1. Analysis & Reconnaissance**

The challenge provided a single file: `confidential.pdf`.

True to its name “confidential,” when opening the PDF, its contents appeared messy and meaningless.
That was a clear sign that the real information wasn’t on the surface — and the author’s hint confirmed it!

For any Forensics challenge involving files, my first instinct is always to check the **metadata** — the hidden information attached to the file.

---

### **2. Searching for Clues**

I used the familiar `exiftool` utility to “inspect” the PDF file.

```bash
# Move to the directory containing the file
cd /mnt/c/Users/Admin/Downloads

# Use exiftool to read metadata
exiftool confidential.pdf
```

The result returned a list of the file’s metadata.
And immediately, one line stood out:

```
Author : cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV8wZTJkZTVhMX0=
```

The `Author` field contained a long string.
Based on the familiar pattern (uppercase and lowercase letters, numbers, and ending with `=`), I instantly recognized it as **Base64** encoding.

---

### **3. Decoding & Result**

Without hesitation, I used the `echo` and `pipe` commands to decode the Base64 string.

```bash
echo 'cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV8wZTJkZTVhMX0=' | base64 -d
```

And the final flag appeared neatly:

**`picoCTF{puzzl3d_m3tadata_f0und!_0e2de5a1}`**

---

### **4. Lessons Learned**

* **The Power of Reflexes:**
  This challenge showed that with experience, you can develop a kind of “conditioned reflex.”
  `File -> Metadata -> Strange string -> Base64 -> Decode.`
  The entire process happened quickly and naturally.

* **`exiftool` is your best friend:**
  Once again, this confirmed that `exiftool` is an indispensable tool in any Forensics toolkit.

* **Confidence:**
  The best part was being able to apply the skills I had learned from previous challenges to solve this one smoothly — without relying on any hints.
  I was genuinely happy to feel my own progress so clearly.
