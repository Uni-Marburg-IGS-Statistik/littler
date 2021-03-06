# R Grundlagen

Im folgenden werden die R-Befehle aus dem Kurs kurz zusammengefasst. Die Liste ist **nicht** ausführlich, beim Fehlen wichtiger Befehle kann bzw. sollte man darüber Bescheid geben, damit ich die Liste aktuellisieren kann! 

Bemerken Sie, dass die Notation `paket::befehl` genutz wird, um das Paket mit der Funktion anzugeben. Die Beispiele für den Befehl funktionieren nur dann, wenn Sie schon `library(paket)` ausgeführt haben.

# Umgang mit Daten

## aggregate()
*Aggregiert* Daten -- sehr nützlich für Zusammenfassungen, z.B. Mittelwerte über einen Faktor.

```
aggregate(dv ~ bedingung * proband, data=datensatz, FUN=mean) 
```

Daten in tabellarischer Form nach bestimmter Funktion (z.B. `sd`, `var`, `mean`, `sum`) listen. Anwendung d. Funktion auf `Variable1` und Listung nach `Variable2` & `Variable3`.

```
aggregate(variable1 ~ variable2 + variable3, data=data, FUN=function)
aggregate(Lex_Dec ~ Aphasie + Geschlecht, data = aphasiker, FUN=mean)
```
## scale()
Standardisiert eine Variable (Ergebnis: $z$-Wert)

```
scale(x)
```

Zentriert eine Variable

```
scale(x, scale=FALSE)
```

Bei `center=FALSE` und `scale=TRUE`, Skalierungsparameter = nicht mehr Standardabweichung.

## head(), tail()
Ersten (`head()`) oder letzten (`tail()`) Elemente  aus Datensatz anzeigen lassen. `n = number` (number durch Zahl ersetzen) wieviele Elemente angezeigt werden sollen.

```
head(daten, n=number)
tail(daten, n=number)
```

## as.factor() and factor()
Wandelt ein Objekt in Faktor (kateogirische bzw. nominalskalierte Variable) um:

```
datensatz$spalte <- as.factor(datensatz$spalte)
datensatz$spalte <- factor(datensatz$spalte)
``` 

`as.factor()` ändert die Kodierung der Faktorstufen bei einer bestehenden Variable nicht, während `factor()` eine erneute Kodierung macht. Bei bestehenden Faktoren, die durch `subset()` oder ähnliches Stufen verloren haben, macht das einen Unterschied! 

## length() 
Wieviel Elemente ein Vektor enthält.

```
length(vektor)
```

## sort()
Elemente eines Vektors sortieren (p. default: `decreasing=FALSE`)

```
sort(daten)
```

f. tabellarische Darstellung

```
sort(table(daten))
```

## Subsetting

Vektor erstellen:

```
Broca_LexDec <- aphasiker$Lex_Dec[aphasiker$Aphasie=="B"]
```

Teilmenge eines Datensatzes erstellen:

Mit `[]`
```
AphasieBW <- aphasiker[aphasiker$Aphasie=="B" | aphasiker$Aphasie=="W",]
```

Das Komma (`,`) trennt die zu definierenden Zeilen und Spalten.
Vor  dem Komma werden die Zeilen bestimmt, die ausgewählt werden sollen, 
 nach dem Komma werden die Spalten bestimmt, die ausgewählt werden sollen. 

Mit `subset()`:

```
AphasieBW <- subset(aphasiker, Aphasie=="B" | Aphasie=="W")
```

 `|` wird auf der Tastatur mit `Alt+GR <` erzeugt; es bedeutet "logisches oder" (wir wollen Broca- und Wernicke-Aphasiker, aber jeder Patient hat nur Broca- *oder* Wernicke-Aphasie). `&` bedeutet "logisches und"


## reshape2::dcast()
Wandelt Daten aus dem Longformat in andere Formate um. Name kommt aus der Schmieden-Metapher: Longformat ist das "melted" (*geschmozelne*) Urformat, aus dem alle andere Format durch "casting" (*gießen*) hergestellt werden.

## reshape2::melt()
Umformatiert ins Longformat. Name kommt aus der Schmieden-Metapher: Longformat ist das "melted" (*geschmozelne*)  Urformat, aus dem alle andere Format durch "casting" (*gießen*) hergestellt werden. 

```
melt(datensatz, ..., na.rm=FALSE, value.name="value") 
```

## xtable::xtable()
Ermöglicht das Formatieren von tabellarischem Output für LaTeX, HTML, usw. In RMarkdown ist die Block-Option `results='asis'` notwendig:

    ```{r, results='asis'}
    print(xtable(tabelle), type="html"), include.rownames=F) 
    ```
  
# Deskriptive Statistik 

## xtabs()
Erstellt Kreuztabelle (Tabelle für Kombinationen bestimmter Merkmalsausprägungen).


```
tabelle <-xtabs(Freq ~ Sex + Survived,passengers)
tabelle <-xtabs(~ major + sex, kurs) 
```

## table()
Erstellt eine Häufigkeitstabelle für eine Variable, in der die absoluten Häufigkeiten aufgelistet werden

```
table(x)
```

## prop.table()
*proportions* table, gibt die relativen Häufigkeiten einer Häufigkeitstabelle wieder

```
prop.table(tabelle)
```
Durch das Multiplizieren mit 100 werden die relativen Häufigkeiten in Prozent umgewandelt

```
100*prop.table(tabelle)
```

## max()

Gibt den höchsten Wert in einem Objekt an

```
max(x)
```

## min()
Gibt den niedrigsten Wert in einem Objekt an

```
min(x)
```

## sum()
Gibt die Summe aller in einem Objekt enthaltenen Werte an

```
sum(x)
```

## median()
Gibt den Median aller Werte in einem Objekt an

```
median(x)
```

## mean()
Gibt das arithmetische Mittel aller Werte in einem Objekt an

```
mean(x)
```

## range()
Gibt den Wertebereich eines Objektes an

```
range(x)
```

## diff()
Berechnet die Differenz mehrere Objekte

```
diff(x)
```

Besonders nützlich mit `range(x)`

```
diff(range(x))
```

## quantile()
Gibt Minimum, Maximum sowie die drei Quartile eines Vektors an

```
quantile(x)
```

## sd()
Gibt die Standardabweichung aller werte in einem Objekt an. Wird mit Bessels Korrektur berechnet, d.h. mit $n-1$ im Nenner, sodass man die den Populationswert aus einer Stichprobe schätzt.

```
sd(x)
```

## var()
Gibt die Varianz aller Werte in einem Objekt an. Wird mit Bessels Korrektur berechnet, d.h. mit $n-1$ im Nenner, sodass man die den Populationswert aus einer Stichprobe schätzt.

```
var(x)
```

## cor()
Berechnet den Korrelationskoeffizienten (per default Pearsons $r$).

```
cor(x,y)
```

Weitere Möglichkeiten sind Spearmans Rho ($\rho$)

```
cor(x,y, method="spearman")
```

und Kendalls Tau ($\tau$)

```
cor(x,y, method="kendall")
```


## cov()
Gibt Kovarianz aus.

```
cov(x,y)
```


# Verteilungen

## Binomial
`dbinom()`, `pbinom()`, `qbinom()`, `rbinom()`

## Normal
`dnorm()`, `pnorm()`, `qnorm()`, `rnorm()`

## $\chi^2$
`dchisq()`, `pchisq()`, `qchisq()`, `rchisq()`

## F
`df()`, `pf()`, `qf()`, `rf()`

## Poisson
`dpois()`, `ppois()`, `qpois()`, `rpois()`


## t
`dt()`, `pt()`, `qt()`, `rt()`

## Uniform 
`dunif()`, `punif()`, `qunif()`, `runif()`

# Interferenzstatistik

## t.test()

Einstichproben $t$-Test ungerichtet

```
t.test(AV,mu=...)
```

Einstichproben $t$-Test gerichtet

```
t.test(AV,mu=..., alternative="greater")
t.test(AV,mu=..., alternative="less")
```

Zwei unabhängige Stichproben bei Varianzhomogenität

```
t.test(AV~UV, var.equal=TRUE)
t.test(AV~UV, var.equal=TRUE, alternative="greater")
t.test(AV~UV, var.equal=TRUE, alternative="less")
```

Zwei unabhängige Stichproben bei Varianzheterogenität (Welch $t$-Test)

```
t.test(AV~UV)
t.test(AV~UV, alternative="less")
t.test(AV~UV, alternative="greater")
```

Beispiel mit der Syntax für zwei Vektoren

```
Broca_LexDec <- aphasiker$Lex_Dec[aphasiker$Aphasie=="B"] 
Wernicke_LexDec <- aphasiker$Lex_Dec[aphasiker$Aphasie=="W"] 
t.test(Broca_LexDec, Wernicke_LexDec)
```
Beispiel mit der Formelsyntax

```
AphasieBW <- aphasiker[aphasiker$Aphasie=="B" | aphasiker$Aphasie=="W",] 
t.test(Lex_Dec ~ Aphasie)
```

Zwei abhängige Stichproben

```
t.test(V1, V2, paired=TRUE)
t.test(V1, V2, paired=TRUE, alternative="greater") 
t.test(V1, V2, paired=TRUE, alternative="less") 
```

## var.test()

$F$-Test

Vektorsyntax

```
var.test(gruppe1, gruppe2) 
``` 

Formelsyntax

```
var.test(messwert ~ gruppierungswert) 
``` 

## car::leveneTest()
Führt Levenes Test aus. Nur Formelsyntax funktioniert. (Vektorsyntax funtkioniert nicht!) 

```
levene.test(gruppe1 ~ gruppe2)
levene.test(gruppe1 ~ gruppe2 * gruppe3)
```

## shapiro.test()
Berechnet den Shapiro–Wilk-Test.

```
shapiro.test(x)
shapiro.test(rt[rt$subj==f, "RT"]) 
```

## aov()
Einfaktorielle Varianzanalyse ohne Messwiederholung (Varianzen homogen)

```
aov(AV ~ UV)
```
Mehrfaktorielle Varianzanalyse ohne Messwiederholung (Varianzen homogen)

```
# ohne Interaktionen
aov(AV ~ UV1 + UV2)
# mit Interaktionen 
aov(AV ~ UV1 * UV2) 
```

## ez::ezANOVA()
Varianzanalyse mit Messwiederholung

```
ezANOVA(data
    ,dv=.(AV)
    ,wid=.(within-ID)
    ,within=.(UV.innerhalb.Probanden)
    ,between=.(UV.zwischen.Probanden)
    ,detailed=TRUE) 
```

## ez::ezSummary()

## lm()
Lineare Regression

```
lm(AV ~ UV) 
```

Multiple Regression

```
lm(AV ~ UV1 + UV2) 
```

## confint()
Gibt Konfidenzintervall aus z.B. für Regressionsgewicht

```
confint(lm(...))
```

## anova()
Modellvergleich (verschachtelte Modelle)

```
anova(modell1, modell2)
```

## cor.test()
Signifikanztest für Korrelationen. Zeigt auch das Konfidenzintervall an. 

Korrelation (Pearson)

```
cor.test(V1,V2)
```

Rangkorrelation (Spearman)

```
cor.test(V1,V2,method="spearman") 
```

Rangkorrelation (Kendall)

```
cor.test(V1,V2,method="kendall") 
```

## lme4::lmer()
Berechnet ein gemischtes Modell.

Nur Intercepts nach RE

```
Modell <- lmer(AV ~ FE + (1|RE), data=x) 
```

Nur Anstieg nach RE

```
Modell <- lmer(AV ~ FE + (0 + FE|RE), data=x) 
```

Intercept und Anstieg nach RE

```
Modell <- lmer(AV ~ FE + (1 + FE|RE), data=x) 
```

Intercept und Anstieg, getrennt, nach RE

```
Modell <- lmer(AV ~ FE + (1|RE) + (0 + FE|RE), data=x) 
```

## chisq.test()
$\chi^2$-Test

```
chisq.test(tabelle,correct=F)
```

$\chi^2$-Test mit Yates Korrektur

```
chisq.test(tabelle)
```

Erwartungswerte

```
chisq.test(tabelle)$expected
```

## fisher.test()
Exakter Test nach Fisher. Gitb auch Konfidenzintervalle aus; nur für kleinere Tabellen!

```
fisher.test(tabelle)
```

# Grafiken
## base

### qqnorm()

## ggplot2

### qplot()
Quick Plot. 

```
qplot(x=x-Achse, y=y-Achse, daten, geom="form")
```

Mögliche Formen sind u.a. `boxplot`, `density`, `histogram`, `bar`, `violin`.


### ggplot()

### aes()

### geom_point()
Plottet Einzelpunkte. 

Pseudo 3D

```
ggplot(linreg, aes(x=x1,y=x2)) + geom_point(aes(size=y)) 
```

`size` bestimmt die Dicke des Punktes.

### geom_smooth()
Fügt Regressionslinie ein.

Lineare Regression Grafik

```
ggplot(daten, aes(x=UV,y=AV)) + geom_point() + geom_smooth(method="lm") 
```

### geom_bar()

### geom_error()

### geom_line()

### geom_jitter()

### geom_histogram()

### geom_density()

Density Grafik

```
ggplot(daten) + geom_density(x=AV, color=UV, fill=UV), alpha=0.5) 
ggplot(data)+ geom_density(aes(x=RT,color=cond,fill=cond), alpha=0.1) + facet_wrap(~item) 
```

### geom_boxplot()

Bsp.mit einer Gruppe:

```
weight.bw <- ggplot(data) + geom_boxplot(aes(x=I("x"),y=weight)) 
print(weight.bw)
```

Wenn man es nach target/subject aufgeteilt haben will: `+ facet_wrap(~target)` bzw. + `facet_wrap(~subject)`


Bsp. mit 2 Gruppen:

```
weight.bw.sex <- ggplot(data) + geom_boxplot(aes(x=sex,y=weight)) print(weight.bw.sex)
```

### facet_wrap()
Teilt die Grafik nach dem genannten Faktor auf bzw. verschiedene Darstellungen pro Stufe der Variable.

```
... + facet_wrap(~variable)
```

# Attributions
Image adapted from <a href='//blog.revolutionanalytics.com/2010/11/acm-data-mining-camp-1.html'>Revolution Analytics Blog</a>.

