**Question 1**
L'utilisation de gunicorn plutôt que le serveur intégré de Flask permet plus de sécurité, fiabilité et de performance. 
Le serveur intégré de Flask est utile au debug. 


**Question 2**
La différence est qu'un merge avec `no-ff`un commit de fusion est crée automatiquement même si la fusion pourrait être considéré comme une avance rapide. Si `no-ff` n'est pas utilisé, alors git déplace simplement le pointeur de branche sans créer de commit, cela rend l'historique linéraire mais ne garde pas de trace de la fonctionnalité crée. 
On préfère intégrer `no-ff` pour intégrer une branche de fonctionnalité car cela permet de créer un commit et de comprendre qu'un merge de fonctionnalité a eu lieu. 

**Question 3**
Correction d'un bug: `patch`
Ajout d'une route sans casser l'existant: `Minor`
Changement du format de réponse JSON: `Major`

**Question 4**
On place les étapes de formate et lint avant les tests car cela consomme moins de ressources de faire des étapes de lint avant les tests. Si le lint n'est pas correct, alors on ne prend pas la peine d'effectuer des tests. 
Le principe est le `fail-fast`

**Question 5**
L'option `--cov-fail-under=80` de pytest permet de détecter le taux de couverture du code. Si la couverture de code au niveau des tests est inférieur à 80% alors la CI va échouer. Cela permet de s'assurer que le code est bien testé, surtout au niveau des nouvelles fonctionnalités. Cela permet de s'assurer qu'il n'y a pas de dette technique et permet d'imposer un seuil de couverture minimal. 

**Question 6**
*Black (Formatter)* : Il reformate automatiquement le code source pour qu'il respecte strictement un style de codage uniforme, éliminant ainsi les débats sur le style.
*Ruff (Linter)* : Il analyse le code de manière statique pour détecter les erreurs de syntaxe, les mauvaises pratiques et les violations de règles de codage afin d'améliorer la qualité et la lisibilité du code.
*Bandit (SAST - Static Application Security Testing)* : Il scanne le code source à la recherche de failles de sécurité connues et de pratiques de programmation dangereuses directement dans l'application.
*Pip-audit (SCA - Software Composition Analysis)* : Il analyse les bibliothèques tierces et leurs versions utilisées dans le projet (via requirements.txt) pour identifier si elles contiennent des vulnérabilités de sécurité connues.

**Question 7**
La correction d'une fuite de secret impose la révocation immédiate de la clé API compromise afin de la rendre inutilisable. Il est ensuite impératif de nettoyer l'historique Git pour effacer toute trace du secret, puis d'effectuer une rotation des accès en générant de nouvelles identifiants. Enfin, la sécurisation du processus est renforcée par la mise en place d'outils de scan en pré-commit, permettant de détecter et de bloquer préventivement toute fuite de données sensibles avant qu'elles ne soient poussées vers le dépôt distant.

**Question 8**
Il ne faut pas reconstruire l'image entre prod et staging car les tests/lintage/formattage effectués sont généralement effectués en staging. Si on reconstruit une image en production, alors on n'aura pas exactement la même image que celle construire en staging ainsi on pourrait obtenir une différence entre les deux images. 

**Question 9**
L'utilisation du tag `:latest` en production est proscrite car elle induit une incertitude sur la version exacte du code exécuté, rendant le déploiement non reproductible et imprévisible. Ce tag est dynamique par nature et pointera toujours vers la version la plus récente poussée sur le dépôt, ce qui empêche de garantir la stabilité de l'application en cas de mise à jour automatique. En cas de déploiement d'une version contenant des vulnérabilités ou des régressions critiques, l'absence de version fixe complique considérablement le processus de retour arrière (rollback). Enfin, cette pratique compromet la sécurité et l'auditabilité du système, puisque les outils d'analyse de vulnérabilités, comme Trivy, ne peuvent pas valider précisément l'intégrité d'une image dont le contenu varie arbitrairement au cours du temps.

