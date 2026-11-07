# BrutusIPTV : webapp pour iPhone

Une seule mise en ligne (5 min, depuis le téléphone), puis l'app s'ouvre depuis l'écran d'accueil comme une vraie app.
Il n'y a rien à renouveler et aucun ordinateur à utiliser.

## 1. Mettre la webapp en ligne (une seule fois)

1. Sur l'iPhone, dans l'app **Fichiers**, touche le zip reçu pour le décompresser.
2. Dans Safari, va sur **github.com** et crée un compte gratuit.
3. Touche **+ → New repository**, nomme-le `brutus`, laisse-le **Public**, puis touche **Create repository**.
   GitHub Pages n'est gratuit qu'avec un dépôt public. Seul le code de l'app y est visible : tes identifiants restent uniquement dans ton téléphone.
4. Touche **uploading an existing file**, choisis les 5 fichiers du dossier (`index.html`, `manifest.webmanifest`, `icon-180.png`, `icon-192.png`, `icon-512.png`), puis touche **Commit changes**.
5. Va dans **Settings → Pages**. Sous *Branch*, choisis **main**, puis **/(root)**, et touche **Save**.
6. Patiente environ 1 minute. L'adresse de ton app apparaît en haut de la page : `https://TON-PSEUDO.github.io/brutus/`.

## 2. L'installer sur l'écran d'accueil

1. Ouvre cette adresse dans **Safari**.
2. Touche le bouton **Partager**, puis **Sur l'écran d'accueil**, puis **Ajouter**.
3. Lance BrutusIPTV depuis l'icône : l'app s'ouvre en plein écran, sans la barre de Safari.
4. Touche **+**, puis **Xtream Codes**, et entre ton serveur, ton utilisateur et ton mot de passe.

## Utilisation

- **Toucher une chaîne** la lance. Un **appui long** sur une chaîne permet de l'ajouter aux favoris ou d'ouvrir son guide TV.
- **Tourner le téléphone** passe en plein écran. Touche la vidéo pour afficher les infos, puis **☰ Chaînes** pour zapper sans quitter le plein écran.
- **⚽ Match** affiche le match en direct, les stats, les temps forts, les joueurs avec la note Brutus, et la chaîne de ta liste qui le diffuse.
- **⚙** ouvre les réglages : le format du direct (garde **HLS**, c'est celui que l'iPhone lit), la qualité automatique et les logos.

## Si la connexion est refusée

Certains serveurs IPTV bloquent les pages web. Dans ce cas, l'app affiche « connexion refusée ».
La solution est un petit relais gratuit, à créer une seule fois depuis le téléphone :

1. Crée un compte sur **dash.cloudflare.com**, puis va dans **Workers → Create → Hello World** et nomme-le `brutus`.
2. Touche **Edit code**, remplace tout le code par celui ci-dessous, puis touche **Deploy** :

```js
export default {
  async fetch(req) {
    const url = new URL(req.url).searchParams.get('url');
    if (!url) return new Response('url manquante', { status: 400 });
    const r = await fetch(url, { headers: { 'User-Agent': 'Mozilla/5.0 BrutusIPTV/3.0' } });
    const h = new Headers(r.headers);
    h.set('Access-Control-Allow-Origin', '*');
    return new Response(r.body, { status: r.status, headers: h });
  }
};
```

3. Dans BrutusIPTV, ouvre **⚙ → Relais** et colle `https://brutus.TON-PSEUDO.workers.dev/?url=`.

## Limites de l'iPhone

- Les films en **.mkv** ne se lisent pas dans Safari. Les .mp4 et le direct en HLS fonctionnent.
- La mémoire tampon est gérée par l'iPhone et ne se règle pas.
- Les playlists et favoris sont enregistrés dans le téléphone. Si tu supprimes l'icône de l'écran d'accueil, ils sont effacés.
