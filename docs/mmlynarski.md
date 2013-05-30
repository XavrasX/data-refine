Zawodnicy UFC
====================

Lista zawodników największej organizacji MMA na świecie. Dane znalezione na [Wiki](http://wikipedia.org). Dane oczyściłem i stworyłem dodatkoew kolumny. Przykładowo dla zapisu "3-2-1 (4NC)" podzieliłem dane na kolumny: Wins-Loses-Draws (No contests).

## Przykładowe dane:
```json
{ 
  "_id" : ObjectId("519285b409f2c36427483957"), 
  "ISO" : "USA", 
  "Name" : "Frank Mir", 
  "Nickname" : null, 
  "Wins" : 14, 
  "Loses" : 7, 
  "Draws" : null, 
  "No contests" : null 
}
```

Plik json z danymi: [UFC_Fighters.json](/data/json/UFC_Fighters.json)

Import do mongoDB:

```bash
mongoimport --db nosql --collection UFC_Fighters --type json --file UFC_Fighters.json --jsonArray
```

## Agregacje

### 5 krajów z najwiekszą ilością zawodników
```
db.UFC_Fighters.aggregate( 
	{ $group : { _id : "$ISO", ilosc : { $sum : 1}}},
	{ $sort : { ilosc: -1} },
	{ $limit : 5 }
)
```
```
{
	"result" : [
		{
			"_id" : "USA",
			"ilosc" : 202
		},
		{
			"_id" : "BRA",
			"ilosc" : 70
		},
		{
			"_id" : "CAN",
			"ilosc" : 22
		},
		{
			"_id" : "UK",
			"ilosc" : 18
		},
		{
			"_id" : "JPN",
			"ilosc" : 9
		}
	],
	"ok" : 1
}
```

![wykres](../images/mmlynarski/UFC_top5krajow.png)

### Pięciu zawodników z największą ilością wygranych
```
db.UFC_Fighters.aggregate(
	{ $group : { _id : "$Wins"}},
	{ $sort : { ilosc: -1} },
	{ $limit : 5 }
)
```
```
{
	"result" : [
		{
			"_id" : 13
		},
		{
			"_id" : 11
		},
		{
			"_id" : 15
		},
		{
			"_id" : 18
		},
		{
			"_id" : 16
		}
	],
	"ok" : 1
}
```
### Liczba zawodników z USA
```
db.UFC_Fighters.aggregate( 
	{ $match : { ISO : "USA"}},
	{ $group : { _id : "$ISO", liczbaZawodnikowUSA : { $sum : 1}}}
)
```
```
{
	"result" : [
		{
			"_id" : "USA",
			"liczbaZawodnikowUSA" : 202
		}
	],
	"ok" : 1
}
```
