# GhostPixel – Image Steganography with AES Encryption

**GhostPixel** is a straightforward Python tool that combines **text encryption** and **image steganography** to hide messages (such as passwords, notes, or reminders) inside ordinary photos.

The message is first encrypted using a user-provided master password, then secretly embedded into the least significant bits (LSB) of an image's pixels. The result looks like a normal picture, but contains your hidden (and encrypted) information.

### How It Works – Step by Step

1. **You enter a message**  
   Example: your banking password or a Wi-Fi key

2. **You provide a master password**  
   This password is never stored — it's only used temporarily to derive an encryption key.

3. **Encryption happens**  
   - A random 16-byte **salt** is generated  
   - A 32-byte AES-256 key is derived from your master password + salt using PBKDF2 (SHA256, 60,000 iterations)  
   - The message is encrypted using **AES-256-GCM** (authenticated encryption)  
   - A random 12-byte nonce is created for each encryption  
   - Salt + nonce + ciphertext are base64-encoded and joined with a separator

4. **The encrypted string is hidden in the image**  
   - You select any PNG, JPG or JPEG image  
   - The program converts the encrypted text into bits  
   - It modifies only the **least significant bit** (LSB) of each RGB channel in the first few pixels  
   - On average: ~3 bits of data per pixel → a 100-character message needs roughly 270–300 pixels  
   - The rest of the image remains unchanged

5. **Modified image is saved**  
   Always saved as PNG (lossless format) so the tiny changes are preserved

6. **To retrieve the message later**  
   - Select the modified image  
   - Enter the **same master password**  
   - The program reads LSBs from all pixels until it finds a complete encrypted string  
   - Decrypts it using the derived key  
   - Shows the original message (or reports wrong password / invalid data)

### Features
- AES-256-GCM encryption (confidentiality + integrity)
- PBKDF2 key derivation with per-message random salt
- LSB steganography (very simple, but effective for casual hiding)
- Cross-platform file selection via Tkinter dialogs
- Console menu with splash screen
- Automatic PNG output to avoid quality loss

### 📂 Project Structure
GhostPixel <br> <br>
├── main.py # Main menu + program flow <br>
├── encryption.py # AES-GCM encryption, decryption, key derivation <br>
├── hideNseek.py # Image loading, LSB hiding & extraction <br>
├── .gitignore <br>
└── README.md <br>

---

### 🔐 Security & Realism Notes

- Suitable for **personal / low-risk secrets**  
  (protecting from casual snooping: family, roommates, friends)

- **Not undetectable** — statistical analysis of LSBs can reveal modifications if someone is actively looking.

- **Not recommended** for valuable accounts, financial data, or high-threat scenarios.

- Instead, use established password managers like:
  - Bitwarden
  - 1Password
  - KeePassXC

---

### ⚠️ Known Limitations

- Reads LSBs from the **entire image during extraction**  
  (works, but inefficient on very large photos)

- **No length prefix** → relies on detecting the end of valid encrypted data

---

### 🚀 Possible Future Improvements

- Add **32-bit length prefix** at the start (read only needed pixels)

- Replace **console menu with a simple GUI**

- Use a **random starting pixel position** instead of always beginning from the first pixel

---


### 📚 Learning Goals

This project was built as a **learning exercise** to explore:

- Object-Oriented Programming in **Python**
- Basic **Applied Cryptography**
- Image manipulation using **Pillow**
- File selection dialogs
- **Modular project structure**

---

### 🖼️🔒 Happy (and careful) hiding!
