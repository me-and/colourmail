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

[mail]: https://man.freebsd.org/cgi/man.cgi?format=html&query=mail%281%29
[mimeconstruct]: http://www.argon.org/~roderick/
[colorizedlogs]: https://github.com/kilobyte/colorized-logs
