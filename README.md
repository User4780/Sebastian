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

## Regressionsmodell:
### regression_model <- lm(price ~ powerPS, data = daten_sub)
Der R-squared-Wert ist sehr klein (0.0003257), was darauf hinweist, dass die Leistung allein nur einen sehr geringen Anteil an der Variation des Fahrzeugpreises erklärt.


''' 
# Variante 1: Nur Leistung und Kilometer
model_1 <- lm(price ~ powerPS + kilometer, data = daten_sub)

# Variante 2: Leistung, Kilometer und unreparierter Schaden
model_2 <- lm(price ~ powerPS + kilometer + notRepairedDamage, data = daten_sub)

# Variante 3: Leistung, Kilometer, unreparierter Schaden und erste Ziffer der PLZ
model_3 <- lm(price ~ powerPS + kilometer + notRepairedDamage + firstDigitOfPLZ, data = daten_sub)

# Vergleiche die Modelle
summary(model_1)
summary(model_2)
summary(model_3)

'''

Modell 1:

Unabhängige Variablen: "powerPS" und "kilometer"
Ergebnisse:
Beide Variablen ("powerPS" und "kilometer") sind statistisch signifikant.
Das Modell hat eine sehr geringe erklärte Varianz (R-squared: 0.0004572).
F-Test zeigt, dass das Modell als Ganzes signifikant ist.
Modell 2:

Unabhängige Variablen: "powerPS", "kilometer" und "notRepairedDamage"
Ergebnisse:
"powerPS" und "kilometer" sind statistisch signifikant.
"notRepairedDamage" ist nicht signifikant.
Erklärte Varianz leicht verbessert (R-squared: 0.0004638).
F-Test zeigt statistische Signifikanz.
Modell 3:

Unabhängige Variablen: "powerPS", "kilometer", "notRepairedDamage" und "firstDigitOfPLZ"
Ergebnisse:
"powerPS" und "kilometer" sind statistisch signifikant.
"notRepairedDamage" und "firstDigitOfPLZ" sind nicht signifikant.
Geringfügige Verbesserung der erklärten Varianz (R-squared: 0.0004647).
F-Test zeigt statistische Signifikanz.
Allgemeine Beobachtungen:

Die Modelle haben eine sehr geringe erklärte Varianz, was darauf hindeutet, dass die gewählten Variablen den Fahrzeugpreis nur minimal erklären können.
"notRepairedDamage" und "firstDigitOfPLZ" scheinen nicht signifikant mit dem Fahrzeugpreis zusammenzuhängen.
Es könnte sinnvoll sein, weitere Variablen zu berücksichtigen oder die Daten genauer zu analysieren, um die Modellgenauigkeit zu verbessern.


