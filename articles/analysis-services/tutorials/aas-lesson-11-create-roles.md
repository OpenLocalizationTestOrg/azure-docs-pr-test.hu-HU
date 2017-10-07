---
cím: aaa "Azure Analysis Services-oktatóanyag lecke 11: szerepkörök létrehozása |} Microsoft Docs"Leírás: ismerteti, hogyan toocreate szerepköreinek hello Azure Analysis Services-oktatóanyag projekt. szolgáltatások: analysis-szolgáltatások documentationcenter: "Szerző: minewiskan manager: erikre szerkesztőben:" címkék: "

MS.AssetId: ms.service: analysis-szolgáltatások ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend
---
# <a name="lesson-11-create-roles"></a>11. lecke: Szerepkörök létrehozása

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Ebben a leckében szerepköröket hozhat létre. Szerepkörök tartalmazzák modell adatbázis objektum és az adatok biztonsági hozzáférési tooonly korlátozza azoknak a felhasználóknak, akik szerepkör tagjai. Minden szerepkör egyetlen engedéllyel van definiálva: Nincs, Olvasás, Olvasás és feldolgozás, Feldolgozás vagy Rendszergazda. A szerepkörök a Szerepkörkezelő használatával definiálhatók a modell létrehozása során. A modell üzembe helyezése után az SQL Server Management Studio (SSMS) használatával kezelheti a szerepköröket. több, lásd: toolearn [szerepkörök](https://docs.microsoft.com/sql/analysis-services/tabular-models/roles-ssas-tabular).
  
> [!NOTE]  
> Szerepkörök létrehozása nem szükséges toocomplete van ebben az oktatóanyagban. Alapértelmezés szerint a is van jelenleg bejelentkezve hello fióknak rendszergazdai jogosultságai vannak a hello modellen. Azonban a küldő ügyfél segítségével a szervezet toobrowse belüli más felhasználók kell legalább egy szerepkör létrehozása az olvasási engedélyek és azoknak a felhasználóknak felvenni.  
  
Három szerepkört kell létrehoznia:  
  
-   **Az értékesítési igazgató** – ehhez a szerepkörhöz felhasználók tartalmazhatnak a szervezetében, amelyekhez toohave olvasási engedélyt tooall modell objektumokat és adatokat.  
  
-   **Értékesítési elemző USA** – ehhez a szerepkörhöz felhasználók tartalmazhatnak a szervezet legyen csak toobe képes toobrowse adatok az Egyesült Államok hello toosales kapcsolatos. Ehhez a szerepkörhöz használni egy DAX-képlet toodefine egy *sorainak szűrője*, amely korlátozza, hogy csak az Egyesült Államok hello tagok toobrowse adatokat.  
  
-   **Rendszergazda** – ehhez a szerepkörhöz felhasználók legyen toohave rendszergazdai jogosultsággal, amely lehetővé teszi az korlátlan hozzáférést és engedélyeket tooperform felügyeleti feladatok hello modell adatbázison tartalmazhatnak.  
  
Mivel a szervezet felhasználói és csoportfiókokat a Windows egyedi, az adott szervezet toomembers fiókokat is hozzáadhat. Azonban a jelen oktatóanyag esetében is üresen hello tagokat. Minden egyes szerepkör hello hatásának tesztelése későbbi részében lecke 12: elemzés az Excelben.  
  
Becsült idő toocomplete Ez a lecke: **15 perc**  
  
## <a name="prerequisites"></a>Előfeltételek  
Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni. Ez a lecke hello feladatok elvégzése előtt kell befejeződött hello előző lecke: [lecke 10: partíciókat](../tutorials/aas-lesson-10-create-partitions.md).  
  
## <a name="create-roles"></a>Szerepkörök létrehozása  
  
#### <a name="toocreate-a-sales-manager-user-role"></a>toocreate értékesítési igazgató felhasználói szerepkör  
  
1.  A Táblázatosmodell-tallózóban kattintson a jobb gombbal a **Szerepkörök** > **Szerepkörök** elemre.  
  
2.  A Szerepkörkezelőben kattintson az **Új** elemre.  
  
3.  Kattintson az új szerepkör hello, majd a hello **neve** oszlop, hello szerepkör átnevezése túl**értékesítési igazgató**.  
  
4.  A hello **engedélyek** oszlopban kattintson hello legördülő listára, és válassza a hello **olvasási** engedéllyel. 

    ![aas-lesson11-new-role](../tutorials/media/aas-lesson11-new-role.png) 
  
5.  Választható lehetőség: Kattintson a hello **tagok** fülre, majd **Hozzáadás**. A hello **felhasználók vagy csoportok kiválasztása** párbeszédpanelen adja meg a Windows hello-felhasználók vagy csoportok a szervezete hello szerepkörben tooinclude kívánt.  
  
#### <a name="toocreate-a-sales-analyst-us-user-role"></a>toocreate USA értékesítési elemzői felhasználói szerepkör  
  
1.  A Szerepkörkezelőben kattintson az **Új** elemre.    
  
2.  Nevezze át a hello szerepkör túl**értékesítési elemző USA**.  
  
3.  Adjon **Olvasás** engedélyt a szerepkörnek.  
  
4.  Hello sorszűrőket lapon, és majd a hello **DimGeography** csak, hello DAX-szűrő oszlop, tábla típusú hello képlet a következő:  
  
    ```Administrator
    =DimGeography[CountryRegionCode] = "US" 
    ```
    
    A Sorszűrőt képlet tooa logikai (igaz/hamis) értéket kell tartoznia. Ezzel a képlettel meg, hogy csak az "USA" hello ország régiókód érték rendelkező sor látható toohello felhasználókként szerepelnek.  
    ![aas-lesson11-role-filter](../tutorials/media/aas-lesson11-role-filter.png) 
  
6.  Választható lehetőség: Kattintson a hello **tagok** fülre, majd **Hozzáadás**. A hello **felhasználók vagy csoportok kiválasztása** párbeszédpanelen adja meg a Windows hello-felhasználók vagy csoportok a szervezete hello szerepkörben tooinclude kívánt.  
  
#### <a name="toocreate-an-administrator-user-role"></a>egy rendszergazda felhasználói szerepkör toocreate  
  
1.  Kattintson az **Új** lehetőségre.  
  
2.  Nevezze át a hello szerepkör túl**rendszergazda**.  
  
3.  Adjon **Rendszergazda** engedélyt a szerepkörnek.  
  
4.  Választható lehetőség: Kattintson a hello **tagok** fülre, majd **Hozzáadás**. A hello **felhasználók vagy csoportok kiválasztása** párbeszédpanelen adja meg a Windows hello-felhasználók vagy csoportok a szervezete hello szerepkörben tooinclude kívánt. 
  
  
## <a name="whats-next"></a>A következő lépések
[12. lecke: Elemzés az Excelben](../tutorials/aas-lesson-12-analyze-in-excel.md).

  
  
