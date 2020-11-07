# Propose a cloud application to our users.

## Features:


## Implementation requirements:

- the app would leave under one domain, for instance `cloud.club1.fr`
- the file publication system is using symbolic links
- the root of the explorer must be the unix user's home.
- compatible with LDAP and PAM
- synchronization with WebDav
- generic enough to be portable across diferent unix installs
- no database

# Propositions:

## WebDav server + JS WebDav file explorer

The app would consist of these main endpoints:

- `/`:       a gateway landing page
- `/login`:  login using post form
- `/webdav`: the standard webdav API
- `/files`:  the js file explorer app (static)
- `/public`: to access the published links (static)

all endpoints noted static will be served directly by the web server statically,
the other ones are shared by a PHP app which checks that tokens are correct.

### Gateway landing page

This endpoint will do the following exection:

```
if token not exist or is invalid {
    display login form
}
redirect to '/files'
```

### Login

```
user = login using form values
if user exist {
    write token username in clear
    create session
}
```


### Standard WebDav API

using PHP and https://sabre.io/dav

To write the files with the correct user, an FPM pool will be created for each
user. The apache vhost will use the correct pool based on the username contained
in a cookie stored in clear.

### Js file explorer app

using Typescript and https://github.com/perry-mitchell/webdav-client
