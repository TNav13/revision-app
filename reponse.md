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

**Question 10**
La documentation as code offre 3 avantages principaux :
*Toujours à jour et versionnée* : Comme elle est gérée exactement comme le code, chaque modification est historisée (versionnée) et validée par une relecture de l'équipe (code review). Cela garantit sa fiabilité.
*Propre et structurée* : Le contenu est découpé en fichiers distincts, simples et bien organisés, ce qui rend la maintenance beaucoup plus propre et lisible.
*Facile à publier* : Elle peut être automatiquement déployée sous forme de site web statique moderne, agréable à lire et accessible à tous.

**Question 11**
L'Infrastructure as Code (avec Terraform) transforme une gestion cloud artisanale et opaque en un processus industriel et sécurisé. Contrairement à la création manuelle dans la console web, qui est sujette aux erreurs humaines et difficile à tracer, Terraform permet de définir l'infrastructure sous forme de fichiers texte. Cela garantit une infrastructure parfaitement reproductible à l'identique pour les environnements de test ou de production. De plus, comme ce code est versionné (sur Git), vous bénéficiez d'une traçabilité totale sur les modifications et pouvez facilement revenir en arrière en cas de problème. Enfin, vous obtenez une visibilité maximale grâce aux plans d'exécution qui permettent de valider précisément chaque changement avant qu'il ne soit appliqué, évitant ainsi toute destruction accidentelle.

**Question 12** 
Choisir Cloud Run est idéal si vous voulez déployer rapidement sans vous soucier de la complexité et de la maintenance de Kubernetes, avec en prime une mise à l'échelle automatique qui peut descendre jusqu'à zéro pour économiser des coûts. À l'inverse, optez pour Kubernetes si vous avez besoin d'un contrôle total sur l'infrastructure, le réseau, le stockage persistant ou si vous devez gérer une architecture multi-cloud complexe.

**Question 13**

'est une très bonne base, mais on peut la rendre plus percutante. Voici une version courte en un seul bloc :
Le GitOps consiste à utiliser Git comme unique source de vérité pour l'infrastructure et les applications. Son principal intérêt est la sécurité et l'automatisation : toute modification du code déclenche automatiquement le déploiement, et le système se répare tout seul en cas d'écart avec ce qui est défini dans Git.

**Question 14**

*Nom et principe*
Le service de conteneurs managés d'AWS est Amazon ECS (Elastic Container Service), souvent associé à AWS Fargate pour sa déclinaison sans serveur (serverless).

Son principe est de permettre l'orchestration, l'exécution et la mise à l'échelle automatique de conteneurs Docker sur le cloud d'Amazon sans avoir à gérer la complexité d'un cluster Kubernetes complet.

**Points communs avec Cloud Run**
Abstraction des serveurs (avec Fargate) : Dans les deux cas, vous n'avez pas à gérer, patcher ou provisionner des machines virtuelles sous-jacentes. Vous fournissez simplement votre image de conteneur.
Intégration écosystémique forte : Les deux services s'intègrent nativement avec les outils de sécurité (IAM), de journalisation (CloudWatch pour AWS, Cloud Logging pour GCP) et les registres de conteneurs de leur fournisseur respectif.

**Différences avec Cloud Run**

Le modèle de tarification et mise à l'échelle à zéro : Cloud Run est un modèle purement basé sur les requêtes et peut scaler automatiquement jusqu'à zéro instance (vous ne payez rien s'il n'y a pas de trafic). Amazon ECS (même avec Fargate) nécessite généralement qu'au moins une tâche tourne en continu pour maintenir le service actif, générant un coût fixe minimal.
Le niveau de contrôle réseau et d'infrastructure : Cloud Run masque presque toute la couche réseau par défaut. Amazon ECS s'exécute obligatoirement au sein d'un VPC (Virtual Private Cloud) AWS, vous donnant un contrôle total mais complexe sur les sous-réseaux, les groupes de sécurité et le routage interne.