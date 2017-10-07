---
title: aaaWhat az Azure Analysis Services |} Microsoft Docs
description: "Az Analysis Services hello nagy vonalakban tekinti beolvasása az Azure-ban."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 83d7a29c-57ae-4aa0-8327-72dd8f00247d
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/01/2017
ms.author: owend
ms.openlocfilehash: 48830a86f47a8ddc7770e6c44dd56c29927fe582
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-analysis-services"></a>Mi az Azure Analysis Services?
![Azure Analysis Services](./media/analysis-services-overview/aas-overview-aas-icon.png)

Az Azure Analysis Services modellezési hello felhőben található vállalati szintű adatokat biztosít. Ez egy teljes körűen felügyelt platformszolgáltatás, amely integrálva van az Azure adatplatform-szolgáltatásaival. 

Az Analysis Services segítségével több forrásból egyesítheti és kombinálhatja az adatokat, mérőszámokat határozhat meg, és egyetlen, megbízható szemantikai adatmodellben biztosíthatja az adatok védelmét. hello adatmodell segítségével egyszerűbb és gyorsabb a felhasználók toobrowse a nagy mennyiségű adatot az ügyfél-alkalmazások, például a Power bi-ban, az Excel, a Reporting Services, a külső, és az egyéni alkalmazások.

![Adatforrások](./media/analysis-services-overview/aas-overview-data-sources.png)

Tekintse meg [Ez a videó](https://sec.ch9.ms/ch9/d6dd/a1cda46b-ef03-4cea-8f11-68da23c5d6dd/AzureASoverview_high.mp4) toolearn, hogy az Azure Analysis Services hogyan igazodik a Microsoft által általános BI képességeit, és hogyan előnyeit úgy használhatja ki a adatmodellekben első hello felhőbe.

## <a name="built-on-sql-server-analysis-services"></a>Az SQL Server Analysis Servicesre épül
Az Azure Analysis Services kompatibilis az SQL Server Analysis Services Enterprise Editionben már meglévő számos nagyszerű funkcióval. Az Azure Analysis Services táblázatos modellek: hello 1200-as és 1 400 támogatja [kompatibilitási szintnek](analysis-services-compat-level.md). Támogatja a partíciókat, a sorszintű biztonságot, a kétirányú kapcsolatokat és a fordításokat. A memóriában tárolt és a DirectQuery módok villámgyors lekérdezéseket tesznek lehetővé nagy és összetett adatkészletekben.

A táblázatos modellek gyors fejlesztést biztosítanak, és nagymértékben testre szabhatók. A fejlesztők számára táblázatos modellek hello táblázatos Object Model (TOM) toodescribe modellobjektumokat tartalmaznak. TOM a JSON-ban van közzétéve az hello [táblázatos modell Scripting Language (TMSL)](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference) és hello AMO adatdefiníciós nyelv keresztül hello [Microsoft.AnalysisServices.Tabular](https://msdn.microsoft.com/library/microsoft.analysisservices.tabular.aspx) névtér.

## <a name="better-with-azure"></a>Hatékonyabb munkavégzés az Azure-ral
Az Azure Analysis Services toobuild így több Azure-szolgáltatásokkal integrálja kifinomult elemzési megoldásokat. Integráció a [Azure Active Directory](../active-directory/active-directory-whatis.md) biztonságos, szerepköralapú hozzáférési tooyour kritikus fontosságú adatokat biztosít. Integrálása [Azure Data Factory](../data-factory/data-factory-introduction.md) folyamatok adatokat tölt be hello modell tevékenység-ot. Az [Azure Automation](../automation/automation-intro.md) és az [Azure Functions](../azure-functions/functions-overview.md) egyéni kódot használó modellek egyszerűbb vezénylésére használható.

## <a name="get-up-and-running-quickly"></a>Gyors beállítás és használat
Percek alatt [létrehozhat egy kiszolgálót](analysis-services-create-server.md) az Azure Portalon. Az Azure Resource Manager-[sablonok](../azure-resource-manager/resource-manager-create-first-template.md) és a PowerShell használatával pedig deklaratív sablonokkal helyezheti üzembe a kiszolgálókat. Egyetlen sablonnal több szolgáltatást is üzembe helyezhet, egyéb Azure-összetevőkkel (például tárfiókokkal és az Azure Functions szolgáltatással) együtt. 

A kiszolgáló létrehozása után közvetlenül az Azure Portalon létrehozhat egy táblázatos modellt. Az új hello (előzetes verzió) [Tervező webszolgáltatáshoz](analysis-services-create-model-portal.md), csatlakozás tooan Azure SQL adatbázis Azure SQL Data Warehouse adatforrás, vagy a Power BI Desktop .pbix fájlok importálása. Táblázatok közötti kapcsolatok jönnek létre automatikusan, és létrehozhat mértékeket vagy hello model.bim fájl megfelelő json formátumban szerkesztése a böngészőből.

## <a name="scale-tooyour-needs"></a>Skála tooyour igények
Az Azure Analysis Services fejlesztői, alap- és standard szinten is elérhető. Minden egyes réteg belül terv költségek függően tooprocessing tápellátáshoz, a QPUs és a memória méretét eltérőek lehetnek. Amikor létrehoz egy kiszolgálót, egy adott szinten belül választ ki egy csomagot. Módosíthatja a tervek vagy le hello belül azonos szinten, vagy frissítési tooa magasabb szintű használható, de nem lehet használni egy magasabb réteg tooa alsó réteg alapján.

Vertikálisan fel- vagy leskálázhatja, illetve szüneteltetheti a kiszolgálóját. Használja a hello Azure-portálon vagy teljes ellenőrzést a azonnali PowerShell használatával. A fizetés használat alapján történik. További információ az hello különböző csomagok és a Rétegek és a használata hello Számológép toodetermine hello jobb tarifacsomagot, toolearn lásd: [Azure Analysis Services szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="keep-your-data-close"></a>Tartsa adatait a közelben
Az Azure Analysis Services-kiszolgálók hello következő hozhatók létre [Azure-régiók](https://azure.microsoft.com/regions/):

| Amerika | Európa | Ázsia és a Csendes-óceáni térség |
|----------|--------|--------------|
|  Dél-Brazília<br> Közép-Kanada<br> USA 2. keleti régiója<br> USA északi középső régiója<br> USA déli középső régiója<br> USA nyugati középső régiója<br> USA nyugati régiója | Észak-Európa<br> Az Egyesült Királyság déli régiója<br> Nyugat-Európa |   Délkelet-Ausztrália<br> Kelet-Japán<br> Délkelet-Ázsia<br> Nyugat-India  |

Új régiók közé éppen felvett összes hello idő, így előfordulhat, hogy a lista nem teljes. A helyet megadhatja az Azure Resource Manager-sablonok használatával, vagy a kiszolgáló Azure Portalon történő létrehozásakor. tooget hello a legjobb teljesítmény, a legnagyobb felhasználói bázis legközelebbi helyének kiválasztása. Ha a modelljeit több régióban található redundáns kiszolgálókon helyezi üzembe, biztosíthatja a [magas rendelkezésre állást](analysis-services-bcdr.md).

## <a name="migrate-your-existing-tabular-models"></a>Meglévő táblázatos modellek migrálása
Ha már rendelkezik meglévő helyszíni SQL Server Analysis Services-modell megoldások, tooAzure Analysis Services jelentős változtatások nélkül telepítheti át. toomigrate, használhatja az SSDT toodeploy modell tooyour kiszolgálóját. Az SSMS biztonsági mentés és visszaállítás funkcióját vagy a TMSL-t is használhatja.

Ha a helyszíni adatforrások, tooinstall kell, és konfigurálja az [helyszíni adatátjáró](analysis-services-gateway.md). Ha a szerepkörök és a szerepkör tagjai már be van állítva, a szerepkörök áttelepítése, de rendelkezik tooreadd szerepkör tagjai SSMS vagy a PowerShell használatával.

## <a name="connect-toopopular-data-sources"></a>Toopopular adatforrások csatlakoztatásához
Támogatja az Azure Analysis Services [toodata források csatlakozás](analysis-services-datasource.md) helyszíni a szervezetében, és hello felhőben. A helyszíni és a felhőalapú adatforrásokból származó adatokat kombinálhatja is egy hibrid megoldás létrehozásához. 

1 400 új táblázatos modellek szolgáltatásával hello modern adatok beolvasása SSDT hello M képlet lekérdezés alapján. Adatok beolvasása a további adatok átalakítása és az Adategyesítés szolgáltatást és a hello képességét toocreate, és a saját speciális millió képlet nyelvi lekérdezés szerkesztése. A Tabular 1400-modellekkel például modellezheti az Azure Blob Storage-ban lévő adatfájlokat.

## <a name="use-hello-tools-you-already-know"></a>Már ismeri a hello eszközök használata

![BI-fejlesztői eszközök](./media/analysis-services-overview/aas-overview-dev-tools.png)

#### <a name="sql-server-data-tools-ssdt-for-visual-studio"></a>SQL Server Data Tools (SSDT) for Visual Studio
Fejlesztésekor és telepítésekor szabad hello modellek [SQL Server Data Tools (SSDT) a Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx). Az SSDT Analysis Services-projektsablonokat is tartalmaz a gyors üzembe állítás érdekében. Az SSDT most funkcióit tartalmazza: hello modern adatok beolvasása adatforrás lekérdezési és adategyesítési 1 400 táblázatos modellek. Az adatok beolvasása a Power BI Desktop és Excel 2016 ismeri, ha már ismeri milyen egyszerűen toocreate nagymértékben testreszabott adatok forrása lekérdezésekben.

#### <a name="sql-server-management-studio"></a>Sql Server Management Studio
Felügyelheti kiszolgálóit és modelladatbázisait az [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx) segítségével. Csatlakoztassa a hello felhőben tooyour kiszolgálókat. Származó hello XMLA lekérdezés ablak jobb TMSL parancsfájlokat futtasson, és a feladatok automatizálásához TMSL-parancsfájlok használatával. Gyakran jelennek meg új szolgáltatások és funkciók – az SSMS havonta frissül.

#### <a name="powershell"></a>PowerShell
Azure Resource Manager (AzureRM) parancsmagokat használja a kiszolgálói erőforrás felügyeleti feladatok kiszolgálók létrehozása, felfüggesztése vagy folytatása a kiszolgáló műveletek vagy hello szolgáltatási szint (réteg) módosítása. Egyéb feladatok hozzáadásával vagy eltávolításával szerepkör tagjai például adatbázisok kezeléséhez, feldolgozás, vagy TMSL parancsfájlok futtatásához a parancsmagokat hello SqlServer modul. Azure RM-ben és az SQL Server modulok érhetők el hello [PowerShell-galériában](https://www.powershellgallery.com/).


## <a name="your-data-is-secure"></a>Az adatok biztonságban vannak
![Adatmegjelenítések](./media/analysis-services-overview/aas-overview-secure.png)

#### <a name="authentication"></a>Authentication
Az Azure Analysis Services felhasználóhitelesítését az [Azure Active Directory (AAD)](../active-directory/active-directory-whatis.md) kezeli. Egy szervezeti fiók identitás access toohello adatbázissal toolog tooan Azure Analysis Services adatbázisában, felhasználók használja megkísérelte kívánt tooaccess. A felhasználói identitások tagjának kell lennie hello alapértelmezett Azure Active Directory hello előfizetés ahol hello Azure Analysis Services-kiszolgáló található. több, lásd: toolearn [hitelesítés és a felhasználói engedélyek](analysis-services-manage-users.md).

#### <a name="data-security"></a>Adatbiztonság
Az Azure Analysis Services használja az Azure Blob storage-toopersist tároló és az Analysis Services adatbázisok metaadatok. A Blobon belüli adatfájlok az Azure Blob kiszolgálóoldali titkosításával (SSE) vannak titkosítva. Közvetlen lekérdezési mód használatakor a rendszer csak a metaadatokat tárolja. hello tényleges adatokhoz hello adatforrásból időben.

#### <a name="on-premises-data-sources"></a>Helyszíni adatforrások
Biztonságos hozzáférés toodata elhelyezkedő helyszíni a szervezet megvalósítani telepítése és konfigurálása egy [helyszíni adatátjáró](analysis-services-gateway.md). Átjárók hozzáférés toodata a közvetlen lekérdezés és a memórián belüli módot adja meg. Amikor az Azure Analysis Services-modell tooan a helyszíni adatforráshoz kapcsolódik, egy lekérdezés létrehozása együtt Titkosított hello hello helyszíni adatforrás hitelesítő adatait. hello átjáró felhőszolgáltatáshoz hello lekérdezés elemzi, és leküldéses értesítések hello kérelem tooan Azure Service Bus. a helyszíni átjáró hello hello Azure Service Bus a függőben lévő kérelmek kérdezi le. hello átjáró majd hello lekérdezés lekérdezi, visszafejti hello hitelesítő adatokat, és toohello adatforrás végrehajtásra csatlakozik. hello eredmények akkor küldi hello adatforrás, biztonsági toohello átjáró, és majd toohello Azure Analysis Services-adatbázis.

Az Azure Analysis Services hello szabályozza [Microsoft Online Services használati](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) és hello [Microsoft Online Services adatvédelmi nyilatkozatát](https://www.microsoft.com/privacystatement/OnlineServices/Default.aspx).
További információ az Azure biztonsági toolearn lásd: hello [Microsoft Trust Center](https://www.microsoft.com/trustcenter/Security/AzureSecurity).

## <a name="supports-hello-latest-client-tools"></a>Támogatja a legújabb ügyféleszközök hello
![Adatmegjelenítések](./media/analysis-services-overview/aas-overview-clients.png)

Az olyan modern adatáttekintési és vizualizációs eszközök, mint például a Power BI, az Excel, illetve a harmadik felektől származó eszközök interaktív és vizuálisan gazdag modelladat-elemzéseket biztosítanak a felhasználóknak.

Az ügyfelek használják a MSOLAP, AMO vagy ADOMD [klienskódtárak](analysis-services-data-providers.md) tooconnect tooAnalysis szolgáltatási kiszolgálókra. Az olyan Microsoft-ügyfélalkalmazások, mint a Power BI Desktop és az Excel mindhárom ügyfélkódtárat telepítik. De tartsa szem előtt attól függően, hogy hello verziója vagy a frissítési gyakoriságának klienskódtárak nem, előfordulhat, hello Azure Analysis Services által igényelt legújabb verziói. hello Ugyanez vonatkozik toocustom alkalmazások vagy egyéb felületek, például AsCmd, TOM, ADOMD.NET. Ezek az alkalmazások használatához általában szükség van kézi telepítésének hello szalagtárak csomag részeként.


## <a name="get-help"></a>Segítségkérés

#### <a name="documentation"></a>Dokumentáció
Az Azure Analysis Services mentése egyszerű tooset és toomanage. Minden hello info toocreate kell, és itt a kiszolgáló-szolgáltatások kezelése található. Adatok modell toodeploy tooyour kiszolgáló létrehozása, ha az rendelkezik sokkal hello azonos, mert az tooan helyszíni kiszolgálón telepít adatmodellt létrehozásához. Az [Analysis Services](https://docs.microsoft.com/sql/analysis-services/analysis-services) oldalán egy fogalmakat, eljárásokat, oktatóanyagokat és referenciaanyagokat tartalmazó tár található.

#### <a name="videos"></a>Videók
Itt találhat útmutató videókat: [Azure Analysis Services – Channel 9](https://channel9.msdn.com/series/Azure-Analysis-Services).

#### <a name="blogs"></a>Blogok
A dolgok gyorsan változnak. Mindig kaphat a legfrissebb információkat hello hello [Analysis Services csapat blogján](https://blogs.msdn.microsoft.com/analysisservices/) és [Azure blog](https://azure.microsoft.com/blog/).

#### <a name="community"></a>Közösség
Az Analysis Services felhasználói pezsgő közösséget alkotnak. Csatlakoztatás hello beszélgetés [Azure Analysis Services fórum](https://aka.ms/azureanalysisservicesforum).

## <a name="feedback"></a>Visszajelzés
Javaslatai vagy új funkciókkal kapcsolatos felvetései vannak? Lehet, hogy tooleave megjegyzéseit [Azure Analysis Services visszajelzés](https://aka.ms/azureanalysisservicesfeedback).

Hello dokumentációhoz javaslatai? Megjegyzések Livefyre használatával minden cikknek hello alján adhat hozzá.

## <a name="next-steps"></a>Következő lépések
Most, hogy ismeri az Azure Analysis Services bővebben, az az idő tooget elindult. Ismerje meg, hogyan túl[kiszolgáló létrehozása](analysis-services-create-server.md) az Azure-ban. Ha a kiszolgáló készen áll, hello lépésenként [Adventure Works oktatóanyag](tutorials/aas-adventure-works-tutorial.md) toolearn hogyan toocreate egy teljesen működőképes táblázatos modell, és telepítse azt tooyour kiszolgáló.
