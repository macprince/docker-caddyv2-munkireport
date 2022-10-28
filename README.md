# docker-caddyv2-munkireport
Docker Compose config for setting up MunkiReport, a MySQL backend database for it, and the Caddy web server to reverse proxy MunkiReport and also serve a Munki repo and MDS images.

Heavily stolen from @arice's blog post: https://arice.github.io/MunkiReport_server_setup.html

Pull requests welcome! (I probably did a lot of this wrong.)

## Required settings

### Caddyfile

* In place of `munki.example.com`, put in the fully qualified public DNS name of your server
* **TLSEMAILADDRESSGOESHERE** - Replace this with an email addresss that Let's Encrypt can use to contact you about your certificates.
* **HASHEDMUNKIREPOPASSWORDGOESHERE** - Before starting your stack, run this: `docker run -it --rm caddy /usr/bin/caddy hash-password` and enter your chosen Munki repo HTTP basic auth password. Paste the resulting string in place of this.

### mrdb.env

* **MYSQLROOTPASSWORDGOESHERE** - generate a long and very strong root password for your MySQL database and put it here. You'll need this in the future to export your database using `mysqldump`, but you shouldn't need it much day-to-day.
* **MYSQLUSERPASSWORDGOESHERE** - generate another long and very strong password for MunkiReport to use to talk to the MySQL database. You'll also need to put this in the .env file for MunkiReport later.
* If you have a .sql file containing an exported MunkiReport MySQL database, place it in the `import` directory and it will be automatically imported when the MySQL container starts.

### munkireport.env

* **MUNKIREPORTCLIENTPASSPHRASEGOESHERE** - enter the passphrase your MunkiReport clients are going to use to authorize their check-ins. 
* **MYSQLUSERPASSWORDGOESHERE** - the same MySQL user password you generated for the mrdb.env file.
