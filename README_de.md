# Primäre Datenanalyse und Empfehlungssystem für H&M-Kunden

## Einführung

Der Schwerpunkt dieses Projekts liegt auf der primären Datenanalyse und der Entwicklung eines personalisierten Empfehlungssystems für H&M-Kunden. Die Analyse erfolgt mit Python und umfasst eine umfangreiche explorative Datenanalyse (EDA) und eine anfängliche Datenanalyse (IDA).

## Wie man das Projekt startet
- Klone das Repository auf deinen lokalen Computer:
```sh
git clone https://github.com/your-username/your-repository.git
```
- Daten für die Analyse:
Die Daten sind zu groß, um sie auf GitHub zu speichern. Im Projekt werden direkte Links zu 3 Dateien auf meinem Google Drive verwendet: articles, customers und transactions.

- Öffne die Notebooks:
Öffne die Notebooks im Ordner /notebooks über Google Colab oder Jupyter Notebook, um den Code anzusehen und auszuführen.

- Installiere die erforderlichen Abhängigkeiten:
Installiere die erforderlichen Abhängigkeiten mit dem folgenden Befehl:
```sh
pip install -r requirements.txt
```
(Es wird angenommen, dass du eine Datei requirements.txt erstellt hast, die alle notwendigen Bibliotheken auflistet).

**Beispielcode für den Zugriff auf Daten auf Google Drive in Google Colab**
```python
from pydrive.auth import GoogleAuth
from pydrive.drive import GoogleDrive
from google.colab import auth
from oauth2client.client import GoogleCredentials
import pandas as pd

# Authentifizierung und Autorisierung
auth.authenticate_user()
gauth = GoogleAuth()
gauth.credentials = GoogleCredentials.get_application_default()
drive = GoogleDrive(gauth)

# Herunterladen von Dateien von Google Drive
file_ids = {
    'articles': '1H-g4oAbw1ZjYDOncx3xU3oro3tH3UIg6',
    'customers': '1KLM7Vum2O35MR17ULRLneM0uGVX61-VR',
    'transactions': '18ESMoz4VDe_ZCZzRPKKZS-UargXvrRdi'
}

for name, file_id in file_ids.items():
    downloaded = drive.CreateFile({'id': file_id})
    downloaded.GetContentFile(f'{name}.csv')
    print(f'{name}.csv downloaded')

# Lesen der Daten in DataFrames
df_articles = pd.read_csv('articles.csv')
df_customers = pd.read_csv('customers.csv')
df_transactions = pd.read_csv('transactions.csv')

# Anzeige der ersten Zeilen jedes DataFrames
print(df_articles.head())
print(df_customers.head())
print(df_transactions.head())
```
## Ziele und Aufgaben
### Ziele
### Aufgaben
1. **Daten sammeln und vorverarbeiten**
2. **Datenanalyse**
3. **Datenvisualisierung**
4. 
## Detaillierte Methodik
### Werkzeuge und Bibliotheken
In diesem Projekt wurden die folgenden Werkzeuge und Bibliotheken verwendet:

- **Python:** Die Hauptprogrammiersprache für Datenanalyse und Modellierung.

- **Pandas:** Bibliothek zur Datenverarbeitung und -analyse. Verwendet für die Arbeit mit DataFrames.

- **NumPy:** Bibliothek zur Arbeit mit Arrays und zur Durchführung numerischer Berechnungen.

- **Seaborn:** Auf matplotlib basierende Bibliothek zur Datenvisualisierung. Verwendet zur Erstellung statistischer Diagramme.

- **Matplotlib:** Die Hauptbibliothek zur Erstellung von Diagrammen und zur Datenvisualisierung.

- **Squarify:** Eine Bibliothek zur Erstellung von Treemap-Diagrammen.

- **textwrap:** Eine Bibliothek zum Umbruch von Textzeilen, damit der Text in die angegebenen Grenzen passt.

- **Plotly:** Eine Bibliothek für interaktive Datenvisualisierung.

### Datensammlung und Vorverarbeitung
**Datenquelle:**
Die Daten für dieses Projekt stammen von https://www.kaggle.com/competitions/h-and-m-personalized-fashion-recommendations/data

**Datenbankstruktur:**
Die Datenbank ist in drei Dateien unterteilt: ArticlesHM, CustomersHM und TransactionsHM.

**ArticlesHM** ist eine Tabelle, die alle Artikel von H&M enthält, mit insgesamt **105 542 Zeilen** und den folgenden Spalten:
- **article_id:** Artikel-ID, Datentyp - int64
- **prod_name:** Artikelname, Datentyp - object
- **product_type_name:** Produkttypname, Datentyp - object
- **product_group_name:** Produktgruppenname, Datentyp - object
- **graphical_appearance_name:** Name des grafischen Erscheinungsbildes, Datentyp - object
- **colour_group_name:** Farbgruppenname, Datentyp - object
- **index_name:** Name des Unterabschnitts, Datentyp - object
- **index_group_name:** Name des Abschnitts, Datentyp - object
- **section_name:** Sektionsname, Datentyp - object
- **garment_group_name:** Kategorie, Datentyp - object
- **detail_desc:** Details, Datentyp - object

**CustomersHM** ist eine Tabelle, die Informationen über H&M-Kunden enthält, mit insgesamt **1 371 979 Zeilen** und den folgenden Spalten:
- **customer_id:** Kunden-ID, Datentyp - object
- **Active:** Aktivitätsstatus, Datentyp - float64
- **club_member_status:** Clubmitgliedsstatus, Datentyp - object
- **fashion_news_frequency:** Häufigkeit der Modenachrichten, Datentyp - object
- **age:** Alter des Kunden, Datentyp - float64
- **postal_code:** Postleitzahl, Datentyp - object

**TransactionsHM** ist eine Tabelle, die Informationen über H&M-Verkäufe enthält, mit insgesamt **31 788 323 Zeilen** und den folgenden Spalten:
- **t_dat:** Verkaufsdatum, Datentyp - object
- **customer_id:** Kunden-ID, Datentyp - object
- **article_id:** Artikel-ID, Datentyp - int64
- **price:** Preis, Datentyp - float64
- **sales_channel_id:** Vertriebskanal, Datentyp - int64

#### Datenoptimierung

Der DataFrame **df_transactions** stellt ein sehr großes Informationsvolumen dar und erfordert daher die folgende zusätzliche Optimierung:

1. **Umwandlung der Spalte `t_dat` in `datetime64[ns]`:**
    - Speicherersparnis durch die Verwendung eines effizienteren Formats für Datumsangaben, das das Speichervolumen im Vergleich zum Typ object reduziert.

2. **Umwandlung der Spalte `customer_id` in den kategorialen Datentyp (`category`):**
    - Erhebliche Speicherreduktion, da kategoriale Daten in Form von Codes anstelle von Zeichenfolgen gespeichert werden, was den erforderlichen Speicherplatz für die Speicherung von Zeichenfolgendaten erheblich reduziert. Die Anzahl der eindeutigen Werte in customer_id beträgt 1 362 281 bei einer Gesamtanzahl von 31 788 324 Einträgen, was darauf hinweist, dass die Umwandlung der Spalte in einen kategorialen Datentyp nützlich sein kann, um den Speicherbedarf zu reduzieren.

3. **Umwandlung der Spalte `article_id` in `int32`:**
    - Speicherreduktion durch die Verwendung von 32-Bit-Ganzzahlen anstelle von 64-Bit-Ganzzahlen. Dies spart Speicherplatz bei der Speicherung von ganzzahligen Daten. Der minimale Wert von article_id beträgt 108 775 015 und der maximale Wert 956 217 002, was innerhalb des Bereichs von int32 (-2 147 483 648 bis 2 147 483 647) liegt.

4. **Umwandlung der Spalte `price` in `float32`:**
    - Speicherreduktion durch die Verwendung von 32-Bit-Gleitkommazahlen anstelle von 64-Bit-Gleitkommazahlen. Dies ist besonders nützlich für numerische Daten, bei denen keine hohe Genauigkeit erforderlich ist.

5. **Umwandlung der Spalte `sales_channel_id` in `int8`:**
    - Minimierung des Speicherbedarfs für diese Spalte durch die Verwendung von 8-Bit-Ganzzahlen. Da es in der Spalte sales_channel_id nur zwei eindeutige Werte (1 und 2) gibt, ist diese Umwandlung effektiv.

Diese Umwandlungen haben den Speicherbedarf des DataFrame df_transactions erheblich reduziert, von **1,2 GB** vor der Optimierung auf **679,3 MB** nach der Optimierung, was die Gesamtleistung des Systems bei der Arbeit mit großen Datenmengen verbessert.

**Datenbeispiele**
Produktgruppen:
```python
product_group_name
Garment Upper body       42741
Garment Lower body       19812
Garment Full body        13292
Accessories              11158
Underwear                 5490
Shoes                     5283
Swimwear                  3127
Socks & Tights            2442
Nightwear                 1899
Unknown                    121
Underwear/nightwear         54
Cosmetic                    49
Bags                        25
Items                       17
Furniture                   13
Garment and Shoe care        9
Stationery                   5
Interior textile             3
Fun                          2
```
Beispiele für Produkttypen in der Gruppe "Accessories":
```python
product_group_name     product_type_name       
Accessories            Accessories set                 7
                       Alice band                      6
                       Baby Bib                        3
                       Bag                          1280
                       Beanie                         56
                       Belt                          458
                       Bracelet                      180
                       Braces                          3
                       Bucket hat                      7
                       Cap                            13
                       Cap/peaked                    573
                       Dog Wear                       20
                       Earring                      1159
                       Earrings                       11
                       Eyeglasses                      2
                       Felt hat                       10
                       Giftbox                        15
                       Gloves                        367
                       Hair clip                     244
                       Hair string                   238
                       Hair ties                      24
                       Hair/alice band               854
                       Hairband                        2
                       Hat/beanie                   1349
                       Hat/brim                      396
                       Headband                        1
                       Necklace                      581
                       Other accessories            1034
                       Ring                          240
                       Scarf                        1013
                       Soft Toys                      46
                       Straw hat                       6
                       Sunglasses                    621
                       Tie                           141
                       Umbrella                       26
                       Wallet                         77
                       Watch                          73
                       Waterbottle                    22
```
Produktbereiche und ihre Kategorien
```python
index_group_name  index_name                      section_name                  
Baby/Children     Baby Sizes 50-98                Baby Boy                          1717
                                                  Baby Essentials & Complements     4932
                                                  Baby Girl                         1760
                                                  Kids & Baby Shoes                  457
                                                  Kids Local Relevance                 9
                  Children Accessories, Swimwear  Kids & Baby Shoes                 1685
                                                  Kids Accessories, Swimwear & D    1731
                                                  Kids Outerwear                    1199
                  Children Sizes 134-170          Boys Underwear & Basics            842
                                                  Girls Underwear & Basics          1485
                                                  Kids Outerwear                     636
                                                  Young Boy                         2352
                                                  Young Girl                        3899
                  Children Sizes 92-140           Boys Underwear & Basics           1192
                                                  Girls Underwear & Basics          2005
                                                  Kids Boy                          3328
                                                  Kids Girl                         4469
                                                  Kids Local Relevance               183
                                                  Kids Outerwear                     830
Divided           Divided                         Divided Accessories               1732
                                                  Divided Asia keys                  280
                                                  Divided Basics                    1723
                                                  Divided Collection                7124
                                                  Divided Complements Other           35
                                                  Divided Projects                  2364
                                                  Divided Selected                   991
                                                  EQ Divided                          26
                                                  Ladies Denim                       874
Ladieswear        Ladies Accessories              Womens Big accessories            1665
                                                  Womens Shoes                      2026
                                                  Womens Small accessories          3270
                  Ladieswear                      Collaborations                     559
                                                  H&M+                              2337
                                                  Ladies Denim                       227
                                                  Ladies Other                         4
                                                  Mama                              2266
                                                  Special Collections                682
                                                  Womens Casual                     2725
                                                  Womens Everyday Basics            1581
                                                  Womens Everyday Collection        7295
                                                  Womens Jackets                     829
                                                  Womens Nightwear, Socks & Tigh     228
                                                  Womens Premium                    1270
                                                  Womens Tailoring                  3376
                                                  Womens Trend                      2622
                  Lingeries/Tights                Womens Lingerie                   3598
                                                  Womens Nightwear, Socks & Tigh    1338
                                                  Womens Swimwear, beachwear        1839
Menswear          Menswear                        Contemporary Casual               1560
                                                  Contemporary Smart                1778
                                                  Contemporary Street               1490
                                                  Denim Men                          521
                                                  Men Accessories                   1337
                                                  Men Edition                        330
                                                  Men Other                           25
                                                  Men Other 2                        190
                                                  Men Project                        298
                                                  Men Shoes                          645
                                                  Men Suits & Tailoring             1428
                                                  Men Underwear                     2322
                                                  Mens Outerwear                     629
Sport             Sport                           Kids Sports                        626
                                                  Ladies H&M Sport                  1894
                                                  Men H&M Sport                      872
```
**CustomersHM** ist eine Tabelle, die Informationen über H&M-Kunden enthält, mit insgesamt **1 371 979 Zeilen** und den folgenden Spalten:
- **customer_id:** Kunden-ID, Datentyp - object
- **Active:** Aktivitätsstatus, Datentyp - float64
- **club_member_status:** Clubmitgliedsstatus, Datentyp - object
- **fashion_news_frequency:** Häufigkeit der Modenachrichten, Datentyp - object
- **age:** Alter des Kunden, Datentyp - float64
- **postal_code:** Postleitzahl, Datentyp - object

**TransactionsHM** ist eine Tabelle, die Informationen über H&M-Verkäufe enthält, mit insgesamt **31 788 323 Zeilen** und den folgenden Spalten:
- **t_dat:** Verkaufsdatum, Datentyp - object
- **customer_id:** Kunden-ID, Datentyp - object
- **article_id:** Artikel-ID, Datentyp - int64
- **price:** Preis, Datentyp - float64
- **sales_channel_id:** Vertriebskanal, Datentyp - int64

### Analyse und Visualisierung der Daten

**Verteilung der Produkte nach Produktgruppen**

Die Datenbank enthält insgesamt 19 Produktgruppen. Die größte Anzahl an Produkten gehört zur Gruppe "Garment Upper body", die 40,5% aller Produkte ausmacht. Die am wenigsten vertretenen Gruppen sind: 'Nightwear', 'Unknown', 'Underwear/nightwear', 'Cosmetic', 'Bags', 'Items', 'Furniture', 'Garment and Shoe care', 'Stationery', 'Interior textile', 'Fun'. Alle kleineren Gruppen sind in der Visualisierung unter "Other" zusammengefasst.

![VerteilungProdukteNachProduktgruppen](https://github.com/OlhaAD/EDA_IDA_H_and_M_Python/blob/main/visualizations/ProductGroupPie.png)

**Verteilung der Produkte nach Produkttypen innerhalb jeder Gruppe**

Jede der 19 Gruppen hat zusätzlich eine Unterteilung nach Produkttypen. Die Gruppe "Accessories" hat die meisten verschiedenen Produkttypen. Beispiele für Produkttypen in der Gruppe "Accessories" sind im oberen Abschnitt dargestellt. Die Verteilung der drei größten Gruppen "Garment Full body", "Garment Lower body" und "Garment Upper body" nach Produkttypen ist im untenstehenden Balkendiagramm dargestellt.

![VerteilungProdukteNachProdukttypenInnerhalbJederGruppe](https://github.com/OlhaAD/EDA_IDA_H_and_M_Python/blob/main/visualizations/ProductTypesIn3Groups.png)

**Verteilung der Produkte nach Drucktypen**

Die Analyse der Drucktypen zeigt, welche Drucke unter den Produkten am häufigsten vorkommen. Die Datenbank enthält insgesamt 30 verschiedene Drucktypen. Aus dem Diagramm geht hervor, dass der beliebteste Druck "Solid" ist, der 47,1% aller Produkte ausmacht. Die seltensten Drucktypen sind in der Gruppe "Other" zusammengefasst.

![VerteilungProdukteNachDrucktypen](https://github.com/OlhaAD/EDA_IDA_H_and_M_Python/blob/main/visualizations/ProductDruckePie.png)


**Verteilung der Produkte nach Farben**

Von den 50 verschiedenen Farben ist Schwarz die am häufigsten vorkommende Farbe. Für eine bessere Übersichtlichkeit der folgenden Visualisierung wurde ein Farblexikon erstellt und die Bibliothek Squarify verwendet.

![VerteilungProdukteNachFarben](https://github.com/OlhaAD/EDA_IDA_H_and_M_Python/blob/main/visualizations/ProductColorsTreemap.png)

**Verteilung der Produkte nach Abteilungen**

Eine weitere wichtige Visualisierung zeigt, wie die Produkte auf die verschiedenen Abteilungen verteilt sind. Dies hilft zu verstehen, welche Produkte beliebter sind. Aus dem Diagramm geht hervor, dass die Abteilung "Ladieswear" mit einem Anteil von 24,6% den größten Teil ausmacht. Die zweitgrößte Abteilung "Divided" umfasst jugendliche und preisgünstigere Kleidung.

![VerteilungProdukteNachAbteilungen](https://github.com/OlhaAD/EDA_IDA_H_and_M_Python/blob/main/visualizations/ProductIndexPie.png)

**Verteilung der Produkte nach Sektionen und Abteilungen innerhalb der Abteilungsgruppen**

Zusätzlich sind einige Abteilungen in Gruppen zusammengefasst, und jede Abteilung enthält Produktsektionen. Im Abschnitt "Datenbankstruktur" ist ein Beispiel für die Daten in Form einer Liste aller Sektionen mit ihrer Unterteilung nach Abteilungen und Abteilungsgruppen dargestellt. Unten ist ein Sunburst-Diagramm dargestellt, das diese Aufteilung visualisiert.

![VerteilungProdukteNachSektionenUndAbteilungenInnerhalbAbteilungsgruppen](https://github.com/OlhaAD/EDA_IDA_H_and_M_Python/blob/main/visualizations/ProductSectionsInCategories.png)

**Verteilung der Produkte nach Sektionen und Kategorien innerhalb der Abteilungsgruppe "Ladieswear"**

Aus dem obigen Diagramm sehen wir, dass nur zwei Abteilungsgruppen eine weitere Unterteilung in mehrere Kategorien haben, von denen jede ihre eigenen Sektionen hat. Diese Abteilungsgruppen sind "Ladieswear" und "Baby/Children". Diese beiden Abteilungsgruppen sind jeweils separat visualisiert.

![VerteilungProdukteNachSektionenUndKategorienInnerhalbAbteilungsgruppeLadieswear](https://github.com/OlhaAD/EDA_IDA_H_and_M_Python/blob/main/visualizations/ProductSectionsInLadieswear.png)

**Verteilung der Produkte nach Sektionen und Kategorien innerhalb der Abteilungsgruppe "Baby/Children"**

![VerteilungProdukteNachSektionenUndKategorienInnerhalbAbteilungsgruppeBabyChildren](https://github.com/OlhaAD/EDA_IDA_H_and_M_Python/blob/main/visualizations/ProductSectionsInBaby_Children.png)

**Verteilung der Produkte nach Kategorien**

Wir sehen, dass die größte Kategorie "Jersey Fancy" ist, gefolgt von der Kategorie "Accessories".

![VerteilungProdukteNachKategorien](https://github.com/OlhaAD/EDA_IDA_H_and_M_Python/blob/main/visualizations/ProductGarmentPie.png)

**Verteilung der Kunden nach ihrem Clubstatus**

Die Mehrheit der Kunden sind Clubmitglieder (92.7%), etwa 6.8% befinden sich im Prozess der Clubmitgliedschaft, und ein sehr kleiner Teil — 0.0003% (467 Kunden) hat den Club verlassen. Diese Gruppe wurde zusammen mit den unbestimmten Werten in die Kategorie "Other" aufgenommen.

![VerteilungKundenNachIhremClubstatus](https://github.com/OlhaAD/EDA_IDA_H_and_M_Python/blob/main/visualizations/ClubStatusClientPie.png)

**Analyse der Häufigkeit des Versands von Nachrichten an Kunden**

Etwas mehr als ein Drittel der Kunden (34.8%) sind für regelmäßige Nachrichten angemeldet, 0.1% erhalten Nachrichten monatlich. Der Großteil der Kunden (65.1%) hat überhaupt kein Abonnement. Dies ist ein bedeutender Anteil, und es sollten Schritte unternommen werden, um diesen Anteil zu verringern. Wichtig ist, dass die Nachrichten nicht zu Spam werden, sondern moderelevante Inhalte enthalten, die Interesse wecken, die Website des Unternehmens besuchen und idealerweise Käufe tätigen.

![AnalyseDerHäufigkeitDesVersandsVonNachrichtenAnKunden](https://github.com/OlhaAD/EDA_IDA_H_and_M_Python/blob/main/visualizations/FashionNewsFrequencytPie.png)

**Visualisierung der Altersverteilung der Kunden**

Das Histogramm zeigt zwei Haupt-Altersspitzen unter den Käufern:

- Der erste Gipfel liegt bei etwa 21 Jahren.
- Der zweite Gipfel liegt bei etwa 50 Jahren.

Es ist auch ein Rückgang der Aktivität im Bereich um die 40 Jahre zu erkennen. Dies könnte ein interessanter Bereich für eine detaillierte Analyse sein. Zum Beispiel sollte untersucht werden, welche Produktgruppen diese Altersgruppe kauft, zu welcher Zeit sie einkaufen und wie oft sie Newsletter erhalten. Das Verständnis dieser Faktoren kann helfen, die Gründe für den Rückgang der Aktivität in dieser Altersgruppe zu erkennen.

Darüber hinaus zeigt das Histogramm einen recht langen Schwanz in Richtung der älteren Kunden, was auf eine beträchtliche Anzahl älterer Käufer hinweist. Dies ist bei der Entwicklung von Marketingstrategien wichtig zu berücksichtigen, da sie eine bedeutende Kundengruppe darstellen.

![AltersverteilungDerKundenMithilfeHistogramm](https://github.com/OlhaAD/EDA_IDA_H_and_M_Python/blob/main/visualizations/ClientsAgeHistogramm.png)

Das Boxplot zeigt:

- Der Median des Altersverteilung liegt bei 31 Jahren, was darauf hinweist, dass die Hauptgruppe der Kunden jung ist.
- Das erste Quartil (25%) umfasst die Altersgruppe von 24 bis 31 Jahren.
- Das dritte Quartil (75%) umfasst die Altersgruppe von 31 bis 49 Jahren, was deutlich breiter ist als die Altersgruppe des zweiten Quartils.

Diese Analyse unterstreicht erneut die Bedeutung der Kunden im dritten Quartil und die Notwendigkeit zusätzlicher Analysen und Maßnahmen seitens des Unternehmens, um die Bedürfnisse dieser Gruppe zu erfüllen.

![AltersverteilungDerKundenMithilfeBoxplot](https://github.com/OlhaAD/EDA_IDA_H_and_M_Python/blob/main/visualizations/ClientsAgeBoxPlot.png)

**Verteilung der Nachrichtensendehäufigkeit nach Altersquantilen**

Das vorherige Boxplot-Diagramm zeigte die Notwendigkeit einer zusätzlichen Analyse der Kunden nach Altersquantilen. Insbesondere interessiert uns das dritte Quartil und der Aktivitätsrückgang dieser Altersgruppe. Im folgenden Balkendiagramm sehen wir, dass alle Gruppen ungefähr gleich verteilt sind, mit geringfügigen Unterschieden.

Der niedrigste Prozentsatz an Newsletter-Abonnements ist in der aktivsten Kundengruppe im Alter von 24-31 Jahren zu beobachten. Es lohnt sich, zusätzlich mit dieser Gruppe zu arbeiten, um ihr Interesse an Abonnementmaterialien zu erhöhen und die Bedürfnisse dieser Gruppe genauer zu untersuchen. Dies gilt auch für die Kunden des dritten Quartils.

![VerteilungNachrichtensendehäufigkeitNachAltersquantilen](https://github.com/OlhaAD/EDA_IDA_H_and_M_Python/blob/main/visualizations/FashionNewsFrequencyAgeQuartile.png)

**Verteilung des Aktivitätsstatus nach Altersquantilen**

Das folgende Diagramm zeigt die Verteilung der aktiven Kunden nach Altersquantilen. Es ist zu erkennen, dass der Prozentsatz der aktiven Kunden ebenfalls relativ gleichmäßig auf alle Quartile verteilt ist, obwohl es immer noch kleine Unterschiede gibt, die untersucht und genutzt werden können, um die Aktivität in jeder Altersgruppe zu steigern.

![VerteilungAktivitätsstatusNachAltersquantilen](https://github.com/OlhaAD/EDA_IDA_H_and_M_Python/blob/main/visualizations/ActiveStatusAgeQuartile.png)

**Verteilung der Kunden nach Postleitzahl**

Wir sehen, dass sehr viele Kunden die gleiche Postleitzahl haben. Ich gehe davon aus, dass dieser Index nichts mit dem physischen Index der Kunden zu tun hat und höchstwahrscheinlich der Index eines Paketterminals oder einer Lieferstelle für große Bestellungen ist

| Index  | Postal Code                                                      | Counts |
|--------|------------------------------------------------------------------|--------|
| 61034  | 2c29ae653a9282cce4151bd87643c907644e09541abc28ae87dea0d1f6603b1c | 120303 |
| 281937 | cc4ed85e30f4977dae47662ddc468cd2eec11472de6fac                   | 261    |
| 156090 | 714976379549eb90aae4a71bca6c7402cc646ae7c40f6c                   | 159    |
| 171208 | 7c1fa3b0ec1d37ce2c3f34f63bd792f3b4494f324b6be5                   | 157    |
| 126228 | 5b7eb31eabebd3277de632b82267286d847fd5d44287ee                   | 156    |
