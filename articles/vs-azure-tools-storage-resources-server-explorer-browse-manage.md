---
title: "aaaBrowsing és a Server Explorer tárolási erőforrások kezelése |} Microsoft Docs"
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
ms.openlocfilehash: f5b456b812f2ad8103c50538d532a57397bcccbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="browsing-and-managing-storage-resources-with-server-explorer"></a>Keresse meg és a Server Explorer tárolási erőforrások kezelése
[!INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Áttekintés
Ha hello Azure eszközök a Microsoft Visual Studio telepítése, megtekintheti blob, queue és table adatok a storage-fiókok a az Azure-bA. a Server Explorer hello Azure Storage csomópontban jelennek meg. a helyi storage emulator fiókja és az egyéb Azure storage-fiókok adatokat.

a Visual Studio Server Explorer tooview hello menüsávban válassza **nézet**, **Server Explorer**. hello tárolás csomópont alatt minden Azure előfizetés/tanúsítvány kapcsolódik hello tárfiókok mutatja. A tárfiók nem jelenik meg, ha egyszerűen hozzáadhatja hello utasításokat követve [a témakör későbbi részében](#add-storage-accounts-by-using-server-explorer).

Az Azure SDK 2.7-től kezdődően hello új Cloud Explorer tooview használja, és az Azure erőforrások kezeléséhez-e. Lásd: [Azure-erőforrások kezelése Cloud Explorer](vs-azure-tools-resources-managing-with-cloud-explorer.md) további információt.

## <a name="view-and-manage-storage-resources-in-visual-studio"></a>Megtekintheti, és a Visual Studio tárolási erőforrások kezelése
Server Explorer automatikusan emulátor a tárfiókban lévő blobokat, üzenetsorokat és táblákat listája látható. hello emulátor tárfiók megtalálható a Server Explorer-e hello tárolási csomópontnak, hello **fejlesztési** csomópont.

toosee hello emulátor tárfiók erőforrásokat, bontsa ki a hello **fejlesztési** csomópont. Ha hello storage emulator még nem indult el hello kibővítésekor **fejlesztési** csomópont, akkor automatikusan elindul. Ez eltarthat néhány másodpercig. Egyéb területein a Visual Studio toowork folytatható, amíg hello storage emulator elindul.

a tárfiók erőforrásainak tooview bontsa ki a Server Explorer hello tárolási fiók csomópontot. a következő alárendelt csomópontok hello jelennek meg:

* Blobok
* Üzenetsorok
* Táblák

## <a name="work-with-blob-resources"></a>A Blob erőforrásokat
hello Blobok csomópont hello kiválasztott tárfiók tárolók listáját jeleníti meg. BLOB tárolók blob fájlokat tartalmazza, és ezek a blobok rendezze mappákban és almappáiban. Lásd: [hogyan toouse Blob Storage a .NET-](storage/blobs/storage-dotnet-how-to-use-blobs.md) további információt.

### <a name="toocreate-a-blob-container"></a>a blob-tároló toocreate
1. Nyissa meg hello helyi menüje hello **Blobok** csomópont, és válassza a **Blob-tároló létrehozása**.
2. A hello **Blob-tároló létrehozása** párbeszédpanelen adja meg az új tároló hello hello neve.  
3. Nyomja le az **ENTER** a billentyűzet vagy az Ön is kattintson vagy koppintson hello neve mező toosave hello blob tároló kívül.
   
   > [!NOTE]
   > hello blobtároló neve egy szám (0-9) vagy kisbetűt (a – z) kell kezdődnie.
   > 
   > 

### <a name="toodelete-a-blob-container"></a>a blob-tároló toodelete
* Helyi menü megnyitása hello hello blob tároló tooremove szeretne, és válassza a **törlése**.

### <a name="toodisplay-a-list-of-hello-items-contained-in-a-blob-container"></a>toodisplay a blobtárolóban található hello elemek listája
* Nyissa meg a blob-tároló neve hello helyi menüje hello listában, és válassza a **nyitott**.
  
    A blob tároló tartalmának hello megtekintésekor egy hello blob tároló nézet néven lapján jelenik meg.
  
    ![VST_SE_BlobDesigner](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC749016.png)
  
    Hello követő műveleteket a blobok hello jobb felső sarkában hello blob tároló nézet hello gombok használatával végezheti el:
  
  * Adjon meg egy szűrő értéket, és alkalmazza azt
  * Hello tárolóban lévő blobok hello listájának frissítése
  * Fájl feltöltése
  * Blob törlése
    
    > [!NOTE]
    > A fájl egy blob-tárolóból nem törlése hello alapul szolgáló fájlt. Ez csak eltávolítja hello blob tároló.
    > 
    > 
  * Nyissa meg a blob
  * Mentse a blob toohello helyi számítógépről

### <a name="toocreate-a-folder-or-subfolder-in-a-blob-container"></a>toocreate egy mappát vagy almappát blob tároló
1. A Cloud Explorer hello blob tároló kiválasztása Hello tároló ablakban válassza ki a hello **Blob feltöltése** gombra.
   
    ![A fájl feltöltése a blob mappába](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766037.png)
2. A hello **új fájl feltöltése** párbeszédpanelen válassza ki a hello **keresse meg** gomb toospecify hello kívánt fájlt, tooupload, majd adja meg a mappa nevét a hello **mappa (nem kötelező)** mezőbe .
   
    A következő tároló mappa almappákat hello azonos is hozzáadhat eljárást. Ha nem adja meg a mappa nevét, hello fájl lesz feltöltve toohello legfelső szintű hello blob tároló. hello fájl hello tárolóban hello megadott mappában jelenik meg.
   
    ![Fel tooa blob tároló mappa](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766038.png)
3. Kattintson duplán a hello mappát, vagy nyomja le az ENTER toosee hello hello mappa tartalmát. Ha éppen hello tároló mappa, hátsó toohello legfelső szintű hello tároló navigálhat hello kiválasztásával **nyitott Szülőkönyvtárában** (felfelé mutató nyíl) gombra.

### <a name="toodelete-a-container-folder"></a>toodelete tároló mappa
* Törölje az összes hello mappában levő fájlok hello
  
  > [!NOTE]
  > Mivel a blob tárolók mappák virtuális mappák, üres mappa nem hozható létre, és nem is töröl egy mappát toodelete fájl tartalmát. Lehetősége van toodelete hello teljes tartalma egy toodelete hello mappáját.
  > 
  > 

### <a name="toofilter-blobs-in-a-container"></a>a tárolóban lévő blobok toofilter
Szűrheti a hello blobot, amely megjeleníti azokat a közös előtag megadása.

Például, ha hello előtag meg `hello` a hello szűrő szövegmezőbe, és válassza a hello **Execute** (**!**) gomb, csak a "hello" karakterrel kezdődő blobok jelenik meg.

![VST_SE_FilterBlobs](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC519076.png)

> [!NOTE]
> hello szűrőmező kis-és nagybetűket, és nem támogatja a helyettesítő karaktereket is tartalmazó szűrést. Blobok csak előtag alapján szűrhetők. hello előtag tartalmazhatják elválasztó használata egy elválasztó tooorganize blobok virtuális hierarchiába. Például a szűrés előtag HelloFabric hello / adja vissza az összes BLOB karakterláncokat kezdve.
> 
> 

### <a name="toodownload-blob-data"></a>toodownload Blobadatok
* A **Cloud Explorer**, nyissa meg egy vagy több blobot hello helyi menüt, és válassza **nyissa meg a**, vagy válasszon hello blob nevet, és válassza a hello **nyissa meg a** gombra, vagy kettős kattintással hello blob neve.
  
    megjelenik egy blob letöltés előrehaladását hello hello **Azure tevékenységnapló** ablak.
  
    hello blob az adott fájltípus hello alapértelmezett-szerkesztő megnyitása. Ha hello operációs rendszer hello fájltípus felismeri, hello nyílik meg a helyileg telepített alkalmazások; Ellenkező esetben felkéri toochoose hello blob típusú hello fájl megfelelő alkalmazás. egy blob letöltésekor létrehozott hello helyi fájl csak olvashatóként van megjelölve.
  
    BLOB adatok helyben és összeveti hello blob utolsó módosítási időpontjának a Blob szolgáltatás hello. Ha hello blob óta utolsó letöltési frissítették, hogy újból letöltődnek; Ellenkező esetben hello blob innen lesz betöltve: hello helyi lemez. Alapértelmezés szerint a blob az letöltött tooa ideiglenes könyvtár. toodownload blobok tooa adott címtárhoz, nyissa meg hello helyi menüje kijelölt hello blob-nevek, és válassza a **Mentés másként**. Az ilyen módon blob mentésekor hello blob fájl nincs megnyitva, és hello helyi fájl írható-olvasható attribútumokkal jön létre.

### <a name="tooupload-blobs"></a>tooupload blobok
* Válassza ki a hello **Blob feltöltése** gomb, hello tároló meg nyitva hello blob tároló nézetben megtekinthető.
  
    Válasszon egyet, vagy további fájlok tooupload, és feltöltheti bármilyen típusú fájlokat. Hello **Azure tevékenységnapló** mutat be hello hello feltöltés előrehaladását. További információ a blobadatokat, toowork lásd: [hogyan toouse hello Azure Blob Storage szolgáltatást a .NET](http://go.microsoft.com/fwlink/p/?LinkId=267911).

### <a name="tooview-logs-transferred-tooblobs"></a>tooview naplók átvitt tooblobs
* Ha Azure Diagnostics toolog adatokat az Azure-alkalmazás használ, és amelyre átmásolta a naplók tooyour tárfiókot, láthatja a tárolók, ezek a naplók az Azure által létrehozott. Ezek a naplók megtekintéséhez a Server Explorer egy egyszerűen tooidentify problémák az alkalmazással, különösen akkor, ha a telepített tooAzure lett. Azure Diagnostics kapcsolatos további információkért lásd: [naplózási adatok gyűjtése Azure Diagnostics használatával](https://msdn.microsoft.com/library/azure/gg433048.aspx).

### <a name="tooget-hello-url-for-a-blob"></a>a BLOB tooget hello URL-címe
* Hello blob helyi menü megnyitásához, és válassza a **URL-CÍMÉT**.

### <a name="tooedit-a-blob"></a>a blob tooedit
* Válassza ki a hello blob, és válassza a hello **nyissa meg a Blob** gombra.
  
    hello fájl letöltött tooa ideiglenes helyre, és megnyitott hello helyi számítógépen. Módosítása után újra kell tölteni a hello blob.

## <a name="work-with-queue-resources"></a>A várólista erőforrásokat
Egy Azure storage-fiókban tárolt szolgáltatások tárüzenetsort és használhatja őket tooallow a felhőalapú szolgáltatás szerepkörök toocommunicate, egymással, és más szolgáltatások által üzenettovábbítási mechanizmus. Van-e hozzáférési hello várólista programozott módon keresztül egy felhőalapú szolgáltatás, és a külső ügyfelek webszolgáltatáson keresztül. Hello várólista közvetlenül a Visual Studio Server Explorer használatával is elérhető.

Egy felhőalapú szolgáltatás által használt várólisták fejlesztésekor előfordulhat, hogy szeretné, hogy toouse Visual Studio toocreate várólisták, és azokat interaktív használatához közben az Ön által fejlesztett és tesztelheti a kódját.

A Server Explorer eszközben, megtekintheti hello várólisták tárfiókokban, hozzon létre és Várólisták törlése, nyissa meg a várólista tooview az üzeneteket, és adja hozzá az üzenetek tooa várólista. Egy várólista megtekintésre megnyitásakor megtekintheti az egyes köszönőüzenetei, és hello követő műveletek hello várólista hello bal felső sarokban hello gombok használatával végezheti el:

* Hello várólista hello nézet frissítése
* Egy üzenetsor toohello hozzáadása
* Created legfelső köszönőüzenetei
* Törölje a jelet hello teljes várólista

a következő kép hello két üzeneteket tartalmaz várólista jeleníti meg.

![A várólista megtekintéséhez](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC651470.png)

További információ a tárolási szolgáltatási várólisták létrehozásáról: [hogyan: használata hello Queue Storage szolgáltatás](http://go.microsoft.com/fwlink/?LinkID=264702). Hello webszolgáltatás tárolási szolgáltatási várólisták, lásd: [a Queue szolgáltatás alapfogalmai](http://go.microsoft.com/fwlink/?LinkId=264788). Hogyan toosend üzenetek tooa tárolási szolgáltatásainak várólista Visual Studio használatával kapcsolatos információkért lásd: [üzenetek küldésekor tooa tárolási szolgáltatások várólista](https://msdn.microsoft.com/library/azure/jj649344.aspx).

> [!NOTE]
> A service bus-üzenetsorok elkülönülnek a tárolási szolgáltatások sorok. Service bus-üzenetsorok kapcsolatos további információkért tekintse meg a Service Bus-üzenetsorok, témakörök és előfizetések.
> 
> 

## <a name="work-with-table-resources"></a>A tábla erőforrásokat
hello Azure Table storage szolgáltatást a strukturált adatok nagy mennyiségű tárolja. hello szolgáltatás áll, egy NoSQL-adattár, amely elfogadja érkező hitelesített hívásokat belüli és kívüli hello Azure felhőben. Az Azure-táblák strukturált, nem relációs adatok tárolására alkalmasak.

### <a name="toocreate-a-table"></a>egy tábla toocreate
1. A Cloud Explorerben válassza ki a hello **táblák** hello tárhely csomópontot fiókot, és válassza a **Create Table**.
2. A hello **Create Table** párbeszédpanelen adja meg a hello tábla nevét.

### <a name="tooview-table-data"></a>tooview tábla adatai
1. A Cloud Explorerben nyissa meg a hello **Azure** csomópontját, és ezután nyissa meg hello **tárolási** csomópont.
2. Nyissa meg hello tárolási fiók csomópontnak, hogy érdekli, és nyissa meg a hello **táblák** csomópont toosee hello tárfiók táblák listáját.
3. Egy tábla hello helyi menü megnyitásához, és válassza a **nézet tábla**.
   
    ![Egy Azure-tábla a Megoldáskezelőben](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744165.png)

hello tábla entitásokat (látható sorok) és a Tulajdonságok (látható oszlopok) szerint van rendezve. Például a következő ábra hello látható entitások hello felsorolt **tábla Designer**:

### <a name="tooedit-table-data"></a>tooedit tábla adatai
1. A hello **tábla Designer**, nyissa meg a hello helyi menü egy entitás (egy sorban) vagy a tulajdonsághoz (egy cellát), és válassza a **szerkesztése**.
   
    ![Hozzáadása vagy módosítása egy tábla entitás](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC656238.png)
   
    Egyetlen tábla az entitások nem szükséges toohave hello ugyanaz a Tulajdonságok (oszlop) készletét. Ne feledje hello korlátozások tábla adatok megtekintése és módosítása a következő.
   
   * Nem tekinthetők meg és a bináris adatok szerkesztéséhez (típus byte[]), de érdemes egy táblázatban.
   * Nem lehet szerkeszteni hello **PartitionKey** vagy **RowKey** érték található, mivel a table storage Azure-ban Ez a művelet nem támogatja.
   * Nem hozható létre a tulajdonságot, Timestamp, Azure Storage szolgáltatás tulajdonsággal egy ezen a néven.
   * Egy dátum/idő értéket ad meg, ha a számítógép területi és nyelvi beállítások megfelelő toohello formátuma kell követnie (például hh/nn/éééé óó: pp: [AM |} PM] az USA Angol nyelvű).

### <a name="tooadd-entities"></a>tooadd entitások
1. A hello **tábla Designer**, válassza ki a hello **entitás hozzáadása** gombra.
   
    ![Entitás hozzáadása](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655336.png)
2. A hello **entitás hozzáadása** párbeszédpanelen adja meg a hello hello értékének **PartitionKey** és **RowKey** tulajdonságok.
   
    ![Entitás párbeszédpanel hozzáadása](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655335.png)
   
    Adja meg gondosan hello értékeket már nem módosíthatók őket, kivéve, ha törölni hello entitást, és vegye fel újra hello párbeszédpanel bezárása után.

### <a name="toofilter-entities"></a>toofilter entitások
Testre szabhatja a hello Lekérdezésszerkesztő használatakor a táblázatban látható entitások hello készletét.

1. tooopen hello Lekérdezésszerkesztő megnyitni egy táblát a megtekintéshez.
2. Válassza ki a hello Lekérdezésszerkesztő gombjára hello tábla nézet.
   
    Hello **Lekérdezésszerkesztő** párbeszédpanel jelenik meg. hello alábbi ábrán egy lekérdezést, amely éppen készül hello Lekérdezésszerkesztőben.
   
    ![Lekérdezés-szerkesztő](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC652231.png)
3. Ha elkészült hello lekérdezés felépítése, hello párbeszédpanel bezárásához. hello megjelenő szöveg képernyőn hello lekérdezés a WCF Data Services szűrő beviteli mezőben jelenik meg.
4. toorun hello lekérdezéséhez hello zöld háromszögnek ikonja.
   
    Szűrhető, hogy megjelenik-hello Entitásadatok **tábla Designer** Ha közvetlenül hello szűrő mezőben adja meg a WCF Data Services szűrési karakterláncot. A karakterlánc típusú hasonló tooa SQL WHERE záradék, de legyen elküldve a toohello kiszolgáló, egy HTTP-kérelem. További információ a hogyan tooconstruct szűrése karakterláncok: [Szűrőkarakterláncokban hozhat létre a tábla Designer hello](https://msdn.microsoft.com/library/azure/ff683669.aspx).
   
    hello alábbi ábrán egy példa egy érvényes szűrési karakterláncot:
   
    ![VST_SE_TableFilter](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655337.png)

### <a name="refresh-storage-data"></a>Tárolási adatok frissítése
Amikor Server Explorer tooor lekérdezi adatok csatlakozik a tárfiókból, hello művelet toocomplete tooa perces másolatot is eltarthat. Ha nem tud kapcsolódni, hello művelet lehet, hogy túllépi az időkorlátot. Adatainak lekérése közben folytathatja a toowork a Visual Studio többi részével. Ha túl sokáig tart toocancel hello művelet kiválasztása hello **frissítés leállítása** hello Server Explorer gombjára.

#### <a name="toorefresh-blob-container-data"></a>toorefresh blob tároló adatok
* Jelölje be hello **Blobok** csomóponton sem **tárolási** hello válassza **frissítése** hello Server Explorer gombjára.
* toorefresh hello blobok megjelenő listában, válasszon hello **Execute** gombra.

#### <a name="toorefresh-table-data"></a>toorefresh tábla adatai
* Jelölje be hello **táblák** csomóponton sem **tárolási** hello válassza **frissítése** gombra.
* toorefresh hello entitások listájának hello megjelenő **tábla Designer**, válassza ki a hello **Execute** hello gombjára **tábla Designer**.

#### <a name="toorefresh-queue-data"></a>toorefresh várólista adatok
* SELECT hello **várólisták** csomópontot, majd válassza a hello **frissítése** gombra.

#### <a name="toorefresh-all-items-in-a-storage-account"></a>az összes tárfiók elem toorefresh
* Válassza ki a hello fiók nevét, és válassza a hello **frissítése** a Server Explorer hello gombjára.

### <a name="add-storage-accounts-by-using-server-explorer"></a>A Server Explorer storage-fiókok hozzáadása
Két módon tooadd tárfiókok Server Explorer használatával. Létrehozhat egy új tárfiókot az Azure-előfizetése, vagy csatolhat a meglévő tárfiókot.

#### <a name="toocreate-a-new-storage-account-by-using-server-explorer"></a>egy új tárfiókot, a Server Explorer toocreate
1. A Server Explorer eszközben hello tárolási csomópont hello helyi menü megnyitásához, és válassza a Storage-fiók létrehozása.
   
    ![Új Azure storage-fiók létrehozása](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744166.png)
2. Válassza ki vagy adja meg a következő információ hello hello az új tárfiók hello **Storage-fiók létrehozása** párbeszédpanel megnyitásához.
   
   * hello Azure-előfizetés toowhich tooadd hello tárfiókot szeretne.
   * hello nevet toouse hello új tárfiók.
   * hello régiót vagy affinitáscsoportot (például az USA nyugati régiója vagy Kelet-Ázsia).
   * hello típus replikációs hello tárfiók, például a Georedundáns kívánt toouse.
3. Válassza a **Létrehozás** elemet.
   
    új tárfiók hello hello megjelenik **tárolási** lista a Megoldáskezelőben.

#### <a name="tooattach-an-existing-storage-account-by-using-server-explorer"></a>a Server Explorer meglévő tárfiók tooattach
1. A Server Explorer eszközben hello az Azure storage csomópont hello helyi menü megnyitásához, és válassza **külső tárterület csatolása**.
   
    ![Meglévő tárfiók hozzáadása](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766039.png)
2. Válassza ki vagy adja meg a következő információ hello hello az új tárfiók hello **Storage-fiók létrehozása** párbeszédpanel megnyitásához.
   
   * hello meglévő tárfiók tooattach kívánt hello neve. Adjon meg egy nevet, vagy válassza ki a hello listából.
   * hello hello kulcsa kiválasztott tárfiók. Ez az érték általában biztosított, válasszon egy tárfiókot. Ha azt szeretné, hogy a Visual Studio tooremember hello tárfiók kulcsa, válassza a hello tárolása fiók kulcs mezőben.
   * hello protokoll toouse tooconnect toohello tárfiókot, például HTTP, HTTPS vagy egy egyéni végpontot. Lásd: [hogyan tooConfigure kapcsolódási karakterláncokat](https://msdn.microsoft.com/library/azure/ee758697.aspx) egyéni végpontokat további információt.

### <a name="tooview-hello-secondary-endpoints"></a>tooview hello másodlagos végpontok
* Ha létrehozott egy tárfiókot, hello segítségével **írásvédett Georedundáns** replikációs beállítás, megtekintheti a másodlagos végpontját. Nyissa meg a helyi menüje hello hello fiók nevét, és válassza **tulajdonságok**.
  
    ![Tárolási másodlagos végpontok](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766040.png)

### <a name="tooremove-a-storage-account-from-server-explorer"></a>a storage-fiókok a Server Explorer tooremove
* A Server Explorer eszközben hello nevének hello helyi menü megnyitásához, és válassza **törlése**. Ha törli a tárfiókot, minden mentett kulcsadatokat fiók is törlődnek.
  
  > [!NOTE]
  > A Server Explorer a storage-fiók törlése esetén nem változik a tárfiók vagy adatokat tartalmaz; a Server Explorer hello hivatkozás egyszerűen eltávolítja. toopermanently tárfiók törléséhez használja a hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885).
  > 
  > 

## <a name="next-steps"></a>Következő lépések
További tudnivalók a toolearn az Azure storage szolgáltatásainak használatába, lásd: [hello Azure tárolási szolgáltatások eléréséhez használt](https://msdn.microsoft.com/library/azure/ee405490.aspx).

