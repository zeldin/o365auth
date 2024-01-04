o365auth
========

This is a small python script to convert a name and a password into
a bearer token for use with IMAP on outlook.office365.com.

It uses the [msal](
https://github.com/AzureAD/microsoft-authentication-library-for-python)
module for performing the actual token retrieval.

The default client id and secret used are for "Marcus' OAUTH hack",
you can replace them with those of your own client if so desired.

Also included is a patch to the file `nnimap.el` of
[GNUS](http://www.gnus.org/) to add a new `nnimap-authenticator`
method called `o365`, which calls the script.  To use it, configure
your select GNUS (secondary) method like so:

```
 (nnimap "server-nick"
         (nnimap-address "outlook.office365.com")
         (nnimap-server-port 993)
         (nnimap-stream ssl)
         (nnimap-authenticator o365))
```

The patch assumes the script is installed as `/usr/local/bin/o365auth`,
modify this as needed.

The script includes a function to get the username and password from
the `~/.authinfo` file, which removes the need for the password to
appear in cleartext on the command line.  However, the `nnimap.el`
patch does not currently use this mode.  A future improvement, perhaps.
The `~/.authinfo` mode is activated by specifying which `machine` attribute
to match in `~/.authinfo`, by using the `--machine` command line argument
in place of `--user` and `--password`.
