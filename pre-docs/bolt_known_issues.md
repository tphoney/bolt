# Known issues

Known issues for the Bolt 1.x release series.

## False errors for SSH Keys generated with ssh-keygen OpenSSH 7.8 and later

The OpenSSH 7.8 release introduced a change to SSH key generation. It now generates private keys with its own format rather than the OpenSSL PEM format. Because the Bolt SSH implementation assumes any key that uses the OpenSSH format uses the public-key signature system ed25519, false errors have resulted. For example:

```
OpenSSH keys only supported if ED25519 is available
  net-ssh requires the following gems for ed25519 support:
   * ed25519 (>= 1.2, < 2.0)
   * bcrypt_pbkdf (>= 1.0, < 2.0)
  See https://github.com/net-ssh/net-ssh/issues/565 for more information
  Gem::LoadError : "ed25519 is not part of the bundle. Add it to your Gemfile."
```

or

```
Failed to connect to HOST: expected 64-byte String, got NUM 
```

Workaround: Generate new keys with the ssh-keygen flag `-m PEM`. For existing keys, OpenSSH provides the export \(`-e`\) option for exporting from its own format, but export is not implemented for all private key types. [\(BOLT-920\)](https://tickets.puppet.com/browse/BOLT-920) 

## String segments in commands must be triple-quoted in PowerShell

When running Bolt in PowerShell with commands to be run on \*nix nodes, string segments that can be interpreted by PowerShell need to be triple quoted. [\(BOLT-159\)](https://tickets.puppet.com/browse/BOLT-159)

