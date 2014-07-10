# R Grundlagen

Im folgenden werden die R-Befehle aus dem Kurs kurz zusammengefasst. Die Liste ist **nicht** ausführlich, beim Fehlen wichtiger Befehle kann bzw. sollte man darüber Bescheid geben, damit ich die Liste aktuellisieren kann! 

Bemerken Sie, dass die Notation `paket::befehl` genutz wird, um das Paket mit der Funktion anzugeben. Die Beispiele für den Befehl funktionieren nur dann, wenn Sie schon `library(paket)` ausgeführt haben.

# Umgang mit Daten

## aggregate()
*Aggregiert* Daten -- sehr nützlich für Zusammenfassungen, z.B. Mittelwerte über einen Faktor.
```
aggregate(dv~bedingung*proband, data=datensatz, FUN=mean) 
```

## scale()
Standardisiert eine Variable(Ergebnis: $z$-Wert)
```
scale(x)
```

Zentriert eine Variable
```
scale(x, scale=FALSE)
```

## as.factor() and factor()
Wandelt ein Objekt in Faktor (kateogirische bzw. nominalskalierte Variable) um:
```
datensatz$spalte <- as.factor(datensatz$spalte)
datensatz$spalte <- factor(datensatz$spalte)
``` 
`as.factor()` ändert die Kodierung der Faktorstufen bei einer bestehenden Variable nicht, während `factor()` eine erneute Kodierung macht. Bei bestehenden Faktoren, die durch `subset()` oder ähnliches Stufen verloren haben, macht das einen Unterschied! 

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

## cov()

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

Einstichproben t-Test ungerichtet
```
t.test(AV,mu=...)
```

Einstichproben t-Test gerichtet
```
t.test(AV,mu=…, alternative="greater")
t.test(AV,mu=…,alternative="less")
```

Zwei unabhängige Stichproben bei Varianzhomogenität
```
t.test(AV~UV, var.equal=TRUE)
t.test(AV~UV, var.equal=TRUE, alternative="greater")
t.test(AV~UV, var.equal=TRUE, alternative="less")
```

Zwei unabhängige Stichproben bei Varianzheterogenität
```
t.test(AV~UV)
t.test(AV~UV, alternative="less")
t.test(AV~UV, alternative="greater")
```

Zwei abhängige Stichproben
```
t.test(V1, V2, paired=TRUE)
t.test(V1, V2, paired=TRUE, alternative="greater") 
t.test(V1, V2, paired=TRUE, alternative="less") 
```

## var.test()

## car::leveneTest()

## shapiro.test()

## aov()
Einfaktorielle Varianzanalyse ohne Messwiederholung (Varianzen homogen)
```
aov(AV~UV)
```
Mehrfaktorielle Varianzanalyse ohne Messwiederholung (Varianzen homogen)

```
# ohne Interaktionen
aov(AV~UV1+UV2)
# mit Interaktionen 
aov(AV~UV1*UV2) 
```

## ez::ezANOVA()
Varianzanalyse mit Messwiederholung
```
ezANOVA(data,dv=. ,wid=. ,within=. , between=. ) 
```

## ez::ezSummary()

## lm()
Lineare Regression
```
lm(AV~UV) 
```

Multiple Regression
```
lm(AV~UV1+UV2) 
```
## confint()

## anova()
Modellvergleich (verschachtelte Modelle)
```
anova(modell1, modell2)
```

## cor.test()
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

## chisq.test()

# Grafiken
## base

### qqnorm()

## ggplot2

### qplot()

### ggplot()

### aes()

### geom_point()

### geom_smooth()

### geom_bar()

### geom_error()

### geom_line()

### geom_jitter()

### geom_histogram()

### geom_density()

### geom_boxplot()

# Attributions
Image adapted from <a href='//blog.revolutionanalytics.com/2010/11/acm-data-mining-camp-1.html'>Revolution Analytics Blog</a>.

