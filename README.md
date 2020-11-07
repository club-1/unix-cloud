# Propose a cloud application to our users.

## Features:


## Implementation requirements:

- the app would leave under one domain, for instance `cloud.club1.fr`
- the file publication system is using symbolic links
- the root of the explorer must be the unix user's home.
- compatible with LDAP and PAM
- synchronization with WebDav
- generic enough to be portable across diferent unix installs

# Propositions:

## WebDav server + JS WebDav file explorer

The app would consist of 3 main endpoints:

- `/`:       a gateway landing page with a login form
- `/webdav`: the standard webdav API
- `/files`:  the js file explorer app
- `/public`: 

### WebDav back end

using PHP and https://sabre.io/dav

### WebDav file explorer

using Typescript and https://github.com/perry-mitchell/webdav-client
