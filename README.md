# Bot Forces Spéciales

## Configuration du bot

Le bot utilise Firebase Authentication, sans clé privée de compte de service.

1. Conserve le nouveau jeton Discord dans `DISCORD_TOKEN` du fichier `.env`.
2. Crée un compte Firebase dédié avec une adresse e-mail vérifiée. Le compte doit être approuvé dans le portail et avoir l'autorisation de l'onglet ENA.
3. Renseigne son adresse dans `FIREBASE_AUTH_EMAIL` et son mot de passe dans `FIREBASE_AUTH_PASSWORD` du fichier `.env`. Ne partage jamais ce fichier.
4. Publie les règles de `C:\Users\pcace\Downloads\firebase-rules.json` dans Firebase Realtime Database.
5. Depuis un terminal ouvert dans ce dossier, lance `npm start`.

Les demandes sont créées uniquement par un compte Firebase connecté, vérifié, approuvé et autorisé à l'onglet ENA. Les règles n'autorisent pas les écritures publiques. Le compte du bot ne peut créer que des demandes « En attente ».

## Carte d'identité RP

Un responsable du serveur lance `/identite-configurer` et choisit le rôle des instructeurs, le salon public du panneau et un salon privé réservé aux instructeurs. Cette commande ne publie rien. Quand la configuration est prête, le responsable lance `/identite-publier` pour mettre le panneau en ligne. Le salon de traitement doit être invisible pour `@everyone` et visible par le rôle instructeur. Le bot doit pouvoir voir, lire l'historique et envoyer des messages dans les deux salons.

Les militaires et civils utilisent le bouton du panneau pour soumettre uniquement leur état civil. Le grade, le matricule et l'affectation ne sont pas demandés au demandeur : un membre du rôle configuré ouvre la demande, peut corriger tous les champs de la carte, puis complète les informations du dossier ou refuse la demande.

La configuration et les demandes en attente sont conservées dans `identity-state.json` sur la machine du bot ; ce fichier est ignoré par Git. Après validation, le bot synchronise le dossier dans `effectifs` et la carte civile dans `cartesIdentite`, avec le compte Firebase configuré dans `.env`. Ce compte doit être vérifié, approuvé et autorisé à l'onglet ENA.

Dans le portail, le personnel disposant de l'accès ENA peut modifier les champs civils depuis la fiche militaire ; les changements sont enregistrés dans Firebase et visibles par les autres sessions. Le nom, le grade, le matricule et l'affectation du dossier restent synchronisés dans `effectifs`. La copie JSON jointe par Discord est un export ponctuel et ne se met pas à jour après coup. Utilise uniquement des informations fictives de roleplay, notamment pour l'adresse et la date de naissance, et protège le fichier local.

Après validation, le militaire ou civil peut utiliser `/carte-identite` pour afficher sa carte personnelle dans une réponse éphémère, visible uniquement par lui. Le bot retrouve la fiche grâce à l’identifiant Discord conservé dans `effectifs` et lit les données actuelles dans Firebase.
