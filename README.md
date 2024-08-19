# My NeoMutt config feat. Office 365 IMAP

This is my NeoMutt config that is heavily based on https://github.com/ork/mutt-office365. The difference is that my configuration is modified to use the OAuth2 authentication method. See `scripts/mutt_oauth2/mutt_oauth2.py.README` for further information.

I have successfully used this configuration on the following systems:

- macOS 14 Sonoma (x2)
- NixOS 23.11 Tapir
- NixOS 24.05 Uakari

## Mailcap rules for displaying different MIME parts

Mailcap rules can be used for displaying certain MIME parts in NeoMutt.

### Display HTML e-mail using Lynx through mailcap

`text/html` MIME parts can be displayed using Lynx by adding the following line to `~/.mailcap`:

```
text/html; lynx -dump -display_charset=utf-8 '%s' | more; nametemplate=%s.html; copiousoutput;
```

### Display images using Kitty's icat kitten through mailcap

Image attachments can be displayed using Kitty's icat kitten by adding the following line to `~/.mailcap`:

```
image/*; echo '%s' && kitten icat '%s' && read -p "Press enter to continue";
```

## Remarks

Here are some remarks regarding getting this thing running on different systems.

### macOS

The OAuth2 script requires a rather recent version of Python 3. Something like 3.12.x should do. You can try running the script in a Nix shell with `nix-shell -p python3`.

### NixOS

Getting `gpg` to work requires the following setup: first add the system packages `gnupg` and `pinentry-curses` and then enable `programs.gnupg.agent` and configure it to use the `curses` flavor of Pinentry:

```
programs.gnupg.agent = {
  enable = true;
  pinentryFlavor = "curses";
};
```

On NixOS 24.05 Uakari the `pinentryFlavor` key does not exist. It's replaced by `pinentryPackage = pkgs.pinentry-curses`.

