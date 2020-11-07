# Propose a cloud application to our users.

## Requirements:

1. feel comparable to popular clouds app, for instance GoogleDrive.
2. properly integrated with the unix user's home.
3. compatible with LDAP and PAM
4. synchronization with other devices
5. generic enough to be portable across diferent unix installs

## Base:

- the app would leave under one domain, for instance `cloud.club1.fr`


# Propositions:

## WebDav server + JS WebDav front

The app would consist of 3 main endpoints:
- `/`:       a gateway landing page with a login form
- `/webdav`: the standard webdav API
- `/app`:    the js front end app

### WebDav back end

using PHP and https://sabre.io/dav 

### WebDav front end

using Typescript and https://github.com/perry-mitchell/webdav-client
