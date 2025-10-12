<img width="230" height="125" alt="image" src="https://github.com/user-attachments/assets/5968ceb1-6747-4907-9efc-aecdc79ee09c" />


Hint: I wonder why I encrypt "SKR{" the encrypted message (base64) is all A's?

Solution：  
First, I connected to skrctf.me 3013.
There are two options in the service - encrypt and decrypt.
<img width="236" height="53" alt="image" src="https://github.com/user-attachments/assets/00a8511c-ae5b-4032-b694-b90041b55c76" />




To verify how it behaves, I tried both ecrypt and decrypt with the plaintext ‘SKR{ ’ and the ciphertext ‘AAAAAA==’


<img width="329" height="62" alt="image" src="https://github.com/user-attachments/assets/54832620-9fe2-4c8f-a346-6a0aacab641b" />


Since A in Base64 corresponds to value 0, this means the raw ciphertext bytes are all 0x00.
In Base64 encoding, the character A corresponds to the numeric value 0 (binary 000000).
When the encoded output is all A’s, it means the ciphertext bytes are all 0 (0x00).

Then, I asked ChatGPT what kind of encryption method this might be, and it told me it was XOR encryption


<img width="301" height="114" alt="image" src="https://github.com/user-attachments/assets/61d7aa8e-e11f-4709-a74d-94f193b3692c" />




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


<img width="301" height="141" alt="image" src="https://github.com/user-attachments/assets/5c7e61d2-c534-4f4b-bf3f-92d7f861fbbe" />


After inserted many A’s. Finally, I got the flag.
