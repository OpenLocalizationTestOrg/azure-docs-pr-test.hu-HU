---
cím: aaa "Azure Analysis Services-oktatóanyag lecke 1: hozzon létre egy új táblázatos modell projektet |} Microsoft Docs"Leírás: ismerteti, hogyan toocreate egy új Azure Analysis Services-oktatóanyag projekt. szolgáltatások: analysis-szolgáltatások documentationcenter: "Szerző: minewiskan manager: erikre szerkesztőben:" címkék: "

MS.AssetId: ms.service: analysis-szolgáltatások ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 06/01/2017 ms.author: owend
---
# <a name="lesson-1-create-a-tabular-model-project"></a>1. lecke: Táblázatosmodell-projekt létrehozása

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Ez a lecke használja a SQL Server Data Tools (SSDT) toocreate egy új táblázatos jelentésmodell-projekt hello 1 400 kompatibilitási szintet. A projekt létrehozása után megkezdheti az adatok hozzáadását és a modell létrehozását. Ez a lecke is lehetővé teszi a rövid bevezető toohello táblázatos modellek szerzői környezetben SSDT-ben.  
  
Becsült idő toocomplete Ez a lecke: **10 perc**  
  
## <a name="prerequisites"></a>Előfeltételek  
Ez a témakör első lecke hello szerzői útmutató egy táblázatos modell képezi. toocomplete Ez rész, toohave helyben kell több előfeltételnek. több, lásd: toolearn [Azure Analysis Services - oktatóanyag az Adventure Works](../tutorials/aas-adventure-works-tutorial.md).  
  
## <a name="create-a-new-tabular-model-project"></a>Új táblázatosmodell-projekt létrehozása  
  
#### <a name="toocreate-a-new-tabular-model-project"></a>egy új táblázatos jelentésmodell-projekt toocreate  
  
1.  Az SSDT a hello **fájl** menüben kattintson a **új** > **projekt**.  
  
2.  A hello **új projekt** párbeszédpanelen bontsa ki **telepített** > **üzleti intelligencia** > **Analysis Services**, és kattintson a **Analysis Services rendszerbeli táblázatos projekt**.  
  
3.  A **neve**, típus **AW Internet értékesítési**, és hello projektfájlok helyét adja meg.  
  
    Alapértelmezés szerint **megoldásnév** van hello azonos hello projekt neve; azonban beírhatja egy másik megoldás nevét.  
  
4.  Kattintson az **OK** gombra.  
  
5.  A hello **táblázatos modellek tervezőjében** párbeszédpanelen jelölje ki **integrált munkaterület**.  
  
    hello munkaterület azonos nevet hello projektként modell létrehozási hello rendelkező táblázatos modell adatbázist üzemelteti. Integrált munkaterület azt jelenti, hogy az SSDT beépített példányt használ, hello kell tooinstall egy külön Analysis Services kiszolgálópéldányhoz csak tartalomkészítéshez modell kiküszöbölése.
      
6.  A **Kompatibilitási szint** mezőben válassza az **SQL Server 2017 / Azure Analysis Services (1400)** lehetőséget.   
 
    ![aas-lesson1-tmd](../tutorials/media/aas-lesson1-tmd.png)
      
    SQL Server 2017 / Azure Analysis Services (1 400) hello kompatibilitási szint listbox nem jelenik meg, ha nem használja az SQL Server Data Tools hello legújabb verzióját. tooget hello legújabb verziójára, lásd: [telepítse az SQL Server-adatok eszközök](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).  
      
  
## <a name="understanding-hello-ssdt-tabular-model-authoring-environment"></a>Hello SSDT táblázatos modell szerzői környezetben ismertetése  
Most, hogy létrehozott egy új táblázatos jelentésmodell-projekt, vessen egy kicsit tooexplore hello táblázatos modell szerzői környezetben SSDT-ben.  
  
A létrehozott projekt megnyílik az SSDT-ben. A jobb oldalsó, a hello **táblázatos modell Explorer**, megjelenik egy fa nézet hello objektumok a modellben. Még nem még importálta az adatokat, mert hello mappák nincsenek megadva. Kattintson a jobb egérgombbal egy objektum mappa tooperform műveletek, hasonló toohello menüsáv. Ez az oktatóanyag lépéseit, mert a jelentésmodell-projekt hello táblázatos modell Explorer toonavigate különböző objektumok használható.

![aas-lesson1-tme](../tutorials/media/aas-lesson1-tme.png)

Kattintson a hello **Megoldáskezelőben** fülre. Itt láthatja a **Model.bim** fájlt. Ha nem jelenik meg hello Tervező ablak toohello balra (hello üres ablakban hello Model.bim lap) **Megoldáskezelőben**, a **AW Internet értékesítési projekt**, kattintson duplán a hello  **Model.bim** fájlt. hello Model.bim fájl hello metaadatait a jelentésmodell-projekt tartalmazza. 

![aas-lesson1-se](../tutorials/media/aas-lesson1-se.png)
  
Kattintson a **Model.bim** fájlra. A hello **tulajdonságok** ablakban megjelennek hello modell tulajdonságai, legfontosabb, ami hello **DirectQuery módban** tulajdonság. Ez a tulajdonság határozza meg, ha hello modell memórián belüli mód (ki) vagy DirectQuery módban (a) van-e telepítve. A jelen oktatóanyagban Memóriában tárolt módban fogja létrehozni és üzembe helyezni a modellt.

![aas-lesson1-properties](../tutorials/media/aas-lesson1-properties.png)
  
A jelentésmodell-projekt létrehozásakor egyes modell tulajdonságai vannak beállítva, toohello adatok modellezési hello megadható beállítások szerint automatikusan **eszközök** menü > **beállítások** párbeszédpanel megnyitásához. Az adatok biztonsági mentése, a munkaterület megőrzési és a munkaterület-kiszolgáló adja meg, hogyan és hol hello a munkaterület-adatbázis (a authoring database modell) biztonsági mentése, memóriában tartott és beépített. Szükség esetén később módosíthatja ezeket a beállításokat, de egyelőre hagyja őket változatlanul.  

A **Megoldáskezelő** területén kattintson a jobb gombbal az **AW internetes értékesítés** projektre, majd kattintson a **Tulajdonságok** elemre. Hello **AW Internet értékesítési tulajdonságlapjain** párbeszédpanel jelenik meg. Néhány tulajdonságot ezek közül a modell üzembe helyezése során adhat meg.  
  
Amikor telepítette az SSDT, számos új menüelemek toohello Visual Studio környezet lettek hozzáadva. Kattintson a hello **modell** menü. Itt adatimportálás, frissítheti az adatai, keresse meg a modellnek az Excel programban, szempontok és a szerepkörök, jelölje be hello modell nézet, hozzon létre és állítsa be a számítási beállítások módosításához. Kattintson a hello **tábla** menü. Itt kapcsolatokat hozhat létre és kezelhet, megadhatja a dátumtáblázat beállításait, partíciókat hozhat létre és szerkesztheti a tábla beállításait. Ha hello **oszlop** menü hozzáadása és törlése a táblázatban levő oszlopkészleteket, oszlopok rögzítése, és adja meg a rendezési sorrend. Az SSDT is rendelkezik néhány gombok toohello sáv. A leghasznosabb hello AutoSzum funkció toocreate egy szabványos összesítési mértéket a kijelölt oszlop. Más gombok segítségével a gyors elérés érdekében toofrequently használt funkciók és parancsok.  
  
Felfedezheti hello párbeszédpanelek és helyek különböző szolgáltatások adott tooauthoring táblázatos modellek esetén. Bizonyos elemek még nem aktív, amíg kaphat hello táblázatos modell szerzői környezetben hasznos.  
  

## <a name="whats-next"></a>A következő lépések
[2. lecke: Az adatok beszerzése](../tutorials/aas-lesson-2-get-data.md).

  
  
  
