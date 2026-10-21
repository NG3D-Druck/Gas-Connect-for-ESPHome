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

Gas Connect for ESPHome is a reed contact pulse sensor for BK gas meters (Elster/Honeywell BK-G2.5 to BK-G25,
Pipersberg and compatible) with a built-in Waveshare ESP32-C3-Zero. It ships with ESPHome pre-installed.

Website: [NG3D-Druck.de](https://ng3d-druck.de)

# Installation

You can use the button below to install the pre-built firmware directly to your device via USB from the browser.

</div>

<div data-lang="de" markdown="1">

# Über

Gas Connect for ESPHome ist ein Reedkontakt-Impulssensor für BK-Gaszähler (Elster/Honeywell BK-G2.5 bis BK-G25,
Pipersberg und kompatible) mit eingebautem Waveshare ESP32-C3-Zero. ESPHome ist bereits vorinstalliert.

Webseite: [NG3D-Druck.de](https://ng3d-druck.de)

# Installation

Mit dem Button unten kannst du die fertige Firmware direkt aus dem Browser per USB auf dein Gerät installieren.

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
