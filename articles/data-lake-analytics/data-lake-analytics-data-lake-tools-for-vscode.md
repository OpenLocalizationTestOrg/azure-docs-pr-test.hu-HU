---
title: "Az Azure Data Lake Tools: Használata Azure Data Lake Tools Visual Studio Code |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure Data Lake Tools for Visual Studio Code toocreate tesztelése, és a U-SQL-parancsfájlok futtatása. "
Keywords: "VScode, az Azure Data Lake Tools, a helyi futtatáskor, helyi hibakeresése a helyi debug preview tárolófájl feltöltése toostorage elérési útja"
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/14/2017
ms.author: jejiang
ms.openlocfilehash: 77771c5d5dae3bfce4ad2df240ea6c6ef848f288
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-data-lake-tools-for-visual-studio-code"></a>Az Azure Data Lake Tools használja a Visual Studio Code

Ismerje meg, hogyan toouse Azure Data Lake Tools for Visual Studio (kód VS) toocreate, teszteléséhez, és a U-SQL-parancsfájlok futtatása. hello információkat is tartalmazza, az alábbi videó hello:

<a href="https://www.youtube.com/watch?v=J_gWuyFnaGA&feature=youtu.be"><img src="./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-video.png"></a>

## <a name="prerequisites"></a>Előfeltételek

A Data Lake Tools VS kód által támogatott hello platformokon telepíthető. hello támogatott platformok a következők: a Windows, Linux és MacOS. hello különböző platformokon a következő előfeltételek hello rendelkezik:

- Windows

    - [A Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).
    - [Java-futtatókörnyezet Ánjon 8-as verzió frissítési 77 vagy újabb](https://java.com/download/manual.jsp). Adja hozzá a hello java.exe elérési toohello környezeti változó elérési útja. Konfigurációs útmutatásért lásd: [hogyan beállítása vagy módosítása hello elérési rendszerváltozó?]( https://www.java.com/download/help/path.xml). elérési út hello hasonló tooC:\Program Files\Java\jdk1.8.0_77\jre\bin.
    - [A .NET core SDK 1.0.3 vagy a .NET Core 1.1 futásidejű](https://www.microsoft.com/net/download).
    
- Linux (ajánlott Ubuntu 14.04 LTS)

    - [A Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx). tooinstall hello kódja, írja be a következő parancs hello:

              sudo dpkg -i code_<version_number>_amd64.deb

    - [Monó 4.2.x](http://www.mono-project.com/docs/getting-started/install/linux/). 

        - tooupdate hello deb csomag forrás-, adja meg a következő parancsok hello:

                sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
                echo "deb http://download.mono-project.com/repo/debian wheezy/snapshots 4.2.4.4/main" | sudo tee /etc/apt/sources.list.d/mono-xamarin.list
                sudo apt-get update

        - tooinstall Monó, adja meg a következő parancs hello:

                sudo apt-get install mono-complete

            > [!NOTE] 
            > Monó 4.6 nem támogatott. Távolítsa el a 4.6-os verzió, teljes mértékben 4.2.x telepítése előtt.  

        - [Java-futtatókörnyezet Ánjon 8-as verzió frissítési 77 vagy újabb](https://java.com/download/manual.jsp). A telepítési utasításokért lásd: hello [Linux 64 bites telepítési utasításokat Java]( https://java.com/en/download/help/linux_x64_install.xml) lap.
        - [A .NET core SDK 1.0.3 vagy a .NET Core 1.1 futásidejű](https://www.microsoft.com/net/download).
- MacOS

    - [A Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).
    - [Monó 4.2.4](http://download.mono-project.com/archive/4.2.4/macos-10-x86/). 
    - [Java-futtatókörnyezet Ánjon 8-as verzió frissítési 77 vagy újabb](https://java.com/download/manual.jsp). A telepítési utasításokért lásd: hello [Linux 64 bites telepítési utasításokat Java](https://java.com/en/download/help/mac_install.xml) lap.
    - [A .NET core SDK 1.0.3 vagy a .NET Core 1.1 futásidejű](https://www.microsoft.com/net/download).

## <a name="install-data-lake-tools"></a>A Data Lake Tools telepítése

Hello előfeltételek telepítése után a Data Lake Tools for VS kód is telepítheti.

**a Data Lake Tools tooinstall**

1. Nyissa meg a Visual Studio Code.
2. Válassza ki a Ctrl + P, és írja be a következő parancs hello:
```
ext install usql-vscode-ext
```
A Visual Studio code kiterjesztések listája látható. Az egyik **Azure Data Lake Tools**.

3. Válassza ki **telepítése** következő túl**Azure Data Lake Tools**. Néhány másodperc múlva hello **telepítése** túl gombra a módosítások**Újrabetöltés**.
4. Válassza ki **Újrabetöltés** tooactivate hello bővítmény.
5. Válassza ki **OK** tooconfirm. Azure Data Lake Tools hello látható **bővítmények** ablaktáblán.
    ![Data Lake Tools for Visual Studio Code bővítmények ablaktábla](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extensions.png)

## <a name="activate-azure-data-lake-tools"></a>Az Azure Data Lake Tools aktiválása
Hozzon létre egy új .usql fájlt, vagy nyisson meg egy meglévő .usql tooactivate hello fájlkiterjesztés. 

## <a name="connect-tooazure"></a>Csatlakozás tooAzure

Mielőtt fordítási, és a Data Lake Analytics U-SQL parancsfájlt, csatlakoznia kell a tooyour Azure-fiók.

**tooconnect tooAzure**

1.  Válassza ki a Ctrl + Shift + P tooopen hello parancs palettát. 
2.  Adja meg **ADL: bejelentkezési**. hello bejelentkezési adatok megjelennek-e hello **kimeneti** ablaktáblán.

    ![A Data Lake Visual Studio Code parancs paletta eszközei](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login.png)
    ![a Data Lake Tools for Visual Studio Code eszköz bejelentkezési adatok](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-login-info.png)
3. Válassza ki a Ctrl + kattintson a hello bejelentkezési URL-címe: https://aka.ms/devicelogin tooopen hello bejelentkezési képernyőn látható weblapon. Írja be a kódot hello **G567LX42V** hello szövegmezőbe, majd válassza ki azt a **Folytatás**.

   ![A Data Lake Tools for Visual Studio Code bejelentkezési illessze be a kódot](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login-paste-code.png )   
4.  Kövesse a hello utasításokat toosign hello weblap. Ha kapcsolódik, az Azure-fiók neve megjelenik hello állapotsor hello bal alsó sarkában hello **Visual STUDIO Code** ablak. 

    > [!NOTE] 
    > A fiókjának van engedélyezve, a két tényező, azt javasoljuk, hogy használja-e PIN-kóddal helyett phone hitelesítési.

toosign, adja meg a hello parancsot **ADL: kijelentkezési**.

## <a name="list-your-data-lake-analytics-accounts"></a>A Data Lake Analytics-fiókok listázása

tootest hello kapcsolat, a Data Lake Analytics-fiókok listájának beolvasása.

**toolist hello Data Lake Analytics-fiókok alatt az Azure-előfizetéshez**

1. Válassza ki a Ctrl + Shift + P tooopen hello parancs palettát.
2. Adja meg **ADL: fiókok listában**. hello fiókok megjelennek-e hello **kimeneti** ablaktáblán.

## <a name="open-hello-sample-script"></a>Nyissa meg hello parancsfájlpéldát
Nyissa meg a hello parancs paletta (Ctrl + Shift + P), és adja meg **ADL: Nyissa meg a minta-parancsprogram**. Ekkor megnyílik a minta egy másik példánya. Szerkeszthető, konfigurálása, és küldje el a parancsfájl ezen a példányon.

## <a name="work-with-u-sql"></a>U-SQL műveletek

A U-SQL-fájl vagy egy mappa toowork U-SQL kell megnyitni.

**egy mappa a U-SQL projekt tooopen**

1. A Visual Studio Code, válassza ki a hello **fájl** menüben, majd válassza ki **mappa megnyitása**.
2. Adjon meg egy mappát, majd válassza ki **Mappaválasztás**.
3. Jelölje be hello **fájl** menüben, majd válassza ki **új**. Egy névtelen-1 fájl toohello projekt kerül.
4. Adja meg a kódot hello névtelen-1-fájlba a következő hello:

        @departments  = 
            SELECT * FROM 
                (VALUES
                    (31,    "Sales"),
                    (33,    "Engineering"), 
                    (34,    "Clerical"),
                    (35,    "Marketing")
                ) AS 
                      D( DepID, DepName );
         
        OUTPUT @departments
            TO “/Output/departments.csv”

    hello parancsfájl fájlt hoz létre departments.csv néhány hello/output mappában szereplő adatokat.

5. Hello fájl mentése másként **myUSQL.usql** hello a megnyitni a mappát. Adltools_settings.json konfigurációs fájlt is bővült toohello projekt.
4. Nyissa meg, majd konfigurálja adltools_settings.json hello következő tulajdonságai:

    - Fiók: Egy Data Lake Analytics-fiók alatt az Azure-előfizetéshez.
    - Adatbázis: A fiók alatt adatbázis. hello alapértelmezett érték a **fő**.
    - Séma: Az adatbázis a séma. hello alapértelmezett érték a **dbo**.
    - Választható beállítások:
        - Prioritás: hello prioritás tartomány: 1 1 too1000 hello legmagasabb prioritás szerint. hello alapértelmezett értéke **1000**.
        - Párhuzamossági: hello párhuzamossági tartomány: 1 too150. hello alapértelmezett értéke hello maximális párhuzamossági engedélyezett az Azure Data Lake Analytics-fiókja. 
        
        > [!NOTE] 
        > Ha hello beállítások érvénytelenek, hello alapértelmezett értékeket fogja használni.

    ![A Data Lake Tools for Visual Studio Code konfigurációs fájl](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-configuration-file.png)

    A Data Lake Analytics-fiók számítási toocompile és run U-SQL feladatok szükséges. Fordítsa le és U-SQL feladatok futtatása előtt konfigurálnia kell az hello számítógépfiókját.
    
Hello konfiguráció mentése után hello állapotsor hello megfelelő .usql fájlt hello bal alsó sarkában megjelenő hello fiók, adatbázis és séma információk. 
 
 
Az összehasonlított tooopening egy fájlt, amikor megnyit egy mappát is:

- A háttérkód fájl használható. Háttérkód hello egyfájlos módban nem támogatott.
- A konfigurációs fájlt használja. Ha a mappa megnyitásához munkamappa hello hello parancsfájlok osztják meg egyetlen konfigurációs fájlban.


U-SQL parancsfájl hello távolról lefordítja a Data Lake Analytics szolgáltatás hello keresztül. Hello küldésekor **fordítási** parancs hello U-SQL parancsfájl küldött tooyour Data Lake Analytics-fiók. Később a Visual Studio Code hello fordítási eredménye kap. Miatt toohello távoli fordítási a Visual Studio Code van szükség, hogy Ön tooconnect tooyour Data Lake Analytics-fiók hello konfigurációs fájlban hello-információinak felsorolásához.

**toocompile egy U-SQL parancsfájl**

1. Válassza ki a Ctrl + Shift + P tooopen hello parancs palettát. 
2. Adja meg **ADL: parancsfájl összeállításához**. hello fordítási eredményei jelennek meg hello **kimeneti** ablak. Jobb gombbal egy olyan parancsfájlt, és válassza **ADL: parancsfájl összeállításához** toocompile egy U-SQL-feladatot. hello fordítási eredménye megjelenik hello **kimeneti** ablaktáblán.
 

**toosubmit egy U-SQL parancsfájl**

1. Válassza ki a Ctrl + Shift + P tooopen hello parancs palettát. 
2. Adja meg **ADL: elküldeni a feladatot**.  Jobb gombbal egy olyan parancsfájlt, és válassza **ADL: feladat elküldése**. 

A U-SQL-feladat elküldése után hello küldésének naplók megjelennek-e hello **kimeneti** ablak a Visual STUDIO Code. Ha hello elküldése sikeres, hello feladat URL-címet is megjelenik. Hello feladat URL egy webes böngésző tootrack hello valós idejű feladat állapotát a nyithatja meg.

tooenable hello kimenete hello feladat részleteit, állítsa be **jobInformationOutputPath** a hello **vs helykódja hello u-sql_settings.json** fájlt.
 
## <a name="use-a-code-behind-file"></a>A háttérkód fájl használata

A háttérkód fájlt egy U-SQL parancsfájl társított C# fájlban. Hello háttérkód fájlban definiálhat egy dedikált parancsfájl tooUDO, uda-Értékeket, UDT és UDF. hello UDO, uda-Értékeket, UDT és UDF használható közvetlenül a hello parancsfájl hello szerelvény először regisztrálása nélkül. hello háttérkód fájl kerül, hello azonos mappában található, mint a társviszony-létesítési U-SQL-parancsfájlt. Ha hello parancsfájl neve xxx.usql, hello háttérkód, xxx.usql.cs neve. Ha manuálisan törli hello háttérkód fájlt, hello háttérkód funkció le van tiltva, a társított U-SQL parancsfájl. További információ a felhasználói kódot a U-SQL parancsfájl: [írási és egyéni kód használata U-SQL: felhasználó által definiált függvényeket]( https://blogs.msdn.microsoft.com/visualstudio/2015/10/28/writing-and-using-custom-code-in-u-sql-user-defined-functions/).

toosupport háttérkód, meg kell nyitnia egy munkamappa. 

**a háttérkód fájl toogenerate**

1. A forrásfájl megnyitásakor. 
2. Válassza ki a Ctrl + Shift + P tooopen hello parancs palettát.
3. Adja meg **ADL: mögött kód generálása**. A háttérkód fájl jön létre hello azonos mappába. 

Jobb gombbal egy olyan parancsfájlt, és válassza **ADL: kód generálása mögött**. 

toocompile, és küldje el a háttérkód fájlt tartalmazó U-SQL parancsfájl van hello megegyeznek a hello önálló U-SQL parancsfájlt.

a következő két képernyőképeket hello megjelenítése a háttérkód fájl- és a kapcsolódó U-SQL-parancsfájlt:
 
![A Data Lake Tools for Visual Studio Code háttérkód](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-behind.png)

![A Data Lake Tools for Visual Studio Code háttérkód parancsfájl](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-behind-call.png) 

## <a name="use-assemblies"></a>Szerelvények használata

Szerelvények fejlesztéséről további információkért lásd: [fejlesztése U-SQL-szerelvények Azure Data Lake Analytics-feladatok](data-lake-analytics-u-sql-develop-assemblies.md).

A Data Lake Tools tooregister egyéni kód szerelvények hello Data Lake Analytics-katalógus használható.

**egy szerelvény tooregister**

Hello szerelvény hello segítségével regisztrálhatja **ADL: szerelvény regisztrálása** vagy **ADL: szerelvény regisztrálása konfigurálással** parancsok.

**tooregister keresztül hello ADL: szerelvény regisztrálása parancs**
1.  Válassza ki a Ctrl + Shift + P tooopen hello parancs palettát.
2.  Adja meg **ADL: szerelvény regisztrálása**. 
3.  Adja meg a hello helyi szerelvény elérési útja. 
4.  Válassza ki a Data Lake Analytics-fiók.
5.  Válasszon ki egy adatbázist.

Eredmények: hello portál egy böngészőben meg van nyitva, és hello szerelvény regisztrációs folyamat jeleníti meg.  

Egy másik kényelmesen tootrigger hello **ADL: szerelvény regisztrálása** parancs a Fájlkezelőben tooright kattintással hello .dll fájl. 

**tooregister azonban hello ADL: szerelvény regisztrálása konfigurációs paranccsal**
1.  Válassza ki a Ctrl + Shift + P tooopen hello parancs palettát.
2.  Adja meg **ADL: szerelvény konfigurálással regisztrálása**. 
3.  Adja meg a hello helyi szerelvény elérési útja. 
4.  hello JSON-fájl jelenik meg. Tekintse át, és szükség esetén szerkessze a hello szerelvény függőségeit és erőforrás-paraméterek. Utasítások jelennek meg hello **kimeneti** ablak. tooproceed toohello szerelvények regisztrálásakor, (Ctrl + S) hello JSON-fájl.

![A Data Lake Tools for Visual Studio Code háttérkód](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-register-assembly-advance.png)
>[!NOTE]
>- Szerelvény függőségek: Azure Data Lake Tools egy hello dll-fájl rendelkezik-e függőségekről. hello függőségek hello JSON-fájlban jelennek meg, miután azokat. 
>- Erőforrás: A DLL-erőforrások (például .txt, .png és .csv) feltöltheti hello szerelvények regisztrálásakor részeként. 

Egy másik módja tootrigger hello **ADL: szerelvény regisztrálása konfigurálással** parancs a Fájlkezelőben tooright kattintással hello .dll fájl. 

U-SQL-kódot a következő hello bemutatja, hogyan toocall szerelvényt. Hello mintában hello szerelvény neve: *tesztelése*.

```
REFERENCE ASSEMBLY [test];

@a = 
    EXTRACT 
        Iid int,
    Starts DateTime,
    Region string,
    Query string,
    DwellTime int,
    Results string,
    ClickedUrls string 
    FROM @"Sample/SearchLog.txt" 
    USING Extractors.Tsv();

@d =
    SELECT DISTINCT Region 
    FROM @a;

@d1 = 
    PROCESS @d
    PRODUCE 
        Region string,
    Mkt string
    USING new USQLApplication_codebehind.MyProcessor();

OUTPUT @d1 
    too@"Sample/SearchLogtest.txt" 
    USING Outputters.Tsv();
```


## <a name="access-hello-data-lake-analytics-catalog"></a>Hello Data Lake Analytics-katalógus elérése

Miután csatlakozott a tooAzure, a következő lépéseket tooaccess hello U-SQL catalog hello is használhatja.

**tooaccess hello Azure Data Lake Analytics metaadatok**

1.  Válassza ki a Ctrl + Shift + P, és írja be **ADL: listáját táblákat**.
2.  Válasszon ki egy hello Data Lake Analytics-fiókok.
3.  Válasszon ki egy hello Data Lake Analytics-adatbázisokat.
4.  Válasszon ki egy hello sémák. Hello táblák listáját tekintheti meg.

## <a name="view-data-lake-analytics-jobs"></a>Nézet Data Lake Analytics-feladatok

**tooview Data Lake Analytics-feladatok**
1.  Nyissa meg a hello parancs paletta (Ctrl + Shift + P), és válassza ki **ADL: a feladat megjelenítése**. 
2.  Válasszon egy Data Lake Analytics vagy helyi fiók. 
3.  Várjon, amíg hello fiók tooappear hello feladatok listája.
4.  Jelölje ki a feladatot feladatot a listából, Data Lake Tools hello feladat részletei megnyitja hello Azure-portálon, és hello Feladatinformáció fájl és a kód jelenik meg.

![A Data Lake Tools for Visual Studio Code IntelliSense objektumtípusok](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-show-job.png)

## <a name="azure-data-lake-storage-integration"></a>Azure Data Lake tároló integrációja

Az Azure Data Lake tárolással kapcsolatos parancsokat használhatja:
 - Tallózzon a hello Azure Data Lake tárolási erőforrások. 
 - Előzetes hello Azure Data Lake-tárolási fájl.  
 - Töltse fel a hello közvetlenül tooAzure Data Lake-tárolási fájl a Visual STUDIO Code. 

### <a name="list-hello-storage-path"></a>Lista hello. tárolási elérési útja 
Hello. tárolási elérési útja vagy kattintson a jobb gombbal az hello parancs paletta szolgáltatással is listázhatja.

**hello parancs paletta keresztül toolist hello tárolási elérési útja**

1.  Nyissa meg a hello parancs paletta (Ctrl + Shift + P), és adja meg **ADL: lista. tárolási elérési útja**.

    ![A Data Lake Tools for Visual Studio Code lista. tárolási elérési útja](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-storage.png)

2.  Válassza ki a kívánt módon felsoroló hello. tárolási elérési útja. Az átjáró használja **adjon meg egy elérési utat** példaként.

    ![A Data Lake Tools for Visual Studio Code egyirányú toolist hello. tárolási elérési útja](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account-selectoneway.png)

    > [!NOTE]
    >- Visual STUDIO Code tartja hello utoljára meglátogatott elérési út minden Data Lake Analytics-fiók. Például: / tt/ss.
    >- Legfelső szintű elérési útról böngésző: hello lista gyökérelérési útja a kijelölt Data Lake Analytics-fiók vagy helyi elérési utat.
    >- Adjon meg egy elérési utat: egy adott elérési úton, a kiválasztott Data Lake Analytics-fiókhoz tartozó vagy egy helyi elérési útját listában.
    
3. Válassza ki a hello helyi elérési út vagy a Data Lake Analytics-fiók egy fiókot.

    ![A Data Lake Tools for Visual Studio Code több kiválasztása](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

4. Válassza ki **további** toolist további Data Lake Analytics-fiókok, és válassza a Data Lake Analytics-fiók.

    ![A Data Lake Tools for Visual Studio Code használt fiók kijelölése](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-select-adla-account.png)

5.  Az Azure storage elérési útjának megadása Például/kimeneti.

    ![A Data Lake Tools for Visual Studio Code, adja meg. tárolási elérési útja](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-input-path.png)

6.  Eredmények: hello parancs paletta hello útvonal-információinak alapján sorolja fel.

    ![A Data Lake Tools for Visual Studio Code tárolási elérési útja eredmények felsorolása](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-path.png)

Toolist hello relatív elérési út keresztül hello kényelmesebb úgy kattintson a jobb gombbal a helyi menü.

**Kattintson jobb gombbal az toolist hello. tárolási elérési útja keresztül**

1.  Kattintson a jobb gombbal a hello elérési út karakterlánc tooselect **lista. tárolási elérési útja**.

       ![A Data Lake Tools for Visual Studio Code kattintson a jobb gombbal a helyi menü](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-path.png)

2. hello kijelölt relatív elérési út hello parancs paletta jelenik meg.

   ![A Data Lake Tools for Visual Studio Code kijelölt relatív elérési útja](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-relative-path.png)

3.  Válassza ki a hello helyi elérési út vagy a Data Lake Analytics-fiók egy fiókot.

       ![A Data Lake Tools for Visual Studio Code használt fiók kijelölése](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

4.  Eredmények: hello parancs paletta hello mappák és fájlok hello aktuális útvonal sorolja fel.

       ![Data Lake Tools for Visual Studio Code listáját a következőtől: hello aktuális elérési útja](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-current.png)

### <a name="preview-hello-storage-file"></a>Előzetes hello tárolófájl
Megtekintheti a hello tárolófájl, vagy kattintson a jobb gombbal az hello parancs paletta szolgáltatással.

**toopreview hello tárolófájl hello parancs paletta keresztül**

1.  Nyissa meg a hello parancs paletta (Ctrl + Shift + P), és adja meg **ADL: Preview tárolófájl**.

       ![A Data Lake Tools for Visual Studio Code preview tárolófájl](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-preview.png)

2.  Válassza ki a hello helyi elérési út vagy a Data Lake Analytics-fiók egy fiókot.

       ![A Data Lake Tools for Visual Studio Code listában fiók](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

3.  Válassza ki **további** toolist további Data Lake Analytics-fiókok, és válassza a Data Lake Analytics-fiók.

       ![A Data Lake Tools for Visual Studio Code használt fiók kijelölése](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-select-adla-account.png)

4.  Adjon meg egy Azure storage elérési út vagy fájlnév. Például /output/SearchLog.txt.

       ![A Data Lake Tools for Visual Studio Code adja meg a tároló elérési útja és fájlneve](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-input-preview-file.png)

5.  Eredmények: hello parancs paletta hello útvonal-információinak alapján sorolja fel.

       ![A Data Lake Tools for Visual Studio Code előzetes fájl eredménye](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-preview-results.png)

**Kattintson jobb gombbal az toolist hello. tárolási elérési útja keresztül**

1.  toopreview egy fájlt, kattintson a jobb gombbal hello fájl elérési útját.

   ![A Data Lake Tools for Visual Studio Code kattintson a jobb gombbal a helyi menü](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-preview.png) 

2.  Válassza ki a hello helyi elérési út vagy a Data Lake Analytics-fiók egy fiókot.

       ![A Data Lake Tools for Visual Studio Code használt fiók kijelölése](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

3.  Eredmények: Visual STUDIO Code hello fájl hello előnézeti eredményeit jeleníti meg.

       ![A Data Lake Tools for Visual Studio Code előzetes fájl eredménye](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-preview-results.png)

### <a name="upload-a-file"></a>Fájl feltöltése 

Fájlok feltöltheti hello parancsok beírásával **ADL: fájl feltöltése** vagy **ADL: a konfigurációs fájl feltöltése**.

**tooupload fájlok azonban hello ADL: a fájl feltöltése parancs**
1. Válassza ki a Ctrl + Shift + P tooopen hello parancs paletta, vagy kattintson a jobb gombbal a hello parancsprogram-szerkesztő, és írja be **fájl feltöltése**.
2.  tooupload hello adja meg a helyi elérési utat.

    ![A Data Lake Tools for Visual Studio Code helyi elérési utat adjon meg.](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-input-local-path.png)

3. Válasszon ki egy listát hello. tárolási elérési útja hello módjait. Az átjáró használja **adjon meg egy elérési utat** példaként.

    ![A Data Lake Tools for Visual Studio Code lista. tárolási elérési útja](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account-selectoneway.png)
    >[!NOTE]
    >- Visual STUDIO Code tartja hello utoljára meglátogatott elérési út minden Data Lake Analytics-fiók. Például: / tt/ss.
    >- Legfelső szintű elérési útról böngésző: hello lista gyökérelérési útja a kijelölt Data Lake Analytics-fiók vagy helyi elérési utat.
    >- Adjon meg egy elérési utat: egy adott elérési úton, a kiválasztott Data Lake Analytics-fiókhoz tartozó vagy egy helyi elérési útját listában.

4. Válassza ki a hello helyi elérési út vagy a Data Lake Analytics-fiók egy fiókot.

    ![A Data Lake Tools for Visual Studio Code kattintson a jobb gombbal a tároló](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

5. Az Azure storage elérési útjának megadása Például: / kimeneti.

       ![Data Lake Tools for Visual Studio Code enter storage path](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-input-preview-file.png)

6. Az Azure storage elérési út található. Válassza ki **válasszon aktuális mappa**.

    ![A Data Lake Tools for Visual Studio Code, válasszon ki egy mappát](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-choose-current-folder.png)

7.  Eredmények: hello **kimeneti** ablak hello fájl feltöltési állapotot jeleníti meg.

       ![A Data Lake Tools for Visual Studio Code feltöltési állapot](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-status.png)    

**tooupload fájlok azonban hello ADL: konfigurációs paranccsal fájl feltöltése**
1.  Válassza ki a Ctrl + Shift + P tooopen hello parancs paletta, vagy kattintson a jobb gombbal a hello parancsprogram-szerkesztő, és írja be **fájl feltöltése konfigurálással**.
2.  Visual STUDIO Code jeleníti meg a JSON-fájl. Adja meg az elérési utat, és a hello több fájl feltöltése ugyanannyi időt vesz igénybe. Utasítások jelennek meg hello **kimeneti** ablak. tooproceed tooupload hello fájl (Ctrl + S) hello JSON-fájl.

       ![A Data Lake Tools for Visual Studio Code-fájl elérési útja](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-file.png)

3.  Eredmények: hello **kimeneti** ablak hello fájl feltöltési állapotot jeleníti meg.

       ![A Data Lake Tools for Visual Studio Code feltöltési állapot](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-status.png)     

A fájl toostorage hello keresztül történik egy másik módja tooupload kattintson a jobb gombbal hello fájl teljes elérési útja vagy az hello fájl relatív elérési út az hello parancsprogram-szerkesztő. Hello helyi fájl elérési útját adja meg, és válassza a hello fiók. Hello **kimeneti** ablak hello feltöltési állapotot jeleníti meg. 

### <a name="open-azure-storage-explorer"></a>Nyissa meg az Azure Storage Explorer
Megnyithatja **Azure Tártallózó** hello parancs megadásával **ADL: Nyissa meg a webalkalmazás Azure Tártallózó** vagy hello kattintson a jobb gombbal helyi menüből való kiválasztása.

**Azure Tártallózó tooopen**

1. Válassza ki a Ctrl + Shift + P tooopen hello parancs palettát.
2. Adja meg **megnyitása webes Azure Tártallózó** , vagy kattintson a jobb gombbal egy relatív elérési út vagy hello hello parancsprogram-szerkesztő a teljes elérési útja, és válassza **megnyitása webes Azure Tártallózó**.
3. Válassza ki a Data Lake Analytics-fiók.

A Data Lake Tools hello az Azure storage elérési hello Azure-portál megnyitása. Hello elérési útját és preview hello fájl hello webkiszolgálóról található.

### <a name="local-run-and-local-debug-for-windows-users"></a>Helyi futtatáskor és a helyi hibakeresése Windows felhasználók
U-SQL helyi futtatásával teszteli a helyi adatok, és érvényesíti a parancsfájl helyi, a kódot csak közzétett tooData Lake Analytics. hello helyi hibakeresési funkció lehetővé teszi, hogy Ön toocomplete hello feladatok követően a kód benyújtott tooData Lake Analytics: 
- A C# háttérkód hibakeresési. 
- Hello kód lépéseit. 
- A parancsfájl helyi ellenőrzése.

A helyi futtatás helyi hibakeresési útmutatásért lásd: [U-SQL helyi futtatásával és a helyi hibakeresése a Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).

## <a name="additional-features"></a>További funkciók

A Data Lake Tools for Visual STUDIO Code hello a következő szolgáltatásokat támogatja:

-   Automatikus kitöltés IntelliSense: javaslatok elemek, például a kulcsszavak, a metódusok és a változók körüli előugró ablak jelenik meg. Különböző ikonjai különböző hello objektumokat:

    - Scala adattípus
    - Összetett adattípusú
    - Beépített UDTs
    - .NET adatgyűjtési és -osztályok
    - C# kifejezések
    - Beépített C# felhasználó által megadott függvények, udo-k és UDAAGs 
    - U-SQL-funkciók
    - U-SQL Ablakozó függvény
 
    ![A Data Lake Tools for Visual Studio Code IntelliSense objektumtípusok](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-objects.png)
 
-   Automatikus kitöltés a Data Lake Analytics metaadatok IntelliSense: Data Lake Tools hello Data Lake Analytics metaadat-információ helyileg tölti le. hello IntelliSense szolgáltatás automatikusan tölti fel az objektumok, például hello adatbázis, séma, tábla, nézet, táblázat értékű függvényben, eljárások és C#-szerelvények, hello Data Lake Analytics-metaadatok.
 
    ![A Data Lake Tools for Visual Studio Code IntelliSense metaadatok](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-metastore.png)

-   IntelliSense hiba jelölő: Data Lake Tools kiemeli az U-SQL és C# hibák Szerkesztés hello. 
-   Szintaxis emeli ki: a Data Lake Tools különböző színek toodifferentiate elemeket, például a változók, kulcsszavakat, adattípus és funkciók használja. 

    ![A Data Lake Tools for Visual Studio Code szintaxis kiemelések](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a>Következő lépések

- U-SQL helyi futtatása és a Visual Studio Code helyi debug: [U-SQL helyi futtatásával és a helyi hibakeresése a Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).
- A Data Lake Analytics-bevezető információkat lásd: [oktatóanyag: Ismerkedés az Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).
- A Data Lake Tools for Visual Studio információ: [oktatóanyag: fejlesztése U-SQL-parancsfájlok Data Lake Tools for Visual Studio használatával](data-lake-analytics-data-lake-tools-get-started.md).
- Szerelvények fejlesztéséről további információkért lásd: [fejlesztése U-SQL-szerelvények Azure Data Lake Analytics-feladatok](data-lake-analytics-u-sql-develop-assemblies.md).



