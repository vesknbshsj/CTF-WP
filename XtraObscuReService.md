<img width="159" height="129" alt="image" src="https://github.com/user-attachments/assets/cf570113-db3f-436f-8a31-94a84e3facf9" />

Hint: I wonder why I encrypt "SKR{" the encrypted message (base64) is all A's?

Solution：  
First, I connected to skrctf.me 3013.
There are two options in the service - encrypt and decrypt.
<img width="118" height="27" alt="image" src="https://github.com/user-attachments/assets/86de78b6-2618-4da3-baa5-6f25d621da70" />



To verify how it behaves, I tried both ecrypt and decrypt with the plaintext ‘SKR{ ’ and the ciphertext ‘AAAAAA==’
<img width="165" height="31" alt="image" src="https://github.com/user-attachments/assets/8a0ac901-7dcb-4a40-bd73-3d7056b3cf2d" />

Since A in Base64 corresponds to value 0, this means the raw ciphertext bytes are all 0x00.
In Base64 encoding, the character A corresponds to the numeric value 0 (binary 000000).
When the encoded output is all A’s, it means the ciphertext bytes are all 0 (0x00).

Then, I asked ChatGPT what kind of encryption method this might be, and it told me it was XOR encryption

<img width="151" height="57" alt="image" src="https://github.com/user-attachments/assets/15cd146f-9076-468e-b732-c0ce8f5ef4c6" />



How XOR works?
Formula: cipher_byte = plaintext_byte XOR key_byte
XOR means: same bits = 0, different bits = 1.
In this challenge, the key happens to be the same as the plaintext (SKR{), so:
0x53 XOR 0x53 = 0x00  
0x4B XOR 0x4B = 0x00  
0x52 XOR 0x52 = 0x00  
0x7B XOR 0x7B = 0x00  


So, based on the output, we know that the flag_byte = key_byte.
That’s why when we encrypt ‘SKR{’ will get ‘AAAAAA==’
Then, I started to decrypt with A’s
<img width="151" height="71" alt="image" src="https://github.com/user-attachments/assets/5dbeb7b1-8a41-4f89-b62d-0c27691fa5d2" />

After inserted many A’s. Finally, I got the flag.
