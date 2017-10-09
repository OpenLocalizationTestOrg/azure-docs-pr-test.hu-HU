## <a name="what-is-blob-storage"></a>Mi az a Blob Storage?
Az Azure Blob storage egy olyan szolgáltatás nagy mennyiségű strukturálatlan adatok, például szövegek vagy bináris adatok, hozzáfér a bárhol a HTTP vagy HTTPS PROTOKOLLON keresztül hello world tárolásához. Használhatja a Blob storage tooexpose adatok nyilvánosan toohello globális vagy toostore alkalmazásadatok közvetlenül a Microsoftnak.

A Blob Storage gyakori használati módjai többek között:

* Képek vagy dokumentumok közvetlen tooa böngésző szolgáló
* Fájlok tárolása megosztott hozzáférésre
* Video- és hangtartalom online továbbítása
* Adattárolás biztonsági mentésekhez és helyreállításhoz, vészhelyreállításhoz és archiváláshoz
* Adattárolás egy helyszíni vagy egy Azure által üzemeltetett szolgáltatás elemzéseihez

## <a name="blob-service-concepts"></a>A Blob szolgáltatással kapcsolatos fogalmak
Blob szolgáltatás hello hello a következő összetevőket tartalmazza:

![Blob-architektúra](./media/storage-blob-concepts-include/blob1.png)

* **Tárfiók:** összes keresztül férnek hozzá tooAzure tárolási történik egy tárfiókot. Ez a tárfiók lehet **általános célú tárfiók** vagy **Blob Storage-fiók**, amely kifejezetten objektumok/blobok tárolására szolgál. További információk: [Tudnivalók az Azure Storage-fiókokról](../articles/storage/common/storage-create-storage-account.md).
* **Tároló:** A tárolók blobkészletek csoportosítását biztosítják. Az összes blobnak tárolóban kell lennie. Egy fiók korlátlan számú tárolót tartalmazhat. Egy tároló korlátlan számú blob tárolására használható. Vegye figyelembe, hogy hello a tároló neve kisbetűnek kell lennie.
* **Blob:** Bármilyen típusú és bármekkora méretű fájl. Az Azure Storage háromféle blobot biztosít: blokkblobokat, lapblobokat és hozzáfűző blobokat.
  
    A *blokkblobok* ideálisak szövegek és bináris fájlok, például dokumentumok és médiafájlok tárolására. *Hozzáfűző blobokat* vannak hasonló tooblock blobok abban a épülnek fel blokkok, de vannak optimalizálva műveletek hozzáfűzésére, ezért hasznosak lehetnek a naplózási forgatókönyvekben. Egyetlen blokkblob tartalmazhat be too50, 000 blokkok too100 MB minden, a maximális méretük ezért valamivel több mint 4.75 TB-os (100 MB X 50 000). Egy hozzáfűző blob tartalmazhat be too50, 000 blokkok too4 MB minden, az a maximális méretük ezért valamivel több mint 195 GB (4 MB X 50 000).
  
    *Lapblobok* mentése too1 TB méretű is lehet, és hatékonyabbak a gyakori olvasási/írási műveletek. Az Azure virtuális gépek lapblobokat használnak operációs rendszerként és adatlemezként.
  
    A tárolók és blobok elnevezésével kapcsolatos részletekért lásd: [Naming and Referencing Containers, Blobs, and Metadata](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) (Tárolók, blobok és metaadatok elnevezése és hivatkozása).

