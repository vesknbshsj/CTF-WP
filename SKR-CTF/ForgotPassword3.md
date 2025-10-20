## Effective Summary


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


###Registering an Account
<p align="center">
First, I registered a new account on the website to see how it works. I used "test1" as username and "1" as the password.
<img src="https://github.com/user-attachments/assets/93388ec6-9373-41dd-84f6-bc2a02f3ed69" width="70%" alt="image" />
</p>



###Logging In and Observing the Website
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


