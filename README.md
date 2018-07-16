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

### Use

These patches are in addition to all the other patches in the Debian Gosa(2.7) packaging source. To use these, get the source package for gosa 2.7, the orig tarball and unpack. CD into the gosa source dir. Check in debian/patches that the patch sequencing has not moved past 1043 and 1044 (don't want to overwrite other patches! renumber if needed) and move these into the debian/patches directory. Edit the series file and add these to the end of the list. Edit debian/changelog and add an entry (properly formatted!) bumping the patch version (ie: deb8u3 from deb8u2 or deb9u2 from deb9u1). Use dpkg-buildpackage to rebuild the debian packages, they will show up in the parent dir.
