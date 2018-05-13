# 1802_Betriebssysteme

Antworten auf die prüfungsvorbereitenden Fragen des Lehrstuhls

## PDF erzeugen

Pandoc muss auf dem Rechner installiert sein

```
Brice187$ pandoc -t markdown_github --atx-headers --normalize --reference-location=block -s -o all.md Kurseinheit1.md Kurseinheit2.md Kurseinheit3.md Kurseinheit4.md Kurseinheit5.md Kurseinheit6.md Kurseinheit7.md && pandoc -t latex --latex-engine=xelatex -o Fragen.pdf all.md && rm all.md
```
