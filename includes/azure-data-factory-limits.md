Adat-előállító egy több-bérlős szolgáltatást a következő alapértelmezett korlátokat érdekében győződjön meg arról, hogy az ügyfél-előfizetések védettek, a másik fél munkaterhelések érvényben. A korlátok számos könnyen emelhető az előfizetéséhez, a maximális határértéket akár lépjen kapcsolatba az ügyfélszolgálattal.

| **Erőforrás** | **Alapértelmezett korlát** | **Felső korlát** |
| --- | --- | --- |
| az Azure-előfizetés adat-előállítók |50 |[Kapcsolatfelvétel az ügyfélszolgálattal](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| belül egy adat-előállító adatcsatornák |2500 |[Kapcsolatfelvétel az ügyfélszolgálattal](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| egy adat-előállító belül adatkészletek |5000 |[Kapcsolatfelvétel az ügyfélszolgálattal](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| adatkészlet száma párhuzamos szeletek |10 |10 |
| adatcsatorna objektumok objektumonként bájt <sup>1</sup> |200 KB |200 KB |
| bájt / adatkészlet-objektumot, és a társított szolgáltatás objektumok <sup>1</sup> |100 KB |2000 KB |
| A HDInsight fürt igény szerinti magok előfizetésen belül <sup>2</sup> |60 |[Kapcsolatfelvétel az ügyfélszolgálattal](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Felhő adatok adatátviteli egység <sup>3</sup> |32 |[Kapcsolatfelvétel az ügyfélszolgálattal](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Újbóli próbálkozások száma az adatcsatorna tevékenység fut |1000 |MaxInt (32 bites) |

<sup>1</sup> folyamat, a DataSet adatkészlet és a társított szolgáltatás objektumok határoz meg a számítási feladatok logikai csoportosítása. Ezek az objektumok korlátairól áthelyezni, és az Azure Data Factory szolgáltatással feldolgozni adatok mennyisége nem kapcsolódnak. Adat-előállító méretezési adatmennyiségig kezelésére szolgál.

<sup>2</sup> igény szerinti HDInsight magok kívül az előfizetést, amely tartalmazza az adat-előállítóban foglal le. Ennek eredményeképpen a fenti határértéke a Data Factory igény szerinti HDInsight magok core korlát érvényes, és eltér a fő korlát az Azure-előfizetéshez társított.

<sup>3</sup> felhő adatok adatátviteli egység (DMU) használja a felhő-felhő másolási műveletek. Egy mérték, amely jelöli az (a Processzor, memória és a hálózatierőforrás-lefoglalás kombinációja) adat-előállítóban egyetlen egységben. Másolás magasabb átviteli, ami bizonyos esetekben több DMUs érhet el. Tekintse meg [adatátviteli adategységek Cloud](../articles/data-factory/data-factory-copy-activity-performance.md#cloud-data-movement-units) részletei szakaszban.

| **Erőforrás** | **Alapértelmezett alacsonyabb korlátot** | **Minimális korlát** |
| --- | --- | --- |
| Ütemezési időköz |15 perc |15 perc |
| Újrapróbálkozási kísérletek közötti időközt |1 másodperc |1 másodperc |
| Ismételje meg az időtúllépés értéke |1 másodperc |1 másodperc |

### <a name="web-service-call-limits"></a>Webes szolgáltatás hívása korlátok
Az Azure Resource Manager API-hívások korlátairól rendelkezik. Az API-hívásokban belül sebességgel végezheti el a [Azure Resource Manager API korlátozza](../articles/azure-subscription-service-limits.md#resource-group-limits).