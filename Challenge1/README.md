# Challenge 1: Let’s do some revision

## Description
The Upside Down is leaking secrets.  
Before vanishing, Jim Hopper left a message for Eleven — but it’s fragmented across **four parts**.  

Use your **forensics skills** to retrieve all four parts and reconstruct the full flag.  
No Demogorgons — just your investigative grit.  
FYI: It's basic  
flag format: questCON{}

## Attachments
- [challenge1.zip](challenge1.zip)

## Flag
`questCON{K33p_0n_Gr0w1ng_Up_K1d_1L0v3Y0u}`  

## Author  
Crazy8(Rudraksh)

## How To Solve?  

Based on the challenge title and description, this challenge focuses on **basic digital forensics techniques**.    
The description mentions that the flag is **fragmented into 4 parts**, indicating that each part must be recovered separately.  

After extracting `challenge1.zip`, four directories are observed:  
1/  
2/  
3/  
4/  
Each directory contains one fragment of the final flag.  

### 📁 Part 1 – PDF Analysis

#### Files Observed
- A PDF file containing:
  - A poster
  - A report
  - Multiple fake flags

At first glance, no valid flag is visible.

#### Investigation
- Metadata analysis → **No flag found**
- Content review → A hint at the end:
  > **“Go find somewhere else”**

This suggests hidden or embedded data.

#### Technique Used
- **Binwalk** on the PDF file
```bash  
binwalk Research.pdf
binwalk -e Research.pdf
```

#### Result
Binwalk reveals an embedded file: part1.txt  
After extraction, the **first fragment of the flag** is obtained.  
K33p_0n_  

### 📁 Part 2 – Disk Image Analysis

#### Files Observed
- A disk image file

#### Technique Used
- **Strings analysis** on the disk image

#### Result
Running `strings` reveals readable text containing the **second fragment of the flag**.  
Gr0w1ng_  

This confirms the challenge is intentionally kept simple.  

### 📁 Part 3 – Image Steganography

#### Files Observed
- A `.jpg` image file

####Initial Checks
- Metadata analysis:
  - No flag present
  - User comment:  
    > **“Nice try”**

#### Steganography Attempt
Using `steghide` to extract hidden data.

Based on the image context, possible passphrases tested:
- `Eleven`
- `Hawkins`
- `Jim`

❌ All failed.

#### Key Insight
The challenge description states:  
> **FYI: it’s basic**

Trying the passphrase:  

#### Result
🎉 Steghide successfully extracts the hidden data, revealing the **third fragment of the flag**.  
Up_K1d_  

### 📁 Part 4 – Git Forensics

#### Files Observed
- Multiple HTML files containing messages like:
  > **“Can you see what’s hidden?”**

#### Investigation
Looking for hidden content reveals the presence of a **`.git` directory**.

#### Technique Used
- Git log analysis

#### Key Commit Found  
20070b65cbe0c3078631b002f37daeccc464d53c  
#### Result
Inspecting this commit’s changes reveals the **fourth and final fragment of the flag**.

---

### 🏁 Final Flag

After combining all four fragments:  
questCON{K33p_0n_Gr0w1ng_Up_K1d_1L0v3Y0u}  
