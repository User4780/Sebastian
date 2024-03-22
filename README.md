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
```
regression_model <- lm(price ~ powerPS, data = daten_sub)
```
Der R-squared-Wert ist sehr klein (0.0003257), was darauf hinweist, dass die Leistung allein nur einen sehr geringen Anteil an der Variation des Fahrzeugpreises erklärt.


```
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

```

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

## Zusammenfassung
Die Modelle haben eine sehr geringe erklärte Varianz, was darauf hindeutet, dass die gewählten Variablen den Fahrzeugpreis nur minimal erklären können.
"notRepairedDamage" und "firstDigitOfPLZ" scheinen nicht signifikant mit dem Fahrzeugpreis zusammenzuhängen.
Es könnte sinnvoll sein, weitere Variablen zu berücksichtigen oder die Daten genauer zu analysieren, um die Modellgenauigkeit zu verbessern.

# Code
```
# Lade das caret Paket
library(caret)
library(glmnet)


# Lese den Datensatz
daten <- read.csv("C:\\Users\\ABGGS\\OneDrive - ADAC SE\\Desktop\\autos.csv")

# Filtere den Datensatz
daten <- subset(daten, offerType == "Angebot")
daten <- subset(daten, powerPS > 10 & powerPS <= 800)
daten <- subset(daten, price > 2500 & price <= 1000000)
daten <- subset(daten, fuelType == "benzin" | fuelType == "diesel")
daten$firstDigitOfPLZ <- as.numeric(substr(daten$postalCode, 1, 1))
daten <- subset(daten, yearOfRegistration >= 1990)
daten <- subset(daten, select = -c(dateCreated, name, postalCode, seller, monthOfRegistration, nrOfPictures))
daten$notRepairedDamage <- ifelse(daten$notRepairedDamage %in% c("", "nein"), 0, 1)
daten <- na.omit(daten)
daten$dateCrawled <- as.Date(strptime(daten$dateCrawled, format="%Y-%m-%d %H:%M:%S"))
daten$lastSeen <- as.Date(strptime(daten$lastSeen, format="%Y-%m-%d %H:%M:%S"))

# Filtere nach ausgewählten Marken
selected_brands <- c("porsche", "mercedes_benz", "bmw", "audi")
daten_sub <- subset(daten, brand %in% selected_brands)
daten_sub <- daten_sub[, c("price", "powerPS", "yearOfRegistration", "brand", "kilometer", "notRepairedDamage", "gearbox", "fuelType")]

# Teile den Datensatz in Trainings- und Testdaten (80% Training, 20% Test)
set.seed(123)
index <- createDataPartition(daten_sub$price, p = 0.8, list = FALSE)
train_data <- daten_sub[index, ]
test_data <- daten_sub[-index, ]

# Erstelle eine Matrix mit den unabhängigen Variablen für Training
x_train <- model.matrix(price ~ . - 1, data = train_data)
# Erstelle die abhängige Variable für Training
y_train <- train_data$price

# Erstelle eine Matrix mit den unabhängigen Variablen für Test
x_test <- model.matrix(price ~ . - 1, data = test_data)
# Erstelle die abhängige Variable für Test
y_test <- test_data$price

# Erstelle das Lasso-Modell für Training
lasso_model <- cv.glmnet(x_train, y_train, alpha = 1)

# Finde den besten Lambda-Wert
best_lambda <- lasso_model$lambda.min 

# Wende das Lasso-Modell auf die Testdaten an
lasso_predictions <- predict(lasso_model, newx = x_test, s = best_lambda)


# Bewertung des Modells 
rmse <- sqrt(mean((lasso_predictions - y_test)^2))
print(paste("RMSE: ", rmse))

# Extrahiere die Koeffizienten des Lasso-Modells
lasso_coefficients <- coef(lasso_model, s = best_lambda)

# Zeige die Koeffizienten an
print(lasso_coefficients)


set.seed(123)  
random_indices <- sample(length(y_test), 100)
random_y_test <- y_test[random_indices]
random_lasso_predictions <- lasso_predictions[random_indices]

plot(random_y_test, random_lasso_predictions, 
     xlab = "Tatsächliche Preise", ylab = "Vorhergesagte Preise",
     main = "Vergleich von zufälligen tatsächlichen und vorhergesagten Preisen",
     xlim = c(min(y_test), max(y_test)), ylim = c(min(lasso_predictions), max(lasso_predictions)))

abline(a = 0, b = 1, col = "red")




```
# Ergebnis
RMSE:  16309.3680865425"
12 x 1 sparse Matrix of class "dgCMatrix"
                              s1
(Intercept)        -1.160795e+06
powerPS             8.146299e+01
yearOfRegistration  5.840779e+02
brandaudi           .           
brandbmw           -2.510059e+02
brandmercedes_benz  3.468114e+02
brandporsche        2.192542e+04
kilometer          -1.222966e-01
notRepairedDamage  -3.052038e+03
gearboxautomatik    .           
gearboxmanuell      6.009724e+02
fuelTypediesel      2.486132e+03
>
PowerPS: Der Koeffizient für PowerPS beträgt 81,463. Dies deutet darauf hin, dass eine Erhöhung der Leistung um ein PS zu einer Erhöhung des Fahrzeugpreises um etwa 81,46 Euro führt.

YearOfRegistration (Erstzulassung): Der Koeffizient für das Jahr der Zulassung beträgt 584,08. Dies zeigt an, dass neuere Fahrzeuge  höhere Preise haben, was den Erwartungen entspricht.

Marke (Brand): Die Koeffizienten für die verschiedenen Marken (Audi, BMW, Mercedes-Benz, Porsche) zeigen die geschätzten Preisunterschiede im Vergleich zu der Basismarke(Audi). 
Porsche hat einen Koeffizienten von 21.925,42 € was bedeutet, dass Porsche-Fahrzeuge im Durchschnitt einen deutlich höheren Preis haben.
Mercedes hat einen Koeffizienten von  346,81 €.
BMW ist im Durchschnitt 251,00 € günstiger als ein Audi.

Kilometer: Der Koeffizient für Kilometer beträgt -0,122. Das bedeutet das wenn pro Kilometer den das Auto mehr gefahren ist der Preis sich um durchschnittlich 12 ct verringert.

NotRepairedDamage (nicht reparierter Schaden): Der Koeffizient für nicht reparierten Schaden beträgt -3.052,04. Dies deutet darauf hin, dass Fahrzeuge mit nicht reparierten Schäden tendenziell niedrigere Preise haben.

Getriebe (Gearbox): Das manuelles Getriebe hat einen Koeffizienten von 600,97, was darauf hinweist, dass Fahrzeuge mit manuellem Getriebe im Durchschnitt einen höheren Preis haben als Automatik Getriebe.

FuelType (Kraftstoffart): Diesel hat einen Koeffizienten von 2.486,13, was darauf hinweist, dass Fahrzeuge mit Dieselkraftstoff im Durchschnitt einen höheren Preis haben als Autos die mit Benzin laufen.

![image](https://github.com/User4780/cl500/assets/148768739/7cda745e-be66-4030-98e5-7eb73f059959)
