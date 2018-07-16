# gosa_debian_patches
### Overview
This repo contains a few patches for the Debian maintained Gosa packages (for 2.7, the PHP based Gosa). Hopefully they will be accepted upstream (still waiting on PR access to Salsa to do so). Currently there are 2 patches here:

* Enable sha256 and sha512 passwords
* base64() encode passwords sent via shell hook commands

Fairly simple and obvious what they do.

The first adds selections in all the password drop-downs for using sha256 and sha512 password hashing via system CRYPT libs, compatible with the way OpenLdap uses these methods, using {CRYPT} with $5$ and $6$ hashing styles. These patches allow Gosa to encrypt these passwords and lock accounts using them.

The second base64() encodes any password sent to shell for password hooks (both new_password and current_password vars). The current method of using escapeshellargs() is broken and can lead to RCE if hooks are used with carefuly crafted values (See patches 0006, 0007 and 1004 for other examples). Note that this requires hook calls to use $(base64 -d %password) or similar instead of the direct variable.

Other patches will be added here as I clean up and generalize them.
ToDo: php-native Samba hash generation (no reason to call out to Perl to do this)
