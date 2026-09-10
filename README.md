# `platform-bazzite-linux`

Erich's configuration for his Bazzite Linux machines, according to his own
[standard](https://github.com/erichdongubler-dotfiles/standard) for
configuration.

This implements `standard` version 0.2.4.

## `minimal`

You're already mostly done! `git` and `ssh` are on Bazzite's OS image by
default. You just need `brew install gpatch` now (which is included in
`productive`'s step to install from `./Brewfile`, if you just want to follow
that).

## `productive`

1. Install the Rust toolchain via <https://rustup.rs/>.
   1. `cargo install cargo-binstall`
   1. `cargo binstall bellboy`
1. Use the `Brewfile` adjacent to this `README` to install the remainder
   of preferred binarines/applications via `brew bundle install`.
1. Install `espanso`. As of Bazzite 44, this is somewhat complicated, and may
   need to be repaired between releases.

   1. Install dependencies:

      ```sh
      rpm-ostree install wl-clipboard wxGTK
      ```

   1. Install the binary from TODO

   1. Set capabilities the binary needs:

      ```nushell
      sudo setcap "cap_dac_override+p" (which espanso | get path)
      ```

   1. Restart, so `rpm-ostree` can take effect.

1. Set up `zellij web` to auto-start by:

   ```nushell
   # In this project's root:
   cp ./zellij-web.service ~/.config/systemd/user/
   systemctl --user enable zellij-web
   systemctl --user start zellij-web
   ```

## `full`

### KDE

1. In `Settings` > `Apps & Windows` > `Window Management` > `Window Behavior` > `Focus`, set:
   1. `Window activation policy` to `Focus follows mouse (mouse precedence)`
   1. `Delay focus by` to 300 ms
1. Window dragging uses the `Meta` modifier key.

### GNOME

TODO

## Fun extras

I also really like `Wobbly Windows`. 😄
