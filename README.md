# Propose a cloud application to our users.

## Concept

For many little servers , like the [chatons](https://chatons.org/fr), who are looking for a *google drive*, or *drop box* alternative tool for ther users, the choice is often to use something like *NextCloud*, that is quite a fully featured software (groupware). On the over hand, the only *web file managers* we've founded are meant to be manualy installed by each users.
We are actually thinking of a intermediate form of web tool for managing files, more close to the OS (linux) the users are connected on. This is part of a reflexion about transaprency of how a tool work. Instead of leting the cloud being an unreal, boundaryless space, we prefer to rely it with the more trivial idea of users sharing an existing hardware located somewhere. We'd like to have the fewest possible layers beetwen the tool interface and how it really works without sacrifying experience.

This application is meant to be used on a unix based server having multiple users registered. The idea is that every user should be able to access his or her directory using the web browsner in addition to CLI or FTP softwares.


## Features

- [ ] access user unix personnal folder within a modern and graphical UI
    - [ ] responsive
    - [ ] gallery of images
- [ ] Basic navigation and files/folders edition (rename, copy, move)
- [ ] trash ?
- [ ] Download file
- [ ] One click file or folder publishing link
- [ ] Synchronization with multiple devices
- [ ] fast
- [ ] open with... button to edit a file with other hosted apps


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
