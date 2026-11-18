# CLAUDE.md — Oink (ex-Écos)

## Informations du projet (vérifiées dans le dépôt)
- **Nom** : Oink (anciennement Écos) — gestion des dépenses (version affichée : `Oink · vX.Y` dans `www/index.html`)
- **Stack** : un seul fichier `www/index.html` (HTML/CSS/JS vanilla) + Capacitor 8 → APK Android. Plugins : app, filesystem, haptics, local-notifications, share, capgo native-biometric.
- **applicationId** : `be.alexandrebrun.ecos` (`capacitor.config.json`)
- **Workflow** : `.github/workflows/blank.yml` (déclenché sur push `main` ou manuellement). `android/` n'est pas versionné : régénéré à chaque build via `npx cap add android` + `scripts/patch-android.js`.
- **Signature** : keystore `android-signing/ecos-release.jks`, alias `ecos`, mot de passe via secret GitHub `KEYSTORE_PASSWORD` (store et key). Empreinte SHA-256 attendue : `58:A5:8F:91:E2:89:14:0D:F4:A6:DD:71:B7:3A:C8:12:D9:C9:34:9C:99:B3:C0:CC:0A:01:67:AC:3B:0F:4E:25`.
- **Versions** : `versionCode` = `github.run_number`, `versionName` = `1.0.<run_number>`. Version de l'app à incrémenter manuellement dans la chaîne `Oink · vX.Y` de `www/index.html`.
- **Livraison** : APK publié en GitHub Release (tag `v1.0.<run>`, asset `Oink-vX.Y-build<run>.apk`) + artifact Actions.
- **Données locales** : `localStorage` clé `ecos_v1` (objet `db` : `tx`, `cats`, `budget`, réglages…). Fonctions `load()` / `save()`. Export/import manuel via Filesystem/Share.
- **Héritage Écos à conserver** : clé `ecos_v1`, canal de notification `ecos`, icône `ic_stat_ecos`, keystore `ecos-release.jks`, `app:'ecos'` dans les sauvegardes JSON.

## Points sensibles propres à ce dépôt
- Ne jamais changer la clé `ecos_v1`, ni `server.hostname` / `androidScheme` de Capacitor : les données deviendraient inaccessibles.
- Toute évolution du format de `db` passe par une migration dans `load()` (valeurs par défaut via `Object.assign`), jamais par une réinitialisation.
- `versionCode` dépend de `run_number` : renommer/recréer le workflow ou changer de dépôt le remet à 1 → mise à jour refusée. Signaler avant toute action de ce type.
- `LISEZMOI.md` contient le mot de passe du keystore en clair : ne jamais le recopier ; proposer son retrait.
- Le workflow utilise un `index.html` racine s'il est plus récent que `www/index.html` : toujours éditer `www/index.html`.

## Collaboration
- Je décris les fonctionnalités en français ; prends en charge la technique et réponds brièvement en français.
- Comprends la demande et les fichiers concernés ; ne relis pas tout le dépôt à chaque tâche.
- Modifications ciblées, réutilise l'existant. Évite sous-agents et opérations coûteuses sans bénéfice clair.
- Décision importante ou ambiguë : propose brièvement une solution et demande confirmation.

## Sécurité des mises à jour Android
- Ne jamais changer applicationId, clé de signature, certificat ou keystore sans accord explicite.
- Vérifier la signature existante avant une mise à jour. Ne jamais générer une nouvelle clé pour remplacer une clé indisponible : signaler.
- Incrémenter versionCode pour toute mise à jour distribuée ; ajuster versionName selon la convention.
- Préserver données locales, migrations et compatibilité. Ne jamais désinstaller pour résoudre un problème de signature.
- Ne jamais publier keystore, mots de passe, jetons ou secrets dans le dépôt, les logs ou les réponses. Utiliser les secrets GitHub Actions.

## GitHub et compilation
- Respecter le workflow existant ; ne le modifier que par nécessité expliquée.
- Avant un push : résumer les fichiers modifiés et demander une autorisation explicite.
- Vérifier les tests pertinents si possible. En cas d'échec de build, analyser les lignes d'erreur utiles et corriger la cause, sans relancer en boucle.
- Ne supprimer ni historique Git, ni branche, ni données sans accord.

## Livraison de l'APK — lien direct prioritaire
- Après un build réussi, identifier l'APK final et donner en priorité le lien direct de l'asset `.apk` de la Release.
- Ne pas créer de Release ni publier sans autorisation explicite (hors Release créée automatiquement par le workflow existant).
- Si seul un artifact Actions existe : préciser que c'est un ZIP, qu'une connexion GitHub est requise et que le lien est temporaire ; donner le lien précis de l'artifact du run.
- Ne jamais fabriquer d'URL ; vérifier que le build a réussi et que le fichier existe.
- Compte rendu final : d'abord `Télécharger l'APK : [lien]`, puis version et bref résumé.

## Design et expérience utilisateur
- Interface cohérente, soignée, moderne, accessible et fluide ; priorité à la compatibilité Xiaomi 17T Pro (encoche/caméra, safe areas, barre de gestes).
- Respecter la direction artistique et les composants existants ; demander des références si une refonte majeure est ambiguë.
- Soigner typographie, espacements, états de chargement/erreur, navigation et animations.
- Ne pas refondre des écrans non concernés.

## Économie de quota et qualité
- Lire uniquement les fichiers utiles ; préférer la recherche ciblée à la lecture intégrale ; ne pas relire un fichier inchangé.
- Regrouper les modifications liées ; comptes rendus concis.
- Pas de compilations multiples, agents parallèles ou recherches externes sans raison.
- Pas de refactorisation non demandée ni de fichiers inutiles ; solutions simples et maintenables.
- Tests nécessaires uniquement, sans négliger les contrôles importants.
- Ne jamais sacrifier design, animations, UX ou fonctionnalités pour économiser des tokens.
- Ne supprimer ni modifier aucune fonctionnalité existante sans nécessité ou demande explicite.
