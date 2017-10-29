# mutter

Mutt email client configuration

## Encrypted password file

Passwords are encrypted using [GnuPG](https://gnupg.org/).

The example below demonstrates how to generate a file that contains the password for the GMAIL account.
Replace `<password>` and `<identity>` with your password and the name associated with your public key that should be used to encrypt the file, respectively.

```None
cd /tmp
echo "set my_gmail_pass = '<password>'" > pass
gpg --recipient '<identity>' --encrypt pass
rm pass
mv pass.gpg $HOME/.pass.gpg
```

## Email accounts

The `muttrc` file configures two accounts: `work` and `gmail`.

### GMAIL account template

```bash
set from = '<username>@gmail.com'

set folder = 'imaps://imap.gmail.com:993/'
set imap_user = '<username>@gmail.com'
set imap_pass = $my_gmail_pass

set smtp_url = "smtp://username@smtp.gmail.com:587"
set smtp_pass = $imap_pass

set spoolfile = +INBOX
unset record
unset postponed

color status cyan default

account-hook $folder "set imap_user=$imap_user imap_pass=$imap_pass"

set smtp_authenticators = 'gssapi:login'
```
