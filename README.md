# Évolution du climat urbain mondial (1743-2013)

Projet Big Data réalisé en BUT Science des Données (parcours EMS), année 2025/2026.
Analyse faite avec PySpark. Tous les graphiques et les résultats sont visibles directement dans le notebook, sans rien installer.

## L'idée de départ

On sait tous que les températures montent. Ce qui m'intéressait, c'est de voir si ça se passe de la même façon partout et toute l'année. Ma question de départ :

> Le réchauffement des villes est-il le même partout, en été comme en hiver, ou y a-t-il des différences marquées (saisons, extrêmes, régions du monde) ?

## Les données

Les données viennent de Kaggle : [Climate Change: Earth Surface Temperature Data](https://www.kaggle.com/datasets/berkeleyearth/climate-change-earth-surface-temperature-data) (Berkeley Earth). J'ai utilisé le fichier *GlobalLandTemperaturesByCity* :

- environ 1 million de lignes (1 048 576)
- une ligne = la température moyenne d'une ville pour un mois
- colonnes : date, température moyenne, incertitude, ville, pays, latitude, longitude
- période : 1743 à 2013

Le fichier CSV n'est pas dans le dépôt, il est trop lourd. Il se télécharge gratuitement sur Kaggle avec le lien ci-dessus.

## Ce que j'ai fait

**1. Tendance globale.** J'ai lissé les températures avec une moyenne mobile sur 10 ans pour enlever le bruit d'une année à l'autre. La moyenne passe d'environ 17,5 °C au milieu du XIXe siècle à 19,6 °C aujourd'hui. J'ai aussi comparé la distribution des températures sur trois périodes : non seulement tout se décale vers le chaud, mais les valeurs les plus hautes s'étirent.

J'ai ensuite entraîné une régression linéaire pour projeter la tendance jusqu'en 2050. C'est un modèle très simple et j'en parle dans les limites plus bas.

**2. Nord contre Sud.** Les saisons sont décalées de six mois entre les deux hémisphères, comme prévu. Mais l'écart entre l'été et l'hiver est beaucoup plus grand au Nord (environ 15,5 °C) qu'au Sud (environ 4,7 °C), sûrement à cause des océans qui lissent les températures dans l'hémisphère Sud.

**3. Le climat est-il plus instable ?** J'ai calculé l'écart-type des températures par décennie : il passe de 10,24 °C en 1900 à 10,42 °C en 2010. La hausse est faible mais va dans le sens d'un climat plus variable.

J'ai aussi fait un test de Student pour comparer le réchauffement du Nord et du Sud (entre 1900-1930 et 1990-2013). Résultat : +0,96 °C au Nord contre +0,92 °C au Sud, avec une p-value de 0,019. La différence est petite mais statistiquement significative.

**4. Les villes les plus touchées.** J'ai classé les villes selon leur hausse de température entre 1900-1930 et 1980-2013. Le haut du classement est surtout en Eurasie : Atyrau au Kazakhstan (+1,61 °C), Blagoveshchensk (+1,59 °C) et Angarsk (+1,56 °C) en Russie.

**5. Une heatmap décennie / mois.** Elle montre que le réchauffement ne touche pas tous les mois pareil. Les étés sont de plus en plus chauds, et les mois de transition (mai, septembre, octobre) se réchauffent aussi nettement.

**6. Zoom sur la France.** Pour rendre ça plus concret, j'ai fait des cartes par département (Plotly + GeoJSON) pour comparer 1900-1910 et 2002-2012, en été et en hiver. Les étés passent d'environ 17,9 °C à presque 20 °C, et les hivers s'adoucissent aussi. Les cartes et courbes Plotly sont interactives : elles ne s'affichent pas toujours dans l'aperçu GitHub, il faut alors exécuter le notebook pour les voir.

## Conclusion

Le réchauffement ne se résume pas à une hausse de la moyenne. Les étés montent, les hivers s'adoucissent, la variabilité augmente un peu, et ça ne touche pas toutes les régions de la même façon. Pour les villes, ça veut dire qu'il faut se préparer à des épisodes de chaleur plus fréquents, pas seulement à des températures moyennes plus hautes.

## Les limites de mon analyse

- Les valeurs manquantes sont remplacées par la moyenne globale. C'est simple, mais ça peut lisser les différences entre villes. Une moyenne par ville et par mois serait mieux.
- Avant 1850, il y avait très peu de stations, donc les données de cette époque sont peu fiables.
- La base ne couvre pas tous les départements français, d'où les zones grises sur les cartes.
- La régression linéaire ne capte pas l'accélération récente du réchauffement, donc la projection à 2050 est probablement trop basse. Je pourrais tester un modèle de séries temporelles.
- Le nombre de villes mesurées change selon les années, ce qui peut jouer sur les moyennes globales.

## Lancer le notebook

Le notebook se lit directement sur GitHub, avec les résultats. Pour l'exécuter soi-même :

1. Télécharger le fichier `GlobalLandTemperaturesByCity.csv` sur [Kaggle](https://www.kaggle.com/datasets/berkeleyearth/climate-change-earth-surface-temperature-data)
2. Ouvrir le notebook avec Jupyter, VS Code ou Google Colab, et placer le CSV dans le même dossier
3. Vérifier le séparateur dans la cellule de chargement (`sep=";"` dans mon notebook, à passer à `","` si le fichier Kaggle est séparé par des virgules)
4. Lancer les cellules dans l'ordre

Bibliothèques à installer : `pyspark`, `pandas`, `numpy`, `scipy`, `scikit-learn`, `matplotlib`, `seaborn`, `plotly`, `requests`. PySpark a aussi besoin de Java sur l'ordinateur.

## Outils utilisés

Python, PySpark, Pandas, NumPy, SciPy, scikit-learn, Matplotlib, Seaborn, Plotly. Code écrit sur Google Colab.
