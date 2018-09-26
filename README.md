# puppet-add-cli-auth-to-certificate

puppetlabs/puppetserver#1790 introduced a new extension in the certificate of
the master's host, needed to authenticate against the
`/puppet-ca/v1/certificate_status` and `/puppet-ca/v1/certificate_statuses` API
endpoints.  This extension is necessary to use
[puppetlabs/puppetserver-ca-cli](https://github.com/puppetlabs/puppetserver-ca-cli),
so when running a puppet server with certificates generated with a previous
version of puppet, the new `puppetserver ca` which replace `puppet ca`, `puppet
cert` and other removed commands will be non-functionnal.

This projects is intended to update a node certificate and add the appropriate
extension for CLI authentication.

```sh
./add-cli-auth-to-certificate -k /var/puppet/ssl/ca/ca_key.pem -c /var/puppet/ssl/ca/ca_crt.pem /var/puppet/ssl/certs/$(hostname).pem
```
