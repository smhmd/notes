back up your `/etc/wpa_supplicant.conf` if you have one.
Use `locate` from the `mlocate` package to `locate wpa_supplicante.conf`.

```
$ wpa_passphrase your_wifi_SSID_without_quotes "your password in quotes" > /etc/
```

This will put a network configuration inside `wpa_supplicant.conf` that contains your SSID, a commented-out password, and a calculated (from your password) pre-shared key (psk).

```
$ cat /etc/wpa_supplicant.conf
network={
        ssid="your_wifi_SSID_without_quotes"
        #psk="your password in quotes"
        psk="a calculated pre-shared key"
}
```

Now, run `ipconfig` or `ip addr` to determine your interface name (something like `wlan0` -- mine was `wlp0s29u1u7`.)

Ultimately, run the following, replacing `wlp0s29u1u7` with your interface:

```
$ wpa_supplicant -B -Dnl80211 -i wlp0s29u1u7 -c /etc/wpa_supplicant.conf
```

If it fails with the reason being `wrong_key`, reboot your system and try again.
If it fails for another reason, swap `-Dnl80211` with `-Dwext`, or remove it all together.
The `-B` flag is important.

If it succeeds, wait 15 seconds and run `dhcpcd wlp0s29u1u7` or whatever your interface was.

`ping google.com` to verify your connection.

