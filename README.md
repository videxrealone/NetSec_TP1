# Network Security - TP 1

## Introduction

![image](https://user-images.githubusercontent.com/91763346/196042993-c25e5244-fafb-422c-92eb-9a92a66df4c4.png)

* **John the Ripper**

One of the best security tools which can be used to crack passwords is John the Ripper.

Originally, “Cracker Jack” was developed for the sake of cracking Unix /etc/passwd files with the help of a dictionary. Then, John the Ripper came into existence afterward. Moreover, a “Pro” version was developed to include more features than the ordinary version. Especially that it has the capability to include and deal with many more hash types on which encrypted passwords are based in the first place. The Rapper’s commercial version is the most used among penetration testers for cracking passwords. This is essentially because of both its speed and great performance.

* **How does John the Ripper crack passwords?**

This field of science is basically perceived as cryptanalysis. In fact, there exist some vulnerabilities in passwords, which opens the gate for hackers to exploit in order to get the password back from its encrypted format following the use of a hashing method.

One of the most common methods to crack passwords is brute-force attacks.It is simply a method which mainly depends on performing a cross-checking against a cryptographic hash which is available for the password.
  
  . It is simply a method which mainly depends on performing a cross-checking against a cryptographic hash which is available for the password.
  . On the other hand, a password could be recovered through what is called ‘rainbow’ table. It is much faster and contains password hashes from which a password is guessed by a computer system.
  
* **Common types of attacks used by JTR**
  There are essentially two main types of attacks harnessed by John the Ripper in order for it to crack any password.
  
    1.Dictionary Attack
    
      a. String samples are essentially taken from a specific wordlist, text file, a dictionary or past cracked passwords.
      b. They are then encrypted identically to the method, key, and algorithm in which the desired password was encrypted originally.
      c. Dictionary words could also be altered in a randomized manner to check if they work this way
      d. Single attack mode of John the Ripper can do such alterations. Accordingly, different hashes’ variations are compared when using different alterations.
      
    2.Brute Force Attack
    
      a. All possible plaintexts composed of usernames with encrypted passwords are all exhausted to find the right one.
      b. They are all hashed and compared to the originally inputted hash.
      c. Character frequency tables are used by the program for the sake of including the most probable used characters first.
      d. This method is so slow, yet it could identify those passwords having no existence in a dictionary.
      
## Cracking linux user passwords using JTR

* **Creating users**

We will start by creating new users with different password complexity.

  user 1:
    mohamed:mohamed
  user 2:
    ali:isetcom
  user 3:
    asma:J&s#ecds1
    
The command we'll be using is ***useradd <name>***
    
```
$ sudo useradd mohamed
$ sudo passwd mohamed
$ Changing password for user mohamed.
$ New password:
$ Retype new password:
$ passwd: all authentication tokens updated successfully.
```
```
$ sudo useradd ali
$ sudo passwd ali
$ Changing password for user ali.
$ New password:
$ Retype new password:
$ passwd: all authentication tokens updated successfully.
```
```
$ sudo useradd asma
$ sudo passwd asma
$ Changing password for user asma.
$ New password:
$ Retype new password:
$ passwd: all authentication tokens updated successfully.
```
The file "/etc/passwd" contains the names of the users (aka logins) and the file "/etc/shadow" contains the ***hashed*** passwords.

![image](https://user-images.githubusercontent.com/91763346/196044274-3113d408-3390-422c-9ea5-6a1976b79807.png)

it's important to note the we need to assemble those 2 files to have 1 file that has the logins and hashes and to also remove duplicates, and overall optimize the process when using a dictionary attack.

the command to do combine those 2 files is ***unshadow***

```
$ unshadow /etc/passwd /etc/shadow > ./Security/TP1_JTR/mypasswd
```
The content of the unshadow file has a significant byte it's the ***$y$***.
after googling it seems to be using the yescrypt hashing algorithm.

![image](https://user-images.githubusercontent.com/91763346/196044878-5e624c1a-ffb8-4fd8-82bc-2a3572536879.png)

To make things easier we proceed to add the --format=yescrypt to make things easier for john to identify the hash and crack it.

Next we will attempt to run John The Ripper using the command ***john [OPTIONS] <file> [WORDLIST]***

![image](https://user-images.githubusercontent.com/91763346/196045018-0a40ce04-f498-4da8-b145-814072cc1646.png)

We can also add the --show to force JTR to immediatly show results.

* **Final command with  optimized parameters**

```
$ john mypasswd --show --format=crypt /usr/share/wordlists/rockyou.txt
```
After a while we get the following output

```
$ mohamed:mohamed
```
4.Why couldn't JTR crack the other passwords rapidly?

The reason is because JTR doesn't have leads to fill up the rainbow tables using the dictionary in /usr/share/john/password.lst .
We should also state that the password cracking is just a matter of time and password complexity.

A fast fix is to append the word isetcom to /usr/share/john/password.lst using the ***>>*** to append it to the file.

```
$ echo "isetcom" >> /usr/share/john/password.lst
$ john mypasswd --show --format=crypt /usr/share/wordlists/rockyou.txt
```
Upon relaunching JTR, we notice that the results take **much** less time than before.







   
