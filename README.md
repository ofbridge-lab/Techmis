<div align="center">

<img width="128" height="128" alt="logo" src="https://github.com/user-attachments/assets/034ddeb3-1684-41fc-8413-60e6d1b1b8b3" />

# TECHMIS

**// RIEN D'OUBLIÉ**

Outil de **validation finale d'intervention** pour technicien — C#/WPF, 100 % natif Windows.

</div>

---

Techmis fait passer un PC (portable ou fixe) par une checklist de contrôle complète après une
réparation : matériel, périphériques, capteurs, réseau — pour ne rien oublier avant de le rendre
au client. Parcours **guidé par questionnaire** (seuls les tests pertinents pour l'intervention
sont proposés) ou **diagnostic libre** (tous les tests compatibles avec le matériel détecté),
suivi d'un **bilan HTML** prêt à imprimer ou archiver.

<div align="center">
<img width="780" height="470" alt="accueil" src="https://github.com/user-attachments/assets/9e414f7e-4ea9-4865-9c40-74f3be60a99e" />>
</div>

## Sommaire

- [Fonctionnalités](#fonctionnalités)
- [Les 19 tests](#les-19-tests)
- [Installation](#installation--utilisation)
- [Compiler depuis les sources](#compiler-depuis-les-sources)
- [Architecture](#architecture)
- [Notes techniques](#notes-techniques)
- [Historique des versions](#historique-des-versions)

## Fonctionnalités

- **Parcours guidé** : un questionnaire Oui/Non adapté au type de machine (portable/fixe) et à la
  réparation effectuée n'active que les tests pertinents.
- **Diagnostic libre** : tous les tests compatibles avec le matériel détecté, sans questionnaire —
  le technicien choisit lui-même quoi tester.
- **Détection matérielle automatique** : châssis (portable/fixe), GPU dédié, écran tactile,
  accéléromètre, lecteur d'empreinte, Wi-Fi/Ethernet — un test n'apparaît que si le matériel existe.
- **Test clavier multi-profils** : 6 dispositions (13" compact → Full ISO 100 %), diagonale d'écran
  détectée via EDID (WMI), changement de profil en direct, validation par groupes pondérés
  (critique / secondaire / informatif) pour ne pas faire échouer un clavier fonctionnel sur des
  touches non universelles (F-keys en mode média, Ctrl droit absent sur portable, etc.).
- **Contrôle date/heure en ligne** : référence [time.is](https://time.is/), écart important signalé
  en erreur ; **hors connexion**, bascule sur un contrôle manuel (confirmation technicien).
- **Bilan HTML** : verdict global (conforme / à revoir / défaut détecté), infos machine, tests
  regroupés par catégorie, icônes par test, **thème sombre à l'écran et version claire à
  l'impression** (Ctrl+P).
- **Élévation à la demande** : l'appli tourne sans droits admin ; seul le test de refroidissement
  relance une instance élevée le temps de lire la température (UAC).

## Les 19 tests

| Catégorie | Tests |
|---|---|
| **Automatiques** | Pilotes manquants, Date & heure, Secure Boot, GPU dédié, Stockage, Mémoire (RAM), Wi-Fi (RSSI), Ethernet, Lecteur d'empreinte |
| **Interactifs** | Dalle / pixels morts, Écran tactile, Clavier, Souris / trackpad, Haut-parleurs G/D, Micro (VU-mètre), Webcam (aperçu live), Accéléromètre, Batterie & chargeur |
| **Capteur** | Refroidissement (charge CPU/GPU + température ACPI + confirmation ventilateur) |

## Installation & utilisation

1. Télécharger le paquet `Techmis.zip` (voir [Releases](../../releases) ou `dist\Techmis\` après
   compilation).
2. Décompresser où vous voulez — **aucune installation requise**.
3. Lancer `Techmis.exe`.

Le bilan est généré dans `output\` ; les journaux dans `logs\` (créés au premier lancement).

## Notes techniques

- **Aucune dépendance LibreHardwareMonitor** : la température CPU passe par WMI ACPI
  (`MSAcpi_ThermalZoneTemperature`, admin requis, sans pilote tiers).
- **Webcam** : aperçu `getUserMedia` embarqué via **WebView2** (repli navigateur si indisponible).
- **Micro** : niveau capté via **winmm/waveIn** (P/Invoke), réagit en temps réel.
- **Accéléromètre** : WinRT `Windows.Devices.Sensors.Accelerometer` par réflexion tardive (aucune
  métadonnée WinRT à ajouter au build).
- **Écran interne vs externe** (clavier) : le panneau intégré est repéré via
  `WmiMonitorConnectionParams.VideoOutputTechnology`, un moniteur externe branché est ignoré.

## Historique des versions

Voir [`CHANGELOG.md`](CHANGELOG.md).

---

<div align="center">
<sub>TECHMIS · OFBRIDGE LAB</sub>
</div>
