---
| Korlátazonosító | Korlát | Megjegyzések |
| --- | --- | --- |
| A Streamelési egységek maximális száma előfizetésenként és régiónként |50 |A kérelem tooincrease adatfolyam-egységek az 50 túl az előfizetés történő kommunikáció tehető [Microsoft Support](https://support.microsoft.com/en-us). |
| A Streamelési egység maximális átviteli sebessége |1 MB/s* |Maximális átviteli sebesség / SU hello forgatókönyv függ. A tényleges átviteli sebesség lehet alacsonyabb is, és függ a lekérdezés összetettségétől és a particionálásától. További részletek találhatók a hello [Scale Azure Stream Analytics-feladatok tooincrease átviteli](../articles/stream-analytics/stream-analytics-scale-jobs.md) cikk. |
| A bemenetek maximális száma feladatonként |60 |A bemenetek rögzített korlátja Stream Analytics-feladatonként 60. |
| A kimenetek maximális száma feladatonként |60 |A kimenetek rögzített korlátja Stream Analytics-feladatonként 60. |
| A függvények maximális száma feladatonként |60 |A függvények rögzített korlátja Stream Analytics-feladatonként 60. |
| Egy feladat adatfolyam-egységek maximális száma |120 |Rögzített legfeljebb 120 adatfolyam-egységek száma Stream Analytics-feladat van. |
| A feladatok maximális száma régiónként |1500 |Előfordulhat, hogy minden egyes előfizetés földrajzi régiónként too1500 feladatot. |
| Referenciaadatok Blob MB-ban megadott mérete | 100 | A referenciaadat Blobok nem lehetnek nagyobbak egyenként 100 MB-nál. |

