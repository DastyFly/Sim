# DastyFlySim — configurazione sicura

Questa versione non contiene più utenti/password nel codice HTML e usa Firebase Authentication.

## 1. Firebase Authentication

Firebase Console → Authentication → Sign-in method → abilita **Email/Password**.

Crea gli account da Authentication → Users.

## 2. Firestore

Crea un database Firestore.
Poi pubblica `firestore.rules` nelle Security Rules.

## 3. Profili utenti

Per ogni account creato in Authentication, copia il suo UID.
In Firestore crea:

`users/{UID}`

con questi campi, per esempio:

```text
name: "Nome Cognome"
role: "admin"
code: "antogi"
```

I ruoli ammessi dal gestionale sono:

- `tutor`
- `supervisor`
- `admin`

La password resta esclusivamente in Firebase Authentication e NON va inserita nel documento Firestore.

## 4. Configurazione Web

Nel file `index.html`, sostituisci i valori `INSERISCI_...` nell'oggetto `firebaseConfig` con quelli mostrati in:

Firebase Console → Project settings → Your apps → Web app → Firebase SDK configuration.

La configurazione Web può essere pubblica nel frontend. Non inserire mai service account JSON, private key o password.

## 5. GitHub Pages

Carica nel repository:

- `index.html`
- `firebase-config.js` (se usi questo file separato)
- `firestore.rules` solo come riferimento; le Rules vanno pubblicate nella console Firebase

Per GitHub Pages:
Settings → Pages → Deploy from a branch → `main` → `/ (root)`.

## Nota importante

La presente modifica mette in sicurezza l'autenticazione: non ci sono più password o lista utenti nel frontend/localStorage.

I dati operativi `sims` e `sops` del gestionale originale sono ancora salvati localmente nel browser. Per renderli realmente condivisi tra più PC e proteggerne anche le modifiche lato server, vanno migrati in documenti Firestore separati con Security Rules per ruolo.
