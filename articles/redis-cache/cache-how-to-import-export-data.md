---
title: "aaaImport és exportálhatja az adatokat az Azure Redis Cache |} Microsoft Docs"
description: "Megtudhatja, hogyan blobtárolóból a premium Azure Redis Cache osztályt adatok tooand tooimport és exportálása"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 4a68ac38-87af-4075-adab-569d37d7cc9e
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: sdanie
ms.openlocfilehash: f17464b207f1c652952f4da63ca147473fee2759
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="import-and-export-data-in-azure-redis-cache"></a>Az Azure Redis Cache adatok importálása és exportálása
Importálási/exportálási egy Azure Redis Cache adatok felügyeleti művelet, amely lehetővé teszi tooimport adatok Azure Redis Cache vagy Azure Redis Cache exportálási adatokat importálni, és a prémium szintű gyorsítótár tooa blobból az Azure Redis Cache-adatbázis (Rekordadatbázis) pillanatkép exportálása Storage-fiók. 

- **Exportálás** -exportálhatja az Azure Redis Cache Rekordadatbázis pillanatképek tooa oldalakra vonatkozó Blob.
- **Importálás** -oldalakra vonatkozó Blob vagy egy Blokkblob importálhatja a Redis gyorsítótár Rekordadatbázis pillanatképeket.

Importálási/exportálási lehetővé teszi a különböző Azure Redis Cache példányai között toomigrate vagy hello gyorsítótár adatokkal használat előtt.

Ez a cikk egy útmutatót biztosít az Azure Redis Cache-adatok importálása és exportálása és hello választ ad toocommonly kérdések.

> [!IMPORTANT]
> Importálási/exportálási jelenleg előzetes verzióban érhető, és csak [prémium csomagban](cache-premium-tier-intro.md) gyorsítótárazza.
>
>

## <a name="import"></a>Importálás
Importálás használt toobring Redis kompatibilis Rekordadatbázis fájlokat minden futtató bármely felhő és a környezet, beleértve a futó Linux, Windows vagy bármely felhőalapú szolgáltató, például az Amazon Web Services, míg mások Redis Redis-kiszolgáló lehet. Adatok importálása egy egyszerűen toocreate egy előre megadott adatokkal gyorsítótár. Hello importálási folyamat során az Azure Redis Cache hello Rekordadatbázis fájlok az Azure storage betölti a memóriába, és, majd a kívánt hello kulcsok hello gyorsítótárába.

> [!NOTE]
> Hello importálási művelet megkezdése előtt győződjön meg arról, hogy a Redis adatbázis (Rekordadatbázis) fájl vagy -fájlok feltöltése oldalhoz vagy blokk-blobok az Azure-tárfiókba, a hello azonos régióban és az előfizetés az Azure Redis Cache példányt. További információkért lásd: [Ismerkedés az Azure Blob storage szolgáltatással](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Ha a Rekordadatbázis fájl hello exportált [Azure Redis Cache exportálása](#export) funkció, a Rekordadatbázis fájl oldalakra vonatkozó blob már tárolja, és készen áll a importálása.
>
>

1. tooimport egy vagy több gyorsítótár blobok exportált [tooyour gyorsítótár Tallózás](cache-configure.md#configure-redis-cache-settings) a hello Azure-portálon, és kattintson a **adatimportálás** a hello **erőforrás menü**.

    ![Adatok importálása][cache-import-data]
2. Kattintson a **válasszon Blob(s)** , és válassza ki, amely tartalmazza a hello adatok tooimport hello tárfiókot.

    ![Válassza ki a tárfiók][cache-import-choose-storage-account]
3. Kattintson a hello adatok tooimport tartalmazó hello tároló.

    ![Válassza ki a tárolót][cache-import-choose-container]
4. Egy vagy több blobot tooimport hello terület toohello hello blob nevétől balra kattintva válassza ki, és kattintson **válasszon**.

    ![Válassza ki a blobok][cache-import-choose-blobs]
5. Kattintson a **importálása** toobegin hello az importálási folyamat.

   > [!IMPORTANT]
   > hello gyorsítótár nem gyorsítótár-ügyfelek által elérhető hello importálási folyamat során, és bármely hello gyorsítótárában adat törlődik.
   >
   >

    ![Importálás][cache-import-blobs]

    Kísérheti hello hello importálási művelet a következő hello értesítést kapnak hello Azure-portálon, vagy hello események megtekintése az hello [napló](../azure-resource-manager/resource-group-audit.md).

    ![Importálás folyamatban][cache-import-data-import-complete]

## <a name="export"></a>Exportálás
Exportálás lehetővé teszi tooexport hello adataihoz az Azure Redis Cache tooRedis kompatibilis Rekordadatbázis (oka) t. Ez a szolgáltatás toomove adatokat egy Azure Redis Cache példány tooanother vagy tooanother Redis-kiszolgáló is használhatja. VM állomások hello Azure Redis Cache server-példány, és hello fájl kijelölve tárfiók feltöltött toohello hello hello exportálás során ideiglenes fájl jön létre. Hello az exportálási művelet befejezése után a vagy állapota sikeres végrehajtásával vagy hibajelzéssel hello ideiglenes fájl törlődik.

1. tooexport hello tartalmát hello gyorsítótár toostorage, [tooyour gyorsítótár Tallózás](cache-configure.md#configure-redis-cache-settings) a hello Azure-portálon, és kattintson a **adatok exportálása** a hello **erőforrás menü**.

    ![A tároló kiválasztása][cache-export-data-choose-storage-container]
2. Kattintson a **válassza ki a tároló** válassza ki a kívánt hello tárfiók. hello tárfiók hello kell lennie ugyanazon előfizetésével és régiójával, a gyorsítótár.

   > [!IMPORTANT]
   > Exportálja a lapblobokat, amelyek klasszikus és Resource Manager tárfiókok által támogatott, de nem támogatja a együttműködik [Blob storage-fiókok](../storage/blobs/storage-blob-storage-tiers.md#blob-storage-accounts) most.
   >
   >

    ![Tárfiók][cache-export-data-choose-account]
3. Válassza ki a hello szükséges blobtároló és kattintson **válasszon**. toouse új tárolót, kattintson a **tároló hozzáadása** tooadd az első, és jelölje ki azt hello listából.

    ![A tároló kiválasztása][cache-export-data-container]
4. Adjon meg egy **Blob előtagja** kattintson **exportálása** toostart hello exportálás során. hello blob előtagja az exportálási művelet által létrehozott fájlok használt tooprefix hello nevét.

    ![Exportálás][cache-export-data]

    Kísérheti hello hello az exportálási művelet a következő hello értesítést kapnak hello Azure-portálon, vagy hello események megtekintése az hello [napló](../azure-resource-manager/resource-group-audit.md).

    ![Az adatok exportálása befejeződött][cache-export-data-export-complete]

    Gyorsítótárak hello exportálási folyamat során elérhetők maradnak.

## <a name="importexport-faq"></a>Importálási/exportálási – gyakori kérdések
Ez a szakasz hello Import/Export szolgáltatás kapcsolatos gyakori kérdésekre.

* [Milyen árképzési tiers használhatja az Import/Export?](#what-pricing-tiers-can-use-importexport)
* [Szeretnék adatokat importálhat a Redis-kiszolgáló?](#can-i-import-data-from-any-redis-server)
* [Milyen Rekordadatbázis verziók lehet importálni?](#what-rdb-versions-can-i-import)
* [A gyorsítótár érhető el az importálási/exportálási művelet közben?](#is-my-cache-available-during-an-importexport-operation)
* [Használhatok Import/Export Redis-fürt?](#can-i-use-importexport-with-redis-cluster)
* [Hogyan működik a Import/Export beállítása egy egyéni adatbázisok?](#how-does-importexport-work-with-a-custom-databases-setting)
* [Miben különbözik Import/Export Redis-adatmegőrzés?](#how-is-importexport-different-from-redis-persistence)
* [Automatizálható a PowerShell, a parancssori felületen vagy a más felügyeleti ügyfelek Import/Export?](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
* [Időtúllépési hiba érkezett a importálási/exportálási művelet közben. Mit jelent?](#i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean)
* [Hiba történt az adatok tooAzure Blob Storage exportálásakor jelent. mi történt?](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened)

### <a name="what-pricing-tiers-can-use-importexport"></a>Milyen árképzési tiers használhatja az Import/Export?
Importálási/exportálási csak hello prémium tarifacsomag érhető el.

### <a name="can-i-import-data-from-any-redis-server"></a>Szeretnék adatokat importálhat a Redis-kiszolgáló?
Igen, továbbá tooimporting Azure Redis Cache példány exportált adatok, importálhat Rekordadatbázis fájlok bármely Redis-kiszolgáló futtatja a felhő vagy a környezetben, például a Linux, Windows vagy például az Amazon Web Services szolgáltatók. toodo, ez a feltöltési hello Rekordadatbázis fájlként szükséges hello Redis-kiszolgáló be egy Azure Storage-fiókot, majd importálja a lap vagy blokk blob azt be a prémium szintű Azure Redis Cache példány. Például előfordulhat, hogy szeretne tooexport hello adatokat a termelési gyorsítótárból, és importálja a fájlt a gyorsítótár tesztelési, illetve áttelepítési egy átmeneti környezet részeként használatos.

> [!IMPORTANT]
> toosuccessfully exportált eltérő Azure Redis Cache Redis-kiszolgálókról, amikor egy oldalakra vonatkozó blob, és a méretet a hello blob egyeztetni kell olyan 512 bájtos határon belül található adatok importálása. A minta kód tooperform el a szükséges bájt kitöltési című [minta lap blog feltöltéséhez](https://github.com/JimRoberts-MS/SamplePageBlobUpload).
> 
> 

### <a name="what-rdb-versions-can-i-import"></a>Milyen Rekordadatbázis verziók lehet importálni?

Azure Redis Cache Rekordadatbázis importálási legfeljebb 7-es verzió Rekordadatbázis keresztül támogat.

### <a name="is-my-cache-available-during-an-importexport-operation"></a>A gyorsítótár érhető el az importálási/exportálási művelet közben?
* **Exportálás** - gyorsítótárak elérhetők maradnak, és folytathatja a toouse a gyorsítótár az exportálási művelet során.
* **Importálás** - gyorsítótárak elérhetetlenné válik, az importálási művelet indításakor, és hello importálási művelet befejeződése után lesz használható.

### <a name="can-i-use-importexport-with-redis-cluster"></a>Használhatok Import/Export Redis-fürt?
Igen, és akkor is importálási/exportálási egy fürtözött gyorsítótár és egy nem fürtözött gyorsítótár között. A Redis-fürt óta [csak által támogatott adatbázis-0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), egyetlen megadott adattal sem adatbázisok 0 kivételével nincs importálva. Fürtözött gyorsítótár-adatok importálása során a rendszer újra hello kulcsok hello szilánkok hello fürt között.

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a>Hogyan működik a Import/Export beállítása egy egyéni adatbázisok?
Különböző tartalmaznak az egyes tarifacsomagok [korlátok adatbázisok](cache-configure.md#databases), úgy, hogy nincs szempontokat importálásakor, ha egy egyéni értéket hello konfigurált `databases` beállítása a gyorsítótár létrehozása közben.

* Az alsó tarifacsomag tooa importálásakor `databases` , amelyből exportált hello réteg határa:
  * Hello alapértelmezett számának használata `databases`, amely az összes tarifacsomagok 16, adatok nem vesztek el.
  * Ha egyéni számos használ `databases` , hogy a hello hello határokon belül esik réteget toowhich importált adatok nem vesztek el.
  * Az exportált adatok egy adatbázisban, amelynek hossza meghaladja a hello korlátok hello új réteg adatokat tartalmazott, ha a rendszer nem importálja ezeket magasabb adatbázisok hello adatait.

### <a name="how-is-importexport-different-from-redis-persistence"></a>Miben különbözik Import/Export Redis-adatmegőrzés?
Azure Redis Cache adatmegőrzési lehetővé teszi a Redis tooAzure tárolási toopersist adataihoz. Ha adatmegőrzési van konfigurálva, az Azure Redis Cache hello Redis gyorsítótár egy Redis bináris formátum toodisk egy konfigurálható biztonsági mentési gyakoriság alapján a pillanatkép továbbra is fennáll. Ha egy katasztrofális esemény történik, amely letiltja a hello elsődleges és replika gyorsítótár, hello gyorsítótárazott adatokat állítja automatikusan hello legutóbbi pillanatkép. További információkért lásd: [hogyan tooconfigure adatok megőrzését egy prémium szintű Azure Redis Cache](cache-how-to-premium-persistence.md).

Importálás / exportálás lehetővé teszi a toobring adatok vagy az Azure Redis Cache exportálása. Biztonsági mentés konfigurálása, és nem visszaállítása a Redis-adatmegőrzés használatával.

### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a>Automatizálható a PowerShell, a parancssori felületen vagy a más felügyeleti ügyfelek Import/Export?
Igen, a PowerShell útmutatásért lásd: [tooimport a Redis gyorsítótár](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) és [tooexport a Redis gyorsítótár](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).

### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a>Időtúllépési hiba érkezett a importálási/exportálási művelet közben. Mit jelent?
Ha a számítógép a hello **adatimportálás** vagy **adatok exportálása** panel több mint 15 percig hello művelet kezdeményezése előtt, hibaüzenetet kap egy hiba üzenet hasonló toohello a következő példa a:

    hello request tooimport data into cache 'contoso55' failed with status 'error' and error 'One of hello SAS URIs provided could not be used for hello following reason: hello SAS token end time (se) must be at least 1 hour from now and hello start time (st), if given, must be at least 15 minutes in hello past.

tooresolve, kezdeményezése hello importálási vagy exportálási művelet előtt 15 perc telt el.

### <a name="i-got-an-error-when-exporting-my-data-tooazure-blob-storage-what-happened"></a>Hiba történt az adatok tooAzure Blob Storage exportálásakor jelent. mi történt?
Exportálás csak Rekordadatbázis fájlokat tárolja a lapblobokat működik. Egyéb blob típusú jelenleg nem támogatottak, beleértve a blob storage-fiókok gyakran és ritkán használt rétegekkel. További információkért lásd: [Blob storage-fiókok](../storage/blobs/storage-blob-storage-tiers.md#blob-storage-accounts).

## <a name="next-steps"></a>Következő lépések
Ismerje meg, hogyan több premium toouse gyorsítótár szolgáltatásokat.

* [Bevezetés toohello Azure Redis Cache prémium szintjének](cache-premium-tier-intro.md)    

<!-- IMAGES -->
[cache-settings-import-export-menu]: ./media/cache-how-to-import-export-data/cache-settings-import-export-menu.png
[cache-export-data-choose-account]: ./media/cache-how-to-import-export-data/cache-export-data-choose-account.png
[cache-export-data-choose-storage-container]: ./media/cache-how-to-import-export-data/cache-export-data-choose-storage-container.png
[cache-export-data-container]: ./media/cache-how-to-import-export-data/cache-export-data-container.png
[cache-export-data-export-complete]: ./media/cache-how-to-import-export-data/cache-export-data-export-complete.png
[cache-export-data]: ./media/cache-how-to-import-export-data/cache-export-data.png
[cache-import-data]: ./media/cache-how-to-import-export-data/cache-import-data.png
[cache-import-choose-storage-account]: ./media/cache-how-to-import-export-data/cache-import-choose-storage-account.png
[cache-import-choose-container]: ./media/cache-how-to-import-export-data/cache-import-choose-container.png
[cache-import-choose-blobs]: ./media/cache-how-to-import-export-data/cache-import-choose-blobs.png
[cache-import-blobs]: ./media/cache-how-to-import-export-data/cache-import-blobs.png
[cache-import-data-import-complete]: ./media/cache-how-to-import-export-data/cache-import-data-import-complete.png
