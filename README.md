# Dépôt F-Droid personnel — SafeFridge

Dépôt F-Droid statique pour l'app Android **SafeFridge** (gestion d'inventaire alimentaire
multi-foyer), généré avec [fdroidserver](https://gitlab.com/fdroid/fdroidserver) et publié via
GitHub Pages.

> **Nécessite Google Play Services** (authentification et synchronisation via Firebase) — ne
> fonctionnera pas sur un appareil entièrement dégooglisé.

## Ajouter ce dépôt dans le client F-Droid

Dans F-Droid → **Paramètres → Dépôts → Ajouter un dépôt** (ou via QR code / lien), renseigner :

- **URL** : `https://xavierlpro-coder.github.io/safefridge-fdroid/repo`
- **Empreinte (fingerprint)** : `2B8A177CAA6DD794896A9201C629C068E02439DE34721CDB899630AE5CE12701`

(ou l'URL combinée `https://xavierlpro-coder.github.io/safefridge-fdroid/repo?fingerprint=2B8A177CAA6DD794896A9201C629C068E02439DE34721CDB899630AE5CE12701`)

## Mettre à jour le dépôt (nouvelle release)

**Voie normale — automatisée en CI.** Pousser un tag `vX.Y.0` sur le dépôt principal SafeFridge
déclenche `.github/workflows/release-fdroid.yml` : `expo prebuild` + build Gradle de l'APK signé,
release GitHub avec l'APK en pièce jointe, puis mise à jour de **ce** dépôt (`repo/` + `metadata/`)
via une clé de déploiement SSH dédiée (accès write limité à ce seul dépôt). Rien ne se publie sans
ce push de tag délibéré.

**Voie de secours — manuelle, en local.** `config.yml` et `keystore.p12` (clé de signature du
**dépôt**, distincte de la clé de signature de l'app) ne sont **jamais committés** (voir
`.gitignore`) — à conserver hors dépôt, en lieu sûr.

1. Builder l'APK release signé côté SafeFridge (voir `docs/guide-publication.md` du dépôt principal).
2. Copier l'APK dans `repo/` sous le nom `com.safefridge.app_<versionCode>.apk`.
3. Depuis un poste avec `config.yml` + `keystore.p12` en place : `fdroid update`.
4. Committer/pousser le contenu de `repo/` et `metadata/` (ce dépôt Git).

Voir `docs/guide-publication.md` dans le dépôt principal SafeFridge pour le contexte complet.
