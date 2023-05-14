---
layout: post
title:  "Brute-Force Attacks Explained"
date:   2021-12-06 12:55:58 +0200
author: Lukas Oberholzer
tags: []
description: More and more sensitive data is being sent over the Internet. Banks, shopping sites, e-mail clients. All this data is (often) protected by encryption and is considered secure. However, a key for decryption must be defined for the exchange of data. And it is precisely this key that is involved in a brute force attack.
image: "/assets/posts/brute-force-attacks-explained/cover.png"
usemathjax: true
---

In the era of the digital revolution, the flow of data is growing exponentially. Likewise, more and more sensitive data is being sent over the Internet. Banks, shopping sites, e-mail clients. They all use the so-called World Wide Web (also called www for short) and send sensitive data over it. All this data is (often) protected by encryption and is considered secure. However, a key for decryption must be defined for the exchange of data. And it is precisely this key that is involved in a brute force attack.

# What is a Brute-Force Attack?
A brute force attack is the name for a type of attack that is usually carried out by hackers in order to obtain passwords or password-protected content.

# How does a Brute-Force Attack work?
A brute force attack simply tries all combinations for a password (or decryption key). Sometimes this is also called an exhaustion method. Mostly, this type of attack is used with a short password or PIN (e.g. mobile-phone pin), because it can try many combinations in a very short time, which is only a maximum of 10,000 combinations for a 4-digit PIN. Since many users still use short passwords, which usually only consist of letters or numbers, this attack method is often used successfully.

In a brute force attack, a way is sought to get either the password/decryption key as a hash or unlimited attempts for password attempts. This makes a brute force attack possible. Once the hash or encrypted password is intercepted, specialized software can be used to test all combinations.

In the RC5-72 project of the Distributed.net organization, the goal was to decrypt a message encrypted with a 72-bit key. By using several computers it was possible to generate up to 800 billion passwords per second. In older projects of Distributed.net, 56-bit and 64-bit keys were also cracked, which took 250 days for 56-bit key and 1,757 days for 64-bit key.

# No Password is Safe!
When creating a password, the following characters can usually be used:

* Decimal numbers from 0 to 9 (10 characters)
* Lower and upper case letters (52 characters)
* Special characters (32 different often used)

This makes a total of 94 characters which are possible for each digit of your password or encryption key. To calculate the number of possible combinations now you just have to take the length of the password to the power of the number of characters.

$$ Number Of Combinations = Number Of Characters ^ {Passwordlength} $$

For a 5-character password, this would now be 7,339,040,224 (7.3 billion) different combinations. With the above mentioned 800 billion keys per second, the password would be cracked within only 0.00917 seconds or 9.17 milliseconds.

However, if we take a 9-digit password, then 572,994,802,228,616,704 (572 quadrillion) different combinations would already be possible. With 800 billion keys per second, this would already take 716’243.5 seconds or 8.28 days.

So now we know:

No password is uncrackable. It is only a question of time until it is cracked. Depending on the length and the number of characters used, the resistance time of a password is lower or higher.
Therefore, the task of a password should not be to make information inaccessible to attackers, but to stop the attackers as long as the information is relevant.

e.g. A password to a bank account is of no use to the attacker after 200 years.

# Quantum Computers
Explaining exactly what quantum computers are is beyond the scope of this post, however, here is Wikipedia’s brief definition of quantum computers.

> The devices that perform quantum computations are known as quantum computers. Though current quantum computers are too small to outperform usual (classical)computers for practical applications, they are believed to be capable of solving certain computational problems, such as integer factorization (which underlies RSA encryption), substantially faster than classical computers.
>
> \- QUANTUM COMPUTING (WIKIPEDIA)

Quantum computers have already been built. For example, the D-Wave 2X (by Google). This is 100 million times faster than a conventional computer and is therefore excellent for solving complex problems and tasks. What would happen if a quantum computer would perform a brute force attack?

The computers of Distributed.net reached 800 billion keys per second. Let’s replace them with quantum computers. Now 800 quadrillion keys per second can be tested (800 billion * 100 million). Then the 9-digit password from above would take only 0.716 seconds or 716.25 milliseconds. And quantum computers are only at the very beginning.

However, this will still take some time until they will be available for the normal consumer. But when they are, many passwords will no longer be safe from brute force attacks.

# Conclusion
Brute force is a powerful algorithm for decrypting passwords. However, it takes its time, which can be very long for long keys.

Quantum computers could make this algorithm much more efficient again and render many security standards (8 characters for a password) useless.

# How to protect yourself
To protect against such an attack password locks after e.g. 5 wrong attempts would be a possibility. (For website operators)
Also, long passwords (at least 8 characters) with upper and lower case letters, numbers and special characters should be used everywhere.