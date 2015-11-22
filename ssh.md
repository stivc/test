## ssh basics

Secure shell aka ssh is a protocol for securely encrypted and authenticated
communication between computers.  An ssh server is running a daemon that
listens for incoming connections from the internet.

An ssh client connects to the server, which provides a command line interface
(CLI) for executing commands on the server.  The CLI provided by the server
is typically bash, and the server is typically Linux or some other 
UNIX-workalike.  Here is a a session where we log in, check to see who is
logged in, what the disk space situation is, how busy the system is.  Then
the session is logged out.  The '$' is a shell prompt, this example assumes
you are logging in from a shell terminal.

The specific ssh command invoked in the example will try to connect to a 
host with name 'remotehost.example.com' on port 22.  It assumes the remote
user name is rname.  Port 22 is in fact the default port for ssh servers
and so "-p 22" was not required, but if the server runs on a nonstandard
port, this command line option tells the client what destination port to
try.

------
$ ssh rname@remotehost.example.com -p 22
Password:
Last login: Sat Nov 21 10:45:48 2015 from your.other.workstation

** Message of the day is: **
** Your system prompt is probably 'remotehost$ ' **

remotehost$ w
(see who is logged in to this server)
remotehost$ df
(see what current disk usage is)
remotehost$ uptime
(see how long system has been up, and how heavy the system load is)
remotehost$ exit
(log out and get back the system prompt of your workstation)
$ next-command-typed-which-will-run-on-your-local-workstation
-----

Thanks to the ssh protocol, an agent that sees all communication between
your workstation and the server cannot determine what transpired.
Though your password was transmitted, it cannot be sniffed out from the
network traffic because it was encrypted.

This is the huge advantage of ssh over older protocols and utilities
such as telnet and rsh.

# Using a private key to log in

Passwords are trouble: you should not re-use them, but then you need to keep
track of them, which is a nuisance.  The SSH protocol is built on public key
cryptography and and public-key cryptography can be used to allow secure
communications without passwords.

PKI (public-key infrastructure) depends on key-pairs.  A key pair consists
of two keys: a public key and a private key.  The public key can be known to
all, in fact, it is convenient to publish your public keys.  The private key
however should be a guarded secret.  The key-pair can be randomly generated
using a utility such as `ssh-keygen`.

There is a mathematical one-to-one correspondence between public keys and
private keys.  The PKI magic comes from the fact that although the public
key can be calculated from the private key, there is no known algorithm
that can derive the private key from the public key.

Skipping details, the key-pair can be used to secure communications so that

- each party knows the public key of the other party.

- each party can /prove/ they have the private key that correspondings to who they claim to be.  They can prove they have this key, without showing any part of it.

That is not all.  

- each party can encrypt communication, using the other party's public key.  
The communications can only be decrypted 
by the possessor of the corresponding private key.

- each party can verify, using the other party's public key, that a received
communication really is from the posessor of the corresponding private key.

This concludes that hard math.

= Generating your own key pair

Use the `ssh-keygen` utility.  This is part of Linux and Macintosh systems
by default.  On a Windows system it is included with various toolsets such
as Cygwin and Git for Windows.

(to be continued.)

(include the why of passphrases, and caution about dispensing with them.)

# some other services that rely on ssh protocol

(expand)
scp including WinSCP, sshfs, GitHub

# uploading your public key

(expand)
to Github, to AWS, to your friends.  MiM attack.

explain how AWS will generate a key pair for you, but that using this feature
is somewhat contrary to The Way of the Secure Shell.

when to share key-pairs.

# sshd on AWS Linux servers

(expand)
general knowledge and configuration settings.
enabling PKI.

# sshd on non-Linux servers

(expand)
Mac an other UNIX systems similar.
Windows very different 
but sshd that comes with Cygwin 
can be set up and can be very useful.

# (continue?)
