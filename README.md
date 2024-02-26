# cl500
# Projekt Maschine Learnig
Collaboraters: Joshua und Sebastian

## Forschungsfrage:
"Wie beeinflussen verschiedene Merkmale wie Fahrzeugtyp, Kraftstoffart, Leistung, Alter des Fahrzeugs usw. den Fahrzeugpreis in einem Online-Marktplatz?"

## Hintergrund:
Der Datensatz enthält Informationen über verschiedene Merkmale von Fahrzeugen, die auf einem Online-Marktplatz zum Verkauf angeboten werden. Das Ziel dieses Machine Learning Projekts ist es, Muster und Zusammenhänge in den Daten zu identifizieren, um vorhersagen zu können, wie verschiedene Eigenschaften den Preis eines Fahrzeugs beeinflussen.

## Hypothesen:

Fahrzeugtyp könnte einen signifikanten Einfluss auf den Preis haben, da verschiedene Typen unterschiedliche Marktpreise haben.
Das Alter des Fahrzeugs könnte einen negativen Einfluss auf den Preis haben, da ältere Fahrzeuge tendenziell weniger wert sind.
Die Leistung des Fahrzeugs (in PS) könnte einen positiven Einfluss auf den Preis haben, da leistungsstärkere Fahrzeuge oft teurer sind.
Der Kilometerstand könnte einen negativen Einfluss auf den Preis haben, da Fahrzeuge mit höherem Kilometerstand normalerweise weniger wert sind.
Reparaturbedarf (notRepairedDamage) könnte den Preis beeinflussen, da Fahrzeuge mit Reparaturschäden oft günstiger sind.
Methodik:
Verwendet werden kann eine Regressionsanalyse, um die Beziehungen zwischen den unabhängigen Variablen (Merkmale) und der abhängigen Variable (Preis) zu modellieren. Zudem können verschiedene Machine Learning-Modelle wie lineare Regression, Random Forest oder Gradient Boosting getestet werden, um die beste Vorhersagegenauigkeit zu erzielen.

## Datenaufbereitung:
Es sollten fehlende oder inkonsistente Daten behandelt werden, um ein genaues Modell zu erstellen. Außerdem können kategoriale Variablen in numerische umgewandelt werden, um sie in den Modellen verwenden zu können.

## Auswertung:
Die Leistung der Modelle sollte anhand von Metriken wie dem Mean Squared Error (MSE) oder dem R-squared-Wert bewertet werden. Zusätzlich kann eine Feature-Importance-Analyse durchgeführt werden, um die wichtigsten Merkmale für die Preisvorhersage zu identifizieren.
