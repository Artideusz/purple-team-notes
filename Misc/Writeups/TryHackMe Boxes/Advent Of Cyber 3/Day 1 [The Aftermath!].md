# The Aftermath!

## Challenge
In this challenge, we have to leverage an IDOR vulnerability. The site has 4 tabs:
- Completed Orders
- Builds
- Inventory
- Your Activity

In the "Your Activity" tab, we can see an interesting query parameter in the URL of the virtual browser, "user_id=11". By changing the value of the parameter, we are able to view information, that possibly should not be accessible by the user.

### Solutions

#### 1. After finding Santa's account, what is their position in the company?
To find Santa's account, we have to "brute-force" the value of the `user_id` parameter. We can do that by starting from value 0, and extracting information for every value, in case we need information about different accounts in later challenges.

```
ID: 0 (N/A)

ID: 1
Name: Santa
Position: The Boss!

ID: 2 (N/A)

ID: 3
Name: McStocker
Position: Build Manager

ID: 4-8 (N/A)

ID: 9
Name: Grinch
Position: Mischief Manager

ID: 10 (N/A)

ID: 11 (Main account)
Name: McSkidy
Position: Jnr Toy Tester
```

From the list above, we now know that the answer for the question is "The Boss!".

#### 2. After finding McStocker's account, what is their position in the company?
We brute-forced information from the accounts in the previous question, so we know that McStocker's position is "Build Manager".

#### 3. After finding the account responsible for tampering, what is their position in the company?
We can easily assume that the question talks about the "Grinch" user, so the answer should be "Mischief Manager".

#### 4. What is the recieved flag when McSkidy fixes the Inventory Management System?
To fix the IMS, we have to revert the orders that were made by Grinch. After reverting all the orders, we get the flag.