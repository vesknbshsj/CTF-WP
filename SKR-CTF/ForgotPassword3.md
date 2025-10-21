## Forgot Password 3


This challenge was from SKR CTF under the Web category, worth 20 points, and marked as *Medium*. At the time of solving, 75 teams had already completed this challenge.
The goal was to retrieve a hidden secret from the dashboard after logging in as the user `godam`. 
Hints suggested that the login process used client-side MD5 encryption, and the password verification logic could be found within the JavaScript source code.



Although it was labeled as a medium-difficulty task, I personally found it quite challenging as a beginner. It took me around 2 hours to solve. 
Overall, it was a very educational challenge that helped me learn how to inspect front-end code and analyze how client-side authentication can be exploited.




Here is the challenge question:
<p align="center">
  <img src="https://github.com/user-attachments/assets/dfcc874f-9216-4233-b366-d9b145b4aaf7" width="70%" alt="Screenshot 2025-10-21 020630" />
</p>

Hint 1: Interesting.. it encrypts our password with MD5, more secure I guess..






Hint 2: Look carefully how the request redirect when you login



This is how the website looks like
<p align="center">
<img src="https://github.com/user-attachments/assets/736c8371-b1ae-4ad5-ac07-1a2bd93f52b7" width="70%" alt="image" />
</p>


### Registering an Account
<p align="center">
First, I registered a new account on the website to see how it works. I used "test1" as username and "1" as the password.
<img src="https://github.com/user-attachments/assets/93388ec6-9373-41dd-84f6-bc2a02f3ed69" width="70%" alt="image" />
</p>



### Logging In and Observing the Website
<p align="center">
<img src="https://github.com/user-attachments/assets/90bb2fc6-26e5-48a6-9cdf-a6cb938cde1f" width="70%" alt="image" />
</p>
After registering successfully, I logged in using my test account to see what features were available.  
The page displayed a simple dashboard with a single input field labeled **“Secret”**, along with navigation links to *Home*, *Profile*, and *Logout*.  

The "Profile" allows users to change their password.
<p align="center">
<img src="https://github.com/user-attachments/assets/292f48a5-ca76-40ae-b475-1dd0c5edf152" width="70%" alt="image" />
</p>

I tested the **“Secret”** input field by entering a simple value — `1234`.
<p align="center">
<img src="https://github.com/user-attachments/assets/122a9933-8ac3-4895-ac40-4ff5f2332551" width="70%" alt="image" />
</p>
To view the secret, we need to enter the correct password.
<p align="center">
<img src="https://github.com/user-attachments/assets/b4d02251-01e3-4e00-8563-415fdaaa603f" width="70%" alt="image" />
</p>
<p align="center">
<img src="https://github.com/user-attachments/assets/6b9ecc05-7ffc-4e28-9b65-bd40743a9b0a" width="70%" alt="image" />
</p>

After observing how the website operates, my objective became clear: I needed to access the `godam` account and use the correct password in order to retrieve the secret (which contains the flag). 

### Login Account
I re-logged in with my test account and opened the browser developer tools (F12) to observe what happens when the site processes credentials.  
Using the **Network**, **Elements**, and **Sources** tabs I monitored the outgoing requests, inspected the DOM and scripts, and searched for any client-side logic that manipulates the password input before submission. The DevTools observation confirmed that the password is hashed in the browser and sent as an MD5 value in the query string.
<p align="center">
<img src="https://github.com/user-attachments/assets/df0a7feb-30e9-4839-a9d8-2999779df535" width="70%" alt="image" />
</p>
<p align="center">
<img src="https://github.com/user-attachments/assets/d48af39d-4528-4ab1-b9b0-2cc8c9163d46" width="70%" alt="image" />
</p>

**Following the hint:** "Look carefully how the request redirect when you login."  
At first I didn't notice anything unusual. After repeating the login flow with DevTools open and carefully inspecting the Network panel, I observed a request to `session.php?id=7`. This indicated a session identifier in the request URL — I interpreted this as the session index and realized I was the 7th session/user.
<p align="center">
<img src="https://github.com/user-attachments/assets/4c46cc81-f871-4c3b-aedb-c03c3e503fc2" width="70%" alt="image" />
</p>
Based on this, I guessed that I can use the session id to login others account and find out godam account.


`https://skrctf.me/ports/a0dab56383431ce3e35eb2147db81fbf/session.php?id=1`

I tested the idea by directly requesting `session.php?id=1`. It worked — the page loaded as the user `michelle`, confirming that supplying different `id` values effectively switched the active session/user.  
<p align="center">
<img src="https://github.com/user-attachments/assets/10df481e-7bab-4ab0-94aa-b2857b1bdde0" width="70%" alt="image" />
</p>
Having confirmed this, I tester other session ids until I located the `godam` account.
After a few attemps, I found that 'godam' account is located at 'id=5'
<p align="center">
<img src="https://github.com/user-attachments/assets/f41e5d83-12c3-4646-950f-62d28b007c8c" width="70%" alt="image" />
</p>
Now, I successfully logged into 'godam' account.

### View the secret
After logged in, the final step is to insert the correct password and view the secret.
At this point, I got stuck for quite a while.  
#### Stuck on getting the password
So, I tried to inspect all the things and I didn't find anything related to the password. The only thing that I know was the password was using 'MD5 Hashing'.
I also noticed that the system allowed duplicate usernames and even duplicate passwords.  
At first, I thought this might be an important clue — perhaps related to overwriting or bypassing existing accounts.  
So, I tried registering another account using the username `godam`, hoping it would override the real one.  
However, it turned out to be a false lead: the duplicated registration was accepted, but it didn’t affect the original `godam` account or provide any advantage.  
This confirmed that the vulnerability wasn’t related to the registration logic.
<p align="center">
<img src="https://github.com/user-attachments/assets/b7836589-1562-448f-aa9b-26ce6e5e71a3" width="70%" alt="image" />
</p>
<p align="center">
<img src="https://github.com/user-attachments/assets/80f70b43-ddd2-49f3-99a2-41c0ea084770" width="70%" alt="image" />
</p>

#### Discovering the checkPassword() Function in Profile Page
After spending some time exploring different parts of the web application,  
I finally found the `checkPassword()` function inside the profile page’s source code.  
This function handled the password change logic — it compared the MD5 hash of the input password against a fixed hash value (`8d1beddb87179b224441ce3a7a66c6a9`).  
That immediately caught my attention, since this meant the real password verification was happening **entirely on the client side** using JavaScript.  
If we could figure out the plain text that produced this MD5 hash, we could directly use it to unlock the secret.
<p align="center">
<img src="https://github.com/user-attachments/assets/8f59a230-5b5c-48ea-8501-152221537b98" width="70%" alt="image" />
</p>

I extracted the hash `8d1beddb87179b224441ce3a7a66c6a9` from the profile page and tried to reverse it using several online MD5 cracking services (for example: md5hashing.net, CrackStation, md5decrypt.net). None of them returned a plaintext result.  
This indicated that the password was not a common string listed in public databases — it was unlikely to be recovered by simple online lookup and would require heavier cracking effort (dictionary/wordlist + rules or targeted guessing) if attempted.
<p align="center">
<img src="https://github.com/user-attachments/assets/04820538-a2af-4377-b557-8d824c4e817a" width="70%" alt="image" />
</p>

Then, I tried submit the extracted MD5 value directly as the `password` parameter:
`https://.../index.php?password=8d1beddb87179b224441ce3a7a66c6a9`. And finally, I got the flag.
<p align="center">
<img src="https://github.com/user-attachments/assets/ebd80618-0408-4833-a760-38ce9d3102a1" width="70%" alt="image" />
</p>
This worked because the application expects the client to send the MD5 digest of the password (the page hashes the plaintext on submit). By supplying the correct digest we effectively bypassed the need to know the original plaintext password — the server accepted the hash as the authentication token and returned the secret.

### Lesson Learned
- Never trust client-side code for authentication; always validate on the server.  
- Do not expose password hashes or security logic in public JavaScript.  
- Use salted, strong password hashing (bcrypt/argon2) instead of MD5.  
- Use POST requests for credentials and avoid sending secrets in URLs.  
- Secure session management is essential; prevent predictable session IDs.  
- Add rate limiting and monitor abnormal login attempts.
