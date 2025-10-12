![Challenge description](https://github.com/user-attachments/assets/69428c7d-039d-45ca-8802-79e5aee65327)
Hint: I wonder why I encrypt "SKR{" the encrypted message (base64) is all A's?

Solution：  
First, I connected to skrctf.me 3013.
There are two options in the service - encrypt and decrypt.
![Menu options](https://github.com/user-attachments/assets/7177b0b6-1d6f-47c4-8ded-9693a893b05c)


To verify how it behaves, I tried both ecrypt and decrypt with the plaintext ‘SKR{ ’ and the ciphertext ‘AAAAAA==’
![Encrypt & decrypt test](https://github.com/user-attachments/assets/1f361d13-79f6-4609-956a-75055bc2f58f)
Since A in Base64 corresponds to value 0, this means the raw ciphertext bytes are all 0x00.
In Base64 encoding, the character A corresponds to the numeric value 0 (binary 000000).
When the encoded output is all A’s, it means the ciphertext bytes are all 0 (0x00).

Then, I asked ChatGPT what kind of encryption method this might be, and it told me it was XOR encryption

![ChatGPT explaining XOR](https://github.com/user-attachments/assets/1e34d89e-9711-4642-b045-e89fca486a3f)


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
![Decryption result](https://github.com/user-attachments/assets/51910f90-2db6-46a3-9a71-6f466e58976a)
After inserted many A’s. Finally, I got the flag.
