---
cím: aaa "Azure Analysis Services-oktatóanyag lecke 13: telepítése |} Microsoft Docs"Leírás: ismerteti, hogyan toodeploy hello oktatóanyag projekt tooAzure Analysis Services.
szolgáltatások: analysis-szolgáltatások documentationcenter: "Szerző: minewiskan manager: erikre szerkesztőben:" címkék: "

MS.AssetId: ms.service: analysis-szolgáltatások ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 07/17/2017 ms.author: owend
---
# <a name="lesson-13-deploy"></a>13. lecke: Üzembe helyezés

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Ez a lecke konfigurál központi telepítési tulajdonságok; Adja meg az Azure Analysis Services kiszolgáló toodeploy tooand hello modell nevét. Ezután hello modell toothat példányát telepíti. A minta telepítése után a felhasználók kapcsolódhatnak tooit egy jelentéskészítő ügyfél alkalmazás használatával. több, lásd: toolearn [tooAzure Analysis Services telepítése](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy).  
  
Becsült idő toocomplete Ez a lecke: **5 perc**  
  
## <a name="prerequisites"></a>Előfeltételek  
Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni. Ez a lecke hello feladatok elvégzése előtt kell befejeződött hello előző lecke: [lecke 12: elemzés az Excelben](../tutorials/aas-lesson-12-analyze-in-excel.md).  

> [!IMPORTANT]  
> Rendelkeznie kell [rendszergazdai jogosultságokkal](../analysis-services-server-admins.md) on hello távoli Analysis Services kiszolgáló sorrendben toodeploy tooit.  

> [!IMPORTANT]  
> Ha telepítette a hello AdventureWorksDW2014 mintaadatbázis egy helyi SQL-kiszolgálón, és telepítése esetén a modell tooan Azure Analysis Services-kiszolgáló, egy [helyszíni adatátjáró](../analysis-services-gateway.md) szükséges.
  
## <a name="deploy-hello-model"></a>Hello modell rendszerbe állítása  
  
#### <a name="tooconfigure-deployment-properties"></a>tooconfigure központi telepítési tulajdonságai  

  
1.  A **Megoldáskezelőben**, kattintson a jobb gombbal hello **AW Internet értékesítési** projektre, és kattintson a **tulajdonságok**.  
  
2.  A hello **AW Internet értékesítési tulajdonságlapjain** párbeszédpanel **rendszerbe állítási kiszolgáló**, a hello **Server** tulajdonság, adja meg a teljes kiszolgáló hello.  

    ![aas-lesson13-deploy-property](../tutorials/media/aas-lesson13-deploy-property.png)
  
3.  A hello **adatbázis** tulajdonság, írja be **Adventure Works Internet értékesítési**.  
  
4.  A hello **modellnév** tulajdonság, írja be **Adventure Works internetes értékesítési modell**.  
  
5.  Ellenőrizze a beállításokat, majd kattintson az **OK** gombra.  
  
#### <a name="toodeploy-hello-adventure-works-internet-sales"></a>az Adventure Works Internet értékesítési toodeploy hello
  
1.  A **Megoldáskezelőben**, kattintson a jobb gombbal hello **AW Internet értékesítési** project > **Build**.  

2.  Kattintson a jobb gombbal hello **AW Internet értékesítési** project > **telepítés**.

    TooAzure Analysis Services való telepítésekor, akkor előfordulhat, hogy a fióknak kell lennie a kért tooenter. Adja meg céges fiókját és jelszavát, például: nancy@adventureworks.com. Ennek a fióknak kell lennie a rendszergazdák hello kiszolgálón.
  
    hello telepítés párbeszédpanel jelenik meg, és hello metaadatok hello központi telepítési állapotának és hello modellben szereplő mindegyik tábla jeleníti meg.  
    
    ![aas-lesson13-deploy-status](../tutorials/media/aas-lesson13-deploy-status.png)
  
3. Miután az üzembe helyezés sikeresen befejeződött, nyugodtan kattintson a **Bezárás** gombra.  
  
## <a name="conclusion"></a>Összegzés  
Gratulálunk! Végzett az első Analysis Services-beli táblázatos modellje elkészítésével és üzembe helyezésével. Ez az oktatóanyag hozzájárult, amelyek hello leggyakoribb feladatokat egy táblázatos modell létrehozásának befejezése. Most, hogy az Adventure Works Internet értékesítési modellje telepít, a rendszer csak akkor használhatja az SQL Server Management Studio toomanage hello; folyamat parancsfájlok és biztonsági mentési terv létrehozása. Felhasználók mostantól is kapcsolódhatnak toohello modell jelentéskészítési ügyfél alkalmazások, például a Microsoft Excel alkalmazást vagy a Power BI segítségével.  

![aas-lesson13-ssms](../tutorials/media/aas-lesson13-ssms.png)
  
  
  
## <a name="whats-next"></a>A következő lépések
[Kapcsolódás Power BI Desktoppal](../analysis-services-connect-pbi.md)   
[Kiegészítő lecke – Dinamikus biztonság](../tutorials/aas-supplemental-lesson-dynamic-security.md)   
[Kiegészítő lecke – Részletsorok](../tutorials/aas-supplemental-lesson-detail-rows.md)   
[Kiegészítő lecke – Hézagos hierarchiák](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)   
