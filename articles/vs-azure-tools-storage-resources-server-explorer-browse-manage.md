---
title: "Keresse meg és a Server Explorer tárolási erőforrások kezelése |} Microsoft Docs"
description: "Keresse meg és a Server Explorer tárolási erőforrások kezelése"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 658dc064-4a4e-414b-ae5a-a977a34c930d
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/24/2017
ms.author: kraigb
ms.openlocfilehash: 43ab501c69c0c1e3271dbfcf08e5342a3507ab82
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="browsing-and-managing-storage-resources-with-server-explorer"></a>Keresse meg és a Server Explorer tárolási erőforrások kezelése
[!INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Áttekintés
Ha telepítette az Azure-eszközök a Microsoft Visual Studio, megtekintheti blob, queue és table adatok a storage-fiókok a az Azure-bA. Az Azure Storage-csomópont a Server Explorer adatokat, amelyek a helyi storage emulator fiókja és az egyéb Azure storage-fiókok jeleníti meg.

A Visual Studio Server Explorer tekinti meg az egérrel a menüsoron, válassza ki a **nézet**, **Server Explorer**. A tárolási csomópontnak minden Azure előfizetés/tanúsítvány kapcsolódik a meglévő tárfiókok mutatja. A tárfiók nem jelenik meg, ha egyszerűen hozzáadhatja az utasításokat követve [a témakör későbbi részében](#add-storage-accounts-by-using-server-explorer).

Az Azure SDK 2.7-től kezdődően is használhatja az új Cloud Explorer megtekintése és kezelése az Azure-erőforrások. Lásd: [Azure-erőforrások kezelése Cloud Explorer](vs-azure-tools-resources-managing-with-cloud-explorer.md) további információt.

## <a name="view-and-manage-storage-resources-in-visual-studio"></a>Megtekintheti, és a Visual Studio tárolási erőforrások kezelése
Server Explorer automatikusan emulátor a tárfiókban lévő blobokat, üzenetsorokat és táblákat listája látható. A storage emulator-fiók megtalálható a Server Explorer-e a tárolási csomópontnak, mint a **fejlesztési** csomópont.

A storage emulator fiók erőforrások megtekintéséhez bontsa ki a **fejlesztési** csomópont. Ha a storage emulator még nem indult el kibővítésekor a **fejlesztési** csomópont, akkor automatikusan elindul. Ez eltarthat néhány másodpercig. Továbbra is működik egyéb területein a Visual Studio, amíg a storage emulator elindul.

A tárfiókban lévő erőforrások megtekintéséhez bontsa ki a tárfiók csomópontot a Server Explorer. A következő alárendelt csomópont jelenik meg:

* Blobok
* Üzenetsorok
* Táblák

## <a name="work-with-blob-resources"></a>A Blob erőforrásokat
A Blobok csomópont jeleníti meg a kiválasztott tárolási fiók tárolók listája. BLOB tárolók blob fájlokat tartalmazza, és ezek a blobok rendezze mappákban és almappáiban. Lásd: [használata a .NET-Blob Storage](storage/blobs/storage-dotnet-how-to-use-blobs.md) további információt.

### <a name="to-create-a-blob-container"></a>A blob-tároló létrehozása
1. Nyissa meg a helyi menüje a **Blobok** csomópont, és válassza a **Blob-tároló létrehozása**.
2. Az a **Blob-tároló létrehozása** párbeszédpanelen adja meg az új tároló nevét.  
3. Nyomja le az **ENTER** a billentyűzet vagy az Ön is kattintson vagy koppintson a blobtárolóba menti a név mező kívül.
   
   > [!NOTE]
   > A blob-tároló neve egy szám (0-9) vagy kisbetűt (a – z) kell kezdődnie.
   > 
   > 

### <a name="to-delete-a-blob-container"></a>A blob-tároló törlése
* Nyissa meg a helyi menüben a blob tároló, távolítsa el, és válassza a kívánt **törlése**.

### <a name="to-display-a-list-of-the-items-contained-in-a-blob-container"></a>A blob-tárolóban található elemek listájának megjelenítéséhez
* Nyissa meg a blob-tároló neve helyi menüje a listában, és válassza a **nyitott**.
  
    Egy blob-tároló tartalmának megtekintésekor egy lapon, a blob-tároló nézet néven jelenik meg.
  
    ![VST_SE_BlobDesigner](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC749016.png)
  
    A blob-tároló nézet jobb felső sarkában a gombok segítségével blobok a következő műveleteket hajthatja végre:
  
  * Adjon meg egy szűrő értéket, és alkalmazza azt
  * A tárolóban lévő blobok listájának frissítése
  * Fájl feltöltése
  * Blob törlése
    
    > [!NOTE]
    > A fájl törlése a blob-tároló nem törli a fájl; csak eltávolítja azt a blob-tároló.
    > 
    > 
  * Nyissa meg a blob
  * Mentse a blob a helyi számítógépen

### <a name="to-create-a-folder-or-subfolder-in-a-blob-container"></a>A blob-tároló mappát vagy almappát létrehozásához
1. Adja meg a blob-tároló Cloud Explorer. A tároló ablakban válassza ki a **Blob feltöltése** gombra.
   
    ![A fájl feltöltése a blob mappába](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766037.png)
2. A a **új fájl feltöltése** párbeszédpanelen válassza ki a **Tallózás** gombra kattintva adja meg a feltölteni kívánt fájlt, és írja be a mappa nevét a a **mappa (nem kötelező)** mezőbe.
   
    Tároló mappa almappákat ugyanezt az eljárást követve adhat hozzá. Ha nem adja meg a mappa nevét, a fájl feltöltődik a legfelső szintű a blob-tároló. A fájl jelenik meg a megadott mappában a tárolóban.
   
    ![Egy blob-tárolóba felvett mappában találhatók](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766038.png)
3. Kattintson duplán a mappára, vagy lenyomja az ENTER BILLENTYŰT, hogy a mappa tartalmát. Ha a tároló mappában található, lépjen vissza a legfelső szintű a tároló kiválasztásával a **nyitott Szülőkönyvtárában** (felfelé mutató nyíl) gombra.

### <a name="to-delete-a-container-folder"></a>Tároló mappa törlése
* Az összes a mappában lévő fájlok törlése
  
  > [!NOTE]
  > Mivel a blob tárolók mappák virtuális mappák, üres mappa nem hozható létre, és nem is töröl egy mappát törli a fájl tartalmát. Törölje a mappát egy olyan mappa teljes tartalmát törölnie kell.
  > 
  > 

### <a name="to-filter-blobs-in-a-container"></a>A tárolóban lévő blobok szűrése
Szűrheti a blobot, amely megjeleníti azokat a közös előtag megadása.

Például, ha beírja az előtag `hello` a szűrő szöveg mezőbe, majd válassza ki a **Execute** (**!**) gomb, csak a "hello" karakterrel kezdődő blobok jelenik meg.

![VST_SE_FilterBlobs](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC519076.png)

> [!NOTE]
> A Szűrő mezőbe kis-és nagybetűket, és nem támogatja a helyettesítő karaktereket is tartalmazó szűrést. Blobok csak előtag alapján szűrhetők. Az előtag tartalmazhatják elválasztó elválasztó blobot, amely egy virtuális hierarchia rendszerezéséhez használatakor. Például az előtag HelloFabric szűrés / verziótól kezdve, hogy a karakterlánc összes BLOB adja vissza.
> 
> 

### <a name="to-download-blob-data"></a>A blob adatok letöltése
* A **Cloud Explorer**, nyissa meg a helyi menü az egy vagy több blobot, és válassza a **nyissa meg a**, vagy válassza ki a blob nevét, és válassza a **nyissa meg a** gombra, vagy kattintson duplán a blob név.
  
    Megjelenik egy blob letöltés előrehaladását a **Azure tevékenységnapló** ablak.
  
    A blob az adott fájltípus alapértelmezett-szerkesztő megnyitása. Ha az operációs rendszer felismeri a fájl típusa, a fájl megnyitása a helyileg telepített alkalmazások; Ellenkező esetben felkéri a blob típusú fájl megfelelő alkalmazás kiválasztása. A blob letöltésekor létrehozott helyi fájl csak olvashatóként van megjelölve.
  
    BLOB adatok helyben és be van jelölve, a blob utolsó módosítási időpontjának a Blob szolgáltatás ellen. A blob frissült, mivel utoljára lett letöltve, ha azt újból letöltődnek; Ellenkező esetben a blob innen lesz betöltve a helyi lemezen. Alapértelmezés szerint a blob letölti egy ideiglenes könyvtárhoz. Egy adott könyvtár blobok letöltéséhez nyissa meg a helyi menüben a kijelölt blob nevével, és válassza **Mentés másként**. Egy blobba menti ezen a módon kikapcsolja, amikor a blob-fájl nincs megnyitva, és a helyi fájl írható-olvasható attribútumokkal jön létre.

### <a name="to-upload-blobs"></a>Blobok feltöltése
* Válassza ki a **Blob feltöltése** gombra kattint, ha a tároló meg nyitva a blob-tároló nézetben megtekinthető.
  
    Választhat egy vagy több fájlt feltölteni, és feltöltheti az összes fájltípus. A **Azure tevékenységnapló** mutatja a feltöltés előrehaladását. Blob adatokkal kapcsolatos további információkért lásd: [az Azure Blob Storage szolgáltatás használata a .NET](http://go.microsoft.com/fwlink/p/?LinkId=267911).

### <a name="to-view-logs-transferred-to-blobs"></a>A blobok továbbított naplók megtekintése
* Ha Azure Diagnostics használatával adatokat az Azure alkalmazásból jelentkezik, és amelyre átmásolta naplók tárfiókja, látni fogja, ezek a naplók az Azure által létrehozott tárolók. Egyszerűen azonosíthatja a problémákat az alkalmazáshoz, ezek a naplók megtekintéséhez a Server Explorer, különösen akkor, ha üzembe helyezésüket követően az Azure-bA. Azure Diagnostics kapcsolatos további információkért lásd: [naplózási adatok gyűjtése Azure Diagnostics használatával](https://msdn.microsoft.com/library/azure/gg433048.aspx).

### <a name="to-get-the-url-for-a-blob"></a>Az URL-cím lekérése blob
* A blob helyi menü megnyitásához, és válassza a **URL-CÍMÉT**.

### <a name="to-edit-a-blob"></a>Egy blob szerkesztése
* Válassza ki a blob, és válassza a **nyissa meg a Blob** gombra.
  
    A fájl egy ideiglenes helyre letöltése és megnyitni a helyi számítógépen. Módosítása után újra kell tölteni a blob.

## <a name="work-with-queue-resources"></a>A várólista erőforrásokat
Egy Azure storage-fiókban tárolt szolgáltatások tárüzenetsort, és engedélyezi a felhőalapú szerepköreit üzenettovábbítási mechanizmus kommunikálnak egymással, és más szolgáltatásokkal használhatja. A várólista hozzáférhet programozott módon keresztül egy felhőalapú szolgáltatás, és a külső ügyfelek webszolgáltatáson keresztül. A várólista közvetlenül a Visual Studio Server Explorer használatával is elérhető.

Amikor egy felhőalapú szolgáltatás által használt várólisták fejlesztése, előfordulhat, hogy használni kívánt Visual Studio várólisták létrehozásához, és azokat interaktív használatához közben az Ön által fejlesztett és tesztelheti a kódját.

A Server Explorer eszközben, megtekintheti a várólisták tárfiókokban, hozzon létre és Várólisták törlése, nyissa meg a várólisták megtekintheti az üzenetet, és adja hozzá az üzenetek várólistára. Megtekintésre várólista megnyitásakor megtekintheti az egyes üzeneteket, és a következő műveletek végezhetők várakozási bal felső sarokban a gombok használatával:

* Frissítse a nézetet a várólista
* Üzenet a várólistában hozzáadása
* A legfelső üzenet created
* A teljes várólista törlése

Az alábbi ábrán egy két üzeneteket tartalmazó sor.

![A várólista megtekintéséhez](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC651470.png)

További információ a tárolási szolgáltatási várólisták létrehozásáról: [hogyan: használja a Queue Storage szolgáltatás](http://go.microsoft.com/fwlink/?LinkID=264702). A webszolgáltatás tárolási szolgáltatási várólisták, lásd: [a Queue szolgáltatás alapfogalmai](http://go.microsoft.com/fwlink/?LinkId=264788). Üzenetek küldése egy tárolási szolgáltatások üzenetsorba Visual Studio használatával kapcsolatos információkért lásd: [tárolási szolgáltatások várólista-üzenetek küldése](https://msdn.microsoft.com/library/azure/jj649344.aspx).

> [!NOTE]
> A service bus-üzenetsorok elkülönülnek a tárolási szolgáltatások sorok. Service bus-üzenetsorok kapcsolatos további információkért tekintse meg a Service Bus-üzenetsorok, témakörök és előfizetések.
> 
> 

## <a name="work-with-table-resources"></a>A tábla erőforrásokat
Az Azure Table Storage szolgáltatás nagy mennyiségű strukturált adatok tárolására alkalmas. A szolgáltatás egy NoSQL-adattár, amely az Azure-felhőből és azon kívülről érkező hitelesített hívásokat fogadja. Az Azure-táblák strukturált, nem relációs adatok tárolására alkalmasak.

### <a name="to-create-a-table"></a>Tábla létrehozása
1. A Cloud Explorerben válassza ki a **táblák** a tárhely csomópontot fiókot, és válassza a **Create Table**.
2. Az a **Create Table** párbeszédpanelen adja meg a tábla nevét.

### <a name="to-view-table-data"></a>Tábla adatainak megtekintéséhez
1. A Cloud Explorerben nyissa meg a **Azure** csomópont, és nyissa meg a **tárolási** csomópont.
2. Nyissa meg a tárolási fiók csomópont érdekelt, és nyissa meg a **táblák** csomópont a tárfiók táblázatok listájának megjelenítéséhez.
3. Nyissa meg a helyi menü táblázat, és válassza a **nézet tábla**.
   
    ![Egy Azure-tábla a Megoldáskezelőben](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744165.png)

A tábla (látható sorok) entitásokat és a Tulajdonságok (látható oszlopok) szerint van rendezve. Például az alábbi ábrán látható entitások szerepel a **tábla Designer**:

### <a name="to-edit-table-data"></a>A tábla adatainak szerkesztése
1. Az a **tábla Designer**, nyissa meg a helyi menüben a tulajdonsághoz (egy cellát) vagy egy entitás (egy sorban), és válassza a **szerkesztése**.
   
    ![Hozzáadása vagy módosítása egy tábla entitás](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC656238.png)
   
    Egyetlen tábla az entitások nem ugyanazokat a Tulajdonságok (oszlop) rendelkezniük. Vegye figyelembe a következő korlátozások vonatkoznak a tábla adatok megtekintése és módosítása.
   
   * Nem tekinthetők meg és a bináris adatok szerkesztéséhez (típus byte[]), de érdemes egy táblázatban.
   * Nem lehet szerkeszteni a **PartitionKey** vagy **RowKey** érték található, mert az Azure-ban a table storage nem támogatja a műveletet.
   * Nem hozható létre a tulajdonságot, Timestamp, Azure Storage szolgáltatás tulajdonsággal egy ezen a néven.
   * Ha egy dátum/idő értéket ad meg, hajtsa végre az formátuma nem megfelelő. a számítógép területi és nyelvi beállítások a (például hh/nn/éééé óó: pp: [AM |} PM] az USA Angol nyelvű).

### <a name="to-add-entities"></a>Entitás hozzáadása
1. Az a **tábla Designer**, válassza ki a **entitás hozzáadása** gombra.
   
    ![Entitás hozzáadása](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655336.png)
2. Az a **entitás hozzáadása** párbeszédpanelen adja meg az értékeket a **PartitionKey** és **RowKey** tulajdonságok.
   
    ![Entitás párbeszédpanel hozzáadása](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655335.png)
   
    Adja meg az értékeket gondosan már nem módosíthatók őket, kivéve, ha törölni egy entitást, és vegye fel újra a párbeszédpanel bezárása után.

### <a name="to-filter-entities"></a>Entitások szűrése
Testre szabhatja az entitásokat, amely egy táblázat jelenik meg, ha a lekérdezés-szerkesztő használata.

1. A lekérdezés-szerkesztő megnyitásához megtekintésre tábla.
2. Válassza ki a lekérdezés-szerkesztő gombjára a tábla nézet.
   
    A **Lekérdezésszerkesztő** párbeszédpanel jelenik meg. A következő ábrán egy lekérdezést, amely éppen készül a Lekérdezésszerkesztőben.
   
    ![Lekérdezés-szerkesztő](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC652231.png)
3. Ha elkészült a lekérdezés létrehozása a párbeszédpanel bezárásához. A lekérdezés megjelenő szöveg képernyőn a WCF Data Services szűrő beviteli mezőben jelenik meg.
4. A lekérdezés futtatásához, válassza a zöld háromszögnek ikont.
   
    Entitás megjelenő adatok is végezhet a **tábla Designer** Ha megad egy a WCF Data Services szűrési karakterláncot közvetlenül a Szűrő mezőbe. Az ilyen típusú karakterlánc egy SQL WHERE záradék hasonló, de a kiszolgálón, a HTTP-kérelem érkezik. Szűrőkarakterláncokban összeállításához kapcsolatos információkért lásd: [Szűrőkarakterláncokban hozhat létre a tábla Designer](https://msdn.microsoft.com/library/azure/ff683669.aspx).
   
    Az alábbi ábrán egy példa egy érvényes szűrési karakterláncot:
   
    ![VST_SE_TableFilter](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655337.png)

### <a name="refresh-storage-data"></a>Tárolási adatok frissítése
Amikor Server Explorer csatlakozik, vagy az adatok beolvasása a tárfiókból, eltarthat egy perc alatt, a művelet elvégzéséhez. Ha ez sikertelen, a művelet lehet, hogy túllépi az időkorlátot. Adatok lekérésére, miközben továbbra is működik, a Visual Studio többi részével. Ha túl sokáig tart a művelet megszakításához válassza a **frissítés leállítása** a Server Explorer gombjára.

#### <a name="to-refresh-blob-container-data"></a>A blob-tároló adatok frissítése
* Válassza ki a **Blobok** csomóponton sem **tárolási** , és válassza a **frissítése** a Server Explorer gombjára.
* -Listájának frissítése blobot, amely akkor jelenik meg, válassza ki a **Execute** gombra.

#### <a name="to-refresh-table-data"></a>A tábla adatainak frissítése
* Válassza ki a **táblák** csomóponton sem **tárolási** , és válassza a **frissítése** gombra.
* Az alkalmazásban megjelenő entitások listájának frissítése a **tábla Designer**, válassza ki a **Execute** gombra a **tábla Designer**.

#### <a name="to-refresh-queue-data"></a>A várólista adatok frissítése
* Válassza ki a **várólisták** csomópontot, majd válassza a **frissítése** gombra.

#### <a name="to-refresh-all-items-in-a-storage-account"></a>A tárfiók szereplő összes frissítése
* Válassza ki a fiók nevét, és válassza a **frissítése** gombra az eszköztáron a Server Explorer.

### <a name="add-storage-accounts-by-using-server-explorer"></a>A Server Explorer storage-fiókok hozzáadása
Tárfiók hozzáadása a Server Explorer használatával két módja van. Létrehozhat egy új tárfiókot az Azure-előfizetése, vagy csatolhat a meglévő tárfiókot.

#### <a name="to-create-a-new-storage-account-by-using-server-explorer"></a>Új tárfiók létrehozása a Server Explorer használatával
1. A Server Explorer eszközben a tárhely csomópontot a helyi menü megnyitásához, és válassza a Storage-fiók létrehozása.
   
    ![Új Azure storage-fiók létrehozása](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744166.png)
2. Válassza ki vagy adja meg az új tárfiók a következő adatokat a **Storage-fiók létrehozása** párbeszédpanel megnyitásához.
   
   * Az Azure-előfizetés kívánt tárfiók hozzáadása.
   * Az új tárfiók használni kívánt nevét.
   * A régiót vagy affinitáscsoportot (például az USA nyugati régiója vagy Kelet-Ázsia).
   * A tárfiók, például a Georedundáns használni kívánt replikációs típusát.
3. Válassza a **Létrehozás** elemet.
   
    Az új tárfiók megjelenik a **tárolási** lista a Megoldáskezelőben.

#### <a name="to-attach-an-existing-storage-account-by-using-server-explorer"></a>Meglévő tárfiók csatolása a Server Explorer használatával
1. A Server Explorer eszközben az Azure storage csomóponthoz a helyi menü megnyitásához, és válassza **külső tárterület csatolása**.
   
    ![Meglévő tárfiók hozzáadása](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766039.png)
2. Válassza ki vagy adja meg az új tárfiók a következő adatokat a **Storage-fiók létrehozása** párbeszédpanel megnyitásához.
   
   * A meglévő tárfiók csatolásához neve. Adjon meg egy nevet, vagy válassza ki azt a listából.
   * A kiválasztott tárfiók kulcsa. Ez az érték általában biztosított, válasszon egy tárfiókot. Ha azt szeretné, hogy a Visual Studio jegyezze meg a tárfiók hívóbetűjét, válassza ki a felhasználó megjegyzését fiók kulcs mezőben.
   * A tárfiók, például HTTP, HTTPS vagy egy egyéni végpont való csatlakozáshoz használni kívánt protokollt. Lásd: [kapcsolati karakterláncok konfigurálása hogyan](https://msdn.microsoft.com/library/azure/ee758697.aspx) egyéni végpontokat további információt.

### <a name="to-view-the-secondary-endpoints"></a>A másodlagos végpontok megtekintése
* Ha létrehozott egy tárolási fiók a **írásvédett Georedundáns** replikációs beállítás, megtekintheti a másodlagos végpontját. Nyissa meg a helyi menüben a fiók neve, és válassza **tulajdonságok**.
  
    ![Tárolási másodlagos végpontok](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766040.png)

### <a name="to-remove-a-storage-account-from-server-explorer"></a>A tárfiók eltávolítása a Server Explorer
* A Server Explorer eszközben nyissa meg a helyi menüben a fiók neve, és válassza **törlése**. Ha törli a tárfiókot, minden mentett kulcsadatokat fiók is törlődnek.
  
  > [!NOTE]
  > A Server Explorer a storage-fiók törlése esetén nem változik a tárfiók vagy adatokat tartalmaz; egyszerűen eltávolítja a hivatkozás a Server Explorer. Véglegesen törli a tárfiókot, használja a [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885).
  > 
  > 

## <a name="next-steps"></a>Következő lépések
További információt az Azure storage szolgáltatások használatára, lásd: [elérése az Azure Storage szolgáltatásainak](https://msdn.microsoft.com/library/azure/ee405490.aspx).

