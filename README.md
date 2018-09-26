# puppet-add-cli-auth-to-certificate

Update a Puppet certificate to make it suitable for CLI authentication

## Context

Puppet 6.0.0 introduced a new way to authorize CLI requests by adding a *cli-auth* extension in the master's node certificate.  The `puppetserver ca` command use the node certificate to authenticate requests, and the default `puppetserver/conf.d/auth.conf` only allow certificates with the appropriate *cli-auth* extension to perform actions.

Users upgrading from a previous Puppet version, users who are not generating a new PKI and using their old PKI will not be able to use the `puppetserver ca` command out of the box.  They need to either adjust the `puppetserver/conf.d/auth.conf` configuration [to explicitly allow the nodes which should be able to access the proper API endpoints](https://puppet.com/docs/puppetserver/6.0/subcommands.html#available-actions) or add the *cli-auth* extension to their master mode certificate.

The first approach is simpler, but your local configuration will diverge from the default one.  For this reason, you might prefer to take the second approach.   This project is all about doing this easily.

Related links:

* [Backport CA CLI auth extension for master cert](https://tickets.puppetlabs.com/browse/SERVER-2323)
* [Update puppetserver's CA bootstrapping code to add CLI tool auth extension](https://tickets.puppetlabs.com/browse/SERVER-2308)
* [The master cert created by `generate` should have custom extensions for the `cert_status` endpoint auth](https://tickets.puppetlabs.com/browse/SERVER-2287)
* [Pull Request on GitHub](https://github.com/puppetlabs/puppetserver/pull/1790)

## Usage

On AIO nodes, the call sequence is as simple as:

```sh
./add-cli-auth-to-certificate /etc/puppetlabs/puppet/ssl/certs/$(hostname -f).pem
```

If the `ssldir` is not the AIO default one, it's possible to explicitly provide the path to the ca certificate and key:

```sh
./add-cli-auth-to-certificate -k /var/puppet/ssl/ca/ca_key.pem -c /var/puppet/ssl/ca/ca_crt.pem /var/puppet/ssl/certs/$(hostname).pem
```
