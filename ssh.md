# ssh basics

Secure shell (SSH) is a TCP protocol for securely encrypted and authenticated
communication between computers.  An ssh user may use an ssh client program
to connect to an ssh server and then run commands on that server.

    $ ssh remoteuser@remotehost -p remoteport

The commandline in the above example logs into the `remoteuser` account on the
`remotehost` machine.  `-p remoteport` is an option to specify which TCP port
to use on the remote system.

There are at least two ways to use ssh to log in to a remote account.  One
is to have a password:

    $ ssh remoteuser@remotehost
    Password:

This establishes an encrypted connection and sends the password over that 
connection so that the server can decide to allow the login or not.

A different approach is to use a private key.  Instructions on making and
propagating a private key appear below.  The way that this would work is:

    $ ssh remoteuser@remotehost
    Enter passphrase for key '/home/localuser/.ssh/id_rsa':

This might not appear to have any advantages over the password approach, but
it does.  The private key is kept in your workstations home directory in an
encrypted format.  Your client program, takes the entered passphrase and uses
it to decrypt the private key.  The client program then uses the private key
to /negotiate/ with the server.  The client /negotiates/ a proof that you are
the holder of the private key that corresponds with a public key.  The
public key matching the private key is like an identity -- at the conclusion
of the negotiation, the client program has proved that it posessed an identity.

The identity is stored in a file, sometimes called `id_rsa`.  The identity file
is encrypted so that someone reading your hard disk cannot steal it.

Note that the private key isn't sent to the server.  Whereas you shouldn't
use the same password for different services and managing the plethora of
passwords is a hassle, private keys have no such problem.  For services
that accept private-key crytographic systems like ssh, you can safely
use the same private key everywhere.

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

The file `id_rsa` contains the private key and the file `id_rsa.pub` contains
the private key.  `id_rsa.pub` is a one line file containing your public key.
Distribute the `id_rsa.pub` to systems where you would like to log in.

SSH servers must know the /public/ keys of those that are allowed to
log in.  On UNIX-like systems, the list of public keys for which access
is allowed is kept in the file `$HOME/.ssh/authorized_keys`.

## Coming soon

How to register your public key on GitHub.

How to grant login access to a github user with a published public key.
