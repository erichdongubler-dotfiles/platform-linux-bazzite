# `platform-bazzite-linux`

Erich's configuration for his Bazzite Linux machines, according to his own
[standard](https://github.com/erichdongubler-dotfiles/standard) for
configuration.

This implements `standard` version 0.2.5.

## `minimal`

You're already mostly done! `git` and `ssh` are on Bazzite's OS image by
default. You just need `brew install gpatch` now (which is included in
`productive`'s step to install from `./Brewfile`, if you just want to follow
that).

## `productive`

1. Use the `Brewfile` adjacent to this `README` to install the majority
   of preferred binarines/application:

   ```nushell
   brew bundle install --file ./Brewfile
   ```

1. Finish installing a stable Rust toolchain from the `rustup` Brew package:

   ```nushell
   rustup toolchain install stable
   ```

   N.B. that binaries from this install are _not_ in `~/.cargo/bin/`, like
   a typical Rust installation.

1. `cargo-binstall` the remaining Rust binaries that have no `brew` formulae
   in `./cargo-binstall-pkgs.txt`:

   ```nushell
   cargo-binstall -y ...(
     open ./cargo-binstall-pkgs.txt
       | lines
       | each { str trim }
       | where { not ($in | is-empty) and not ($in | str starts-with '#') }
       | lines
       | each { str replace --regex '(.*?)(\#.*)?' '$1' }
       | each { str trim }
       | where { is-not-empty }
   )
   ```

   N.B. that binaries installed by this _are_ in `~/.cargo/bin/`.

1. Set up `espanso` manually,

   1. Install the `espanso` binary.

      Combine Espanso upstream's instructions for installing from Terra repo
      on Fedora <https://espanso.org/docs/install/linux/#terra-wayland> the
      Terra's instructions for Bazzite:
      <https://docs.terrapkg.com/usage/installing/#bazzite>. A script to do
      this is as follows:

      ```nushell
      sudo sed -i '/^\[terra\]/,/^\[/s/enabled=0/enabled=1/' /etc/yum.repos.d/terra.repo
      for pkg in [wl-clipboard wxGTK espanso-wayland] {
         rpm-ostree install $pkg
      }
      sudo systemctl reboot
      ```

   1. Set up `espanso` to run as a `systemd` service for your user:

      ```nushell
      espanso service register
      systemctl --user start espanso
      ```

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
