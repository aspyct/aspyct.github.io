GPG SmartCard
=============

The commands below have been tested with gpg (GnuPG) 2.1.18 and a smartcard v3.3.

Verify that a card is detected
------------------------------

::

    $ gpg --card-status

This command will output a card status. If a "No such device" message appears,
then something is wrong with your system.

Change the user and admin PIN code
----------------------------------

This is the output when using a card reader with a physical pin pad.

::

    $ gpg --change-pin
    gpg: OpenPGP card no. D2760001240103030005000067A80000 detected

    1 - change PIN
    2 - unblock PIN
    3 - change Admin PIN
    4 - set the Reset Code
    Q - quit

    Your selection? 1
    PIN changed.

Generate a private key on the card
----------------------------------

::

    $ gpg --edit-card

    Reader ...........: ...
    Application ID ...: ...
    Version ..........: 3.3
    Manufacturer .....: ...
    Serial number ....: ...
    Name of cardholder: [not set]
    Language prefs ...: de
    Sex ..............: unspecified
    URL of public key : [not set]
    Login data .......: [not set]
    Signature PIN ....: forced
    Key attributes ...: rsa2048 rsa2048 rsa2048
    Max. PIN lengths .: 64 64 64
    PIN retry counter : 3 0 3
    Signature counter : 0
    Signature key ....: [none]
    Encryption key....: [none]
    Authentication key: [none]
    General key info..: [none]

    gpg/card> admin
    Admin commands are allowed

    gpg/card> name
    Cardholder's surname: <your surname>
    Cardholder's given name: <your first name>

    gpg/card> generate
    Make off-card backup of encryption key? (Y/n) n
    What keysize do you want for the Signature key? (2048) 4096
    The card will now be re-configured to generate a key of 4096 bits
    Note: There is no guarantee that the card supports the requested size.
        If the key generation does not succeed, please check the
        documentation of your card to see what sizes are allowed.
    What keysize do you want for the Encryption key? (2048) 4096
    The card will now be re-configured to generate a key of 4096 bits
    What keysize do you want for the Authentication key? (2048) 4096
    The card will now be re-configured to generate a key of 4096 bits
    Please specify how long the key should be valid.
            0 = key does not expire
        <n>  = key expires in n days
        <n>w = key expires in n weeks
        <n>m = key expires in n months
        <n>y = key expires in n years
    Key is valid for? (0) 1y
    Key expires at Thu 09 Jan 2020 07:54:41 PM CET
    Is this correct? (y/N) y

    GnuPG needs to construct a user ID to identify your key.

    Real name: <your name>
    Email address: <your email>
    Comment:
    You selected this USER-ID:
        "<your name> <your email>"

    Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? o
