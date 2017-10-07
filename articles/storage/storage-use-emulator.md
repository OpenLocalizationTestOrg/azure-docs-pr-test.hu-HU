---
title: "aaaUse hello az Azure storage emulator a fejlesztéshez és teszteléshez |} Microsoft Docs"
description: "hello az Azure storage emulator egy szabad helyi fejlesztési környezetet biztosít az fejlesztés és tesztelés az Azure Storage-alkalmazások. Ismerje meg, hogyan kérések hitelesítése, hogyan tooconnect toohello emulátor az alkalmazást, és hogyan toouse hello parancssori eszközt."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: f480b059-df8a-4a63-b05a-7f2f5d1f5c2a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: marsma
ms.openlocfilehash: 42637dcd9f476069e6ecd19ed04e7ed93fe38ff7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-storage-emulator-for-development-and-testing"></a>Hello Azure storage emulatorral a fejlesztéshez és teszteléshez

hello Microsoft Azure storage emulator emulálja hello Azure Blob, Queue és Table szolgáltatások fejlesztési célokra szolgál egy helyi környezet biztosít. Hello storage emulator használatával, akkor nélkül lehet tesztelni az alkalmazást helyileg, tárolószolgáltatások hello Azure-előfizetés létrehozása vagy az ezzel járó költségeket. Ha elégedett az alkalmazás alakulását hello emulátorban, megváltoztathatja az toousing hello felhőben Azure storage-fiók.

## <a name="get-hello-storage-emulator"></a>Hello storage emulator beolvasása
hello storage emulator érhető hello részeként [Microsoft Azure SDK](https://azure.microsoft.com/downloads/). Hello storage emulator hello segítségével is telepíthet [önálló telepítő](https://go.microsoft.com/fwlink/?linkid=717179&clcid=0x409) (közvetlen letöltése). tooinstall hello storage emulatort, a rendszergazdai jogosultsággal kell rendelkeznie a számítógépen.

hello storage emulator jelenleg kizárólag Windows rendszeren fut. Annak eldöntéséhez, hogy a storage emulator Linux azok egy beállítás akkor fenn, nyílt forráskódú storage emulator hello közösségi [Azurite](https://github.com/arafato/azurite).

> [!NOTE]
> Hello storage emulator egy verziójában létrehozott adatokat nem garantált toobe érhető el egy másik verzió használata esetén. Szüksége toopersist az adatok hosszú távú hello, azt javasoljuk, hogy az Azure-tárfiók, nem pedig a hello storage emulator tárolja ezeket az adatokat.
> <p/>
> hello storage emulator hello OData-tárak egy adott verzióját függ. Hello storage emulator más verziókkal által használt hello OData dll cseréje nem támogatott, és nem várt működést eredményezhet. Előfordulhat azonban, hogy az OData hello tároló szolgáltatás által támogatott bármely verziójának használt toosend kérelmek toohello emulátor.
>

## <a name="how-hello-storage-emulator-works"></a>Hello storage emulator működése
hello storage emulator egy Microsoft SQL Server helyi példányát fájl- és hello helyi rendszer tooemulate az Azure storage használja. Alapértelmezés szerint a hello storage emulator egy adatbázist használ a Microsoft SQL Server 2012 Express LocalDB. Választhat tooconfigure hello storage emulator tooaccess hello LocalDB példányához helyett az SQL Server helyi példányát. További információkért lásd: hello [kezdő- és inicializálási hello storage emulator](#start-and-initialize-the-storage-emulator) szakasz a cikk későbbi részében.

hello storage emulator tooSQL Server vagy Windows-hitelesítést használó LocalDB csatlakozik.

Néhány funkcióbeli különbségek létezik hello storage emulatort, és az Azure storage szolgáltatások között. Ezek a különbségek kapcsolatos további információkért lásd: hello [hello storage emulator és az Azure Storage különbségei](#differences-between-the-storage-emulator-and-azure-storage) szakasz a cikk későbbi részében.

## <a name="start-and-initialize-hello-storage-emulator"></a>Indítsa el, és hello storage emulator inicializálása
toostart hello az Azure storage emulator:
1. Jelölje be hello **Start** gombra vagy nyomja le az hello **Windows** kulcs.
1. Írja be a szöveget `Azure Storage Emulator`.
1. Válassza ki a hello emulátor hello megjelenített alkalmazások listája.

Hello storage emulator indításakor jelenik meg egy parancssori ablakot. A konzol ablakot toostart és stop hello storage emulatorral, törlésére, állapota és hello emulátor inicializálni. További információkért lásd: hello [Storage emulator parancssori eszköz hivatkozás](#storage-emulator-command-line-tool-reference) szakasz a cikk későbbi részében.

Hello emulátor futtatásakor a Windows tálca értesítési területén hello egy ikon látható.

Hello storage emulator parancssor ablakának bezárásakor hello storage emulator toorun továbbra is. hello Storage Emulator konzol ablakot toobring újra, kövesse az előző lépésekben, mintha hello storage emulator indítása hello.

hello hello storage emulatort, első futtatásakor hello helyi tároló környezet inicializálását meg. hello inicializálási folyamat adatbázist hoz létre LocalDB és fenntartja a HTTP-portok minden egyes helyi tároló szolgáltatás.

hello storage emulator alapértelmezés szerint telepítve van túl`C:\Program Files (x86)\Microsoft SDKs\Azure\Storage Emulator`.

> [!TIP]
> Használhatja a hello [Microsoft Azure Tártallózó](http://storageexplorer.com) toowork a helyi storage emulator erőforrásait. Keresse meg "(fejlesztés)" a "Tárfiókok" hello Tártallózó erőforrások fában telepítve és hello storage emulator lépések után.
>

### <a name="initialize-hello-storage-emulator-toouse-a-different-sql-database"></a>Hello storage emulator toouse egy másik SQL-adatbázis inicializálása
Hello tárolási emulátor parancssori eszköz tooinitialize hello tárolási emulátor toopoint tooa SQL database-példányt eltérő hello alapértelmezett LocalDB példányához is használhatja:

1. Nyissa meg hello Storage Emulator konzolablak hello leírtak [kezdő- és inicializálási hello storage emulator](#start-and-initialize-the-storage-emulator) szakasz.
1. Hello konzolablakban, írja be a hello a következő parancsot, ahol `<SQLServerInstance>` hello hello SQL Server-példány neve. Adja meg a toouse LocalDB, `(localdb)\MSSQLLocalDb` hello SQL Server-példányt.

  `AzureStorageEmulator.exe init /server <SQLServerInstance>`

  A következő parancsot, amely arra utasítja a hello emulátor toouse hello alapértelmezett SQL Server-példány hello is használhatja:

  `AzureStorageEmulator.exe init /server .\\`

  Vagy a következő parancsot, amely hello adatbázis toohello alapértelmezett LocalDB példányához újrainicializálja hello használhatja:

  `AzureStorageEmulator.exe init /forceCreate`

Ezek a parancsok kapcsolatos további információkért lásd: [Storage emulator parancssori eszköz hivatkozás](#storage-emulator-command-line-tool-reference).

> [!TIP]
> Használhatja a hello [Microsoft SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) toomanage az SQL Server-példányok, beleértve a hello LocalDB telepítés. A hello SMSS **tooServer csatlakozás** párbeszédpanelen adja meg `(localdb)\MSSQLLocalDb` hello a **kiszolgálónév:** mező tooconnect toohello LocalDB példányához.

## <a name="authenticating-requests-against-hello-storage-emulator"></a>Hitelesítési kérésekhez hello storage emulatorban
Miután telepített, és elindult hello storage emulatort, tesztelheti rajta a kódot. Mivel az Azure Storage hello felhőben, minden kérelemnél hello storage emulatorban elvégezte hitelesíteni kell, kivéve ha egy névtelen kérelem. Megosztott kulcsos hitelesítést használó hello storage emulatorban vagy a közös hozzáférésű jogosultságkód (SAS) kérelmek hitelesítéséhez.

### <a name="authenticate-with-shared-key-credentials"></a>Hitelesítés megosztott kulcsos hitelesítő adatokkal
[!INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

A kapcsolati karakterláncok további információkért lásd: [konfigurálása Azure Storage kapcsolati karakterláncok](storage-configure-connection-string.md).

### <a name="authenticate-with-a-shared-access-signature"></a>A közös hozzáférésű jogosultságkód hitelesítéshez
Egy Azure storage ügyfélkódtáraival, például hello Xamarin függvénytárat, csak egy közös hozzáférésű jogosultságkód (SAS) jogkivonattal hitelesítés támogatására. Hello SAS-jogkivonat hello hasonló eszköz használatával létrehozhat [Tártallózó](http://storageexplorer.com/) vagy egy másik alkalmazás, amely támogatja a megosztott kulcsos hitelesítést.

Azure PowerShell használatával is létrehozhat egy SAS-jogkivonatot. hello alábbi példa létrehoz egy SAS jogkivonatot teljes körű engedélyekkel tooa blob-tároló:

1. Azure PowerShell telepítése, ha még (hello Azure PowerShell-parancsmagok hello legújabb verzió javasolt). A telepítési utasításokért lásd: [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/install-azurerm-ps).
2. Nyissa meg az Azure PowerShell és a következő parancsokat, hogy hello `ACCOUNT_NAME` és `ACCOUNT_KEY==` a saját hitelesítő adataival, és `CONTAINER_NAME` a néven:

```powershell
$context = New-AzureStorageContext -StorageAccountName "ACCOUNT_NAME" -StorageAccountKey "ACCOUNT_KEY=="

New-AzureStorageContainer CONTAINER_NAME -Permission Off -Context $context

$now = Get-Date

New-AzureStorageContainerSASToken -Name CONTAINER_NAME -Permission rwdl -ExpiryTime $now.AddDays(1.0) -Context $context -FullUri
```

hello eredményül kapott közös hozzáférésű jogosultságkódot URI hello új tároló a következőképpen kell kinéznie:

```
https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2015-07-08T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3Dsss
```

hello közös hozzáférésű jogosultságkódot létrehozni ebben a példában egy napig érvénytelen. hello aláírás hello tárolón belül (olvasási, írási, törlés, lista) teljes körű hozzáférés tooblobs biztosít.

További információ a közös hozzáférésű jogosultságkódokról: [használata közös hozzáférésű jogosultságkód (SAS) az Azure Storage](storage-dotnet-shared-access-signature-part-1.md).

## <a name="addressing-resources-in-hello-storage-emulator"></a>A hello storage emulator címzési erőforrások
hello Szolgáltatásvégpontok hello storage Emulator különböznek az Azure storage-fiók. mivel hello helyi számítógép nem végez a tartománynév feloldásával, hello storage emulator végpontok toobe helyi címek igénylő hello különbség.

Cím egy Azure storage-fiókot az erőforráshoz, használja a következő séma hello. hello fióknév hello URI állomásnév részét képezi, és jelenleg hello erőforrás hello URI elérési út egy része:

`<http|https>://<account-name>.<service-name>.core.windows.net/<resource-path>`

Például hello következő URI Azonosítót az Azure-tárfiók a blob egy érvényes címet:

`https://myaccount.blob.core.windows.net/mycontainer/myblob.txt`

Azonban hello storage emulatort, mert hello helyi számítógép nem végez a tartománynév feloldásával, hello fióknév hello állomásnév helyett hello URI elérési út egy része. A következő URI-formátum hello storage emulator az erőforráshoz tartozó hello használata:

`http://<local-machine-address>:<port>/<account-name>/<resource-path>`

Például hello következő cím felhasználható a hello storage emulator blob eléréséhez:

`http://127.0.0.1:10000/myaccount/mycontainer/myblob.txt`

hello storage Emulator hello végpontok a következők:

* BLOB szolgáltatás:`http://127.0.0.1:10000/<account-name>/<resource-path>`
* Queue szolgáltatás:`http://127.0.0.1:10001/<account-name>/<resource-path>`
* TABLE szolgáltatás:`http://127.0.0.1:10002/<account-name>/<resource-path>`

### <a name="addressing-hello-account-secondary-with-ra-grs"></a>Az RA-GRS másodlagos címzési hello fiók
3.1-es verziójával kezdve hello storage emulator írásvédett georedundáns replikáció (RA-GRS) támogatja. Tárolási erőforrások hello felhő- és hello helyi emulátorban, hello másodlagos helyre próbál hozzáférni, fűznek - másodlagos toohello fióknevet. Például a következő cím hello használhatók például hello storage emulator használatával hello írásvédett másodlagos blob elérése:

`http://127.0.0.1:10000/myaccount-secondary/mycontainer/myblob.txt`

> [!NOTE]
> Programozott hozzáférés toohello másodlagos hello tárolási emulátorral használja hello a Storage ügyféloldali kódtára a .NET-hez 3.2-es vagy újabb verziója. Lásd: hello [Microsoft Azure Storage ügyféloldali kódtára a .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx) részleteiről.
>
>

## <a name="storage-emulator-command-line-tool-reference"></a>Storage emulator parancssori eszköz referencia
3.0-s verziójával kezdődően a konzol ablaka hello Storage Emulator indításakor. Hello parancsot használja hello konzol ablakának toostart és stop hello emulátor, valamint a lekérdezés az állapotot, és egyéb műveleteket végezhet.

> [!NOTE]
> Ha a Microsoft Azure compute emulator telepítve hello, egy tálcaikonjára akkor jelenik meg, amikor a Storage Emulator hello indíthatja el. Kattintson a jobb gombbal a hello ikon tooreveal egy grafikus felületén toostart menü, és állítsa le a Storage Emulator hello.
>
>

### <a name="command-line-syntax"></a>Parancssori szintaxis
`AzureStorageEmulator.exe [start] [stop] [status] [clear] [init] [help]`

### <a name="options"></a>Beállítások
beállítások, típus tooview hello listájának `/help` hello parancssorban.

| Beállítás | Leírás | Parancs | Argumentumok |
| --- | --- | --- | --- |
| **Kezdés** |Hello storage emulator elindul. |`AzureStorageEmulator.exe start [-inprocess]` |*-inprocess*: hello emulátor kezdőkönyvtára hello aktuális folyamat új folyamat létrehozása helyett. |
| **állj** |Leállítja a storage emulator hello. |`AzureStorageEmulator.exe stop` | |
| **Állapot** |Megrendelése hello hello storage emulator állapotát. |`AzureStorageEmulator.exe status` | |
| **Törölje a jelet** |Hello parancssorban megadott összes szolgáltatás hello adatokat törli. |`AzureStorageEmulator.exe clear [blob] [table] [queue] [all]                                                    ` |*a BLOB*: blob-adatok törlése. <br/>*várólista*: várólista adatokat törli. <br/>*tábla*: törli a tábla adatai. <br/>*minden*: az összes szolgáltatás az összes adat törlése. |
| **Init** |Egyszeri inicializálási tooset hello emulátor mentést hajt végre. |<code>AzureStorageEmulator.exe init [-server serverName] [-sqlinstance instanceName] [-forcecreate&#124;-skipcreate] [-reserveports&#124;-unreserveports] [-inprocess]</code> |*-kiszolgáló Kiszolgáló_neve\példány_neve*: hello SQL-példányt üzemeltető hello kiszolgáló határozza meg. <br/>*-sqlinstance példánynév*: hello SQL-példány toobe hello alapértelmezett server példánya hello nevét adja meg. <br/>*-forcecreate*: hello SQL-adatbázis létrehozása kényszeríti, még akkor is, ha már létezik. <br/>*-skipcreate*: hello SQL adatbázis-létrehozásának kihagyja. Ez elsőbbséget élvez - forcecreate.<br/>*-reserveports*: kísérletek hello szolgáltatásokkal társított tooreserve hello HTTP-portok.<br/>*-unreserveports*: hello szolgáltatásokkal társított hello HTTP-port lefoglalását tooremove kísérletek. Ez elsőbbséget élvez - reserveports.<br/>*-inprocess*: inicializálási végez helyett egy új folyamat terjesztése hello jelenlegi folyamatban. hello aktuális folyamat emelt jogosultsági szintű indítható el, ha port foglalások módosítása. |

## <a name="differences-between-hello-storage-emulator-and-azure-storage"></a>Hello storage emulatort, és az Azure Storage közötti különbségek
Mivel hello storage emulator egy helyi SQL-példány fut egy emulált környezet, különbségek vannak a funkciók hello emulátor és az Azure-tárfiók közötti hello felhő:

* hello storage emulator csak egyetlen rögzített fiók és egy jól ismert hitelesítési kulcs támogatja.
* hello storage emulator nem méretezhető tárhelyre, és nem támogatja a nagyszámú egyidejű ügyfelek.
* A [hello storage emulator erőforrások címzési](#addressing-resources-in-the-storage-emulator), erőforrások másképp kiiktatása hello storage emulator egy Azure storage-fiók és a. Ezt a különbséget oka, hogy a tartománynév feloldásával érhető el a hello felhő, de nem hello helyi számítógépre.
* 3.1-es verziójával kezdve hello emulátor tárfiók írásvédett georedundáns replikáció (RA-GRS) támogatja. Hello emulátorban minden fiókok RA-GRS engedélyezve van, és soha nincs bármely lag hello elsődleges és másodlagos replikák között. hello lekérése Blob szolgáltatás statisztikák, a várólista-szolgáltatás statisztikák beolvasása és a Table szolgáltatás statisztikák beolvasása műveletek támogatottak a másodlagos hello fiókot, és mindig visszatér hello hello értékének `LastSyncTime` , az aktuális idő függően toohello az alapul szolgáló hello válasz elem SQL-adatbázis.
* hello szolgáltatás, és az SMB protokoll Szolgáltatásvégpontok jelenleg nem támogatottak a hello storage emulator.
* Hello emulátor által még nem támogatott verziót hello tárolási szolgáltatások használatakor hello storage emulator egy VersionNotSupportedByEmulator hiba (HTTP-állapotkód: 400 - Hibás kérés) adja vissza.

### <a name="differences-for-blob-storage"></a>Blob Storage különbségek
a következő különbségek hello tooBlob tárolási hello emulátorban vonatkoznak:

* hello storage emulator csak a blob mérete too2 GB mentést támogatja.
* Növekményes másolatot lehetővé teszi, hogy a felülírt blobok toobe másolta, ami visszaadja hibát hello szolgáltatás pillanatképeinek.
* Get tartományokat különbözeti nem működik a másolt növekményes másolatot Blob használatával pillanatképek között.
* Egy Put Blob művelet ellen, hogy megtalálható-hello storage emulator egy aktív bérleti jog vonatkozik, a blob telepítése sikerülhet, még akkor is, ha hello bérleti azonosítója nincs megadva hello kérelemben.
* Hozzáfűző Blob hello emulator nem támogatja a műveleteket. A hozzáfűző blob egy műveletet próbált adja vissza egy FeatureNotSupportedByEmulator hiba (HTTP-állapotkód: 400 - Hibás kérés).

### <a name="differences-for-table-storage"></a>A Table storage eltéréseket
a következő különbségek hello hello emulátorban tooTable tárolási vonatkoznak:

* Hello hello storage emulator a Table szolgáltatás dátum tulajdonság csak az SQL Server 2005 (azok szükséges toobe legkésőbb 1753. január 1.) által támogatott hello tartomány támogatja. Minden dátumra 1753. január 1. előtt módosulnak toothis érték. hello dátumok pontosságát az SQL Server 2005, ami azt jelenti, hogy a dátum pontos too1 értékei korlátozott toohello pontosság/másodperc 300th.
* hello storage emulator partíciós kulcs és a sor kulcstulajdonság-értéket kisebb, mint 512 bájtos támogatja. Ezenkívül hello teljes mérete hello fiók nevét, a tábla neve és a kulcstulajdonság neve együtt legfeljebb 900 bájtot.
* hello teljes hello storage emulator egy olyan táblázatában sor mérete 1 MB-nál korlátozott tooless.
* Az adatok tulajdonságok írja be a hello storage emulatort, `Edm.Guid` vagy `Edm.Binary` támogatási csak hello `Equal (eq)` és `NotEqual (ne)` összehasonlító operátorok, a lekérdezési karakterláncok szűrése.

### <a name="differences-for-queue-storage"></a>A Queue storage eltéréseket
Nincsenek nem eltéréseket adott tooQueue tárolási hello-emulátoron.

## <a name="storage-emulator-release-notes"></a>Storage emulator kibocsátási megjegyzései
### <a name="version-52"></a>5.2-es verzió
* hello storage emulator hello tárolószolgáltatások 2017-04-17 verziója mostantól támogatja a Blob, Queue és Table szolgáltatási végpont.
* Rögzített programhiba, ahol tábla tulajdonságértékek volt folyamatban kódolása nem megfelelő.

### <a name="version-51"></a>5.1-es verzió
* Rögzített programhiba, ahol a hello storage emulator lett visszaadó hello `DataServiceVersion` fejléc a következő néhány válaszokat, ahol hello szolgáltatás nem volt.

### <a name="version-50"></a>5.0-s verziója
* hello storage emulator telepítő már nem létező MSSQL keres, és telepíti a .NET-keretrendszer.
* hello storage emulator telepítő már nem a telepítés részeként hello adatbázist hoz létre. Adatbázis továbbra is létrejön, indítási részeként szükség esetén.
* Az adatbázis létrehozása már nem szükséges jogosultságszint-emelés.
* Lefoglalások port már nincs szükség az indításhoz.
* Hozzáadja az alábbi beállítások túl hello`init`: `-reserveports` (jogosultságszint-emelés szükséges), `-unreserveports` (jogosultságszint-emelés szükséges), `-skipcreate`.
* hello Storage Emulator felhasználói felületén a kapcsolót hello tálcaikonjára most elindítja hello parancssori felületen. hello régi grafikus felhasználói Felülettel már nem érhető el.
* Egyes DLL-ek eltávolították vagy átnevezték.

### <a name="version-46"></a>4.6-os verzió
* hello storage emulator hello tárolószolgáltatások 2016-05-31 verziója mostantól támogatja a Blob, Queue és Table szolgáltatási végpont.

### <a name="version-45"></a>4.5-ös verziója
* Rögzített, ami miatt az inicializálási és hello storage emulator toofail telepítése adatbázis biztonsági hello átnevezéskor programhiba.

### <a name="version-44"></a>4.4 verziója
* hello storage emulator hello tárolószolgáltatások 2015-12-11-es verzió most már támogatja a Blob, Queue és Table szolgáltatási végpont.
* hello storage emulator szemétgyűjtés a blob típusú adatok funkciója még hatékonyabb a nagyszámú BLOB meghatározásakor.
* Egy hiba, a tároló hozzáférés-vezérlési lista XML toobe hogyan hello társzolgáltatás minderre érvényesítve mértékben eltér, hogy rögzíteni.
* Rögzített programhiba okozó néha maximum, és minimális dátum/idő értékek toobe jelentett hello megfelelő időzónára.

### <a name="version-43"></a>4.3 verziója
* hello storage emulator hello tárolószolgáltatások 2015-07-08 verziója mostantól támogatja a Blob, Queue és Table szolgáltatási végpont.

### <a name="version-42"></a>4.2-es verzió
* hello storage emulator hello tárolószolgáltatások 2015-04-05-ös verziója mostantól támogatja a Blob, Queue és Table szolgáltatási végpont.

### <a name="version-41"></a>4.1-es verziója
* hello storage emulator hello tárolószolgáltatások 2015-02-21 verziója mostantól támogatja a Blob, Queue és Table szolgáltatás végpontok, kivéve a hello új hozzáfűző Blob szolgáltatások.
* Ha hello tárolási szolgáltatások hello emulátor által még nem támogatott verziót használ, a hello emulátor jelentéssel bíró hibaüzenetet ad vissza. Azt javasoljuk, hogy hello hello emulator legújabb verzióját használja. Ha egy VersionNotSupportedByEmulator hiba (HTTP-állapotkód: 400 - Hibás kérés), töltse le hello hello storage emulator legújabb verzióját.
* A statikus programhiba amelynek egy versenyhelyzet okoz tábla entitás adatok toobe helytelen párhuzamos merge műveletek során.

### <a name="version-40"></a>4.0-s verziója
* hello storage emulator végrehajtható túl át lett nevezve*AzureStorageEmulator.exe*.

### <a name="version-32"></a>3.2-es verziója
* hello storage emulator hello tárolószolgáltatások 2014-02-14-es verziója mostantól támogatja a Blob, Queue és Table szolgáltatási végpont. Fájl Szolgáltatásvégpontok jelenleg nem támogatottak a hello storage emulator. Lásd: [hello Azure Storage szolgáltatásainak verziószámozásának](/rest/api/storageservices/Versioning-for-the-Azure-Storage-Services) a 2014-02-14-es verzióval kapcsolatos adatokat.

### <a name="version-31"></a>3.1-es verziója
* Írásvédett georedundáns tárolás (RA-GRS) most hello storage emulator használata támogatott. hello lekérése Blob statisztikák, a várólista-szolgáltatás statisztikák beolvasása és a Get tábla szolgáltatás statisztikák API-hello fiók másodlagos támogatottak, ezért mindig hello LastSyncTime válasz elem, az aktuális idő függően toohello az alapul szolgáló SQL hello hello értéket ad vissza az adatbázis. Programozott hozzáférés toohello másodlagos hello tárolási emulátorral használja hello a Storage ügyféloldali kódtára a .NET-hez 3.2-es vagy újabb verziója. Hello Microsoft Azure Storage ügyféloldali kódtára a .NET-referencia talál információt.

### <a name="version-30"></a>3.0-s verzió
* hello az Azure storage emulator nem képezte a hello ugyanaz, mint hello compute emulator csomag.
* hello storage emulator grafikus felhasználói felület helyett egy parancsfájlok futtatására alkalmas parancssori felület elavult. Hello parancssori felület a részletekért lásd: a Storage Emulator parancssori eszköz útmutatóban. hello grafikus felület továbbra is toobe 3.0-s verziója található, de csak elérhető hello Compute Emulator telepítésekor kattintson a jobb gombbal a hello tálcaikonjára, majd válassza a Storage Emulator UI megjelenítése.
* Verzió 2013-08-15 hello az Azure storage szolgáltatások most már teljes mértékben támogatott. (Korábban Storage Emulator 2.2.1-en verziója által támogatott az ebben a verzióban csak előzetes.)

## <a name="next-steps"></a>Következő lépések

* Hello platformfüggetlen, közösségi karbantartása nyílt forráskódú storage emulator kiértékelése [Azurite](https://github.com/arafato/azurite). 
* [Azure Storage-minták .NET használatával](storage-samples-dotnet.md) hivatkozások tooseveral mintakódok is használhatja, ha az alkalmazás fejlesztése tartalmazza.
* Használhatja a hello [Microsoft Azure Tártallózó](http://storageexplorer.com) toowork Storage-fiók a felhőben, és a hello storage emulator erőforrásokkal.
