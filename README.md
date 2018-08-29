# anonip.py

Digitale Gesellschaft
https://www.digitale-gesellschaft.ch


formerly
Swiss Privacy Foundation
https://www.privacyfoundation.ch/


## Description

Anonip is a tool to anonymize IP-addresses in log-files of Apache Webserver

It masks the last bits of IPv4- and IPv6-addresses. That way most of the
relevant information is preserved, while the IP-address does not match a
particular individuum anymore.

The log-entries get directly piped from Apache to Anonip. The unmasked IP-
addresses are never written to any file.

With the help of cat, it's also possible, to rewrite existing log-files.

for usage with nginx see here: https://github.com/DigitaleGesellschaft/Anonip/issues/1

## Features

 - Masks IP-addresses in log-files
 - Configurable amount of masked bits
 - The column containing the IP-address can freely be chosen
 - Works for both access.log- and error.log-files

## Dependencies
If you're using python version 3, there are no external dependencies.

For python version 2:
 - [ipaddress module](https://bitbucket.org/kwi/py2-ipaddress/)

## Invocation

```
usage: anonip.py [-h] [-4 INTEGER] [-6 INTEGER] [-i INTEGER] [-o FILE]
                 [-c INTEGER [INTEGER ...]] [-r STRING] [-u USERNAME]
                 [-g GROUPNAME] [-m UMASK] [-d] [-v]

An ip address anonymizer.

optional arguments:
  -h, --help            show this help message and exit
  -4 INTEGER, --ipv4mask INTEGER
                        truncate the last n bits (default: 12)
  -6 INTEGER, --ipv6mask INTEGER
                        truncate the last n bits (default: 84)
  -i INTEGER, --increment INTEGER
                        increment the IP address by n (default: 0)
  -o FILE, --output FILE
                        file to write to
  -c INTEGER [INTEGER ...], --column INTEGER [INTEGER ...]
                        assume IP address is in column n (default: 1)
  -r STRING, --replace STRING
                        replacement string in case address parsing fails
                        Example: 0.0.0.0)
  -u USERNAME, --user USERNAME
                        switch user id
  -g GROUPNAME, --group GROUPNAME
                        switch group id
  -m UMASK, --umask UMASK
                        set umask
  -d, --debug           print debug messages
  -v, --version         show program's version number and exit
```

## Usage

In the Apache configuration (or the one of the vhost) the log-output needs to
get piped to anonip:
```
CustomLog "|/path/to/anonip.py [OPTIONS] --output /path/to/log" combined
```
That's it! All the IP-addresses will be masked in the log now.

Alternative:
```
cat /path/to/orig_log | /path/to/anonip.py [OPTIONS] --output /path/to/log
```

## Motivation

In a time, where the mass-data-collection of certain companies and
organisations gets more and more obvious, it's crutial to realize, that also
we maintain unnecessary huge data-collections.

For example admins of webservers. In the log-files you can find all the IP-
addresses of all visitors in cleartext and all of a sudden we possess a huge
collection of personalized data.

Anonip tries to avoid exactly that, but without losing the undisputed benefit
of those log-files.

With the masking of the last bits of IP-addresses, we're still able to
distinguish them up to a certain degree. Compared to the entire removal of the
IP-adresses, we're still able to make a rough geolocating as well as a reverse
DNS lookup. But the otherwise distinct IP-addresses do not match a particular
individuum anymore.
