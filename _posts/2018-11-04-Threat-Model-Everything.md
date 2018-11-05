---
layout: post
comments: true
title:  "Threat Modeling: An Anecdote"
background: '/img/posts/threatmodel1.jpg'
---

<p>Another post on <b>Threat Modeling</b>?! <i>Oh no!</i></p>

<p>I was working with an IT Manager who insisted on focusing on computer hard drive encryption as a first-priority remediation item. Not server, not laptop, not file system, but hard drive encryption for non-mobile computers. Encryption is a noble cause, and hard disk encryption is crucial to security.  But why prioritize that first?</p>

<p>There are a lot of things you could place first in your remediation plan. Closing unnecessary firewall ports (yes, most small/medium businesses have 3389 open in 2018). Multifactor authentication. User awareness training with phishing simulation. Patching. Upgrading OSes to from 7 and 2008R2 so that you can use SMB3 and not reset admin passwords with just a Windows Boot CD. Email server hardening. Buy layer 3 switches so that you can implement VLANs. Don't use the built-in administrator account. I digress, but let's just say that the IT manager hadn't implemented a single one of these things.</p>

<p>Think about the case (also known as a "threat model") that makes hard drive encryption necessary. Someone needs to be in possession of a hard drive. Think about what it takes to be in possession of a hard drive, you need to walk out of the building with one after getting it out of a computer, or walk out with a computer. That means you need to get into the building. So, in my mind, this means the IT manager thought that hard drive theft was the #1 way that an attacker would compromise data. Interesting perespective, let's play with that.</p>

<p>Let's talk about some things this facility did right. This building was the only place I've been that I haven't been able to tailgate (follow someone) into. There is no receptionist, there's 3 security cameras in the lobby, and you need to badge in.</p>

<p>Let's talk about some things they didn't do right. The server room door was never closed, because the thermostat next door in the IT office, so the room would overheat. The IT office next door was not locked at night so that the cleaning crew could take out the trash.</p>

<p>If you don't get the punchline yet, I'm not going to explain it.</p>

<p>Moral of the story? Put the trash outside the door, move the thermostat to the other side of the wall, and lock your server room door before you push out a technology to hundreds of PCs.</p>