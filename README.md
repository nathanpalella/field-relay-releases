# Field Relay

Group text and voice chat that you host yourself: one small server on a PC or Raspberry Pi at home,
and a desktop app for Windows and Linux. No accounts, no tracking. Your messages stay on your server.

**This repo holds the downloads.** Get them from
[the latest release](https://github.com/nathanpalella/field-relay-releases/releases/latest).

Field Relay is in **beta**: expect rough edges, and please report what breaks to whoever runs your server.

## Join a server (friends)

1. Download the app for your computer from the latest release:
   - **Windows:** `Field.Relay_…_x64-setup.exe`. Windows may warn that it's from an unknown publisher:
     click **More info → Run anyway**.
   - **Ubuntu / Debian:** the `.deb`. Open it with Software Install, or run `sudo apt install ./Field.Relay_…_amd64.deb`.
   - **Other Linux:** the `.AppImage`. Make it executable (`chmod +x Field.Relay_…_amd64.AppImage`), then run it.
2. Open the app and paste the **invite link** the server's owner sent you.
3. Pick a name. That's it.

An invite works like a key: it's usually for one person and expires after a day. Ask for a new one if
yours stopped working. Your identity is a key stored on your computer, one per server, so there's no
password to forget.

## Host a server

You need a computer that stays on (a Raspberry Pi 4/5 with a 64-bit OS, any Linux PC, or Windows),
and access to your home router.

### 1. Router

1. Give the server computer a fixed address on your network (a "DHCP reservation" in the router's
   settings), for example `192.168.1.50`.
2. Forward port **7700**, **TCP and UDP**, to that address. TCP carries chat, UDP carries voice.
3. Set up **dynamic DNS** (No-IP, DuckDNS, or your router's own service) so friends get a name like
   `yourname.ddns.net` that follows your home IP.

### 2. Install

**Linux / Raspberry Pi** (runs as a service and starts on boot). Download
`fieldrelay-server-…-linux-x86_64.tar.gz` (PC) or `…-linux-arm64.tar.gz` (Pi), then:

```bash
tar xzf fieldrelay-server-*.tar.gz
cd fieldrelay-server-*/
sudo ./install-server.sh ./fieldrelay-server --address <your DDNS name> --address <the server's LAN address>
```

If the firewall is on: `sudo ufw allow 7700`.

**Windows.** Unzip `fieldrelay-server-…-windows-x86_64.zip` into a folder, open a terminal there and run:

```powershell
.\fieldrelay-server.exe init --address <your DDNS name> --address <the server's LAN address>
```

Then double-click `fieldrelay-server.exe` to start it (keep the window open). Allow it through
Windows Firewall when asked.

### 3. Claim it and invite people

Setup prints an **owner invite**. Open it in the Field Relay app first: that makes you the owner.
Then make invites for friends in the app (server menu → **Invites**). Tick "require approval" for a
link you post somewhere semi-public, so you can see who joins before they get in.

### 4. Settings, backups, upgrades

- Settings live in `field-relay.toml` (on Linux: `/var/lib/field-relay/field-relay.toml`). Every
  setting has a note explaining it. Restart the server after changing it.
- Back up while it runs: `fieldrelay-server backup <new folder>`. Keep a copy on another device.
- Upgrade: download the new server and run the same install command (Linux) or replace the `.exe`
  (Windows). Your data is kept.
- To point your members at new app versions, set `[clients] download_url` to
  `https://github.com/nathanpalella/field-relay-releases/releases/latest` and `latest_version` to
  the newest version.

The full hosting guide, `HOSTING.md`, is inside every server download. It covers running two servers
on one network, CGNAT, and what each setting does.

## What's in it

Text channels with markdown, replies, reactions and link previews; GIFs, custom emotes and stickers;
image and video attachments; voice rooms with echo cancellation and noise suppression; screen sharing
with watch parties; a soundboard; desktop notifications.

## Privacy

- Friends connect to the server, never to each other, so they don't see each other's IP addresses.
  Link previews and GIFs are fetched by the server too.
- Whoever runs the server can read its messages (they're encrypted on the way, not end to end).
- The server never logs IP addresses.
