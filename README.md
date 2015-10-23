### GnuPG-encrypted CLI for Google Authenticator

This is a simple way to generate Google Authenticator or any other
[TOTP](https://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm)
standard codes from the command line.  It also supports AWS MFA!

It keeps a an encrypted file of your shared secrets and uses the
command-line [GnuPG](https://www.gnupg.org/) program to decrypt it.
It will also operate with the GPP Agent so you don't have to keep
typing in your password every time.

This tool is written in Python.  Check out this repo and make sure you
have the requirements installed:


```
pip install -r requirements.txt
```

(you may have to put `sudo` in front of this)

Then copy the `otp` file to some convenient bin directory and set it
executable:

```
sudo cp otp /usr/local/bin
sudo chmod +x /usr/local/bin/otp
```

Then set up a file with your secrets.  The default is in
`~/.sec/otp.gpg`, you can override that with the `KEYFILE` environment
variable.  You can create the file like this:

```
% mkdir ~/.sec
% vi ~/.sec/otp
```

The contents of the file are labels followed by the shared secret (as
given by Google, AWS, or anyone else):

```
dougluce-google-auth fjvb8sowboxkx283jdknxlc2jbscj0dk
work-google k2xbvo9djsbd2dbsov7,xznv8bodosrs
toma-google-auth O4BUXLBBKXWWEK89
work-aws OOBO8VBEPWBPBVI9SBVPIWEFWENFOd9NV9OUB9ZPXPSCOIJWEBIUFIO0ZXCZXO0C
```

Save the file, then encrypt with your public key:

```
% gpg -e otp
You did not specify a user ID. (you may use "-r")

Current recipients:
1024R/C2DBA41D 1993-01-30 "Doug Luce <doug@github.con.com>"

Enter the user ID.  End with an empty line: <enter>
%
```

That's it!  Run `otp` to get your code:

```
% otp
1) dougluce-google-auth
2) work-google
3) toma-google-auth
4) work-aws
> 1
dougluce-google-auth
173725
%
```

You can also feed it a number to choose a key without prompting:

```
% otp 1
dougluce-google-auth
273928
%
```

The -n option keeps the label from printing (good for scripts):

```
% otp -n 1
382749
%
```

