<img width="459" height="429" alt="image" src="https://github.com/user-attachments/assets/5453871e-7c1e-4ec9-9e31-199638645976" />



Hint: I wonder why I encrypt "SKR{" the encrypted message (base64) is all A's?

Solution：  
First, I connected to skrctf.me 3013.
There are two options in the service - encrypt and decrypt.
<img width="472" height="105" alt="image" src="https://github.com/user-attachments/assets/07342b6e-c54b-486f-81b4-19f058b1be27" />







To verify how it behaves, I tried both ecrypt and decrypt with the plaintext ‘SKR{ ’ and the ciphertext ‘AAAAAA==’


<img width="658" height="124" alt="image" src="https://github.com/user-attachments/assets/c112945a-b773-4b3e-842a-8d814df85356" />









Since A in Base64 corresponds to value 0, this means the raw ciphertext bytes are all 0x00.
In Base64 encoding, the character A corresponds to the numeric value 0 (binary 000000).
When the encoded output is all A’s, it means the ciphertext bytes are all 0 (0x00).

Then, I asked ChatGPT what kind of encryption method this might be, and it told me it was XOR encryption


<img width="602" height="228" alt="image" src="https://github.com/user-attachments/assets/a75a22ad-254c-492e-9918-c3577dc1c295" />












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


<img width="602" height="282" alt="image" src="https://github.com/user-attachments/assets/ceff8525-a66f-413a-a654-289d1721d425" />




After inserted many A’s，I finally got the flag.
