# ssh basics

Secure shell (SSH) is a TCP protocol for securely encrypted and authenticated
communication between computers.  An ssh user may use an ssh client program
to connect to an ssh server and then run commands on that server.

    $ ssh remoteuser@remotehost -p 22

The commandline in the above example logs into the `remoteuser` account on the
`remotehost` machine.  `-p 22` specifies that the ssh client should contact
TCP port 22 on the remote host to make the connection.  Since 22 is the 
standard ssh port, this isn't actually necessary.  But if the server uses a
nonstandard port, then it is necessary to specify.

There are at least two ways that the ssh client can prove to the server that
you are a person allowed to log in to the server.
One way is to have a password:

    $ ssh remoteuser@remotehost
    Password:

This establishes an encrypted connection and sends the password (which
you must type) over that connection so that the server can decide to
allow the login or not.

A different approach is to use a private key.  Instructions on making and
propagating a private key appear below.  The way that this would work is:

    $ ssh remoteuser@remotehost
    Enter passphrase for key '/home/localuser/.ssh/id_rsa':

This might appear to have no advantages over the password approach.  But
it does.  The private key is kept in your workstation's home directory in an
encrypted format.  Your client program, takes the entered passphrase and uses
it to decrypt the private key.  The client program uses the passphrase (which 
you must type) to decrypt the private key.

With the private key in hand, the client program can *negotiate* with
the server a proof that you are the holder of the private key that
corresponds with a public key.  The public key is like a user identity --
at the conclusion of the negotiation, the client program has proved that
it posesses a user identity.

The user who owns the identity needs to keep it stored in a file,
sometimes called `id_rsa`.  The identity file is encrypted with a
passphrase so that someone reading your hard disk cannot steal it.

Note that when you log in to the ssh server, the private key is not sent
to that server.  So, whereas you shouldn't use the same password for
different services and managing the multitude of passwords is a hassle,
private keys have no such problem.  For private-key ssh-like services
like ssh, you can safely use the same private key to access multiple
services.

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

SSH servers must know the *public* keys of those that are allowed to
log in.  On UNIX-like systems, the list of public keys for which access
is allowed is kept in the file `$HOME/.ssh/authorized_keys`.

## Coming soon

How to register your public key on GitHub.

How to grant login access to a github user with a published public key.
