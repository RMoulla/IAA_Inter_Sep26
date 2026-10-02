# IAA_Inter_Sep26

Bienvenue dans le dépôt de la formation des **22 et 23 septembre 2026 à Paris**. Vous trouverez ici le support de cours et deux travaux pratiques à réaliser dans votre navigateur avec **Google Colab**.

Un *notebook* (fichier `.ipynb`) est un document qui alterne explications et blocs de code, appelés **cellules**. GitHub permet de le consulter ; Colab permet de l’exécuter. **Vous n’avez pas besoin d’installer Python ni de cloner le dépôt.**

## Accès rapide

| Ressource | Lien |
| --- | --- |
| Support de cours | [Ouvrir le PDF](IAA_Paris_22-23_septembre_2026.pdf) |
| TP RAG : poser des questions à des PDF | [Ouvrir dans Google Colab](https://colab.research.google.com/github/RMoulla/IAA_Inter_Sep26/blob/main/RAG_Gradio.ipynb) |
| TP Agents : créer un assistant e-commerce | [Ouvrir dans Google Colab](https://colab.research.google.com/github/RMoulla/IAA_Inter_Sep26/blob/main/Copie_de_TP_Agent_LLM.ipynb) |
| Catalogue utilisé par le TP Agents | [Ouvrir products.csv](products.csv) |

## 1. Télécharger le PDF du cours

1. Cliquez sur [IAA_Paris_22-23_septembre_2026.pdf](IAA_Paris_22-23_septembre_2026.pdf), dans le tableau ci-dessus ou dans la liste des fichiers du dépôt.
2. Sur la page du fichier, cliquez sur l’icône **Télécharger** (flèche vers le bas, intitulée **Download raw file**) dans la barre au-dessus du document.
3. Enregistrez le fichier sur votre ordinateur, par exemple dans le dossier **Téléchargements**.

Vous pouvez aussi [ouvrir directement le PDF](https://raw.githubusercontent.com/RMoulla/IAA_Inter_Sep26/main/IAA_Paris_22-23_septembre_2026.pdf), puis utiliser le bouton de téléchargement de votre navigateur. Si GitHub n’affiche pas l’aperçu, le téléchargement reste possible.

## 2. Préparer Colab pour les deux TP

### Ouvrir votre copie du notebook

1. Connectez-vous à votre compte Google, puis ouvrez le lien Colab du TP choisi.
2. Sélectionnez **Fichier → Enregistrer une copie dans Drive** (*File → Save a copy in Drive*). Travaillez dans cette copie pour conserver vos modifications.
3. Cliquez sur **Se connecter** (*Connect*) en haut à droite pour démarrer la session de calcul.
4. Conservez un environnement **Python 3 avec CPU** : les deux TP peuvent fonctionner sans GPU.

### Ajouter la clé API OpenAI

Les deux notebooks utilisent le modèle `gpt-4o` via l’API OpenAI. Il vous faut une **clé API active**, avec un accès à ce modèle et un quota disponible. Un abonnement ChatGPT ne remplace pas la clé demandée par ces notebooks ; les appels API peuvent être facturés.

Si nécessaire, créez votre clé depuis la [page des clés API OpenAI](https://platform.openai.com/api-keys). Le [guide officiel de démarrage](https://developers.openai.com/api/docs/quickstart) explique la configuration de l’accès à l’API.

Dans **votre copie du notebook Colab** :

1. Cliquez sur l’icône en forme de **clé** dans la barre latérale gauche pour ouvrir **Secrets**.
2. Ajoutez un secret nommé exactement `OPENAI_API_KEY` (en majuscules, sans espace).
3. Collez votre clé API dans le champ **Valeur**.
4. Activez l’option **Accès au notebook** (*Notebook access*) pour ce secret.

Répétez l’autorisation d’accès dans chaque notebook. **Ne collez pas votre clé dans une cellule de code ou dans GitHub.**

### Exécuter les cellules dans l’ordre

Cliquez sur le bouton **▶** à gauche de chaque cellule de code, puis attendez sa fin avant de passer à la suivante. Le raccourci **Maj + Entrée** exécute aussi la cellule sélectionnée.

Pour une première prise en main, avancez **de haut en bas, cellule par cellule** : certaines étapes demandent de choisir un fichier ou de saisir une question. L’installation des bibliothèques et le premier téléchargement du modèle peuvent prendre quelques minutes.

## 3. Faire tourner le TP RAG

**Objectif :** poser des questions sur un ou plusieurs PDF. Le programme découpe leur texte en passages, retrouve ceux qui ressemblent à votre question, puis les transmet au modèle pour construire une réponse. C’est le principe du **RAG** (*Retrieval-Augmented Generation*, ou génération augmentée par la recherche).

[**Ouvrir le TP RAG dans Colab**](https://colab.research.google.com/github/RMoulla/IAA_Inter_Sep26/blob/main/RAG_Gradio.ipynb)

### Étape A — Installer et configurer

Après avoir préparé votre copie et le secret `OPENAI_API_KEY` :

1. Exécutez la première cellule de code, qui installe les bibliothèques.
2. Exécutez la cellule sous **Setup OpenAI API Key**, qui charge les bibliothèques et votre clé.

### Étape B — Importer un PDF avant de le traiter

1. Dans la barre latérale gauche de Colab, ouvrez **Fichiers** (icône de dossier).
2. Cliquez sur **Importer** (*Upload*) et sélectionnez le PDF du cours téléchargé à l’étape 1. Attendez la fin du transfert.
3. Vérifiez que le fichier apparaît dans le dossier de travail **`/content/`** de Colab.
4. Exécutez la cellule sous **1. Load and Process PDFs**.

Le notebook recherche les fichiers `.pdf` dans `/content/` et ses sous-dossiers. **Ouvrir le notebook depuis GitHub n’importe pas automatiquement le PDF.**

Vérifiez que le message `Loaded … raw PDF pages and split into … chunks` affiche un nombre de pages et de passages **supérieur à zéro**. Pour vos essais avec d’autres documents, choisissez des PDF dont le texte est sélectionnable : ce TP ne réalise pas de reconnaissance de texte sur les scans.

### Étape C — Créer l’index et afficher l’interface

Exécutez ensuite, dans cet ordre, les cellules des sections :

1. **2. Create Embeddings and Vector Store** : transforme les passages en vecteurs et crée l’index de recherche FAISS. Le message `FAISS index created with … embeddings.` confirme la création de l’index.
2. **3. RAG Function** : prépare la fonction qui recherche les passages et génère la réponse.
3. **4. Gradio Interface** : affiche une petite interface de questions-réponses sous la cellule. Si elle ne s’affiche pas, ouvrez le lien `gradio.live` produit par **votre exécution**.

**La dernière cellule reste en cours d’exécution : c’est normal**, elle maintient l’interface disponible. Laissez-la tourner pendant vos essais.

### Étape D — Poser une question

Saisissez une question liée au PDF, par exemple : **« Comment le cours explique-t-il le fonctionnement du RAG ? »**, puis cliquez sur **Submit / Envoyer** dans l’interface.

Comparez la réponse avec le document : une réponse générée peut contenir des erreurs. Si vous ajoutez ou remplacez un PDF, arrêtez la cellule Gradio, puis réexécutez les sections **1 à 4** pour reconstruire l’index et relancer l’interface.

Le RAG envoie votre question et les passages retrouvés à l’API OpenAI. Pour démarrer, utilisez le support de cours. Le lien Gradio est temporaire et accessible aux personnes qui le possèdent ; arrêtez la cellule lorsque vous avez terminé.

## 4. Faire tourner le TP Agents

**Objectif :** construire un assistant e-commerce nommé **Buddy**, capable de rechercher des produits, d’en ajouter à un panier et de consulter ce panier. L’agent choisit les outils à appeler en fonction de votre demande. Ces outils lui sont fournis par un serveur **MCP** (*Model Context Protocol*), lancé par le notebook.

[**Ouvrir le TP Agents dans Colab**](https://colab.research.google.com/github/RMoulla/IAA_Inter_Sep26/blob/main/Copie_de_TP_Agent_LLM.ipynb)

### Étape A — Récupérer le catalogue

1. Ouvrez [products.csv](products.csv) sur GitHub.
2. Cliquez sur **Download raw file** (flèche vers le bas), comme pour le PDF.
3. Enregistrez le fichier sur votre ordinateur en conservant exactement le nom **`products.csv`**.

### Étape B — Préparer l’agent

Dans votre copie Colab, avec le secret `OPENAI_API_KEY` configuré, exécutez les cellules des sections **1 à 6 dans l’ordre** :

| Section du notebook | Ce que vous devez faire ou observer |
| --- | --- |
| **1. Installation des bibliothèques** | Exécutez la cellule et attendez la fin. Si Colab demande un redémarrage de session, faites-le avant de continuer. |
| **2. Chargement des données et création de la base SQLite** | Quand le sélecteur de fichier apparaît, choisissez `products.csv`. Le message attendu est `1000 produits chargés ; panier vide.` |
| **3. Vectorisation des titres et indexation avec FAISS** | Attendez le message `Index prêt : 1000 produits.` Le TP utilise les 1 000 premiers produits pour limiter le temps de calcul. |
| **4. Création du serveur MCP** | Exécutez la cellule : elle crée le fichier `server.py`. |
| **5. Connexion au serveur et découverte des outils** | Vérifiez que les trois outils s’affichent : `search_products`, `add_to_cart` et `view_cart`. Le notebook lance le serveur pour vous. |
| **6. Configuration de l’agent** | Attendez le message `Agent prêt.` Si le secret n’est pas accessible, cette cellule propose une saisie masquée de la clé. |

### Étape C — Discuter avec Buddy

1. Dans **7. Conversation avec l’agent**, exécutez la cellule qui définit la fonction `interact_with_agent`.
2. Exécutez la **dernière cellule** du notebook. Un champ **Votre demande :** apparaît.
3. Saisissez **« Je cherche un pantalon de sport noir pour homme. »**, puis appuyez sur **Entrée**.
4. Lisez les lignes **[Appel MCP]**, **[Résultat]** et la réponse **Buddy :** pour voir comment l’agent utilise ses outils.
5. Réexécutez **uniquement la dernière cellule** et saisissez **« Ajoute deux exemplaires du premier produit au panier. »**
6. Réexécutez encore cette cellule et demandez **« Montre-moi mon panier. »**

Vous pouvez parler en français : l’agent formule lui-même les recherches en anglais, langue du catalogue. Le panier est une simulation pédagogique ; les ajouts ne déclenchent ni commande ni paiement.

Pour conserver le fil de la conversation, poursuivez avec la dernière cellule. **Réexécuter la section 2 vide le panier ; réexécuter la section 6 réinitialise la mémoire de conversation.**

## 5. En cas de difficulté

| Problème | Que faire ? |
| --- | --- |
| Secret introuvable ou accès refusé | Vérifiez le nom exact `OPENAI_API_KEY` et activez **Accès au notebook** dans les secrets de la copie utilisée. |
| Erreur d’authentification ou de quota OpenAI | Vérifiez que la clé est valide, que le compte dispose d’un quota API et que le modèle `gpt-4o` est accessible. En formation, sollicitez le formateur si nécessaire. |
| Le RAG affiche zéro page ou zéro passage | Importez le PDF dans `/content/`, vérifiez qu’il contient du texte sélectionnable, puis relancez le chargement et l’indexation. |
| Le fichier `products.csv` est introuvable | Importez à nouveau le fichier et vérifiez qu’il ne s’appelle pas `products (1).csv` ou `products.csv.txt`. |
| Variable inconnue (`NameError`), outil ou fichier du serveur manquant | Une cellule précédente n’a probablement pas été exécutée ou a échoué. Reprenez dans l’ordre à partir de la première erreur. |
| Erreur d’import après l’installation | Redémarrez la session via le menu **Exécution**, puis reprenez les cellules dans l’ordre. |
| Une ancienne sortie est visible, mais rien ne fonctionne | Les sorties enregistrées dans le notebook ne signifient pas que votre session a exécuté le code : lancez les cellules dans votre propre session. |
| Le lien Gradio ne répond plus | Reconnectez Colab et relancez les étapes du RAG si la session a été réinitialisée. Utilisez le nouveau lien généré. |

**À retenir pour reprendre plus tard :** votre copie du notebook est conservée dans Drive, mais les fichiers importés, les bibliothèques installées et les données en mémoire dépendent de la session temporaire de Colab. Si celle-ci est réinitialisée, réimportez les fichiers nécessaires et réexécutez les cellules. Voir la [FAQ officielle de Colab](https://research.google.com/colaboratory/faq.html).
