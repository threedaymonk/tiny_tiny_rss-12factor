Tiny Tiny RSS
=============

Web-based news feed aggregator, designed to allow you to read news from 
any location, while feeling as close to a real desktop application as possible.

http://tt-rss.org (http://mirror.tt-rss.org)

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.

Copyright (c) 2005 Andrew Dolgov (unless explicitly stated otherwise).

Uses Silk icons by Mark James: http://www.famfamfam.com/lab/icons/silk/

## Notes on this fork

This fork has been modified to follow the [twelve-factor app][12factor]
principles so that it can be deployed to [Heroku][heroku], [Dokku][dokku], etc.

In particular, the following changes have been made:

* The web install script has been removed and `config.php` added.
* The database and self-URL are configured via environment variables.
* `composer.json` has been added to build the necessary environment.
* `Procfile` has been added and contains lines for the web server and the
  background daemon.

The following limitations currently apply:

* Favicons are lost on deployment.
* Only PostgreSQL is supported as a database, although it would in principle be
  straightforward to implement support for MySQL as well.
* Sphinx and SMTP settings are not read from the environment and thus cannot be
  used at present.

### Deployment

On Dokku, you will need the [dokku-shoreman][shoreman] plugin to run the
background daemon process, and some kind of PostgreSQL plugin (I recommend
[dokku-psql-single-container][psql]).
You'll also have to deploy once (and let it fail) to create the app before you
can set up the database and other environment variables.

Ensure that you have configured a database (this varies depending on your
platform and plugins) and that `DATABASE_URL` is set (it should look something
like `postgresql://user:pass@hostname/dbname`).

Install the database schema: on Dokku with dokku-psql-single-container:

```sh
$ ssh server psql:console < schema/ttrss_schema_pgsql.sql
```

or on Heroku:

```sh
$ heroku pg:psql < schema/ttrss_schema_pgsql.sql
```

Set the `SELF_URL_PATH` environment variable to the public URL of your
application.

Deploy this branch via Git:

```sh
$ git remote add production user@host:repository
$ git push production 12factor:master
```

Visit your installation, log in as `admin` with the password `password` (Tiny
Tiny RSS's default), and set the password to something more sensible!

[12factor]: http://12factor.net/
[heroku]: https://www.heroku.com/
[dokku]: http://progrium.viewdocs.io/dokku
[shoreman]: https://github.com/statianzo/dokku-shoreman
[psql]: https://github.com/Flink/dokku-psql-single-container

## Requirements

* Compatible web browser (http://tt-rss.org/wiki/CompatibleBrowsers)
* Web server, for example Apache
* PHP (with support for mbstring functions)
* PostgreSQL (tested on 8.3) or MySQL (InnoDB and version 4.1+ required)
		
## Installation Notes

http://tt-rss.org/wiki/InstallationNotes

## See also

* FAQ: http://tt-rss.org/wiki/FrequentlyAskedQuestions
* Forum: http://tt-rss.org/forum
* Wiki: http://tt-rss.org/wiki/WikiStart
