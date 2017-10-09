Tárolási korlátozza lemezterület vagy hello rögzített korlátját *maximális* indexek vagy dokumentumok, amelyik előbb következik be.

| Erőforrás | Ingyenes | Alapszintű | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Szolgáltatói szerződés (SLA) |No <sup>1</sup> |Igen |Igen |Igen |Igen |Igen |
| Tárterület partíciónként |50 MB |2 GB |25 GB |100 GB |200 GB |200 GB |
| Partíciók szolgáltatásonként |N/A |1 |12 |12 |12 |3 <sup>2</sup> |
| Partíció mérete |N/A |2 GB |25 GB |100 GB |200 GB |200 GB |
| Replikák |N/A |3 |12 |12 |12 |12 |
| Indexek maximális száma |3 |5 |50 |200 |200 |1000 partíciónként vagy 3000 szolgáltatásonként |
| Indexelők maximális száma |3 |5 |50 |200 |200 |Az indexelők nem támogatottak |
| Adatforrások maximális száma |3 |5 |50 |200 |200 |Az indexelők nem támogatottak |
| Dokumentumok maximális száma |10,000 |1 millió |15 millió partíciónként vagy 180 millió szolgáltatásonként |60 millió partíciónként vagy 720 millió szolgáltatásonként |120 millió partíciónként vagy 1,4 milliárd szolgáltatásonként |1 millió indexenként vagy 200 millió partíciónként |
| Másodpercenkénti lekérdezések (QPS) becsült száma |N/A |~3 replikánként |~15 replikánként |~60 replikánként |~60 replikánként |>60 replikánként |

<sup>1</sup> ingyenes szint és az előzetes funkciók nem tartoznak a szolgáltatásszint-szerződések (SLA). Az összes számlázható rétegek SLA-k érvénybe, ha a szolgáltatás megfelelő redundancia kiépítése. Két vagy több replikák szükségesek (olvasás) lekérdezés SLA-t. Három vagy több replikák lekérdezés és az indexelés (írásra) SLA szükségesek. a partíciók számának hello nincs egy SLA-t szempont. 

<sup>2</sup> S3 HD rendelkezik rögzített legfeljebb 3 partíció, amelyhez verziója alacsonyabb, mint S3 hello partíciós korlát. hello alacsonyabb partíció korlátozva mert hello index száma a S3 HD lényegesen nagyobb. Adott meg, hogy a szolgáltatásra vonatkozó korlátozások mindkét számítási erőforrások (tárolás és feldolgozás) léteznek, és a tartalom (indexekhez és dokumentumokhoz), hello tartalomkorláthoz éri el hamarabb.
