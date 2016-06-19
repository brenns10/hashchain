Hash Chain in C
===============

This is a convenient command-line tool that creates and verifies hash chains. I
implemented it using OpenSSL's crypto library.

Build
-----

    $ gcc -o hashchain hashchain.c -lcrypto

Run
---

### Create

To create a hash chain, the arguments are:

    $ ./hashchain create ALGORITHM LENGTH SEED

A good example:

    $ ./hashchain create sha256 1000 "my secret password"

A better example:

    $ ./hashchain create sha256 1000 `base64 /dev/random | head -c 32` > chains

This randomly generates a chain of length 1000 using sha256 and saves it to the
file `chains`.

Each line of the output is base64 encoded data which hashes to the next line.

### Verify

Say that you have the last two hashes from the previous example:

    $ tail -n 2 chain
    gjZmdTdMNnijpZd0hhkxJSK9/IywQIQ2H5N2BiWC6w0=
    5o/+3BTbOTebzIJGTI0bZPorFatbV1zu070qBSx3Z0k=

You can verify with the command:

    $ ./hashchain verify gjZmdTdMNnijpZd0hhkxJSK9/IywQIQ2H5N2BiWC6w0= 5o/+3BTbOTebzIJGTI0bZPorFatbV1zu070qBSx3Z0k=
    success

The verify command writes "success" and returns 0 if the hashes verify, and
writes "failure" and returns non-0 if they don't.
