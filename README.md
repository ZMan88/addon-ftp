# Community Hass.io Add-ons: FTP

[![GitHub Release][releases-shield]][releases]
![Project Stage][project-stage-shield]
[![License][license-shield]](LICENSE.md)

[![GitLab CI][gitlabci-shield]][gitlabci]
![Project Maintenance][maintenance-shield]
[![GitHub Activity][commits-shield]][commits]

[![Bountysource][bountysource-shield]][bountysource]
[![Discord][discord-shield]][discord]
[![Community Forum][forum-shield]][forum]

[![Buy me a coffee][buymeacoffee-shield]][buymeacoffee]

[![Support my work on Patreon][patreon-shield]][patreon]

A secure and fast FTP server for Hass.io

## About

The FTP protocol might be come in handy sometimes. While old,
it still has its use. For example, most IP Cameras still support the upload
of images or videos via FTP.

This add-on provides an FTP Server for Hass.io in a reasonably secure manner.
While FTP is not entirely secure by its (unencrypted) nature, this add-on
supports FTP over SSL (FTPS) and jails (chroot) the virtual users in their
home directories.

Of course, if you'd really want to, you could also use this add-on to again
access to your Home Assistant configuration via FTP.

## Installation

The installation of this add-on is pretty straightforward and not different in
comparison to installing any other Hass.io add-on.

1. [Add our Hass.io add-ons repository][repository] to your Hass.io instance.
1. Install the "FTP" add-on.
1. Start the "FTP" add-on
1. Check the logs of the "FTP" add-on to see if everything went well.

**NOTE**: Do not add this repository to Hass.io, please use:
`https://github.com/hassio-addons/repository`.

## Docker status

[![Docker Architecture][armhf-arch-shield]][armhf-dockerhub]
[![Docker Version][armhf-version-shield]][armhf-microbadger]
[![Docker Layers][armhf-layers-shield]][armhf-microbadger]
[![Docker Pulls][armhf-pulls-shield]][armhf-dockerhub]
[![Anchore Image Overview][armhf-anchore-shield]][armhf-anchore]

[![Docker Architecture][aarch64-arch-shield]][aarch64-dockerhub]
[![Docker Version][aarch64-version-shield]][aarch64-microbadger]
[![Docker Layers][aarch64-layers-shield]][aarch64-microbadger]
[![Docker Pulls][aarch64-pulls-shield]][aarch64-dockerhub]
[![Anchore Image Overview][aarch64-anchore-shield]][aarch64-anchore]

[![Docker Architecture][amd64-arch-shield]][amd64-dockerhub]
[![Docker Version][amd64-version-shield]][amd64-microbadger]
[![Docker Layers][amd64-layers-shield]][amd64-microbadger]
[![Docker Pulls][amd64-pulls-shield]][amd64-dockerhub]
[![Anchore Image Overview][amd64-anchore-shield]][amd64-anchore]

[![Docker Architecture][i386-arch-shield]][i386-dockerhub]
[![Docker Version][i386-version-shield]][i386-microbadger]
[![Docker Layers][i386-layers-shield]][i386-microbadger]
[![Docker Pulls][i386-pulls-shield]][i386-dockerhub]
[![Anchore Image Overview][i386-anchore-shield]][i386-anchore]

## Configuration

**Note**: _Remember to restart the add-on when the configuration is changed._

Example add-on configuration:

```json
{
  "log_level": "info",
  "port": 21,
  "data_port": 20,
  "banner": "Welcome to the Hass.io FTP service.",
  "pasv": true,
  "pasv_min_port": 30000,
  "pasv_max_port": 30010,
  "pasv_address": "",
  "ssl": false,
  "certfile": "fullchain.pem",
  "keyfile": "privkey.pem",
  "implicit_ssl": false,
  "max_clients": 5,
  "users": [
    {
      "username": "hassio",
      "password": "changeme",
      "allow_chmod": true,
      "allow_download": true,
      "allow_upload": true,
      "allow_dirlist": true,
      "addons": false,
      "backup": true,
      "config": true,
      "share": true,
      "ssl": false
    },
    {
      "username": "camera",
      "password": "changeme",
      "allow_chmod": false,
      "allow_download": false,
      "allow_upload": true,
      "allow_dirlist": true,
      "addons": false,
      "backup": false,
      "config": false,
      "share": true,
      "ssl": false
    }
  ]
}
```

**Note**: _This is just an example, don't copy and past it! Create your own!_

### Option: `log_level`

The `log_level` option controls the level of log output by the addon and can
be changed to be more or less verbose, which might be useful when you are
dealing with an unknown issue. Possible values are:

- `trace`: Show every detail, like all called internal functions.
- `debug`: Shows detailed debug information.
- `info`: Normal (usually) interesting events.
- `warning`: Exceptional occurrences that are not errors.
- `error`:  Runtime errors that do not require immediate action.
- `fatal`: Something went terribly wrong. Add-on becomes unusable.

Please note that each level automatically includes log messages from a
more severe level, e.g., `debug` also shows `info` messages. By default,
the `log_level` is set to `info`, which is the recommended setting unless
you are troubleshooting.

These log level also affects the log levels of the FTP server.

### Option: `port`

The port the FTP will listen on for incoming FTP connections.

### Option: `data_port`

The port from which PORT style connections originate.

### Option: `banner`

This string option allows you to provide the greeting banner displayed by
the FTP server when a connection first comes in.

### Option: `pasv`

Set to `false` if you want to disallow the PASV method of obtaining a data
connection. For more information about passive versus active FTP, see
[this excellent Stack Overflow][passive-vs-active] answer.

### Option: `pasv_min_port`

The minimum port to allocate for PASV style data connections. Can be used to
specify a narrow port range to assist firewalling.

### Option: `pasv_max_port`

The maximum port to allocate for PASV style data connections. Can be used to
specify a narrow port range to assist firewalling.

### Option: `pasv_address`

Use this option to override the IP address that the FTP server will advertise
in response to the PASV command. Provide a numeric IP address, or provide a
hostname which will be DNS resolved for you at startup. When left empty, the
address is taken from the incoming connected socket.

### Option: `ssl`

Enables/Disables SSL on the FTP Server. Set it `true` to enable it,
`false` otherwise.

### Option: `certfile`

The certificate file to use for SSL.

**Note**: _The file MUST be stored in `/ssl/`, which is default for Hass.io_

### Option: `keyfile`

The private key file to use for SSL.

**Note**: _The file MUST be stored in `/ssl/`, which is default for Hass.io_

### Option: `implicit_ssl`

If set to `true`, an SSL handshake is the first thing expect on all connections
(the FTPS protocol).

### Option: `max_clients`

This is the maximum number of clients which may be connected at the same time.
Any additional clients connecting will get an error message.

### Option: `users`

This option allows you to specify a list of one or more users. Each user can
have its own permissions like defined in the sub-options below.

#### Sub-option: `username`

The username the user needs to use to login to the FTP server. A valid username
has a maximum of 32 characters, contains only `A-Z` and `0-9`.
Usernames may contain a hyphen (`-`) but must not start or end with one.

#### Sub-option: `password`

The password the user logs in with.

#### Sub-option: `allow_chmod`

Setting this option to `true` will allow the use of the `SITE CHMOD` command for
that user.

#### Sub-option: `allow_download`

Setting this option to `true` will allow the user to download files from the
FTP server.

#### Sub-option: `allow_upload`

This controls whether any FTP commands which change the filesystem are
allowed or not.

These commands are `STOR`, `DELE`, `RNFR`, `RNTO`, `MKD`, `RMD`,
`APPE`, and `SITE`.

#### Sub-option: `allow_dirlist`

Setting this option to `true`, allows to user to browse all directories
the user was given access to by using the list commands.

#### Sub-option: `addons`

Allow the user to access the `/addons` directory.

#### Sub-option: `backup`

Allow the user to access the `/backup` directory.

#### Sub-option: `config`

Allow the user to access the `/config` directory.

#### Sub-option: `share`

Allow the user to access the `/share` directory.

#### Sub-option: `ssl`

Allow the user to access the `/ssl` directory.

### Option: `i_like_to_be_pwned`

Adding this option to the add-on configuration allows to you bypass the
HaveIBeenPwned password requirement by setting it to `true`.

**Note**: _We STRONGLY suggest picking a stronger/safer password instead of
using this option! USE AT YOUR OWN RISK!_

## Changelog & Releases

This repository keeps a change log using [GitHub's releases][releases]
functionality. The format of the log is based on
[Keep a Changelog][keepchangelog].

Releases are based on [Semantic Versioning][semver], and use the format
of ``MAJOR.MINOR.PATCH``. In a nutshell, the version will be incremented
based on the following:

- ``MAJOR``: Incompatible or major changes.
- ``MINOR``: Backwards-compatible new features and enhancements.
- ``PATCH``: Backwards-compatible bugfixes and package updates.

## Support

Got questions?

You have several options to get them answered:

- The [Community Hass.io Add-ons Discord chat server][discord] for add-on
  support and feature requests.
- The [Home Assistant Discord chat server][discord-ha] for general Home
  Assistant discussions and questions.
- The Home Assistant [Community Forum][forum].
- Join the [Reddit subreddit][reddit] in [/r/homeassistant][reddit]

You could also [open an issue here][issue] GitHub.

## Contributing

This is an active open-source project. We are always open to people who want to
use the code or contribute to it.

We have set up a separate document containing our
[contribution guidelines](CONTRIBUTING.md).

Thank you for being involved! :heart_eyes:

## Authors & contributors

The original setup of this repository is by [Franck Nijhof][frenck].

For a full list of all authors and contributors,
check [the contributor's page][contributors].

## We have got some Hass.io add-ons for you

Want some more functionality to your Hass.io Home Assistant instance?

We have created multiple add-ons for Hass.io. For a full list, check out
our [GitHub Repository][repository].

## License

MIT License

Copyright (c) 2017 Franck Nijhof

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

[aarch64-anchore-shield]: https://anchore.io/service/badges/image/08f6c79325d61206c8eea06a328f274413f304495091ca420341c379d2ddec54
[aarch64-anchore]: https://anchore.io/image/dockerhub/hassioaddons%2Fftp-aarch64%3Alatest
[aarch64-arch-shield]: https://img.shields.io/badge/architecture-aarch64-blue.svg
[aarch64-dockerhub]: https://hub.docker.com/r/hassioaddons/ftp-aarch64
[aarch64-layers-shield]: https://images.microbadger.com/badges/image/hassioaddons/ftp-aarch64.svg
[aarch64-microbadger]: https://microbadger.com/images/hassioaddons/ftp-aarch64
[aarch64-pulls-shield]: https://img.shields.io/docker/pulls/hassioaddons/ftp-aarch64.svg
[aarch64-version-shield]: https://images.microbadger.com/badges/version/hassioaddons/ftp-aarch64.svg
[amd64-anchore-shield]: https://anchore.io/service/badges/image/9464fdaffd256d1e3609966411dd2e18e400d16edcc632ab5f8c78dfac50e3d1
[amd64-anchore]: https://anchore.io/image/dockerhub/hassioaddons%2Fftp-amd64%3Alatest
[amd64-arch-shield]: https://img.shields.io/badge/architecture-amd64-blue.svg
[amd64-dockerhub]: https://hub.docker.com/r/hassioaddons/ftp-amd64
[amd64-layers-shield]: https://images.microbadger.com/badges/image/hassioaddons/ftp-amd64.svg
[amd64-microbadger]: https://microbadger.com/images/hassioaddons/ftp-amd64
[amd64-pulls-shield]: https://img.shields.io/docker/pulls/hassioaddons/ftp-amd64.svg
[amd64-version-shield]: https://images.microbadger.com/badges/version/hassioaddons/ftp-amd64.svg
[armhf-anchore-shield]: https://anchore.io/service/badges/image/858a802abc66321df402e93c77a63fcc4c80c9464baa46782b57f45092a6bdd5
[armhf-anchore]: https://anchore.io/image/dockerhub/hassioaddons%2Fftp-armhf%3Alatest
[armhf-arch-shield]: https://img.shields.io/badge/architecture-armhf-blue.svg
[armhf-dockerhub]: https://hub.docker.com/r/hassioaddons/ftp-armhf
[armhf-layers-shield]: https://images.microbadger.com/badges/image/hassioaddons/ftp-armhf.svg
[armhf-microbadger]: https://microbadger.com/images/hassioaddons/ftp-armhf
[armhf-pulls-shield]: https://img.shields.io/docker/pulls/hassioaddons/ftp-armhf.svg
[armhf-version-shield]: https://images.microbadger.com/badges/version/hassioaddons/ftp-armhf.svg
[bountysource-shield]: https://img.shields.io/bountysource/team/hassio-addons/activity.svg
[bountysource]: https://www.bountysource.com/teams/hassio-addons/issues
[buymeacoffee-shield]: https://www.buymeacoffee.com/assets/img/guidelines/download-assets-sm-2.svg
[buymeacoffee]: https://www.buymeacoffee.com/frenck
[commits-shield]: https://img.shields.io/github/commit-activity/y/hassio-addons/addon-ftp.svg
[commits]: https://github.com/hassio-addons/addon-ftp/commits/master
[contributors]: https://github.com/hassio-addons/addon-ftp/graphs/contributors
[discord-ha]: https://discord.gg/c5DvZ4e
[discord-shield]: https://img.shields.io/discord/478094546522079232.svg
[discord]: https://discord.me/hassioaddons
[forum-shield]: https://img.shields.io/badge/community-forum-brightgreen.svg
[forum]: https://community.home-assistant.io/t/community-hass-io-add-on-ftp/36799?u=frenck
[frenck]: https://github.com/frenck
[gitlabci-shield]: https://gitlab.com/hassio-addons/addon-ftp/badges/master/pipeline.svg
[gitlabci]: https://gitlab.com/hassio-addons/addon-ftp/pipelines
[home-assistant]: https://home-assistant.io
[i386-anchore-shield]: https://anchore.io/service/badges/image/6c3dafb920b3c1e5d61d322bea1bf7600b2806ef42d02f5b3706ccfb9d56c201
[i386-anchore]: https://anchore.io/image/dockerhub/hassioaddons%2Fftp-i386%3Alatest
[i386-arch-shield]: https://img.shields.io/badge/architecture-i386-blue.svg
[i386-dockerhub]: https://hub.docker.com/r/hassioaddons/ftp-i386
[i386-layers-shield]: https://images.microbadger.com/badges/image/hassioaddons/ftp-i386.svg
[i386-microbadger]: https://microbadger.com/images/hassioaddons/ftp-i386
[i386-pulls-shield]: https://img.shields.io/docker/pulls/hassioaddons/ftp-i386.svg
[i386-version-shield]: https://images.microbadger.com/badges/version/hassioaddons/ftp-i386.svg
[issue]: https://github.com/hassio-addons/addon-ftp/issues
[keepchangelog]: http://keepachangelog.com/en/1.0.0/
[license-shield]: https://img.shields.io/github/license/hassio-addons/addon-ftp.svg
[maintenance-shield]: https://img.shields.io/maintenance/yes/2018.svg
[passive-vs-active]: https://stackoverflow.com/a/1699163/299699
[patreon-shield]: https://www.frenck.nl/images/patreon.png
[patreon]: https://www.patreon.com/frenck
[project-stage-shield]: https://img.shields.io/badge/project%20stage-production%20ready-brightgreen.svg
[reddit]: https://reddit.com/r/homeassistant
[releases-shield]: https://img.shields.io/github/release/hassio-addons/addon-ftp.svg
[releases]: https://github.com/hassio-addons/addon-ftp/releases
[repository]: https://github.com/hassio-addons/repository
[semver]: http://semver.org/spec/v2.0.0.htm
