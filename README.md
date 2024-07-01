## colourmail

colourmail is a mail sending tool that will handle ANSI colour escapes.  The
interface is similar to the mail-sending interface of the [BSD mail tool][mail].

### Installation

Just copy the script into your PATH, e.g.

    sudo cp mtimewait /usr/local/bin/

You'll also need the `mime-construct`, `ansi2txt` and `ansi2html` tools.  On
Debian, these are provided by the "[mime-construct][mimeconstruct]" and
"[colorized-logs][colorizedlogs]" packages.  If these aren't present, the
script will fall back to trying to use the normal system [`mail`][mail]
command.

### Usage

    colourmail [-BEt] [-a <header>] [-b <bcc>] [-c <cc>] [-r <from>] [-s <subject>] -- <to>...

`colourmail` accepts most mail-sending arguments that [`mail`][mail] accepts, and has a
few extras of its own.

#### `mail` arguments

-   `-a <header>`: Add/redefine a header.
-   `-b <bcc>`: Add a BCC recipient.
-   `-c <cc>`: Add a CC recipient.
-   `-r <from>`: Set the sender.
-   `-s <subject>`: Set the subject.

#### Extra arguments

-   `-B`: Set the background to be black.  How well or badly this gets rendered
    seems to vary significantly between mail readers.
-   `-E`: Don't send anything if stdin is empty
-   `-t`: Test whether the necessary executables (`mime-construct`, `ansi2txt`
    and `ansi2html`) are present, and exit with a return code of 0 if they are,
    or 69 (`EX_UNAVAILABLE`) if not.  The script will never send any mail if
    this option is specified.

### Example

I have systemd set up to send me an email whenever a service fails, with a unit
file called `mail-state@.service` similar to the following:

```INI
[Unit]
Description=Unit %i state report

[Service]
Type=oneshot
Environment=SYSTEMD_COLORS=True
Environment=SYSTEMD_URLIFY=False
Environment=UNIT=%i
Environment=USER=%u
ExecStart=bash -euc 'systemctl status "$$UNIT" | colourmail -s "Unit $$UNIT $$(systemctl show -PActiveState "$$UNIT")" -- "$$USER"'
KillMode=process
```

This gets called automatically whenever any service unit fails by having the
following in `/etc/systemd/system/service.d/mail-failure.conf`:

```INI
[Unit]
OnFailure=mail-state@%n.service
```

Similar things will work if you want to know a unit has successfully terminated
by configuring `OnSuccess=`, or just for knowing a unit has activated by
configuring `Wants=` and `Before=`.

The above would clearly work without the colours and using the normal `mail`
tools, but I found it easier to read with systemd's colour output.  After all,
there's a reason it's in the terminal output!

[mail]: https://man.freebsd.org/cgi/man.cgi?format=html&query=mail%281%29
[mimeconstruct]: http://www.argon.org/~roderick/
[colorizedlogs]: https://github.com/kilobyte/colorized-logs
