# Challenge 2: The Upside Down Packet Mystery

## Description  

A mysterious network capture has been recovered, containing hidden fragments left behind by the Hawkins Lab.  
Your mission: dive into the network traffic and uncover the hidden message before it vanishes completely.

## Attachments

- [challenge.pcapng](challenge.pcapng)

## Flag
questCON{Fr13nds_D0nt_l13}  

## Author
Crazy8(Rudraksh)

## How To Solve?
This challenge involves **network traffic analysis** using a provided `.pcapng` file.  
The objective is to identify fragmented data transmitted over the network, reconstruct the original file, and decrypt the hidden content to recover the flag.

---

### 📦 Initial Analysis

#### File Provided
- `challenge2.pcapng`

Opening the capture in **Wireshark** reveals multiple:
- HTTP
- TCP
- DNS
- ICMP packets

To reduce noise, an **HTTP display filter** is applied.  
http  
---

### 🧩 Fragment Identification

Upon filtering HTTP packets, **10 packets** are observed where the payload starts with the string: FRAG  

These correspond to the fragments mentioned in the challenge description.

#### Key Observation
Inspecting the first fragment reveals that the fragments together form a **PNG file**.

---

### 🧪 File Reconstruction

### Steps
1. Extract the payload from each fragment
2. Save them as `.bin` files
3. Reassemble them using a Python script

### Result
The fragments are successfully reassembled into an image file.

Inside the image, the string: Unpr3d1ct4bl3K3y  

is found.

⚠️ This **looks like a flag**, but it is actually an **encryption key**.

---

### 🔐 Encrypted Data Discovery

Further inspection of packet data reveals the keyword: Salted  

This strongly indicates **OpenSSL salted encryption**.

---

### 🛠️ Payload Extraction

#### Extract packet payload (hex → binary)

```bash
tshark -r challenge2.pcapng -Y "frame.number==1" -T fields -e data | tr -d '\n' > pkt1.hex
xxd -r -p pkt1.hex > pkt1.bin
```
#### Remove HTTP headers and extract encrypted content
```python
b = open("pkt1.bin", "rb").read()

# Split at the first double CRLF (end of HTTP headers)
sep = b"\r\n\r\n"
payload = b.split(sep, 1)[1] if sep in b else b

open("secret.enc", "wb").write(payload)
print(f"[+] Extracted secret.enc ({len(payload)} bytes)")
```

This produces the encrypted file: secret.enc  

### 🔓 Decryption

Using the previously recovered key, the encrypted file can be decrypted using OpenSSL:  
```bash
openssl enc -d -aes-256-cbc -pbkdf2 \
-in secret.enc \
-out secret.txt \
-pass pass:"Unpr3d1ct4bl3K3y"
```

### 🏁 Final Flag

The decrypted file reveals the final flag:

questCON{Fr13nds_D0nt_l13}
