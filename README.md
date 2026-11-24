<div align="center">

<img width="128" height="128" alt="logo" src="https://github.com/user-attachments/assets/034ddeb3-1684-41fc-8413-60e6d1b1b8b3" />

# TECHMIS

**// RIEN D'OUBLIÉ**

Outil de **validation finale d'intervention** pour technicien — C#/WPF, 100 % natif Windows.

<p align="center">
<a href="https://github.com/ofbridge-lab/Techmis/releases"><img src="https://img.shields.io/badge/version-v1.1.0-41B375?style=flat-square&labelColor=0B0C14" alt="Version" /></a>
<img src="https://img.shields.io/badge/Windows-10%20%2F%2011-00D4FF?style=flat-square&labelColor=0B0C14&logo=windows&logoColor=white" alt="Windows 10 / 11" />
<img src="https://img.shields.io/badge/.NET%20Framework-4.8-7B4FFF?style=flat-square&labelColor=0B0C14" alt=".NET Framework 4.8" />
<a href="https://linktr.ee/ofbridgelab"><img src="https://img.shields.io/badge/Ofbridge%20Lab-blog-FF8C00?style=flat-square&labelColor=0B0C14" alt="Ofbridge Lab" /></a>
</p>

</div>

---

Techmis fait passer un PC (portable ou fixe) par une checklist de contrôle complète après une
réparation : matériel, périphériques, capteurs, réseau — pour ne rien oublier avant de le rendre
au client. Parcours **guidé par questionnaire** (seuls les tests pertinents pour l'intervention
sont proposés) ou **diagnostic libre** (tous les tests compatibles avec le matériel détecté),
suivi d'un **bilan HTML** déposé sur le Bureau, prêt à imprimer ou à remettre au client.

> 🎯 **Fini le « zut, j'ai oublié de tester le micro ».** Techmis déroule la checklist à ta place
> et sort un bilan propre, signé, à joindre au dossier ou à donner au client.

<div align="center">
<img width="780" height="470" alt="accueil" src="https://github.com/user-attachments/assets/9e414f7e-4ea9-4865-9c40-74f3be60a99e" />
</div>

## 📑 Sommaire

- [✨ Fonctionnalités](#-fonctionnalités)
- [🧪 Les 20 tests](#-les-20-tests)
- [📦 Installation & utilisation](#-installation--utilisation)
- [🧰 Notes techniques](#-notes-techniques)
- [📜 Historique des versions](#-historique-des-versions)

---

## ✨ Fonctionnalités

- **Parcours guidé** : un questionnaire Oui/Non adapté au type de machine (portable/fixe) et à la
  réparation effectuée n'active que les tests pertinents.
- **Diagnostic libre** : tous les tests compatibles avec le matériel détecté, sans questionnaire —
  le technicien choisit lui-même quoi tester.
- **Détection matérielle automatique** : châssis (portable/fixe), GPU dédié, écran tactile,
  accéléromètre, lecteur d'empreinte, Wi-Fi/Ethernet — un test n'apparaît que si le matériel existe.
- **Activation Windows** : contrôle l'état d'activation et le canal de licence (OEM / Retail /
  Volume), ajouté automatiquement aux cas **réinstallation de Windows** et **changement de carte
  mère** — là où une licence liée au matériel peut sauter.
- **Test clavier multi-profils** : 6 dispositions (13" compact → Full ISO 100 %), diagonale d'écran
  détectée via EDID (WMI), changement de profil en direct, validation par groupes pondérés
  (critique / secondaire / informatif) pour ne pas faire échouer un clavier fonctionnel sur des
  touches non universelles (F-keys en mode média, Ctrl droit absent sur portable, etc.). Couleur
  d'appui distincte du validé → repère une **touche bloquée**.
- **Wi-Fi sans connexion** : scan forcé via l'API Native WiFi — vérifie la réception des antennes
  après réparation, même hors réseau.
- **Contrôle date/heure en ligne** : référence [time.is](https://time.is/), écart important signalé
  en erreur ; **hors connexion**, bascule sur un contrôle manuel ; en cas d'erreur, proposition
  d'ouvrir directement les réglages Windows de date/heure.
- **Bouton « Passer »** : marque un test non réalisable (ex. prise RJ45 nécessitant un adaptateur
  spécial absent) avec une raison — documenté au rapport, **sans le compter comme un défaut**.
- **Commentaire libre** : note du technicien (gras, italique, surligner, couleurs, correction
  orthographique FR) affichée en tête du bilan.
- **Bilan HTML** : verdict global (conforme / à revoir / défaut détecté), infos machine, tests
  regroupés par catégorie, icônes par test, **thème sombre à l'écran et version claire à
  l'impression** (icônes lisibles en noir & blanc). Déposé sur le **Bureau du PC testé**.
- **Élévation à la demande** : l'appli tourne sans droits admin ; seul le test de refroidissement
  relance une instance élevée le temps de lire la température (UAC).

---

## 🧪 Les 20 tests

| Catégorie | Tests |
|---|---|
| **Automatiques** | Pilotes manquants, Date & heure, Secure Boot, **Activation Windows**, GPU dédié, Stockage, Mémoire (RAM), Wi-Fi (RSSI), Ethernet, Lecteur d'empreinte |
| **Interactifs** | Dalle / pixels morts, Écran tactile, Clavier, Souris / trackpad, Haut-parleurs G/D, Micro (VU-mètre), Webcam (aperçu live), Accéléromètre, Batterie & chargeur |
| **Capteur** | Refroidissement (charge CPU/GPU + température ACPI + confirmation ventilateur) |

---

## 📦 Installation & utilisation

1. Télécharger le paquet `Techmis.zip` (voir [Releases](../../releases) ou `dist\Techmis\` après
   compilation).
2. Décompresser où vous voulez — **aucune installation requise**.
3. Lancer `Techmis.exe`.

> Le **bilan HTML** est enregistré sur le **Bureau du PC testé** (repli sur `output\` si le bureau
> est indisponible). Les journaux sont écrits dans `logs\`.

## 🧰 Notes techniques

- **Aucune dépendance LibreHardwareMonitor** : la température CPU passe par WMI ACPI
  (`MSAcpi_ThermalZoneTemperature`, admin requis, sans pilote tiers).
- **Activation** : lue en local via WMI `SoftwareLicensingProduct` (aucune connexion requise).
- **Webcam** : aperçu `getUserMedia` embarqué via **WebView2** (repli navigateur si indisponible).
- **Micro** : niveau capté via **winmm/waveIn** (P/Invoke), réagit en temps réel.
- **Wi-Fi** : scan forcé via l'**API Native WiFi** (`wlanapi`), indépendant de toute connexion.
- **Accéléromètre** : WinRT `Windows.Devices.Sensors.Accelerometer` par réflexion tardive (aucune
  métadonnée WinRT à ajouter au build).
- **Écran interne vs externe** (clavier) : le panneau intégré est repéré via
  `WmiMonitorConnectionParams.VideoOutputTechnology`, un moniteur externe branché est ignoré.

---

## 📜 Historique des versions

Voir [`CHANGELOG.md`](CHANGELOG.md) et les [Releases](../../releases).

---

<div align="center">
<sub>
<a href="https://ofbridgelab.blogspot.com">Blog</a> ·
<a href="https://github.com/ofbridge-lab/Techmis/releases">Téléchargements</a> ·
<a href="https://linktr.ee/ofbridgelab">Linktree</a>
</sub>

<br/>

<sub><b>TECHMIS · OFBRIDGE LAB</b> — <i>// RIEN D'OUBLIÉ</i></sub>
</div>
