# Plan_Blanc
Optimisation multi-objectif et algorithmes génétiques pour établir un plan blanc

##  Optimisation de l'Ordonnancement Chirurgical avec NSGA-II

### Description :
Ce repositoire contient un notebook Jupyter Python pour l'optimisation des plannings chirurgicaux en utilisant l'**Algorithme Génétique de Tri Non-Dominé II (NSGA-II)**.
C'est une implementation du rapport de thèse : (S. Makboul, A. El Hilali Alaoui), _Developing multiple approaches based on robust and multi-objective optimization to promote health care systems_.  

L'objectif est d'allouer efficacement les patients aux chirurgiens dans un contexte hospitalier, en tenant compte de facteurs tels que les heures d'arrivée, la durée des chirurgies et les contraintes de charge de travail. L'optimisation vise à minimiser à la fois le nombre total de patients en attente de chirurgie et la durée totale des opérations.

### Plan Blanc :
Le "plan blanc" est un protocole d'urgence hospitalier pour gérer efficacement les situations critiques, telles que les catastrophes naturelles ou les crises de santé publique. Son objectif est d'optimiser l'allocation des ressources médicales, y compris les salles d'opération et le personnel, afin de fournir des soins chirurgicaux prioritaires aux patients touchés.


### NSGA-II (Non-dominated Sorting Genetic Algorithm II) :
NSGA-II est un algorithme génétique évolutif utilisé pour résoudre des problèmes d'optimisation multi-objectifs. Il maintient une population de solutions candidates, appelées individus, et utilise des opérateurs génétiques tels que le croisement et la mutation pour évoluer vers des solutions optimales. NSGA-II classe les individus en différentes couches de non-domination, appelées fronts de Pareto, où aucune solution n'est dominée par une autre en termes de tous les objectifs. Cela permet d'explorer l'espace des solutions de manière exhaustive pour trouver un ensemble de solutions optimales, appelé ensemble de Pareto.  

   ### Global Structure of NSGA  
   
![image](https://github.com/najwanaamane/Plan_Blanc/assets/86806375/87099acc-f301-4c02-8a72-29ec6747badf)




### Installation :
1. Clonez le dépôt :
   ```bash
   git clone https://github.com/najwanaamane/Plan_Blanc.git
   ```

2. Installez les dépendances requises :
   ```bash
   pip install -r requirements.txt
   ```

### Utilisation :
1. Exécutez le script `White_Plan.ipynb` pour lancer l'algorithme d'optimisation NSGA-II pour l'ordonnancement chirurgical.
   ```bash
   jupyter notebook white_plan.ipynb

   ```

2. Modifiez les paramètres d'entrée dans le script `White_Plan.ipynb` pour personnaliser le processus d'optimisation selon les exigences spécifiques de l'hôpital.

### Exemple :
```python
# Définir les données des patients
patients = [...]  # Liste des données des patients (heure d'arrivée, durée, date limite)

# Définir les données des chirurgiens
chirurgiens = [...]  # Liste des données des chirurgiens (heure d'arrivée, charge de travail)

# Exécuter l'optimisation NSGA-II
meilleur_planning, performances, séquence = White_Plan(...)

# Visualiser le planning chirurgical optimisé
White_Plan_Visualizer(meilleur_planning, séquence, ...)
```

### Fonctions

| Nom de la Fonction | Arguments principaux | Rôle & Retour de la fonction |
|---|---|---|
| Order_passage_privilege() | *Nombre de patients : N<br>*Date d’arrivée des patients<br>*Date au plus tard des patients<br>*Durée des opérations de chaque patient | Renvoie l’ensemble des patients triés dans l’ordre croissant de leur date au plus tard; la liste triée sera passée en argument à la fonction d’initialisation d’un individu |
| Init_Individu_White_Plan() | *Nombre de patients : N<br>*Nombre de chirurgiens : H<br>*Date d’arrivée des patients<br>*Date au plus tard des patients<br>*Date d’arrivée des chirurgiens<br>*Durée des opérations de chaque patient<br>*Charge maximale de travail des chirurgiens | Générer un individu réalisable s’il existe selon les données passées en argument |
| Init_Population_White_Plan() | *Nombre d’individus à créer dans la population + arguments de la fonction « Init_Individu_White_Plan() » | Renvoie une population d’individus (Il est conseillé de faire spécifier un nombre d’individus  50) |
| Check_out_Population() | *Population d’individus crées + arguments de la fonction « Init_Population_White_Plan() » | Vérifie que la population d’individus crées respecte toutes les contraintes |
| performance_population() | *Population d’individus crées<br>*Date d’arrivée des patients<br>*Durée des opérations de chaque patient | Évalue les individus de la population selon les deux fonctions objectifs |
| Front_Pareto() | Arguments de la fonction « performance_population() » | Détermine le front de Pareto de la population |
| crossing() | *Deux individus A & B à croiser<br>*Une probabilité au dessus de laquelle les deux individus peuvent être croisés | Renvoie les deux enfants issus du croisement |
| heuristique_crossing() | *Un enfant issu du croisement | Corrige l’enfant s’il n’est pas réalisable |
| mutate() | *L’individu à muter<br>*Une probabilité au dessus de laquelle l’individu peut être muté | Renvoie l’individu issu de la mutation |
| heuristique_mutate() | *L’individu issu de la mutation | Corrige l’individu muté s’il n’est pas réalisable |
| divide_non_dominated() | Arguments de la fonction « performance_population() » | Divise la population en plusieurs niveaux de non domination |
| crowding_distance() | *Les différents niveaux de non domination<br>*Les performances des individus à chaque niveau de non domination<br>*Un argument spécifiant le niveau où l’on veut trier les individus | Trie les individus d’un même front de non domination |

**Notes:**

* La fonction `Check_out_Population()` est particulièrement importante dans le croisement et la mutation pour vérifier si la progéniture est réalisable.
* La fonction `performance_population()` calcule les valeurs des deux objectifs pour chaque individu de

 la population.
* La fonction `Front_Pareto()` identifie les individus non dominés, qui constituent le front de Pareto.
* Les fonctions `crossing()` et `mutate()` génèrent de nouveaux individus par croisement et mutation respectivement.
* Les fonctions `heuristique_crossing()` et `heuristique_mutate()` corrigent les enfants issus du croisement et de la mutation s'ils ne sont pas réalisables.
* Les fonctions `divide_non_dominated()` et `crowding_distance()` sont utilisées dans le tri non dominé pour sélectionner les meilleurs individus pour la prochaine génération.

### Simulation

Dans ce notebook, nous avons simulé un scénario d'urgence chirurgicale avec 50 patients et 5 chirurgiens. La charge maximale de chaque chirurgien à leur arrivée à l’hôpital après le début de la catastrophe à T = 0 est fixée à 11,5 h soit 690 minutes. Les chirurgiens ont une date d’arrivée suivant une suite arithmétique de raison 3 (si t_j représente la date d’arrivée d’un chirurgien alors t_(j+1) = t_j+3 ∀ j ϵ {1,...,4} avec t_0 = 0)   


### Acknowledgments:
- Ce projet s'inspire des recherches menées par: **S. Makboul, A. El Hilali Alaoui**, _Developing multiple approaches based on robust and multi-objective optimization to promote health care systems_.





### Détails Additionnels:
Pour plus de détails théoriques, veuillez consulter le document **White_Plan_1_protocole.pdf**.






```
