---
aliases:
- /fr/monitors/service_level_objectives/event/
description: Utiliser des métriques pour définir un Service Level Objective
further_reading:
- link: /metrics/
  tag: Documentation
  text: Plus d'informations sur les métriques
kind: documentation
title: SLO basés sur des métriques
---

## Présentation

Les SLO basés sur des métriques sont particulièrement utiles pour identifier les événements positifs et négatifs à partir d'un flux de données numériques. Une requête de métrique utilise la somme des événements positifs divisée par le nombre total d'événements pour calculer un Service Level Indicator (SLI). Vous pouvez créer des SLO à partir de n'importe quelle métrique, y compris des métriques custom générées par des [spans APM][5], des [événements RUM][6] et des [logs][7].

{{< img src="monitors/service_level_objectives/metric-based-slo-example.png" alt="exemple de SLO basé sur des métriques"  >}}

## Configuration

Sur la [page de statut des SLO][1], sélectionnez **New SLO +**, puis [**Metric**][2].

### Définir les requêtes

1. Vous devez spécifier deux requêtes. La requête du numérateur définit la somme des événements positifs, tandis que la requête du dénominateur définit le nombre total d'événements. Vos requêtes doivent utiliser des métriques COUNT, RATE ou DISTRIBUTION avec centiles afin que les calculs du SLO soient corrects.
2. Utilisez le champ `FROM` pour inclure ou exclure des groupes spécifiques en fonction des tags définis.
3. Pour les métriques DISTRIBUTION avec centiles, vous devez utiliser l'agrégateur `count values...` pour spécifier un seuil numérique. On parle alors de requêtes avec seuil. Cette fonctionnalité vous permet de compter le nombre de valeurs brutes qui correspondent à un seuil numérique afin de générer des counts pour votre numérateur et votre dénominateur. Pour en savoir plus, consultez la rubrique [Requêtes avec seuil][3].
4. Pour ce type de métrique, vous pouvez également utiliser le menu déroulant en regard de l'agrégateur `count values...` pour diviser votre SLI en plusieurs groupes spécifiques.
5. Pour les métriques COUNT ou RATE, vous avez également la possibilité d'utiliser l'agrégateur `sum by` pour diviser votre SLI en plusieurs groupes spécifiques.

**Exemple :** si vous surveillez des codes de réponse HTTP, et si votre métrique comprend un tag similaire à `code:2xx`, `code:3xx` ou `code:4xx`, la somme des événements positifs est `sum:httpservice.hits{code:2xx} + sum:httpservice.hits{code:4xx}`, tandis que le `total` d'événements est `sum:httpservice.hits{!code:3xx}`.

Pourquoi exclure `HTTP 3xx` ? Ces codes correspondent généralement à des redirections, qui ne doivent pas être prises en compte pour le calcul du SLI. Seuls les autres codes servent pour le calcul. Le `total` doit inclure toutes les erreurs, à l'exception des erreurs `HTTP 3xx`. Le `numérateur` doit donc uniquement comprendre les codes de statut `OK`.

#### Regrouper des SLI basés sur des métriques

Les SLI basés sur des métriques vous permettent de vous concentrer sur les attributs les plus importants de vos SLI. Vous avez la possibilité de regrouper vos SLI basés sur des métriques en appliquant des tags depuis l'éditeur, tels que `datacenter`, `partition`, `availability-zone`, `resource` ou tout autre tag correspondant au groupe qui vous intéresse :

{{< img src="monitors/service_level_objectives/metric_editor.png" alt="édition d'un SLO groupé basé sur des métriques"  >}}

Après avoir regroupé vos SLI, vous pouvez visualiser le statut, le nombre de requêtes positives et la marge d'erreur restante pour chaque groupe depuis le volet des détails :

{{< img src="monitors/service_level_objectives/metric_results.png" alt="résultats pour un SLO groupé basé sur des métriques"  >}}

Par défaut, le graphique à barres affiche le nombre total de requêtes ayant réussi ou échoué pour l'ensemble du SLO. Vous pouvez filtrer le graphique à barres de façon à afficher les requêtes d'un groupe individuel en cliquant sur la ligne correspondante dans le tableau. Vous pouvez également afficher ou masquer le nombre de requêtes ayant réussi ou échoué en sélectionnant l'option appropriée dans la légende située directement sous le graphique à barres. 

**Remarque** : si vous utilisez des SLI basés sur des monitors, vous pouvez également [visualiser vos groupes de monitors][4].

### Définir les cibles de votre SLO

Une cible de SLO est composée du pourcentage cible et de l'intervalle de temps. Lorsque vous définissez une cible pour un SLO basé sur des métriques, le pourcentage cible indique la proportion d'événements devant correspondre à des événements réussis par rapport au nombre total d'événements, tandis que l'intervalle de temps indique la période glissante pendant laquelle la cible doit être suivie.

Exemple : `99 % des requêtes doivent avoir été effectuées sans erreur sur les 7 derniers jours`.

Tant que le SLO reste au-dessus du pourcentage cible, le statut du SLO s'affiche en vert. En cas de violation du pourcentage cible, le statut du SLO s'affiche en rouge. Vous pouvez également spécifier une valeur d'avertissement supérieure à la valeur cible afin de savoir lorsque le SLO est sur le point de ne plus être satisfait. En cas de violation du pourcentage d'avertissement (mais pas du pourcentage cible), le statut du SLO s'affiche en jaune.

**Remarque :** jusqu'à trois décimales sont autorisées pour les objectifs des SLO basés sur des métriques. La précision affichée dans la vue détaillée d'un SLO correspond à `num_target_decimal_places + 1 = 4 décimales`. La précision exacte affichée dépend de la grandeur des valeurs de la requête du dénominateur. Plus la grandeur du dénominateur est élevée, plus la précision affichée est élevée (jusqu'à la limite de quatre décimales).

### Identifier cet indicateur

Cette section vous permet d'ajouter des informations contextuelles sur l'intérêt de votre SLO. Vous pouvez notamment spécifier une description, les ressources connexes, ainsi que les tags à associer au SLO.

## Pour aller plus loin

{{< partial name="whats-next/whats-next.html" >}}

[1]: https://app.datadoghq.com/slo
[2]: https://app.datadoghq.com/slo/new/metric
[3]: /fr/metrics/distributions/#threshold-queries
[4]: /fr/monitors/service_level_objectives/monitor/
[5]: https://docs.datadoghq.com/fr/tracing/generate_metrics/
[6]: https://docs.datadoghq.com/fr/real_user_monitoring/generate_metrics
[7]: https://docs.datadoghq.com/fr/logs/log_configuration/logs_to_metrics/#overview