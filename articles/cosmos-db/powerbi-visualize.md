---
title: "Azure Cosmos DB összekötő aaaPower BI oktatóanyaga |} Microsoft Docs"
description: "Használja a Power BI oktatóanyag tooimport JSON osztályon jelentések létrehozása és hello Azure Cosmos DB és a Power BI-összekötővel adatainak megjelenítése."
keywords: "üzleti intelligencia oktatóanyag Power, az adatok, a kiemelt üzleti intelligencia összekötő megjelenítése"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: cd1b7f70-ef99-40b7-ab1c-f5f3e97641f7
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: mimig
ms.openlocfilehash: ca0bb8b76db8ef2ec936722b682af6a9488a3501
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="power-bi-tutorial-for-azure-cosmos-db-visualize-data-using-hello-power-bi-connector"></a>A Power BI oktatóanyag az Azure Cosmos DB: hello Power BI-összekötővel adatok megjelenítése
[Powerbi.com webhelyen](https://powerbi.microsoft.com/) egy olyan online szolgáltatás, amelyben hozhat létre, és irányítópultokat és jelentéseket megosztása a fontos tooyou és a szervezet adatokat.  A Power BI Desktop dedikált jelentés eszköz, amely lehetővé teszi a szerzői tooretrieve adatait a különféle adatforrásokból származó, egyesítése és hello adatok átalakítása, hatékony jelentések és a képi megjelenítés létrehozása és közzététele hello jelentések tooPower BI.  Hello Power BI Desktop legújabb verziójával most már a Power BI is elérheti tooyour Cosmos DB fiók hello Cosmos DB összekötőn keresztül.   

A Power bi-ban az oktatóanyagban a Microsoft hello lépéseket tooconnect tooa Cosmos DB fiókja a Power BI Desktop bízná, keresse meg a tooa gyűjtemény, ahol azt szeretnénk, hogy tooextract hello adatok hello Navigator, átalakító JSON-adatokat a Power BI Desktop lekérdezés használata táblázatos formátumra alakítja Szerkesztő, létrehozása és közzététele egy jelentéskészítő tooPowerBI.com.

A Power BI-oktatóanyag befejezése után be fog tudni tooanswer hello a következő kérdéseket:  

* Hogyan készíthető jelentések adatokkal a Cosmos-Adatbázisból a Power BI Desktop használatával?
* Hogyan kapcsolódhatnak a Power BI Desktop tooa Cosmos DB fiókot?
* Hogyan lehet lekérni a Power BI Desktop egy gyűjteményből adatok?
* Hogyan alakíthatja át a beágyazott JSON-adatokat a Power BI Desktop?
* Hogyan közzététele és megosztása a jelentést a powerbi.com webhelyen?

> [!NOTE]
> a Power BI összekötő hello Azure Cosmos DB tooPower BI Desktop kivonása és az adatok átalakítása csatlakozik. A Power BI Desktopban létrehozott jelentések majd lehet közzétett tooPowerBI.com. Közvetlen kivonása és Azure Cosmos DB adatok átalakítása nem hajtható végre a powerbi.com webhelyen. 

## <a name="prerequisites"></a>Előfeltételek
Előtt hello utasításai a Power BI-oktatóanyag, győződjön meg arról, hogy rendelkezik-e a következő erőforrások hozzáférés toohello:

* [a Power BI Desktop legújabb verziójának hello](https://powerbi.microsoft.com/desktop).
* Hozzáférési tooour bemutató fiók vagy Cosmos DB fiókban tárolt adatok.
  * hello bemutató fiók fel van töltve, ebben az oktatóanyagban szereplő hello mexikói adatokkal. Ez a bemutató fiók bármely SLA-k nem kötődik, és csak bemutató célokra tervezték.  Azt lefoglalni hello jobb toomake módosítások toothis bemutató fiók többek között, de nem kizárólagosan a, leállítja hello fiók hello kulcs történő hozzáférés, módosítása, módosítása és a hello adatok törlése, bármikor előzetes közlemény, illetve ok nélkül.
    * URL-cím: https://analytics.documents.azure.com
    * Csak olvasható kulcs: MSr6kt7Gn0YRQbjd6RbTnTt7VHc5ohaAFu7osF0HdyQmfR + YhwCH2D2jcczVIR1LNK3nMPNBD31losN7lQ/fkw ==
  * Vagy toocreate saját fiókját, lásd: [hozzon létre egy Azure Cosmos DB adatbázisfiókot hello Azure-portálon](https://azure.microsoft.com/documentation/articles/create-account/). Ezt a tooget mexikói mintaadatok, amely hasonló toowhat ebben az oktatóanyagban használt (de nem tartalmaz hello GeoJSON blokkok) című hello [NOAA hely](https://www.ngdc.noaa.gov/nndc/struts/form?t=102557&s=5&d=5) majd importálja az adatokat hello hello [Azure Cosmos DB adatok áttelepítése eszköz](import-data.md).

tooshare a jelentést a powerbi.com webhelyen, a powerbi.com webhelyen fiókkal kell rendelkeznie.  További információ a Power BI szabad és a Power BI Pro toolearn látogasson el [https://powerbi.microsoft.com/pricing](https://powerbi.microsoft.com/pricing).

## <a name="lets-get-started"></a>Első lépések
Ebben az oktatóanyagban most tegyük fel, hogy Ön egy hello világ vulkánok alakították tanulmányozása geologist.  hello mexikói adatai egy Cosmos DB fiókot, és a következő minta dokumentum hello tűnik hello JSON-dokumentumokat.

    {
        "Volcano Name": "Rainier",
           "Country": "United States",
          "Region": "US-Washington",
          "Location": {
            "type": "Point",
            "coordinates": [
              -121.758,
              46.87
            ]
          },
          "Elevation": 4392,
          "Type": "Stratovolcano",
          "Status": "Dendrochronology",
          "Last Known Eruption": "Last known eruption from 1800-1899, inclusive"
    }

Érdemes tooretrieve hello mexikói adatait hello Cosmos DB fiókot, és a például a következő jelentés hello interaktív Power BI-jelentés adatainak megjelenítése.

![Az oktatóprogram a Power BI hello Power BI-összekötőjével, a Power BI Desktop mexikói jelentés hello képes toovisualize adatok lesz](./media/powerbi-visualize/power_bi_connector_pbireportfinal.png)

Készen áll a toogive egy try it? Lássunk neki.

1. A Power BI Desktop futtassa a munkaállomáson.
2. A Power BI Desktop eszköztárat, egy *üdvözlő* képernyő jelenik meg.
   
    ![A Power BI Desktop üdvözlőképernyő - Power BI connector](./media/powerbi-visualize/power_bi_connector_welcome.png)
3. Is **adatok beolvasása**, lásd: **források**, vagy **megnyitható más jelentések** közvetlenül a hello *üdvözlő* képernyő.  Kattintson a hello X hello jobb felső sarokban található tooclose hello képernyőn. Hello **jelentés** ábrázolása a Power BI Desktop jelenik meg.
   
    ![A Power BI Desktop jelentés nézet – Power BI connector](./media/powerbi-visualize/power_bi_connector_pbireportview.png)
4. Jelölje be hello **Home** menüszalag, majd kattintson a **adatok beolvasása**.  Hello **adatok beolvasása** ablak meg kell jelennie.
5. Kattintson a **Azure**, jelölje be **Microsoft Azure DocumentDB (béta)**, és kattintson a **Connect**. 

    ![A Power BI Desktop - adatok beolvasása a Power BI connector](./media/powerbi-visualize/power_bi_connector_pbigetdata.png)   
6. A hello **Preview összekötő** kattintson **Folytatás**. Hello **Microsoft Azure DocumentDB Connect** ablak jelenik meg.
7. Adja meg a hello Cosmos DB fiók végpont URL-címe Ehhez tooretrieve hello adatait, például alább látható módon, és kattintson a **OK**. toouse egy saját fiókot le hello URI URL-cím-hello párbeszédpanel hello  **[kulcsok](manage-account.md#keys)**  panelen található hello Azure-portálon. toouse hello bemutató fiókot, adja meg `https://analytics.documents.azure.com` hello URL-címhez. 
   
    Hello adatbázis neve, a gyűjtemény nevét és az SQL-utasítás üresen hagyja, a mezők kitöltése nem kötelező.  Ehelyett használjuk hello Navigator tooselect hello adatbázis és gyűjtemény tooidentify honnan hello adatok származnak.
   
    ![A Power BI oktatóanyag Azure Cosmos DB Power BI Connector - asztali csatlakozás ablak](./media/powerbi-visualize/power_bi_connector_pbiconnectwindow.png)
8. Ha első alkalommal csatlakozik hello toothis végpontja, hello-fiókjához tartozó kéri. Saját fiók lekérdezni hello kulcs hello **elsődleges kulcs** hello párbeszédpanel  **[írásvédett kulcsok](manage-account.md#keys)**  panelen található hello Azure-portálon. Hello bemutató fiók, hello kulcsa `MSr6kt7Gn0YRQbjd6RbTnTt7VHc5ohaAFu7osF0HdyQmfR+YhwCH2D2jcczVIR1LNK3nMPNBD31losN7lQ/fkw==`. Adja meg a megfelelő hello-kulcsot, és kattintson a **Connect**.
   
    Azt javasoljuk, hogy jelentések készítésekor hello írásvédett kulcsot használja.  Ez megakadályozza a szükségtelen kitettségtől hello főkulcs toopotential biztonsági kockázatok. hello írásvédett nem áll rendelkezésre a hello [kulcsok](manage-account.md#keys) panel az Azure-portálon hello, vagy használhatja a fenti hello bemutató fiók adatait.
   
    ![A Power BI oktatóanyag Azure Cosmos DB Power BI Connector - fiókjához](./media/powerbi-visualize/power_bi_connector_pbidocumentdbkey.png)
    
    > [!NOTE] 
    > Ha a hiba, amely szerint "hello megadott adatbázis nem található." Lásd: hello megkerülő megoldás lépéseit [Power BI probléma](https://community.powerbi.com/t5/Issues/Document-DB-Power-BI/idi-p/208200).
    
9. Amikor hello fiók sikeresen csatlakozik, hello **Navigator** jelenik meg.  Hello **Navigator** hello fiókkal az adatbázisok listája megjeleníti.
10. Kattintson, és bontsa ki a hello adatbázison, ahol hello adatok hello jelentés határozza meg, ha hello bemutató fiók használata válasszon **volcanodb**.   
11. Jelölje ki, egy gyűjteményt, amely be fogja olvasni hello adatait. Hello bemutató fiók használata, válassza ki a **volcano1**.
    
    hello betekintő listája látható **rekord** elemeket.  A dokumentum egy jelenik meg egy **rekord** típus a Power bi-ban. Ehhez hasonlóan belül egy dokumentumot egy beágyazott JSON blokkot is van egy **rekord**.
    
    ![A Power BI oktatóanyag Azure Cosmos DB Power BI Connector - Navigator ablak](./media/powerbi-visualize/power_bi_connector_pbinavigator.png)
12. Kattintson a **szerkesztése** toolaunch hello Lekérdezésszerkesztő egy új ablakban tootransform hello adataiban.

## <a name="flattening-and-transforming-json-documents"></a>Egybesimítását és JSON-dokumentumok átalakítása
1. Váltás toohello Power BI Lekérdezésszerkesztő ablak, ahol hello **dokumentum** oszlop hello középső ablaktáblán.
   ![A Power BI Desktop Lekérdezésszerkesztő](./media/powerbi-visualize/power_bi_connector_pbiqueryeditor.png)
2. Kattintson a hello bővítő hello jobb oldalán hello **dokumentum** oszlop fejlécére.  a mezőket sorolja fel a hello helyi menü megjelenik.  Válassza ki például szükséges a jelentés hello mezők, mexikói neve, ország, régió, hely, jogosultságszint-emelés, típusát, állapota és utolsó tudja határozzák és kattintson **OK**.
   
    ![A Power BI-oktatóanyag Azure Cosmos DB Power BI Connector - bontsa ki a dokumentumok](./media/powerbi-visualize/power_bi_connector_pbiqueryeditorexpander.png)
3. hello center ablaktábla hello eredmény előnézete kijelölt hello mezőkkel.
   
    ![A Power BI-oktatóanyag Azure Cosmos DB Power BI Connector - eredmények Egybesimítására](./media/powerbi-visualize/power_bi_connector_pbiresultflatten.png)
4. A jelen példában hello Location tulajdonság egy GeoJSON letiltása végzését a dokumentumok.  Ahogy látja, a hely egy jelenik meg egy **rekord** a Power BI Desktop típusa.  
5. Kattintson a hello bővítő hello jobb oldalán hello hely oszlop fejlécére.  hello helyi menü típusa és a mezőben jelenik meg.  Most hello koordináták mezőben válassza ki, majd kattintson **OK**.
   
    ![A Power BI oktatóanyag Azure Cosmos DB Power BI Connector - bejegyzés](./media/powerbi-visualize/power_bi_connector_pbilocationrecord.png)
6. hello középső ablaktáblán megjelenik koordináták oszlop **lista** típusa.  Amint hello oktatóanyag hello elején, hello GeoJSON-adatokat ebben az oktatóanyagban pont típusa nem hello koordináták tömb rögzített szélességi és hosszúsági értékeket.
   
    hello koordináták [0] elem hosszúság jelöli, amíg koordináták [1] szélesség jelöli.
    ![A Power BI oktatóanyag Azure Cosmos DB Power BI Connector - koordináták listája](./media/powerbi-visualize/power_bi_connector_pbiresultflattenlist.png)
7. tooflatten hello koordinálja a tömb, létre fogunk hozni egy **egyéni oszlop** LatLong nevezik.  Jelölje be hello **oszlop hozzáadása** , majd kattintson a menüszalag **egyéni oszlop hozzáadása**.  Hello **egyéni oszlop hozzáadása** ablak meg kell jelennie.
8. Adja meg a hello új oszlopot, pl. LatLong nevét.
9. Ezt követően adja meg a hello egyéni képleten hello létrehozandó oszlop értékeit.  A fenti példában, azt fogja összefűzésére hello szélességi és hosszúsági értékeket vesszővel elválasztva, használja a következő képlet hello alább látható módon: `Text.From([coordinates]{1})&","&Text.From([coordinates]{0})`. Kattintson az **OK** gombra.
   
    Az adatok elemzése kifejezések (DAX) DAX-funkciók ideértve további információkért látogasson el a [a Power BI Desktop alapvető DAX](https://support.powerbi.com/knowledgebase/articles/554619-dax-basics-in-power-bi-desktop).
   
    ![A Power BI oktatóanyag Azure Cosmos DB Power BI Connector - egyéni oszlop hozzáadása](./media/powerbi-visualize/power_bi_connector_pbicustomlatlong.png)
10. Most hello középső ablaktáblán megjelenik hello hello szélesség töltődik új LatLong oszlop és a hosszúsági értékeknek vesszővel elválasztva.
    
    ![A Power BI oktatóanyag Azure Cosmos DB Power BI Connector - egyéni LatLong oszlop](./media/powerbi-visualize/power_bi_connector_pbicolumnlatlong.png)
    
    Ha hibaüzenet hello új oszlopban, győződjön meg arról, hogy a lekérdezés beállítások alkalmazása hello lépéseket egyezik-e a következő ábra hello:
    
    ![Alkalmazott lépéseket forrás, a navigációs, a dokumentum kibontva, a Document.Location kibontva, a hozzáadott egyéni kell lennie.](./media/powerbi-visualize/power-bi-applied-steps.png)
    
    A lépések eltérőek, ha törli hello további lépések szükségesek, és próbálja meg újból hozzáadni a hello egyéni oszlop. 
11. Most kitöltöttük simítási hello adatok táblázatos formátumban.  Használja ki az összes elérhető hello Lekérdezésszerkesztő tooshape hello szolgáltatás, és az adatok a Cosmos DB átalakítása.  Ha hello mintát használ, módosítsa hello adattípus jogosultsági szint emeléséhez túl**egész számot** hello módosításával **adattípus** a hello **Home** menüszalagján.
    
    ![A Power BI oktatóanyag Azure Cosmos DB Power BI Connector - oszlop típusának módosítása](./media/powerbi-visualize/power_bi_connector_pbichangetype.png)
12. Kattintson a **zárja be, és alkalmazni** toosave hello adatmodell.
    
    ![A Power BI oktatóanyag az Azure Cosmos DB Power BI connector – zárja be és alkalmazása](./media/powerbi-visualize/power_bi_connector_pbicloseapply.png)

<a id="build-the-reports"></a>
## <a name="build-hello-reports"></a>Hello jelentések létrehozása
A Power BI Desktop jelentés nézet, ahol készítése jelentések toovisualize adatok.  Jelentéseket hozhat létre húzással mezők a hello **jelentés** vászonra.

![A Power BI Desktop jelentés nézet – Power BI connector](./media/powerbi-visualize/power_bi_connector_pbireportview2.png)

A jelentések nézetének hello keresse meg:

1. Hello **mezők** ablaktáblán, ez az adatok modellek is használhatja a jelentések a mezők listája megjelenik.
2. Hello **képi megjelenítések** ablaktáblán. A jelentés egy vagy több vizuális megjelenítés tartalmazhat.  Válassza ki a hello igényeinek illeszkedő hello visual típusok **képi megjelenítések** ablaktáblán.
3. Hello **jelentés** vászonra, ahol fog létrehozni hello látványelemek a jelentés azt.
4. Hello **jelentés** lap. A Power BI Desktop jelentés több oldalra is hozzáadhat.

hello következő hello alapvető lépéseit egy egyszerű interaktív térkép jelentés megtekintése létrehozásának mutatja be.

1. A példa kedvéért létre fogunk hozni egy minden mexikói hello helyét megjelenítő nézet.  A hello **képi megjelenítések** ablaktáblán kattintson a hello térkép vizuális típusú, mint a hello fenti képernyőfelvételen kiemelt.  Megtekintheti az hello visual típusa a hello festeni **jelentés** vászonra.  Hello **képi megjelenítés** ablaktáblán is megjelenjen-e tulajdonságok kapcsolódó toohello visual típusa készlete.
2. Most áthúzása hello LatLong mező a hello **mezők** ablaktábla toohello **hely** tulajdonság **képi megjelenítések** ablaktáblán.
3. A következő áthúzása hello mexikói neve mező toohello **jelmagyarázat** tulajdonság.  
4. Ezt követően áthúzása hello jogosultságszint-emelés mező toohello **mérete** tulajdonság.  
5. Meg kell jelennie hello visual hello méretű adatok hello mexikói toohello jogosultságszint-emelés hello buborék hello minden mexikói helyét jelző buborékok készlete ábrázoló térkép.
6. Most létrehozott egy egyszerű jelentést.  További jelentés személyre szabható hello további képi megjelenítések hozzáadásával.  Ebben az esetben egy mexikói szeletelő toomake hello jelentés interaktív hozzáadott.  
   
    ![Képernyőfelvétel a hello végső Power BI Desktop jelentés befejezésekor hello Power BI oktatóanyag az Azure Cosmos DB rendszerhez](./media/powerbi-visualize/power_bi_connector_pbireportfinal.png)

## <a name="publish-and-share-your-report"></a>Közzététele és megosztása a jelentés
tooshare a jelentést a powerbi.com webhelyen fiókkal kell rendelkeznie.

1. A Power BI Desktop hello, kattintson a hello **Home** menüszalagján.
2. Kattintson a **Publish** (Közzététel) gombra.  A powerbi.com webhelyen fiók lesz felszólító tooenter hello felhasználónevet és jelszót.
3. Miután hello hitelesítő adatok hitelesítése, hello jelentés egy közzétett tooyour megadott helyre.
4. Kattintson a **nyitott "PowerBITutorial.pbix" Power BI-ban** toosee és megosztja a jelentést a powerbi.com webhelyen.
   
    ![Közzétételi tooPower BI sikeres! Nyissa meg az oktatóanyag a Power bi-ban](./media/powerbi-visualize/power_bi_connector_open_in_powerbi.png)

## <a name="create-a-dashboard-in-powerbicom"></a>Hozzon létre egy irányítópultot a powerbi.com webhelyen
Most, hogy rendelkezik egy jelentéssel, lehetővé teszi, hogy ossza meg a powerbi.com webhelyen

A jelentés a Power BI Desktop tooPowerBI.com közzétételekor létrehozott egy **jelentés** és egy **Dataset** a powerbi.com webhelyen-bérlőben. Után például a nevű jelentés közzétett **PowerBITutorial** tooPowerBI.com, PowerBITutorial megjelenő mindkét hello **jelentések** és **adatkészletek** a szakasz A powerbi.com webhelyen.

   ![Képernyőfelvétel a hello új jelentést és egy adatkészletet a powerbi.com webhelyen](./media/powerbi-visualize/powerbi-reports-datasets.png)

toocreate egy megosztható irányítópultot, kattintson a hello **PIN-kód élő oldal** a powerbi.com webhelyen jelentés gombjára.

   ![Képernyőfelvétel a hello új jelentést és egy adatkészletet a powerbi.com webhelyen](./media/powerbi-visualize/power-bi-pin-live-tile.png)

Kövesse az utasításokat hello [rögzítheti egy mozaik jelentésből](https://powerbi.microsoft.com/documentation/powerbi-service-pin-a-tile-to-a-dashboard-from-a-report/#pin-a-tile-from-a-report) toocreate új irányítópult. 

Ad hoc módosítások tooreport irányítópult létrehozása előtt is megteheti. Ajánlott azonban, hogy használja a Power BI Desktop tooperform hello módosításokat, és hello jelentés tooPowerBI.com közzé.

## <a name="refresh-data-in-powerbicom"></a>Adatok frissítése a powerbi.com webhelyre
Két módon toorefresh adatok, az ütemezett és alkalmi.

Az ad hoc frissítéshez egyszerűen kattintson a hello eclipses (...) által hello **Dataset**, pl. PowerBITutorial. Láthatja, beleértve azokat **frissítés most**. Kattintson a **frissítés most** toorefresh hello adatokat.

![Képernyőkép a frissítés most a powerbi.com webhelyen](./media/powerbi-visualize/power-bi-refresh-now.png)

Az hello az ütemezett frissítés után.

1. Kattintson a **ütemezés frissítése** hello művelet listában. 

    ![Képernyőfelvétel a hello ütemezés frissítése a powerbi.com webhelyen](./media/powerbi-visualize/power-bi-schedule-refresh.png)
2. A hello **beállítások** ablaktáblában bontsa ki a **adatforrás hitelesítő adatai**. 
3. Kattintson a **hitelesítő adatok szerkesztése**. 
   
    hello konfigurálása előugró ablak akkor jelenik meg. 
4. Hello kulcs tooconnect toohello Cosmos DB fiókot adjon meg, hogy adatkészletet, majd kattintson a **bejelentkezés**. 
5. Bontsa ki a **ütemezés frissítése** és a kívánt hello ütemezés beállítása toorefresh hello adatkészlet. 
6. Kattintson a **alkalmaz** és befejezte az ütemezett frissítés hello beállítása.

## <a name="next-steps"></a>Következő lépések
* További információ a Power BI-ban toolearn lásd [első lépések a Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/).
* toolearn Cosmos DB kapcsolatos további információkért lásd: hello [Azure Cosmos DB dokumentációjának kezdőlapján](https://azure.microsoft.com/documentation/services/documentdb/).

