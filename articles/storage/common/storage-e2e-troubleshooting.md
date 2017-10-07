---
title: a diagnosztika & Message Analyzer Azure Storage aaaTroubleshooting |} Microsoft Docs
description: "A végpont hibaelhárítás az Azure Storage Analytics, az AzCopy és a Microsoft Message Analyzert, amely tartalmazza az oktatóanyag"
services: storage
documentationcenter: dotnet
author: robinsh
manager: timlt
ms.assetid: 643372a3-1c07-4e88-b4ef-042512a43086
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/15/2017
ms.author: robinsh
ms.openlocfilehash: 2d7a2a14b2e8da01b566ac3dec19996f0ef88cc2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="end-to-end-troubleshooting-using-azure-storage-metrics-and-logging-azcopy-and-message-analyzer"></a>Azure Storage mérőszámainak és a naplózást, a AzCopy és a Message Analyzer végpontok – hibaelhárítás
[!INCLUDE [storage-selector-portal-e2e-troubleshooting](../../../includes/storage-selector-portal-e2e-troubleshooting.md)]

Diagnosztizálás és hibaelhárítási a kulcs szakértelem kialakításához, és a Microsoft Azure Storage ügyfél alkalmazásokat támogató. Az Azure-alkalmazások toohello elosztott jellege, miatt Diagnosztizálás és a hibák és a teljesítménnyel kapcsolatos problémák elhárítása lehet bonyolultabb, mint a hagyományos környezetekben.

Ebben az oktatóanyagban bemutatjuk, hogyan tooidentify bizonyos hibák hatása a teljesítményre, és a végpont e hibák elhárítása rendelés toooptimize hello ügyfélalkalmazás a Microsoft és az Azure Storage által biztosított eszközök segítségével.

Ez az oktatóanyag egy végpontok közötti hibaelhárítási forgatókönyv gyakorlati feltárása biztosít. Egy részletes fogalmi útmutató tootroubleshooting az Azure storage alkalmazások, lásd: [figyelése, diagnosztizálása és elhárítása a Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="tools-for-troubleshooting-azure-storage-applications"></a>Azure Storage alkalmazások hibaelhárítási eszközök
tootroubleshoot ügyfélalkalmazások használatával a Microsoft Azure tárolás, az eszközök toodetermine, ha probléma történt, és lehet, hogy milyen hello hello probléma okát is használhatja. Ezek az eszközök a következőket foglalják magukban:

* **Az Azure Storage Analytics**. [Az Azure Storage Analytics](/rest/api/storageservices/Storage-Analytics) metrikákat és naplózási az Azure Storage biztosít.
  
  * **Storage mérőszámainak** nyomon követi a tranzakció metrikák és a teljesítmény-mérőszámait a tárfiók. Metrikák használ, meghatározhatja, hogyan működik az alkalmazás-tooa függően számos különböző intézkedéseket. Lásd: [Storage Analytics metrikák táblaséma](/rest/api/storageservices/Storage-Analytics-Metrics-Table-Schema) követik nyomon tárolási analitika metrikák hello típusú további információt.
  * **Tárolási naplózási** naplózza az egyes kérelmek toohello Azure Storage szolgáltatások tooa kiszolgálóoldali naplóját. hello számok részletes adatainak naplózása az egyes kérelmek, beleértve a hello műveletet hajtja végre, hello műveletet, és a késési adatok hello állapotát. Lásd: [Storage Analytics naplóformátumban](/rest/api/storageservices/Storage-Analytics-Log-Format) hello kérés- és írt adatok toohello naplók Storage Analytics további információt.

> [!NOTE]
> A Zónaredundáns tárolás (ZRS) replikációs típusú tárfiókok nincs hello metrikák vagy jelenleg engedélyezve van a naplózási szolgáltatás. 
> 
> 

* **Azure-portálon**. A tárfiók a hello konfigurálhatja metrikákat és naplózási [Azure-portálon](https://portal.azure.com). Is diagramok és hogyan működik az alkalmazás időbeli megjelenítő diagramok megtekintése, és ha az alkalmazás másképp hajt végre a mint várt az adott mérőszám riasztások toonotify konfigurálhatja.
  
    Lásd: [hello Azure-portálon a tárfiók figyelése](storage-monitor-storage-account.md) hello Azure-portálon a megfigyelést konfigurálásával kapcsolatos információkat.
* **AzCopy**. Az Azure Storage Server-naplók blobként, tárolják, hogy használhassa a Microsoft Message Analyzert használatával történt elemzésének AzCopy toocopy hello blobok tooa helyi naplókönyvtár. Lásd: [adatátvitel az AzCopy parancssori segédprogram hello](storage-use-azcopy.md) AzCopy további információt.
* **Microsoft Message Analyzert**. Az Üzenetelemző olyan eszköz, amely használ fel a naplófájlok és a naplózási adatokat, így könnyen toofilter, a Keresés és a csoport adatok jelentkezzen be, hogy használhatja-e tooanalyze hibák és a teljesítménnyel kapcsolatos problémák hasznos beállítása visual formátumban jeleníti meg. Lásd: [Microsoft Message Analyzer működő útmutató](http://technet.microsoft.com/library/jj649776.aspx) Message Analyzer további információt.

## <a name="about-hello-sample-scenario"></a>Hello mintaforgatókönyv kapcsolatban
Ebben az oktatóanyagban egy olyan forgatókönyvet, ahol az Azure Storage mérőszámainak, egy alacsony százalékos sikerességi arányát az alkalmazás, amely behívja az Azure storage foglalkozunk. alacsony százalékos sikerességi aránya metrika hello (megjelennek az helyeként **PercentSuccess** a hello [Azure-portálon](https://portal.azure.com) és hello metrikák táblák) követi nyomon, hogy sikeres legyen, de nagyobb HTTP-állapotkódot vissza, amely operations mint 299. Hello kiszolgálóoldali tárolási naplófájlokban, ezek a műveletek vannak rögzített, a tranzakció állapota **ClientOtherErrors**. Hello alacsony százalékos sikerességi metrika kapcsolatos további tudnivalókért lásd: [metrika alacsony PercentSuccess vagy analytics naplóbejegyzések rendelkezik ClientOtherErrors működésére állapotú tranzakció](storage-monitoring-diagnosing-troubleshooting.md#metrics-show-low-percent-success).

Azure Storage-műveleteket adhatnak vissza HTTP-állapotkódok 299 nagyobb normál működés részeként. Ezeket a hibákat, egyes esetekben jelzi, hogy esetleg képes toooptimize az ügyfélalkalmazás számára, de növeli a teljesítményt.

Ebben a forgatókönyvben azt fogja fontolja meg egy alacsony százalékos sikerességi aránya toobe minden, 100 %. Választhat egy másik metrika szintet, azonban tooyour igényeinek megfelelően. Azt javasoljuk, hogy az alkalmazás tesztelése során kapcsolatot hoz létre egy alapkonfiguráció tolerancia az a fontos teljesítménymutatókat. Például dönthet úgy, tesztekre alapozva, az alkalmazás rendelkezik-e a 90 %, vagy a 85 % konzisztens százalékos sikerességi aránya. Ha a metrikák adatait jeleníti meg, hogy hello alkalmazás el eltérő-e, hogy több, majd használatával megvizsgálhatja a Mi okozza hello növelését.

A minta esetünkben után azt mutatja meg, hogy hello százalékos sikerességi aránya metrika 100 % alatt van, a Microsoft hello naplók toofind hello hibák toohello metrikák gyűjtött megvizsgálja, és használhatja őket toofigure, hogy milyen okozza hello alacsonyabb százalékos sikerességi arányát. Megnézzük, pontosabban, a hibák hello 400 közé. Ezután lesz jobban felülvizsgáljuk 404-es (nem található) hibák.

### <a name="some-causes-of-400-range-errors"></a>Tartományon kívüli 400 hibák oka
hello példák az alábbi példák láthatók az egyes 400-tartomány hibaüzeneteket Azure Blob Storage és a lehetséges okok. A hibákat, valamint a hello 300 tartomány- és hello 500 tartomány, a hibák bármelyikét járulhat tooa alacsony százalékos sikerességi arányát.

Vegye figyelembe, hogy az alábbi listák hello messze befejeződött. Lásd: [állapotát és hibakódok](http://msdn.microsoft.com/library/azure/dd179382.aspx) az MSDN-en, és adott tooeach hibák hello tárolási szolgáltatások általános Azure Storage-hibák kapcsolatos részleteket.

**Állapot 404-es (nem található) kódpéldák**

Akkor következik be, amikor egy tárolót vagy blobot egy olvasási művelet sikertelen lesz, mivel hello blob vagy a tároló nem található.

* Akkor következik be, ha egy tárolót vagy blobot törölte egy másik ügyfélszámítógépen, mielőtt ezt a kérelmet.
* Akkor következik be, az API-hívás által létrehozott hello tárolót vagy blobot ellenőrzése, hogy létezik-e után használata. hello CreateIfNotExists API-k ellenőrizze a HEAD hello tárolót vagy blobot; hello megléte első toocheck meghívása Ha még nem létezik, a 404-es hibát ad vissza, és ezután egy második PUT kezdeményezték toowrite hello tárolót vagy blobot.

**Állapot-kódpéldák 409 (Ütközés)**

* Akkor következik be, ha egy API létrehozása toocreate egy új tárolót vagy blobot, először az meglétének ellenőrzése nélkül használja, és egy tárolót vagy blobot ezzel a névvel már létezik.
* Akkor következik be, ha a tároló törlése folyamatban van, és egy új tároló az azonos név hello törlési művelet befejezése előtt hello toocreate kísérli meg.
* Akkor következik be, ha egy tárolót vagy blobot meg kell adnia egy bérletet, és már létezik a jelenlegi címbérlet.

**412 (előfeltétel) állapotkód példák**

* Akkor következik be, ha feltételes fejlécet által megadott hello feltétele nem teljesült.
* A megadott hello bérleti azonosítója nem egyezik meg a címbérlet-Azonosítójú hello hello tárolót vagy blobot következik be.

## <a name="generate-log-files-for-analysis"></a>Naplófájlokat az elemzéshez készítése
Ebben az oktatóanyagban használjuk Message Analyzer toowork három különböző típusú, a naplófájlok, bár ezek egyike toowork jelenítette meg:

* Hello **kiszolgálónapló**, ami akkor jön létre, amikor az Azure Storage naplózását. hello kiszolgálónapló egyes hello Azure Storage szolgáltatás - blob, a várólista, a tábla és a fájl egyik hívást műveletekre vonatkozó adatokat tartalmaz. hello kiszolgálónapló azt jelzi, mely művelet lett meghívva, és milyen állapotkódja: hello kérelem-válasz visszaadott, valamint egyéb adatait.
* Hello **.NET ügyfélnapló**, ami akkor jön létre, amikor a .NET-alkalmazásból engedélyezi az ügyféloldali naplózás. hello ügyfélnapló hogyan hello ügyfél előkészíti hello kérelem és a fogadó és feldolgozó hello válasz részletes információkat tartalmaz.
* Hello **HTTP hálózati nyomkövetési napló**, amely adatokat gyűjt a HTTP/HTTPS-kérelem-válasz adatokat, beleértve az Azure Storage műveleteket. Ebben az oktatóanyagban azt fogja létrehozni hello hálózati nyomkövetés Message Analyzer keresztül.

### <a name="configure-server-side-logging-and-metrics"></a>Metrikák és kiszolgálóoldali naplózásának beállítása
Először azt kell tooconfigure Azure Storage naplózása és metrikákat, hogy a Microsoft hello ügyfél alkalmazás tooanalyze adatait. Naplózás és a metrikák konfigurálhatja a különböző módokon - keresztül hello [Azure-portálon](https://portal.azure.com), powershellel, vagy programon keresztül. Lásd: [Storage mérőszámainak engedélyezése és megtekintése metrikai adatok](http://msdn.microsoft.com/library/azure/dn782843.aspx) és [tárolási naplózás engedélyezése és használata naplóadatok](http://msdn.microsoft.com/library/azure/dn782840.aspx) MSDN naplózás és a metrikák konfigurálásával kapcsolatos részletekért.

**Hello Azure-portálon keresztül**

tooconfigure naplózás és a tárolási fiók a metrikák hello [Azure-portálon](https://portal.azure.com), hajtsa végre a hello található utasítások segítségével: [hello Azure-portálon a tárfiók figyelése](storage-monitor-storage-account.md).

> [!NOTE]
> Már nem lehetséges tooset percenkénti metrikákat hello Azure-portál használatával. Azt javasoljuk azonban, hogy meg őket az oktatóanyag hello alkalmazásában, és az alkalmazással teljesítményproblémák vizsgálata. Percenkénti metrikákat alább látható módon PowerShell használatával, vagy programozott módon hello a storage ügyféloldali kódtára állíthatja be.
> 
> Vegye figyelembe, hogy hello Azure-portál nem jeleníthető meg percenkénti metrikákat, csak óránkénti metrikákat.
> 
> 

**PowerShell használatával**

az Azure PowerShell lépései tooget lásd [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

1. Használjon hello [Add-AzureAccount](/powershell/module/azure/add-azureaccount?view=azuresmps-3.7.0) parancsmag tooadd Azure felhasználói fiókjának toohello PowerShell-ablakban:
   
    ```powershell
    Add-AzureAccount
    ```

2. A hello **jelentkezzen be tooMicrosoft Azure** ablak, típusú hello e-mail cím és a fiókhoz társított jelszót. Azure hitelesíti és menti a hello hitelesítő adatokat, és majd a hello ablak bezárása.
3. Állítsa be a hello alapértelmezett tárolási fiók toohello tárfiók hello oktatóanyag használja a következő parancsok futtatásával hello PowerShell-ablakban:
   
    ```powershell
    $SubscriptionName = 'Your subscription name'
    $StorageAccountName = 'yourstorageaccount'
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
    ```

4. Tárolási Blob szolgáltatás hello naplózásának engedélyezése:
   
    ```powershell
    Set-AzureStorageServiceLoggingProperty -ServiceType Blob -LoggingOperations Read,Write,Delete -PassThru -RetentionDays 7 -Version 1.0
    ```

5. A Blob szolgáltatás, így meg arról, hogy tooset hello storage mérőszámainak engedélyezése **- MetricsType** túl`Minute`:
   
    ```powershell
    Set-AzureStorageServiceMetricsProperty -ServiceType Blob -MetricsType Minute -MetricsLevel ServiceAndApi -PassThru -RetentionDays 7 -Version 1.0
    ```

### <a name="configure-net-client-side-logging"></a>.NET ügyféloldali naplózás konfigurálása
tooconfigure ügyféloldali naplózás a .NET-alkalmazásokat, engedélyezze a .NET diagnosztika hello alkalmazás konfigurációs fájljában (a web.config vagy az App.config fájlt). Lásd: [ügyféloldali naplózás hello .NET a Storage ügyféloldali kódtára](http://msdn.microsoft.com/library/azure/dn782839.aspx) és [ügyféloldali naplózás hello Microsoft Azure Storage SDK for Java](http://msdn.microsoft.com/library/azure/dn782844.aspx) az MSDN Webhelyén, a részletekért.

hello ügyféloldali napló hogyan hello ügyfél előkészíti hello kérelem és a fogadó és feldolgozó hello válasz részletes információkat tartalmaz.

a Storage ügyféloldali kódtára hello ügyféloldali naplóadatokat hello alkalmazás konfigurációs fájljában (a web.config vagy az App.config fájlt) a megadott hello helyen tárolja.

### <a name="collect-a-network-trace"></a>A hálózati nyomkövetés gyűjtése
Az Üzenetelemző toocollect egy HTTP/HTTPS hálózati nyomkövetés is használhatja, az ügyfélalkalmazás futtatása közben. Üzenet-elemző eszköz által használt [Fiddler](http://www.telerik.com/fiddler) hello háttér. Hello hálózati nyomkövetés gyűjt, mielőtt azt javasoljuk, hogy konfigurálja a Fiddler titkosítatlanul toorecord HTTPS-forgalmat:

1. Telepítés [Fiddler](http://www.telerik.com/download/fiddler).
2. Indítsa el a Fiddler.
3. Válassza ki **eszközök |} Fiddler beállítások**.
4. Hello-beállítások párbeszédpanelen győződjön meg arról, hogy **rögzítése HTTPS kapcsolódik** és **a HTTPS-forgalmon visszafejtés** mind van jelölve, alább látható módon.

![Fiddler beállítások konfigurálása](./media/storage-e2e-troubleshooting/fiddler-options-1.png)

Hello az oktatóanyagban gyűjtése és egy hálózati nyomkövetést először mentse a Message Analyzer, majd egy elemző munkamenet tooanalyze hello nyomkövetés létrehozása, és naplók hello. a Message Analyzer egy hálózati nyomkövetést. toocollect:

1. A Message Analyzer, válassza ki **fájl |} Gyors nyomkövetési |} Titkosítás nélkül HTTPS**.
2. hello nyomkövetés, ezért azonnal megkezdődik. Válassza ki **leállítása** toostop hello nyomkövetési így konfigurálhatjuk azt tootrace tárolási forgalmat.
3. Válassza ki **szerkesztése** tooedit hello nyomkövetési munkamenet.
4. Jelölje be hello **konfigurálása** toohello sarkában hello hivatkozás **Microsoft-Pef-WebProxy** ETW-szolgáltató.
5. A hello **speciális beállítások** párbeszédpanel, kattintson a hello **szolgáltató** lapon.
6. A hello **állomásnév szűrő** mezőben adja meg a tároló-végpontokat, választják el egymástól. Például megadhatja a végpontokat az alábbiak szerint; Módosítsa `storagesample` toohello a tárfiók nevét:

    ```   
    storagesample.blob.core.windows.net storagesample.queue.core.windows.net storagesample.table.core.windows.net
    ```

7. Hello párbeszédpanel bezárásához, majd kattintson az **indítsa újra a** toobegin gyűjtése hello nyomkövetési hello állomásnév szűrővel ellenőrzéséhez, hogy csak az Azure tárolási hálózati forgalom hello nyomkövetési szerepel.

> [!NOTE]
> Miután végzett a hálózati nyomkövetés gyűjtése, javasoljuk, hogy visszaállítja hello beállításokat, előfordulhat, hogy módosította a Fiddler toodecrypt HTTPS-forgalmat. Hello Fiddler beállítások párbeszédpanelen törölje a hello **rögzítése HTTPS kapcsolódik** és **a HTTPS-forgalmon visszafejtés** jelölőnégyzeteket.
> 
> 

Lásd: [hello hálózati nyomkövetés funkciókat használ,](http://technet.microsoft.com/library/jj674819.aspx) a TechNet webhelyen olvashat.

## <a name="review-metrics-data-in-hello-azure-portal"></a>Tekintse át a metrikákat adatok hello Azure-portálon
Miután az alkalmazás egy ideig futott, hello metrikák diagramok hello megjelenő tekintse át [Azure-portálon](https://portal.azure.com) tooobserve hogyan rendelkezik lett végrehajtása a szolgáltatás.

Először keresse meg a storage-fiókot tooyour hello Azure-portálon. Alapértelmezés szerint a figyelő a diagramon az hello **sikeres arány** metrika hello fiók panelén jelenik meg. Ha korábban módosította hello diagram toodisplay különböző metrikákat, vegye fel a hello **sikeres arány** metrikát.

Most láthatja **sikeres arány** hello diagram figyelés, a bármely más metrikákkal együtt esetlegesen hozzáadott. Hello százalékos sikerességi arányát lesz felülvizsgáljuk mellett Message Analyzer hello-naplók elemzésével hello esetben némileg alábbi 100 %.

Hozzáadása és metrikákat diagramok testreszabása a további részletekért lásd: [metrikák diagramok testreszabása](storage-monitor-storage-account.md#customize-metrics-charts).

> [!NOTE]
> Ez eltarthat egy ideig a metrikák adatforgalom tooappear a hello Azure-portálon a storage mérőszámainak engedélyezése után. Ennek az az oka az előző órán hello óránkénti metrikák nem jelennek meg hello Azure-portálon elteltéig hello aktuális óra. Emellett percenkénti metrikákat jelenleg nem láthatók a hello Azure-portálon. Így a attól függően, hogy a metrikák engedélyezésekor tootwo óra toosee mérőszámok-adatokat.
> 
> 

## <a name="use-azcopy-toocopy-server-logs-tooa-local-directory"></a>AzCopy toocopy server naplók tooa helyi könyvtár használata
Az Azure Storage server naplózási adatok tooblobs ír, amíg a metrikák írt tootables. Napló blobok érhetők el a jól ismert hello `$logs` tároló a tárfiók. Napló blobok neve hierarchikusan év, hónap, nap és óra, így könnyen elérhetők hello tartomány tooinvestigate időtartamát. Például a hello `storagesample` fiók, 01/02/2015, a 8 – 9 óra, hello napló blobokat hello tárolója `https://storagesample.blob.core.windows.net/$logs/blob/2015/01/08/0800`. hello egyes BLOB a tárolóban található megnevezett egymás után, kezdve `000000.log`.

Használható hello AzCopy parancssori eszköz toodownload a kiszolgálóoldali napló fájlok tooa helyét a helyi számítógépen. Például használhatja a blob-műveletek tartott helyezze a következő parancs toodownload hello naplófájlok hello 2015. január 2 toohello mappa `C:\Temp\Logs\Server`; cserélje le `<storageaccountname>` hello nevet a tárfiók, és `<storageaccountkey>` rendelkező a hívóbetűre:

```azcopy
AzCopy.exe /Source:http://<storageaccountname>.blob.core.windows.net/$logs /Dest:C:\Temp\Logs\Server /Pattern:"blob/2015/01/02" /SourceKey:<storageaccountkey> /S /V
```
AzCopy érhető el a letöltést a hello [Azure letölti](https://azure.microsoft.com/downloads/) lap. AzCopy használatával kapcsolatos részletekért lásd: [adatátvitel az AzCopy parancssori segédprogram hello](storage-use-azcopy.md).

További információ a kiszolgálóoldali naplók letöltése: [naplózási adatokat tároló naplózás letöltése](http://msdn.microsoft.com/library/azure/dn782840.aspx#DownloadingStorageLogginglogdata).

## <a name="use-microsoft-message-analyzer-tooanalyze-log-data"></a>Használja a Microsoft Message Analyzert tooanalyze naplóadatok
Microsoft Message Analyzert egy olyan eszköz, a rögzítés, megjelenítését és elemzése a forgalmat, eseményeket és hibaelhárítási és diagnosztikai forgatókönyvek egyéb rendszer vagy alkalmazás üzeneteket üzenetküldési protokoll. Message Analyzer is lehetővé teszi a tooload, összesített, és naplózási adatok elemzését és nyomkövetési fájlok. Az Üzenetelemző kapcsolatos további információkért lásd: [Microsoft Message Analyzer működő útmutató](http://technet.microsoft.com/library/jj649776.aspx).

Az Üzenetelemző eszközök, amelyek segítenek tooanalyze kiszolgáló, az ügyfél és a hálózati naplók Azure Storage magában foglalja. Ebben a szakaszban mutatjuk hogyan toouse ezen eszközök tooaddress hello alacsony százalékos sikeres kiállításának hello tárolási naplófájljai.

### <a name="download-and-install-message-analyzer-and-hello-azure-storage-assets"></a>Töltse le és telepítse a Message Analyzer és hello Azure Storage-eszközök
1. Töltse le [Üzenetelemző](http://www.microsoft.com/download/details.aspx?id=44226) a hello Microsoft Download Center, és futtassa a telepítőt hello.
2. Indítsa el a Message Analyzer.
3. A hello **eszközök** menü **Eszközkezelő**. A hello **Eszközkezelő** párbeszédablakban válassza **letölti**, majd szűrést végezni **Azure Storage**. Hello Azure Storage eszközök, ahogy az alábbi képen hello jelenik meg.
4. Kattintson a **szinkronizálási összes megjelenített elemek** tooinstall hello Azure Storage-eszközök. hello elérhető eszközök a következők:
   * **Az Azure Storage szín szabályok:** az Azure Storage szín szabályok lehetővé teszik a speciális szűrők toodefine használt szín, a szöveg, és a betűtípus stílusait nyomkövetés tudnivalókat tartalmazó toohighlight üzeneteket.
   * **Az Azure Storage diagramok:** az Azure Storage diagram olyan előre definiált diagramok, amelyek a kiszolgáló naplóadatokat diagramot. Vegye figyelembe, hogy toouse Azure Storage jelenleg diagramokat, akkor előfordulhat, hogy csak terhelés hello kiszolgálónapló hello elemzés rács be.
   * **Az Azure Storage elemzők:** hello Azure Storage elemzők elemzési hello Azure Storage ügyfél, a kiszolgáló és a HTTP bejelentkezik rendelés toodisplay őket az elemzési rács hello.
   * **Az Azure Storage-szűrők:** az Azure Storage szűrők használható tooquery az adatok a hello elemzés rács előre meghatározott feltételek érvényesek.
   * **Az Azure Storage nézet elrendezések:** az Azure Storage nézet elrendezések előre definiált oszlop elrendezések és elemzési rács hello lévő.
5. Indítsa újra a Message Analyzer hello eszközök telepítése után.

![Message Analyzer Eszközkezelő](./media/storage-e2e-troubleshooting/mma-start-page-1.png)

> [!NOTE]
> Ez az oktatóanyag céljából hello hello Azure Storage eszközök teljes telepítése.
> 
> 

### <a name="import-your-log-files-into-message-analyzer"></a>A naplófájlok importálása Message Analyzer
Importálhatja a mentett naplófájlok (kiszolgálóoldali ügyféloldali és hálózati), az összes Microsoft Message Analyzert egyetlen munkamenetében elemzés céljából.

1. A hello **fájl** Microsoft Message Analyzert, menüjében kattintson **új munkamenet**, és kattintson a **üres munkamenet**. A hello **új munkamenet** párbeszédpanelen adja meg az elemzési munkamenet nevét. A hello **munkamenet részletei** panelen, kattintson a hello **fájlok** gombra.
2. tooload hello hálózati nyomkövetés által létrehozott adatok Message Analyzer, kattintson a **fájlok hozzáadása**, toohello helyet, ahová mentette a .matp fájl a webes nyomkövetési munkamenet, jelölje be hello .matp fájl, a Tallózás gombra, és kattintson a **megnyitása**.
3. tooload hello kiszolgálóoldali naplóadatokat, kattintson a **fájlok hozzáadása**, amelybe letöltötte a kiszolgálóoldali naplók toohello helyét keresse meg, válassza ki a naplófájlokat hello hello időtartományt, tooanalyze, majd kattintson a **nyissa meg a**. Ezt követően a hello **munkamenet részletei** panelen, a set hello **szöveges napló konfigurációs** legördülő lista az egyes kiszolgálóoldali naplófájlok túl**AzureStorageLog** tooensure Microsoft Az Üzenetelemző megfelelően hello naplófájl tudja értelmezni.
4. tooload hello ügyféloldali naplóadatokat, kattintson a **fájlok hozzáadása**, keresse meg a toohello helyet, ahová mentette az ügyféloldali naplók, válassza ki a hello naplófájlokat, tooanalyze, majd kattintson **nyitott**. Ezt követően a hello **munkamenet részletei** panelen, a set hello **szöveges napló konfigurációs** legördülő lista az egyes ügyféloldali naplófájlok túl**AzureStorageClientDotNetV4** tooensure, Microsoft Message Analyzert hello naplófájl helyesen tudja értelmezni.
5. Kattintson a **Start** a hello **új munkamenet** párbeszédpanel tooload és elemzési hello naplóadatokat. hello naplóadatokat hello Message Analyzer elemzés rács jeleníti meg.

hello a következő ábrán egy példa munkamenet kiszolgáló, ügyfél és hálózati nyomkövetési naplófájlok konfigurálni.

![Message Analyzer munkamenet konfigurálása](./media/storage-e2e-troubleshooting/configure-mma-session-1.png)

Vegye figyelembe, hogy a Message Analyzer naplófájlok betölti a memóriába. Ha a napló nagy adatkészletet, érdemes toofilter rendelés tooget hello legjobb teljesítmény érdekében az Üzenetelemző legyen.

Először határozza meg a hello időkereten belül, amely érdekli áttekintése, valamint ezen az időn lehető legkisebb tartását. Sok esetben érdemes tooreview időtartamot percben vagy legfeljebb órában. Importáljon hello legkisebb naplókat, amely megfelel az igényeinek.

Ha továbbra is nagy mennyiségű naplóadatokat, majd érdemes lehet egy munkamenet-szűrő toofilter toospecify a naplóadatok betöltése előtt. A hello **munkamenet szűrő** jelölését, jelölje be hello **könyvtár** toochoose előre definiált szűrő gombra; például **globális idő szűrő i.** a hello Azure Storage-szűrők toofilter egy időintervallumban. Hello szűrési feltételek toospecify hello indítása szerkesztheti, és szeretne időbélyeg befejezési hello időköz toosee. Akkor is végezhet egy adott állapotkód; például kiválaszthatja tooload csak naplóbejegyzések hello állapotkód: 404 esetén.

Naplóadatok Microsoft Message Analyzert történő importálásával kapcsolatos további információkért lásd: [üzenet-adatok beolvasása](http://technet.microsoft.com/library/dn772437.aspx) a TechNet webhelyen.

### <a name="use-hello-client-request-id-toocorrelate-log-file-data"></a>Hello ügyfél kérelem azonosítója toocorrelate naplófájl adatainak használata
hello Azure Storage ügyféloldali kódtár automatikusan hoz létre minden egyes kérés egyedi ügyfélkérelem-azonosító. Ez az érték íródik toohello ügyfélnapló hello kiszolgálónapló és hello hálózati nyomkövetést, hogy használhassa az toocorrelate adatok Message Analyzer belül minden három naplók között. Lásd: [ügyfélkérelem-azonosító](storage-monitoring-diagnosing-troubleshooting.md#client-request-id) hello ügyfél további információt kérjen a azonosítóját.

az alábbi hello szakaszok azt ismertetik, hogyan előre konfigurált toouse és egyéni különböző nézeteket toocorrelate és csoport adatai alapján hello ügyfélkérés azonosítóját.

### <a name="select-a-view-layout-toodisplay-in-hello-analysis-grid"></a>Válassza ki a nézet elrendezését toodisplay hello elemzés rács
a Message Analyzer hello tárolási eszközök tartalmazzák a használható toodisplay az adatok hasznos csoportok és oszlopok különböző helyzetek kezelésére előre konfigurált nézetek Azure tárolási nézet elrendezést. Hozhat létre egyéni nézet elrendezést, és mentse őket ismételt felhasználásra.

hello a következő ábrán hello **nézet elrendezését** menü kiválasztásával elérhető **nézet elrendezését** hello eszköztár szalagról. az Azure Storage hello nézet elrendezések hello sorolja **Azure Storage** hello menü csomópontja. Kereshet `Azure Storage` hello Keresés mezőbe toofilter az Azure Storage a elrendezések csak megtekinthetők. Igény szerint kiválaszthatja hello csillag következő tooa nézet elrendezését toomake a kedvenc informatikai, és megjeleníti azt a hello menü hello tetején.

![Nézet elrendezését menü](./media/storage-e2e-troubleshooting/view-layout-menu.png)

Válassza ki a toobegin **ClientRequestID és modul szerint csoportosítva**. A nézet elrendezését csoportok naplózni három naplókból először ügyfélkérelem-azonosító, azután a forrás naplófájl (vagy **modul** az Üzenetelemző). Ez a nézet a részletekbe menően tárhatják fel adott ügyfélkérelem-azonosító, és adott ügyfél kérelem összes három naplófájl adatai azonosítóját.

hello a következő ábrán az elrendezés alkalmazott toohello napló mintaadatainak megtekintése, a megjelenített oszlopok egy részét. Láthatja, hogy az adott ügyfélkérelem-azonosító, hello elemzés rács adatait jeleníti meg hello ügyfél napló, a kiszolgáló naplóját és a hálózati nyomkövetés futtatása.

![Az Azure Storage nézet elrendezés](./media/storage-e2e-troubleshooting/view-layout-client-request-id-module.png)

> [!NOTE]
> Különböző naplófájlokat rendelkezik másik oszlop, ezért ha több naplófájl adatait hello elemzés rács jelenik meg, azokat az oszlopokat, nem tartalmaz egy adott sor adatokat. Például hello képen látható a fenti, az ügyfél napló sorok nem jeleníthetők meg semmilyen hello **időbélyeg**, **TimeElapsed**, **forrás**, és **cél** oszlopok, mert ezeket az oszlopokat hello ügyfélnapló nem szerepelnek, de az hello hálózati nyomkövetés. Ehhez hasonlóan hello **időbélyeg** oszlop hello kiszolgálónapló időbélyeg adatait jeleníti meg, de nem jelennek meg adatok a hello **TimeElapsed**, **forrás**, és  **Cél** oszlopokat, amelyek nem részei hello kiszolgáló naplóját.
> 
> 

Továbbá toousing hello Azure Storage nézet elrendezés esetén is meghatározása és a saját nézet elrendezések mentése. Válassza ki a többi kívánt mezőt adatok csoportosításához, és hello csoportosítási elmentse az egyéni elrendezéséhez.

### <a name="apply-color-rules-toohello-analysis-grid"></a>Szín szabályok toohello elemzés rács alkalmazása
hello tárolási eszközök is szín szabályok, amelyek ajánlat a visual azt jelenti, hogy tooidentify hibák hello elemzés rács a különböző típusú. hello előre definiált színt szabályok érvényes tooHTTP hibák, így csak a hello kiszolgáló naplóját és a hálózati nyomkövetés jelennek meg.

tooapply szín szabályokat, válassza ki **szín szabályok** hello eszköztár szalagról. Látni fogja, hello Azure Storage szín szabályok hello menüben. Hello az oktatóanyagban válassza **ügyfél hibákat (StatusCode 400 és 499)**, ahogy az alábbi képen hello.

![Az Azure Storage nézet elrendezés](./media/storage-e2e-troubleshooting/color-rules-menu.png)

Ezenkívül toousing hello Azure Storage szín szabályok, is határozza meg, és menteni a saját szín szabályokat.

### <a name="group-and-filter-log-data-toofind-400-range-errors"></a>Csoport és a szűrő adatainak toofind 400-tartomány hibák naplózása
Lesz a következő azt csoportnak, és hello napló adatok toofind hello 400 tartomány összes hibák szűrése.

1. Keresse meg a hello **StatusCode** hello elemzés rács, kattintson a jobb gombbal hello oszlop címsor, és válassza ki az oszlop **csoport**.
2. A következő csoport hello **ClientRequestId** oszlop. Megfigyelheti, hogy elemzés rács most állapot szerint vannak rendezve hello hello adatok code, és egy ügyfél kérelem azonosítóját.
3. Hello nézet szűrő eszköz ablak megjelenítése, ha azt már nem jelenik meg. Hello eszköztár menüszalagon válassza **eszköz Windows**, majd **nézet szűrő**.
4. toofilter hello adatok toodisplay csak 400-tartomány hibák naplózása, adja hozzá a következő szűrési feltételek toohello hello **nézet szűrő** ablakot, és kattintson **alkalmaz**:

    ```   
    (AzureStorageLog.StatusCode >= 400 && AzureStorageLog.StatusCode <=499) || (HTTP.StatusCode >= 400 && HTTP.StatusCode <= 499)
    ```

hello a következő ábrán a csoportosítás és a szűrő hello eredményét. Növekvő hello **ClientRequestID** mező alatt hello csoportosítás állapotkód 409, például a mutat be, amelyek adott állapotkód: egy művelet.

![Az Azure Storage nézet elrendezés](./media/storage-e2e-troubleshooting/400-range-errors1.png)

Ez a szűrő alkalmazását követően láthatja, hogy hello ügyfél naplófájl azon sorait nem tartoznak, mint hello ügyfélnapló nem tartalmaz egy **StatusCode** oszlop. a toobegin, azt áttekinti hello kiszolgálói és hálózati nyomkövetési naplók toolocate 404-es hibák, és még majd visszatérünk toohello ügyfél napló tooexamine hello Ügyfélműveletek toothem vezető.

> [!NOTE]
> Szűrést végezhet a hello **StatusCode** oszlop, és továbbra is három naplókat, beleértve az adatok megjelenítése hello ügyfélnapló, ha egy kifejezés toohello szűrőt, amely tartalmazza a naplóbejegyzéseket, ahol hello állapotkód értéke null. tooconstruct a kifejezést, használja:
> 
> <code>&#42;StatusCode >= 400 or !&#42;StatusCode</code>
> 
> Ez a szűrő sorát adja vissza minden hello ügyfélről naplóját, és csak azokat a sorokat, hello kiszolgáló naplóját és a HTTP-napló hello állapotkód nagyobb, mint a 400 esetén. Ha alkalmazása toohello nézet elrendezését ügyfélkérelem-azonosító és a modul szerint csoportosítva, kereshet, vagy hello görgetéséhez jelentkezzen bejegyzések toofind azokat, ahol minden három naplók vannak megadva.   
> 
> 

### <a name="filter-log-data-toofind-404-errors"></a>Naplózási adatok toofind 404-es hibák szűrése
hello tárolási eszközök toonarrow napló adatok toofind hello hibák vagy a keresett trendek előre definiált szűrőktől tartalmazzák. A következő fog érvénybe lépni két előre definiált szűrők: azt, amelyik szűrők hello kiszolgálói és hálózati nyomkövetési naplók a 404-es hibákat, és azt, amelyik a megadott időtartomány hello adatok szűrése.

1. Hello nézet szűrő eszköz ablak megjelenítése, ha azt már nem jelenik meg. Hello eszköztár menüszalagon válassza **eszköz Windows**, majd **nézet szűrő**.
2. Hello nézet szűrő ablakban válassza ki **könyvtár**, és a keresési `Azure Storage` toofind hello Azure Storage-szűrők. A SELECT hello szűrő **404-es (nem található) összes napló állapotüzenetek**.
3. Megjelenítési hello **könyvtár** menü újra, és keresse meg és jelölje ki a hello **globális Time Filter**.
4. Hello időbélyegeket látható hello szűrési toohello tartomány tooview kívánja szerkeszteni. Ez segít az adatok tooanalyze toonarrow hello tartományán.
5. A szűrő toohello hasonló, az alábbi példa kell megjelennie. Kattintson a **alkalmaz** tooapply hello szűrő toohello elemzés rács.

    ```   
    ((AzureStorageLog.StatusCode == 404 || HTTP.StatusCode == 404)) And
    (#Timestamp >= 2014-10-20T16:36:38 and #Timestamp <= 2014-10-20T16:36:39)
    ```

    ![Az Azure Storage nézet elrendezés](./media/storage-e2e-troubleshooting/404-filtered-errors1.png)

### <a name="analyze-your-log-data"></a>A naplózási adatok elemzése
Csoportosítva, és az adatok szűrt, láthatja az egyes kérelmeket a 404-es kapcsolatban hibát okozó hello részleteit. Hello aktuális nézet elrendezését, a hello adatok ügyfélkérelem-azonosító, majd a napló forrás szerint csoportosítva. Mivel azt jelenleg korlátozza a kérelemhez ahol hello StatusCode mezőben 404, megtanulhatja, csak hello kiszolgáló, és hálózati nyomkövetési adatok nem hello ügyfél napló.

hello a következő ábrán egy adott kérelem ahol a Blob beolvasása művelet eredményezte-e a 404 mert hello blob nem létezik. Vegye figyelembe, hogy néhány oszlop el lettek távolítva a szabványos nézetből hello rendelés toodisplay hello vonatkozó adatokat.

![Szűrt kiszolgálói és hálózati nyomkövetési naplók](./media/storage-e2e-troubleshooting/server-filtered-404-error.png)

A következő azt fogja okozták az ügyfélkérelem-azonosító az hello ügyfél napló adatok toosee milyen műveletek hello ügyfél lett véve, amikor hello hiba történt. Egy új elemzési rács nézet a munkamenet tooview hello ügyfél naplóadatokat, amely megnyit egy második lapján jelenítheti meg:

1. Első lépésként másolja az hello hello értékének **ClientRequestId** mező toohello vágólapra. Ehhez vagy sor kiválasztásával hello keresése **ClientRequestId** hello adatérték csomagot jobb gombbal, és válassza a mezőben **másolási "ClientRequestId"**.
2. Hello eszköztár menüszalagon válassza **új Viewer**, majd jelölje be **elemzés rács** tooopen egy új lapra hello új lapja mutatja minden adatot a naplófájlok nem csoportosítás, a szűrés, illetve a szín szabályok.
3. Hello eszköztár menüszalagon válassza **nézet elrendezését**, majd jelölje be **minden .NET ügyfél oszlopok** alatt hello **Azure Storage** szakasz. A nézet elrendezését a kiszolgáló és a hálózat nyomkövetési naplók hello ügyfélnapló, valamint a hello adatainak megjelenítése. Alapértelmezés szerint a hello van rendezve **MessageNumber** oszlop.
4. A következő keresése hello ügyfélnapló hello ügyfél kérelem-azonosítót. Hello eszköztár menüszalagon válassza **található üzenetek**, majd adja meg az egyéni szűrő a hello ügyfélkérelem-azonosító a hello **található** mező. Ez a szintaxis használata hello szűrőt, a saját ügyfélkérelem-azonosító megadása:

    ```
    *ClientRequestId == "398bac41-7725-484b-8a69-2a9e48fc669a"
    ```

Az Üzenetelemző megkeresi és választ hello első naplóbejegyzés ahol hello keresési feltételeknek megfelelő hello ügyfél kérelem-azonosítót. Hello ügyfél naplóban vannak több tételek minden ügyfélkérelem-azonosító, ezért érdemes toogroup hello őket **ClientRequestId** mező toomake azt könnyebb toosee összes együtt. összes hello ügyfél köszönőüzenetei keresse meg a hello az alábbi hello kép megadott ügyfél-kérelem azonosítót.

![Ügyfél naplófájl-megjelenítő 404-es hibák](./media/storage-e2e-troubleshooting/client-log-analysis-grid1.png)

Hello nézet elrendezések ezen két lapok látható hello adatok használatával, elemezheti hello kérelem adatok toodetermine hello hibát okozott mi. Is megtalálhatja a egy toosee megelőző, ha lehetséges, hogy az előző eseményeket vezettek toohello 404-es hiba kérelmek. Például tekintheti meg az ügyfél kérelem azonosítója toodetermine megelőző, hogy hello blob törölték, vagy ha hello hiba miatt a CreateIfNotExists API hívása egy tárolót vagy blobot toohello ügyfélalkalmazás hello ügyfél naplóbejegyzéseket. Hello ügyfél naplóban található hello blob cím hello **leírás** mezőben; hello kiszolgálói és hálózati nyomkövetési naplók, ez az információ jelenik meg a hello **összegzés** mező.

Miután eldöntötte, amely hello 404-es hiba eredményezte hello blob hello címét, használatával megvizsgálhatja a további. Ha további üzenetek társított hello naplóbejegyzések hello műveletek azonos blob, ellenőrizheti, hogy hello ügyfél korábban törölt hello entitás.

## <a name="analyze-other-types-of-storage-errors"></a>Más típusú tárolási hibák elemzése
Most, hogy ismeri a Message Analyzer tooanalyze adatokkal a napló, más típusú hibák nézet használatával elemezheti elrendezések, a színt szabályok és a keresés/szűrés. hello táblák listája alább néhány problémát, előfordulhat, hogy előforduló és szűrési feltételeket használhat toolocate hello őket. További információ a szűrők és hello Message Analyzer szűrés nyelvi,: [szűrés állapotüzenet-adatokat](http://technet.microsoft.com/library/jj819365.aspx).

| tooInvestigate... | Használja a szűrőkifejezés... | Kifejezés érvényesség tooLog (ügyfél, a kiszolgáló, a hálózat, az összes) |
| --- | --- | --- |
| Az üzenet kézbesítési a várólista nem várt késedelmeket |AzureStorageClientDotNetV4.Description tartalmaz "Újrapróbálkozás sikertelen műveletet." |Ügyfél |
| PercentThrottlingError HTTP növekedése |HTTP-ALAPÚ. Response.StatusCode == 500 &#124; &#124; HTTP-ALAPÚ. Response.StatusCode 503-as == |Network (Hálózat) |
| PercentTimeoutError növekedése |HTTP-ALAPÚ. Response.StatusCode == 500 |Network (Hálózat) |
| Az (all) PercentTimeoutError növelése |* StatusCode == 500 |Összes |
| PercentNetworkError növekedése |AzureStorageClientDotNetV4.EventLogEntry.Level < 2. régiója |Ügyfél |
| A HTTP 403-as (tiltott) üzenetek |HTTP-ALAPÚ. Response.StatusCode == 403 |Network (Hálózat) |
| HTTP 404-es (nem található) üzenetek |HTTP-ALAPÚ. Response.StatusCode == 404 |Network (Hálózat) |
| 404-es (összes) |* StatusCode == 404 |Összes |
| Közös hozzáférésű Jogosultságkód (SAS) hitelesítési hiba |AzureStorageLog.RequestStatus == "SASAuthorizationError" |Network (Hálózat) |
| A HTTP 409 (Ütközés) üzenetek |HTTP-ALAPÚ. Response.StatusCode == 409 |Network (Hálózat) |
| 409 (összes) |* StatusCode == 409 |Összes |
| Alacsony PercentSuccess vagy analytics naplóbejegyzés rendelkezik ClientOtherErrors működésére a tranzakció állapota |AzureStorageLog.RequestStatus == "ClientOtherError" |Kiszolgáló |
| Nagle figyelmeztetés |((AzureStorageLog.EndToEndLatencyMS-AzureStorageLog.ServerLatencyMS) > (AzureStorageLog.ServerLatencyMS * 1.5-ös)) és (AzureStorageLog.RequestPacketSize < 1460) és (AzureStorageLog.EndToEndLatencyMS - AzureStorageLog.ServerLatencyMS > = 200) |Kiszolgáló |
| A kiszolgáló és a hálózat naplókban időtartományt |#Timestamp > = 2014-10-20T16:36:38 és #Timestamp < = 2014-10-20T16:36:39 |Kiszolgáló hálózati |
| A kiszolgáló naplóiban időtartományt |AzureStorageLog.Timestamp > = 2014-10-20T16:36:38 és AzureStorageLog.Timestamp < = 2014-10-20T16:36:39 |Kiszolgáló |

## <a name="next-steps"></a>Következő lépések
Az Azure Storage hibaelhárítási végpont forgatókönyvekkel kapcsolatos további információkért lásd: ezeket az erőforrásokat:

* [Figyelése, diagnosztizálása és elhárítása a Microsoft Azure tárolás](storage-monitoring-diagnosing-troubleshooting.md)
* [Storage Analytics](http://msdn.microsoft.com/library/azure/hh343270.aspx)
* [A figyelő a tárfiókokat hello Azure-portálon](storage-monitor-storage-account.md)
* [Adatátvitel az AzCopy parancssori segédprogram hello](storage-use-azcopy.md)
* [Microsoft Message Analyzer üzemeltetési útmutató](http://technet.microsoft.com/library/jj649776.aspx)
