
RestTemplate

20 requêtes exécutées avec 100 % de succès

Durée totale du test : 43,20 s

Latence moyenne : 1300,8 ms

Latence minimale / maximale : 361 ms / 4803 ms

P95 latency : 4803 ms

Débit : 0,46 req/s

 Performances faibles avec une forte variabilité de latence.

Feign

20 requêtes exécutées avec 100 % de succès

Durée totale du test : 15,36 s

Latence moyenne : 660,3 ms

Latence minimale / maximale : 362 ms / 889 ms

P95 latency : 889 ms

Débit : 1,3 req/s

 Meilleures performances globales, faible variance et excellente stabilité.

WebClient (mode synchrone)

20 requêtes exécutées avec 100 % de succès

Durée totale du test : 17,64 s

Latence moyenne : 1041,95 ms

Latence minimale / maximale : 271 ms / 4184 ms

P95 latency : 4184 ms

Débit : 1,13 req/s

 Performances intermédiaires, limitées par l’utilisation du mode bloquant.

 Comparaison Synthétique
Métrique	RestTemplate	Feign	WebClient
Latence moyenne	1300.8 ms	660.3 ms	1041.95 ms
Latence minimale	361 ms	362 ms	271 ms
Latence maximale	4803 ms	889 ms	4184 ms
P95 latency	4803 ms	889 ms	4184 ms
Débit (req/s)	0.46	1.3	1.13
Taux de succès	100 %	100 %	100 %
Durée du test	43.20 s	15.36 s	17.64 s
 Analyse des Résultats
 Feign : le plus performant et le plus stable

Latence moyenne ~660 ms (≈ 2× plus rapide que RestTemplate)

Très faible variation (P95 proche du max)

Meilleur débit observé

Architecture déclarative, intégration native avec le load balancer

 Conclusion : Feign est le meilleur choix pour les appels synchrones en microservices.

 RestTemplate : forte variabilité

Latence élevée et instable

Débit faible

Gestion des connexions HTTP moins optimisée

 Adapté aux cas simples mais peu performant à l’échelle.

 WebClient : potentiel non exploité

Performances intermédiaires

Bonne latence minimale

Mode bloquant (block()) annule les bénéfices du modèle réactif

 Intéressant uniquement en mode non bloquant.

 Overhead Load Balancer vs Service Direct
| Client       | Temps moyen | Multiplicateur |
| ------------ | ----------- | -------------- |
| Service pur  | 20 ms       | 1×             |
| RestTemplate | 1281 ms     | 60×            |
| Feign        | 640 ms      | 30×            |
| WebClient    | 1022 ms     | 50×            |


Causes principales :

Découverte de service

Création de connexions HTTP

Sérialisation / désérialisation

Absence de cache optimisé

 Utilisation des Ressources (Observée)
Service Client (8082)

CPU moyen : 5–10 % (pic à 15 %)

RAM : 185–200 MB

Threads actifs : 25–30

Service Voiture (8081)

CPU moyen : 3–5 % (pic à 8 %)

RAM : ~165 MB

Threads actifs : 10–12

 Complexité et Maintenabilité du Code
| Critère                | RestTemplate | Feign       | WebClient  |
| ---------------------- | ------------ | ----------- | ---------- |
| Lignes de code         | 15–20        | 5–8         | 15–20      |
| Configuration          | Modérée      | Minimale    | Modérée    |
| Lisibilité             | Bonne        | Excellente  | Bonne      |
| Courbe d’apprentissage | Basse        | Très basse  | Moyenne    |
| Flexibilité            | Haute        | Moyenne     | Très haute |
| Maintenabilité         | Facile       | Très facile | Facile     |

 Scénario F3 : Redémarrage du Service Client

Contexte : redémarrage du service client pendant un test continu.

Phase	Observations
Avant redémarrage	~100 % de succès
Pendant redémarrage	0 % (2–3 s)
Après redémarrage	~100 % après 3–5 s
Temps de stabilisation

Enregistrement Eureka : 5–10 s

Enregistrement Consul : 2–5 s

Mise à jour du cache d’instances : 10–15 s

Temps total de récupération : ~15–20 s
