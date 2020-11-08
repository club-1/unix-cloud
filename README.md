# Propose a cloud application to our users.

## Concept

For many little servers, like the [chatons](https://chatons.org/fr), who are
looking for a *google drive*, or *drop box* alternative tool for ther users, the
choice is often to use something like *NextCloud*, that is a fully featured
software (groupware). On the other hand, the only *web file managers*
we have found are meant to be manually installed by each users.
We are actually thinking of an intermediate form of web tool for managing files,
closer to the OS (linux) the users are connected on. This is part of a
reflexion about transparency of how a tool work. Instead of letting the cloud
be an unreal, boundaryless space, we prefer to bound it with the more trivial
idea of users sharing an existing hardware located somewhere. We would like to
have the fewest possible layers beetwen the tool interface and how it really
works without sacrificing the user experience.

This application is meant to be used on a unix based server having multiple
users registered. The idea is that every user should be able to access his or
her directory using the web browsner in addition to CLI or FTP softwares.


## Features

- [ ] access user unix personal folder within a modern and graphical UI
    - [ ] responsive
    - [ ] gallery of images
- [ ] basic navigation and files/folders edition (rename, copy, move)
- [ ] trash ?
- [ ] download file
- [ ] one click file or folder publishing link
- [ ] synchronization with multiple devices
- [ ] fast
- [ ] open with... button to edit a file with other hosted apps


## Implementation requirements:

- the file publication system is using symbolic links
- the root of the explorer must be the unix user's home.
- compatible with LDAP and PAM
- synchronization with WebDav
- generic enough to be portable across diferent unix installs
- no database

# Propositions:

## WebDav server + web interface

two domains are used in this proposition:

- `webdav.club1.fr`: for the WebDav server
- `files.club1.fr`:  for the web interface

### WebDav server `webdav.club1.fr`

The WebDav server is using PHP and https://sabre.io/dav. To write the files with
the correct user, an FPM pool will be created for each user.
The PHP app will be extremely basic as the authentication is handled by Apache.
Still, it must be able to select the correct root based on the user curently
executing it.

The domain must be an apache vhost which handles LDAP http authentication and
redirects to the correct FPM pool based on the username used for the LDAP auth.

### Web interface `files.club1.fr`

This domain can be served by any webserver, it consists of two endpoints:

- `/app`:           the js file explorer app (static)
- `/public/<user>`: to access the published links (static)

#### Js file explorer app `/app`

using Typescript and https://github.com/perry-mitchell/webdav-client

#### published links `/public/<user>`

This location points to the public directory of each user, where the symbolic
links are created, whith the help of URL rewriting.
