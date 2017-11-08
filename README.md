# mutter

Mutt email client configuration

## Requirements

* Mutt
* Vim (used as text editor for composing emails)
* Lynx (used to view HTML email content)

## Installation

```None
git clone https://github.com/hackermd/mutter ~/.mutt
```

## Configuration

Add the following to your `~/.mailcap` file (see [MIME content types](https://en.wikipedia.org/wiki/Media_type)):

```None
text/html; lynx -assume_charset=%{charset} -display_charset=utf-8 -dump %s; nametemplate=%s.html; copiousoutput
```

## Encrypted password file

Passwords are encrypted using [GnuPG](https://gnupg.org/).

The example below demonstrates how to generate a file that contains the password for the GMAIL account.
Replace `<password>` and `<identity>` with your password and the name associated with your public key that should be used to encrypt the file, respectively. Note: Custom Mutt variables must follow the pattern `my_*`.

```None
cd /tmp
echo "set my_pass = '<password>'" > pass
gpg --recipient '<identity>' --encrypt pass
rm pass
mv pass.gpg $HOME/.pass.gpg
```

## Email accounts

The `muttrc` file configures two accounts: `outlook` and `gmail`.

### GMAIL account template

Template for file `./accounts/gmail`:

```bash
set from = '<username>@gmail.com'

set folder = 'imaps://imap.gmail.com:993/'
set imap_user = '<username>@gmail.com'
set imap_pass = $my_gmail_pass

set smtp_url = "smtp://<username>@smtp.gmail.com:587"
set smtp_pass = $imap_pass
set smtp_authenticators = 'gssapi:login'

set spoolfile = +INBOX
unset record
unset postponed

color status cyan default

account-hook $folder "set imap_user=$imap_user imap_pass=$imap_pass"
```


### Outlook account template

Template for file `./accounts/outlook`:

```bash
set from = '<username>@<domain>'

set folder = 'imaps://outlook.office365.com:993'
set imap_user = '<username>@<domain>'
set imap_pass = $my_outlook_pass

set smtp_url = "smtp://<username>@<domain>@outlook.office365.com:587"
set smtp_pass = $imap_pass
set smtp_authenticators = 'login'

set spoolfile = +INBOX
set record = '+Sent'
set postponed = '+Draft'

color status magenta default

account-hook $folder "set imap_user=$imap_user imap_pass=$imap_pass"
```
