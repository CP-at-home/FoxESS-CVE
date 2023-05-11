# Fox ESS Inverter: WiFi Password Exposure
## Bad software design decisions in "Anatel USB to WiFi Datalogger 3.0" lead to full disclosure of customer´s WiFi password

The **Anatel Smart WiFi 3.0 USB Stick** is used as datalogger device by Fox ESS Inverters. Reading the [documentation][FoxLink1] gives the typical way of setup for those devices:

- Connect to an Access Point (AP) spawned by the datalogger,
- enter given password to access datalogger´s AP,
- search for local wireless networks and show a result list to the user,
- user selects appropriate SSID and enters wireless network's password,
- datalogger is configured as client to user's wireless network - so called STA mode.

**What you expect:**
- AP of datalogger should be disabled after successful STA mode setup.
- At least you should be able to configure/remove/disable the AP.

**What really happens:**
- You end up with a datalogger device running both STA & AP mode.
- You cannot configure or disable AP. AP mode is always present regardless of working STA mode.
- You cannot set AP´s SSID name generation. It seems to follow the convention **W-1234567**, where **1234567** are the last seven digits of the serial number of the datalogger (see [documentation][FoxLink1]).
- You have a default password to access AP for the datalogger (see [documentation][FoxLink1]).
- There are no login / auth capabilities beside AP´s static password. Neither further serial number input, nor printed passwords on the device.
- Using AP´s WebInterface, you are always in STA edit mode, means: Password for customer´s WiFi is always transferred in cleartext, so you are able to see it as when you entered it initially.
- Clicking into the password field of AP´s WebInterface, you toggle the HTML input field type from *password* to *text*, clicking outside the password field toggles it back.
- This is possible due to a Websocket call which transfers the password in cleartext, together with other data in a JSON packet.

This means, anyone with knowledge of above facts may connect to a FoxESS datalogger AP and read out customer's WiFi password in cleartext! Password is fully disclosured.

See given HAR snippet in repository, recorded and anonymized.

[FoxLink1]: https://www.fox-ess.com/download/upfiles/10-203-00113-01%20WiFi3.0%20English%20Installation%20guide.pdf "Documentation"
