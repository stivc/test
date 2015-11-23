# ssh basics

Secure shell (SSH) is a TCP protocol for securely encrypted and authenticated
communication between computers.  An ssh user may use an ssh client program
to connect to an ssh server and then run commands on that server.

    $ ssh remoteuser@remotehost -p remoteport
Logged in to remotehost
    remotehost$ run-a-command
See results
    remotehost$ exit
    $

An ssh client connects to the server, which provides 
a command-line interface (CLI).  Usually the server is running Linux or another
UNIX work-alike operating system, and the CLI is usually `bash`.

## What `ssh` does.

Imagine that some agent or agency is eavesdropping on one of the network links
between your laptop and the remote server that you make use of.  If you use
ssh for your back-and-forth communication, then the agent cannot see the
actual content of the communication.  Instead, the agent sees only an encrypted
version that has the appearance of random strings.

An important example of the information that `ssh` protects is your password.
If you sent a password to the server the verify your identity, the eavesdropper
cannot see it.  If the eavesdropper cannot get your password, then the
eavesdropper cannot later reuse your password to pretending to be you and
gain access to resources.

In the bad days of the early 1990s, people commonly sent passwords
over the internet in plaintext.  But the internet had stopped being a
friendly place where everyone knew everyone, so there were problems.
The first implementations of `ssh` were released to the public in 1995.
[Wikipedia article[(https://en.wikipedia.org/wiki/Secure_Shell).

## Concept: Using a private key to log in

Managing passwords is a nuisance even if network encryption does make them
safer to use.  The underlying technology of the SSH protocol is public key
cryptography (PKI) and PKI can be adapted to replace passwords as a method
of authentication.

SSH relies on *key pairs*.  A key pair consists of a *public key* and
a *private key*.  The public key can and should be advertised as being
your key.  Anyone who knows your public key and knows that you claim
it can then *grant you access to their systems*.  So it is mostly not
a problem if everyone knows the key and the fact that you are its owner.

The *private key*, on the other hand, should be a guarded secret.  The remote
server also has a public and private key.  If you want to know the full
glory of how this works, you can try 
[Wikipedia](https://en.wikipedia.org/wiki/Public-key_cryptography).

If an SSH server is configured to allow logins from your public key,
then you can use the corresponding private key to prove your identity and
the server will allow you to login.

Before you can use your key-pair you need to create one.

## Creating your own key-pair

Details will depend on your platform, and graphical programs will have menus
to help with this.  In a command-line environment, generating the key pair
looks something like this:

    $ ssh-keygen -t rsa 
    Generating public/private rsa key pair.
    Enter file in which to save the key (/.../.ssh/id_rsa):
(press return to save the private key in default location.)

    Enter passphrase (empty for no passphrase):
(enter a passphrase to help guard your private key.)

    Enter same passphrase again:
    Your identification has been saved in .../id_rsa.
    Your public key has been saved in .../id_rsa.pub.
(... possibly more output ...)

The file `id_rsa` contains the private key and the file `id_rsa.pub` contains
the private key.

The command-line environment can be a shell (e.g. `bash`) on a Linux or
Mac terminal.  Windows doesn't provide bash and ssh tools by default but
[Cygwin](https://www.cygwin.com/) is a powerful environment for Windows
that does support these utilities.

## Using your private key.

  (fill in)

## Using `ssh-agent`.

  (fill in)

## uploading your public key to GitHub

  (an example of a cloud service that uses ssh for authentication.
   also, this is a way of publishing your public key).

  Explain importance of using a passphrase in this context, alluding to 
  Git for Windows.

  In closing explain the MiM issue when distributing keys.

## uploading your public key to AWS

  (fill in)

  Explain how they will generate a key for you and issues around that.
  When to share a key-pair.
  Couple of ideas for Initializing your new instance.

