<style>
  .lang-toggle { text-align: right; margin-bottom: 1em; }
  .lang-toggle button {
    cursor: pointer; padding: 4px 10px; border: 1px solid #888; background: transparent;
    color: inherit; font-weight: bold; border-radius: 4px;
  }
  .lang-toggle button.active { background: #888; color: #fff; }
  html[data-ui-lang="de"] [data-lang="en"],
  html[data-ui-lang="en"] [data-lang="de"] { display: none !important; }
  html:not([data-ui-lang]) [data-lang="de"] { display: none !important; }
</style>

<div class="lang-toggle">
  <button type="button" data-set-lang="de">DE</button>
  <button type="button" data-set-lang="en">EN</button>
</div>

<div data-lang="en" markdown="1">

# About

Gas Connect for ESPHome is a pulse sensor for gas meters with a reed contact, with a built-in Waveshare ESP32-C3-Zero.
It ships with ESPHome pre-installed and comes in two variants that only differ in the mount for the meter.
This firmware is for both:

- **Gas Connect BK for ESPHome** – fits e.g. Elster/Honeywell BK-G2.5–G25
- **Gas Connect PF for ESPHome** – fits e.g. RS/2001, RS/2.0, RST/2.0 and RS2,4 (G1.6–G10) by Pietro Fiorentini, Ernst Heitland and Samgas

Website: [NG3D-Druck.de](https://ng3d-druck.de)

# Installation

You can use the button below to install the pre-built firmware directly to your device via USB from the browser.

1. **Start the device in flash mode:** press and hold the **BOOT** button, plug in the USB-C cable,
   then release the button. This is only needed for the first installation.
2. Click **Install**, select the serial port of the device (e.g. "USB JTAG/serial debug unit") and confirm.
   Wait until the installation has finished.
3. **Restart the device normally:** unplug the USB-C cable and plug it back in, this time *without* pressing **BOOT**.
4. **Set up Wi-Fi:** click **Install** again and select the port. The installer detects the firmware and offers
   **Configure Wi-Fi**, where you enter your Wi-Fi name and password.
   Alternatively, set up Wi-Fi via Bluetooth with the Home Assistant app, or connect to the hotspot
   `gas-connect-xxxxxx` and open `http://192.168.4.1`.
5. **Add it to Home Assistant:** the device is discovered automatically under
   *Settings → Devices & services*. Set **Pulse volume** if needed, then **Set meter reading**
   to the current value of your meter.

**Was the device added to Home Assistant before?** Then delete the old entry under
*Settings → Devices & services → ESPHome* and restart Home Assistant. Otherwise Home Assistant asks for an encryption
key when adding it.

</div>

<div data-lang="de" markdown="1">

# Über

Gas Connect for ESPHome ist ein Impulssensor für Gaszähler mit Reedkontakt, mit eingebautem Waveshare ESP32-C3-Zero.
ESPHome ist bereits vorinstalliert. Es gibt ihn in zwei Varianten, die sich nur in der Halterung für den Zähler
unterscheiden. Diese Firmware ist für beide:

- **Gas Connect BK for ESPHome** – passend z. B. für Elster/Honeywell BK-G2,5–G25
- **Gas Connect PF for ESPHome** – passend z. B. für RS/2001, RS/2.0, RST/2.0 und RS2,4 (G1,6–G10) von Pietro Fiorentini, Ernst Heitland und Samgas

Webseite: [NG3D-Druck.de](https://ng3d-druck.de)

# Installation

Mit dem Button unten kannst du die fertige Firmware direkt aus dem Browser per USB auf dein Gerät installieren.

1. **Gerät im Flash-Modus starten:** die **BOOT**-Taste gedrückt halten, das USB-C-Kabel einstecken
   und die Taste dann loslassen. Das ist nur bei der ersten Installation nötig.
2. Auf **Installieren** klicken, den seriellen Port des Geräts auswählen (z. B. „USB JTAG/serial debug unit“)
   und bestätigen. Warten, bis die Installation abgeschlossen ist.
3. **Gerät normal neu starten:** das USB-C-Kabel abziehen und wieder einstecken, diesmal *ohne* **BOOT** zu drücken.
4. **WLAN einrichten:** erneut auf **Installieren** klicken und den Port auswählen. Der Installer erkennt die Firmware
   und bietet **Configure Wi-Fi** an, dort WLAN-Name und Passwort eingeben.
   Alternativ das WLAN per Bluetooth mit der Home-Assistant-App einrichten oder mit dem Hotspot
   `gas-connect-xxxxxx` verbinden und `http://192.168.4.1` öffnen.
5. **In Home Assistant hinzufügen:** Das Gerät wird unter *Einstellungen → Geräte & Dienste* automatisch gefunden.
   Bei Bedarf **Pulse volume** anpassen und dann bei **Set meter reading** den aktuellen Zählerstand eintragen.

**War das Gerät schon einmal in Home Assistant eingebunden?** Dann den alten Eintrag unter
*Einstellungen → Geräte & Dienste → ESPHome* löschen und Home Assistant neu starten. Sonst fragt Home Assistant beim
Hinzufügen nach einem Verschlüsselungsschlüssel.

</div>

<esp-web-install-button manifest="firmware/gas-connect.manifest.json">
  <button slot="activate">
    <span data-lang="en">Install</span><span data-lang="de">Installieren</span>
  </button>
  <span slot="unsupported">
    <span data-lang="en">Your browser does not support installing via USB. Please use Google Chrome or Microsoft Edge on a desktop computer.</span>
    <span data-lang="de">Dein Browser unterstützt keine Installation per USB. Bitte nutze Google Chrome oder Microsoft Edge auf einem Desktop-Computer.</span>
  </span>
  <span slot="not-allowed">
    <span data-lang="en">The installation is only possible from a secure (HTTPS) page.</span>
    <span data-lang="de">Die Installation ist nur über eine sichere (HTTPS-)Seite möglich.</span>
  </span>
</esp-web-install-button>

<p data-lang="en"><small>Note: the dialogs of the installer itself are provided by ESP Web Tools and are only available in English.</small></p>
<p data-lang="de"><small>Hinweis: Die Dialoge des Installers selbst stammen von ESP Web Tools und sind nur auf Englisch verfügbar.</small></p>

<script type="module" src="https://unpkg.com/esp-web-tools@10/dist/web/install-button.js?module"></script>

<script>
  (function () {
    var root = document.documentElement;
    function setLang(lang) {
      root.setAttribute("data-ui-lang", lang);
      root.setAttribute("lang", lang);
      document.querySelectorAll("[data-set-lang]").forEach(function (b) {
        b.classList.toggle("active", b.getAttribute("data-set-lang") === lang);
      });
      try { localStorage.setItem("lang", lang); } catch (e) {}
    }
    var saved = null;
    try { saved = localStorage.getItem("lang"); } catch (e) {}
    var browser = (navigator.language || "en").toLowerCase().indexOf("de") === 0 ? "de" : "en";
    setLang(saved === "de" || saved === "en" ? saved : browser);
    document.querySelectorAll("[data-set-lang]").forEach(function (b) {
      b.addEventListener("click", function () { setLang(b.getAttribute("data-set-lang")); });
    });
  })();
</script>
