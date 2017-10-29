# Java Applet Player

This is a Debian Stretch Container with installed Firefox, OpenJRE 8 and Java Applet.

Use this on Platforms where you don't want to install a JRE or need to use Java Applets and want to avoid using them directly.

The Security Settings of Icedtea Plugin set to LOW. So be careful and maybe only use this container when you trust the client.
For example a Hardware Appliance or something similar.

It's based on __DesktopContainers/base-debian__

## Environment variables and defaults

- __WEB\_URL__
    - specify the url the browser will point to by default e.g. the citrix login portal url

## Usage: Run the Client

### Simple SSH X11 Forwarding

Since it is an X11 GUI software, usage is in two steps:
  1. Run a background container as server or start existing one.

```
docker start jap || docker run -d --name jap \
-e 'WEB_URL=https://my-trusted-client/' desktopcontainers/java-applet-player
```

  2. Connect to the server using `ssh -X` (as many times you want).
     _Logging in with `ssh` automatically opens a firefox window_

```
ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no \
-X app@$(docker inspect --format '{{ .NetworkSettings.IPAddress }}' jap)
```

  3. Browse to your Java Applet providing server, start the client and enjoy.

You can configure firefox and set bookmarks. As long as you don't remove the container and you reuse the same container, all your changes persist. You could also tag and push your configuration to a registry to backup (should be your own private registry for your privacy).
