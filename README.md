# INC-004 — Poste ne rejoint plus le domaine

## Contexte

Un utilisateur signale qu'un poste Windows nouvellement installé ne parvient pas à rejoindre le domaine Active Directory de l'entreprise.

## Symptômes
- Impossible de joindre le domaine lors de la procédure d'intégration
- Message d'erreur : "Le domaine spécifié n'existe pas ou n'a pas pu être contacté"
- Le poste reste en groupe de travail local
- Échec de l'authentification domaine

## Diagnostic

### 1. Vérification de la connectivité réseau

Test de connectivité vers le contrôleur de domaine :
```cmd
ping 192.168.1.10
```

Résultat :

- Réponse OK -> connectivité réseau fonctionnelle

### 2. Vérification de la résolution DNS

Test de résolution du contrôleur de domaine :

```cmd
nslookup domaine.local
```

Résultat :

- Échec de résolution OU mauvaise IP DNS configurée

Vérification de la configuration IP :

```cmd
ipconfig /all
```

Observation :

- DNS pointant vers une adresse externe (ex : 8.8.8.8)

### 3. Vérification de l'accès au domaine

Test de contact avec le domaine :

``` cmd
nltest /dsgetdc:domaine.local
```

Résultat :

- Impossible de trouver un contrôleur de domaine


### 4. Vérification côté Active Directory

 Contrôle sur le serveur AD :

 - Service AD DS actifs
 - Contrôleur de domaine opérationnel
 - Aucun incident système détecté

## Cause probable

- Mauvaise configuration DNS sur le poste client
- Poste ne pointe pas vers le serveur DNS interne (contrôleur de domaine)
- Résolution du domaine impossible

## Résolution

Modification de la configuration réseau du poste :

- Configuration DNS corrigée :
-   DNS primaire -> IP du contrôleur de domaine
-   DNS secondaire -> serveur interne secondaire (si existant)

Puis :

```cmd
ipconfig /flushdns
ipconfig /renew
```

Nouvelle tentative de jonction au domaine :

- Ajout au domaine effectué avec succès

## Résultat 

- Poste correctement intégré au domaine Active Directory
- Authentification utilisateur fonctionnelle
- Accès aux ressources réseau rétabli

## Compétences démontrées

- TCP/IP
- DNS (résolution de noms)
- Active Directory
- Diagnostic réseau
- Join domain Windows
- Support utilisateur niveau 1 / niveau 2
