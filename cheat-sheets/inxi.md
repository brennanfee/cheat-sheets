# Inxi

Inxi is a tool to collect hardware information for a Linux machine.

## Installing Inxi

### Debian and derivatives (Ubuntu, Mint)

```bash
sudo apt install inxi
```

### Arch based (Arch, Manjaro)

Inxi is in the AUR, so an aur helper is generally recommended.

For yay:

```bash
yay -S inxi
```

For pamac:

```bash
pamac install inxi
```

## Command for export

I collect machine information and export it to a file so I can record the setup on my
various machines. Here is the command I use to produce consistent (diff-able) results:

```bash
sudo inxi -Fdflmopux -c 0 > <machine-name>.txt
```
