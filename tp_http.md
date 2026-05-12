<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>TP Protocole HTTP</title>
<style>
  body {
    font-family: Georgia, serif;
    background: #fffef9;
    color: #222;
    max-width: 720px;
    margin: 0 auto;
    padding: 2rem 1.25rem 5rem;
    font-size: 16px;
    line-height: 1.8;
  }

  h1 {
    font-size: 26px;
    font-weight: normal;
    border-bottom: 2px solid #222;
    padding-bottom: 0.5rem;
    margin-bottom: 0.3rem;
  }

  .subtitle {
    color: #888;
    font-size: 14px;
    margin-bottom: 2.5rem;
  }

  h2 {
    font-size: 18px;
    font-weight: bold;
    margin-top: 2.5rem;
    margin-bottom: 0.2rem;
    color: #111;
  }

  h3 {
    font-size: 15px;
    font-weight: bold;
    color: #555;
    margin-top: 1.5rem;
    margin-bottom: 0.4rem;
  }

  p { margin: 0.5rem 0; }

  .answer {
    background: #f3f7ff;
    border-left: 4px solid #5b8dee;
    padding: 0.75rem 1rem;
    margin: 0.5rem 0 1.2rem;
    border-radius: 0 6px 6px 0;
  }

  .tip {
    background: #fffbe6;
    border-left: 4px solid #f0c040;
    padding: 0.6rem 1rem;
    margin: 0.5rem 0 1rem;
    border-radius: 0 6px 6px 0;
    font-size: 14px;
    color: #555;
  }

  code {
    font-family: 'Courier New', monospace;
    font-size: 13px;
    background: #eef0f4;
    padding: 1px 5px;
    border-radius: 3px;
    color: #c0392b;
  }

  pre {
    font-family: 'Courier New', monospace;
    font-size: 13px;
    background: #1e1e1e;
    color: #d4d4d4;
    padding: 1rem 1.25rem;
    border-radius: 8px;
    overflow-x: auto;
    line-height: 1.65;
    margin: 0.75rem 0 1.25rem;
  }

  .cm  { color: #6a9955; }
  .kw  { color: #569cd6; }
  .str { color: #ce9178; }
  .num { color: #b5cea8; }
  .fn  { color: #dcdcaa; }

  table {
    width: 100%;
    border-collapse: collapse;
    font-size: 14px;
    margin: 0.75rem 0 1.25rem;
  }
  th {
    background: #f0f0f0;
    padding: 8px 12px;
    text-align: left;
    border: 1px solid #ddd;
    font-size: 13px;
  }
  td {
    padding: 7px 12px;
    border: 1px solid #ddd;
  }
  tr:nth-child(even) td { background: #fafafa; }

  ul, ol { padding-left: 1.3rem; margin: 0.4rem 0; }
  li { margin-bottom: 0.3rem; }

  hr { border: none; border-top: 1px solid #e0e0e0; margin: 2.5rem 0; }

  .question { font-weight: bold; margin-top: 1rem; }
</style>
</head>
<body>

<h1>TP Protocole HTTP</h1>

<hr>

<!-- TP 1 -->
<h2>TP 1 — Explorer avec les DevTools</h2>

<h3>1.2 — Questions sur la requête GET vers httpbin.org/get</h3>

<p class="question">Quel est le code de statut ?</p>
<div class="answer">
  <strong>200 OK</strong> — tout s'est bien passé. Le serveur a trouvé ce qu'on lui demandait et nous renvoie une réponse.
</div>

<p class="question">Quels headers de requête sont envoyés ?</p>
<div class="answer">
  Le navigateur envoie automatiquement plusieurs informations :
  <ul>
    <li><code>Host: httpbin.org</code> — le nom du serveur contacté</li>
    <li><code>User-Agent: Mozilla/5.0 ...</code> — quel navigateur on utilise</li>
    <li><code>Accept: text/html, ...</code> — les formats qu'on accepte en réponse</li>
    <li><code>Accept-Language: fr-FR</code> — notre langue préférée</li>
    <li><code>Connection: keep-alive</code> — garder la connexion ouverte</li>
  </ul>
</div>

<p class="question">Quel est le Content-Type de la réponse ?</p>
<div class="answer">
  <code>application/json</code> — httpbin nous répond en JSON (c'est son format par défaut).
</div>

<h3>1.4 — Tableau des codes observés</h3>

<table>
  <thead>
    <tr><th>URL</th><th>Méthode</th><th>Code</th><th>Content-Type</th></tr>
  </thead>
  <tbody>
    <tr>
      <td>httpbin.org/get</td><td>GET</td><td>200 OK</td><td>application/json</td>
    </tr>
    <tr>
      <td>httpbin.org/post</td><td>POST</td><td>200 OK</td><td>application/json</td>
    </tr>
    <tr>
      <td>httpbin.org/status/201</td><td>GET</td><td>201 Created</td><td>(corps vide)</td>
    </tr>
  </tbody>
</table>

<div class="tip">
  <strong>À retenir :</strong> les codes 2xx veulent dire "succès". 4xx = erreur du client (mauvaise URL, accès refusé...). 5xx = erreur du serveur.
</div>

<hr>

<!-- TP 2 -->
<h2>TP 2 — Maîtrise de cURL</h2>

<h3>2.1 — Différence entre <code>-i</code> et <code>-v</code></h3>

<div class="answer">
  <ul>
    <li><code>-i</code> → affiche les <strong>headers de la réponse</strong> (statut, Content-Type...) en plus du corps. Très utile au quotidien.</li>
    <li><code>-v</code> → mode <strong>debug complet</strong> : on voit tout — ce qu'on envoie, ce qu'on reçoit, la connexion TLS. Pratique quand quelque chose ne marche pas.</li>
  </ul>
</div>

<h3>Exercice avancé — la commande complète</h3>

<pre><span class="cm"># On envoie un POST avec deux headers et un body JSON</span>
curl -i \
  -X POST \
  -H <span class="str">"Content-Type: application/json"</span> \
  -H <span class="str">"X-Custom-Header: MonHeader"</span> \
  -d <span class="str">'{"action": "test", "value": 42}'</span> \
  https://httpbin.org/post</pre>

<p>Ce que fait chaque option :</p>
<ul>
  <li><code>-i</code> → afficher les headers de réponse</li>
  <li><code>-X POST</code> → utiliser la méthode POST</li>
  <li><code>-H "..."</code> → ajouter un header (on peut en mettre plusieurs)</li>
  <li><code>-d '...'</code> → le corps de la requête (notre JSON)</li>
</ul>

<div class="tip">
  <strong>Sous Windows (PowerShell)</strong>, les apostrophes ne fonctionnent pas. Utilisez des guillemets doubles et échappez l'intérieur : <code>-d "{\"action\": \"test\", \"value\": 42}"</code>
</div>

<hr>

<!-- TP 3 -->
<h2>TP 3 — API REST avec JavaScript</h2>

<h3>Exercice — fetchWithRetry</h3>

<p>On veut une fonction qui réessaie automatiquement si le serveur répond avec une erreur 5xx.</p>

<pre><span class="kw">async function</span> <span class="fn">fetchWithRetry</span>(url, options = {}, maxRetries = <span class="num">3</span>) {
  <span class="kw">let</span> lastError;

  <span class="kw">for</span> (<span class="kw">let</span> attempt = <span class="num">0</span>; attempt &lt; maxRetries; attempt++) {

    <span class="cm">// On attend 1 seconde avant chaque nouvel essai (pas le 1er)</span>
    <span class="kw">if</span> (attempt &gt; <span class="num">0</span>) {
      <span class="kw">await new</span> Promise(resolve =&gt; setTimeout(resolve, <span class="num">1000</span>));
      console.log(<span class="str">`⏳ Essai ${attempt + 1} sur ${maxRetries}...`</span>);
    }

    <span class="kw">try</span> {
      <span class="kw">const</span> response = <span class="kw">await</span> fetch(url, options);

      <span class="cm">// Si le serveur répond 500, 502, 503... on réessaie</span>
      <span class="kw">if</span> (response.status &gt;= <span class="num">500</span>) {
        lastError = <span class="kw">new</span> Error(<span class="str">`Erreur serveur : ${response.status}`</span>);
        <span class="kw">continue</span>;
      }

      <span class="cm">// Pour tout le reste (200, 404...) on retourne la réponse</span>
      <span class="kw">return</span> response;

    } <span class="kw">catch</span> (err) {
      <span class="cm">// Pas de connexion internet, timeout, etc.</span>
      lastError = err;
    }
  }

  <span class="cm">// On a épuisé tous les essais</span>
  <span class="kw">throw</span> lastError;
}

<span class="cm">// Exemple d'utilisation</span>
<span class="fn">fetchWithRetry</span>(<span class="str">'https://jsonplaceholder.typicode.com/posts/1'</span>)
  .then(r =&gt; r.json())
  .then(data =&gt; console.log(data))
  .catch(err =&gt; console.error(<span class="str">' Échec :'</span>, err.message));</pre>

<div class="tip">
   <strong>Attention :</strong> <code>fetch()</code> ne déclenche PAS d'erreur quand le serveur répond 500. Il faut vérifier <code>response.status</code> soi-même. Une erreur est lancée uniquement s'il n'y a pas du tout de connexion.
</div>

<hr>

<!-- TP 4 -->
<h2>TP 4 — Headers de sécurité</h2>

<h3>Ce que chaque header protège</h3>

<table>
  <thead>
    <tr><th>Header</th><th>À quoi ça sert (simplement)</th></tr>
  </thead>
  <tbody>
    <tr>
      <td><code>Strict-Transport-Security</code></td>
      <td>Force le navigateur à toujours utiliser HTTPS pour ce site</td>
    </tr>
    <tr>
      <td><code>X-Frame-Options: DENY</code></td>
      <td>Empêche le site d'être mis dans un iframe (anti-piège à clics)</td>
    </tr>
    <tr>
      <td><code>X-Content-Type-Options: nosniff</code></td>
      <td>Le navigateur n'essaie pas de "deviner" le type d'un fichier</td>
    </tr>
    <tr>
      <td><code>Content-Security-Policy</code></td>
      <td>Définit quelles sources sont autorisées (scripts, images...)</td>
    </tr>
    <tr>
      <td><code>Referrer-Policy</code></td>
      <td>Contrôle quelles infos sont partagées quand on clique un lien</td>
    </tr>
  </tbody>
</table>

<h3>Tableau de comparaison</h3>

<table>
  <thead>
    <tr><th>Site</th><th>HSTS</th><th>X-Frame</th><th>CSP</th><th>Note</th></tr>
  </thead>
  <tbody>
    <tr>
      <td>github.com</td><td> Oui</td><td> deny</td><td> Oui</td><td>A+</td>
    </tr>
    <tr>
      <td>google.com</td><td> Oui</td><td> absent</td><td> Oui</td><td>A</td>
    </tr>
    <tr>
      <td><em>votre choix</em></td><td>—</td><td>—</td><td>—</td><td>—</td>
    </tr>
  </tbody>
</table>

<div class="tip">
   Vérifiez un site de votre choix sur <a href="https://securityheaders.com" target="_blank">securityheaders.com</a> et remplissez la dernière ligne.
</div>

<hr>

<!-- TP 5 -->
<h2>TP 5 — Cache HTTP</h2>

<h3>5.2 — Requête conditionnelle avec ETag</h3>

<pre><span class="cm"># Étape 1 : faire une première requête pour récupérer l'ETag</span>
curl -i https://httpbin.org/etag/test123

<span class="cm"># Dans la réponse vous verrez : ETag: "test123"</span>

<span class="cm"># Étape 2 : renvoyer cet ETag au serveur (avec les guillemets !)</span>
curl -i -H <span class="str">'If-None-Match: "test123"'</span> https://httpbin.org/etag/test123

<span class="cm"># Résultat attendu : 304 Not Modified → le cache est encore valide</span></pre>

<div class="tip">
   <strong>Erreur classique :</strong> oublier les guillemets autour de la valeur dans <code>If-None-Match</code>. Sans eux, le serveur répond 200 au lieu de 304.
</div>

<h3>Headers de cache recommandés par type de fichier</h3>

<table>
  <thead>
    <tr><th>Type de fichier</th><th>Header Cache-Control</th><th>Pourquoi</th></tr>
  </thead>
  <tbody>
    <tr>
      <td>Images</td>
      <td><code>public, max-age=31536000, immutable</code></td>
      <td>Rarement modifiées — on garde 1 an</td>
    </tr>
    <tr>
      <td>CSS / JS</td>
      <td><code>public, max-age=86400</code></td>
      <td>Peuvent changer — on garde 1 jour</td>
    </tr>
    <tr>
      <td>HTML</td>
      <td><code>no-cache</code></td>
      <td>Page dynamique — toujours revalider</td>
    </tr>
  </tbody>
</table>

<hr>

<!-- Questions théoriques -->
<h2>Questions théoriques</h2>

<p class="question">1. Quelle est la différence entre <code>no-cache</code> et <code>no-store</code> ?</p>
<div class="answer">
  <ul>
    <li><strong>no-cache</strong> : le navigateur garde bien une copie en cache, mais il doit <em>demander confirmation</em> au serveur avant de l'utiliser. Si le fichier n'a pas changé, le serveur répond 304 et le cache est utilisé.</li>
    <li><strong>no-store</strong> : rien n'est sauvegardé nulle part. Utile pour des pages avec des données sensibles (relevé de compte, résultats médicaux...).</li>
  </ul>
</div>

<p class="question">2. Pourquoi POST n'est-il pas idempotent ?</p>
<div class="answer">
  Envoyer la même requête POST plusieurs fois crée plusieurs ressources. Par exemple, cliquer deux fois sur "Envoyer le formulaire" peut créer deux commandes. À l'inverse, GET ou DELETE sont idempotents : les répéter ne change rien de plus.
</div>

<p class="question">3. Que se passe-t-il si le serveur renvoie un code 301 ?</p>
<div class="answer">
  C'est une <strong>redirection permanente</strong>. Le serveur dit : "cette page a déménagé pour toujours, voici la nouvelle adresse" (dans le header <code>Location</code>). Le navigateur va automatiquement à la nouvelle URL et s'en souvient pour les prochaines fois.
</div>

<p class="question">4. À quoi sert le header <code>Origin</code> ?</p>
<div class="answer">
  Il indique d'où vient la requête — par exemple <code>Origin: https://monsite.com</code>. Le serveur s'en sert pour décider si un site étranger a le droit d'accéder à ses données (mécanisme CORS). C'est une protection contre les requêtes non autorisées depuis d'autres sites.
</div>

<p class="question">5. Pourquoi utiliser <code>HttpOnly</code> sur les cookies de session ?</p>
<div class="answer">
  Sans <code>HttpOnly</code>, du JavaScript malveillant injecté dans une page pourrait lire le cookie de session et usurper l'identité de l'utilisateur. Avec <code>HttpOnly</code>, le cookie est invisible pour JavaScript — seul le navigateur peut l'envoyer au serveur.
</div>

</body>
</html>