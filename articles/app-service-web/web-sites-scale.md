---
title: "egy alkalmazás az Azure-ban aaaScale |} Microsoft Docs"
description: "Megtudhatja, hogyan tooscale be egy alkalmazást az Azure App Service tooadd kapacitás és a szolgáltatásokat."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: f7091b25-b2b6-48da-8d4a-dcf9b7baccab
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2016
ms.author: cephalin
ms.openlocfilehash: 97f46f77aeef95aec90d38e8396a023820e3caeb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-up-an-app-in-azure"></a>Vertikális felskálázás egy alkalmazást az Azure-ban
Ez a cikk bemutatja, hogyan tooscale az alkalmazást az Azure App Service-ben. Két munkafolyamatok méretezési, bővített fel és kibővítési, és ez a cikk azt ismerteti, hello felskálázott munkafolyamat.

* [Vertikális felskálázás](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): több Processzor, memória, lemezterület és extra funkciók, például dedikált virtuális gépek (VM), egyedi tartományok és tanúsítványok, előkészítési tárhelyek, automatikus skálázás és több beolvasása. Vertikális felskálázás az alkalmazáshoz tartozó App Service-csomag tarifacsomag hello módosításával.
* [Horizontális felskálázás](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): növelje az alkalmazást futtató Virtuálisgép-példányok hello számát.
  Ki lehet terjeszteni tooas több mint 20 példányok, attól függően, hogy a tarifacsomagot. [App Service Environment-környezetek](../app-service/app-service-app-service-environments-readme.md) a **prémium** réteg tovább növeli a kibővíthető száma too50 példányok. Kiterjesztésére vonatkozó további információkért lásd: [méretezése példányszám manuális vagy automatikus](../monitoring-and-diagnostics/insights-how-to-scale.md). Nem található ki, hogyan toouse automatikus skálázást, amely tooscale példányszám automatikusan alapján előre meghatározott szabályok és ütemezések.

hello skálázási beállításokat hajtsa végre a megfelelő csak másodperc tooapply, és hatással lévő összes alkalmazásra a [App Service-csomag](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
Nem szükséges, toochange a kódot, és telepítse újra az alkalmazást.

További információ a hello tarifa- és szolgáltatásokat az App Service-csomagokról: [App Service díjszabás](https://azure.microsoft.com/pricing/details/web-sites/).  

> [!NOTE]
> App Service-csomagot a hello váltottunk **szabad** réteg, először el kell távolítania hello [költségkeretek](https://azure.microsoft.com/pricing/spending-limits/) biztosítani az Azure-előfizetéshez. Tekintse meg a tooview, vagy módosítsa a Microsoft Azure App Service előfizetési beállítások [Microsoft Azure-előfizetések][azuresubscriptions].
> 
> 

<a name="scalingsharedorbasic"></a>
<a name="scalingstandard"></a>

## <a name="scale-up-your-pricing-tier"></a>Vertikális felskálázás tarifacsomag
1. A böngészőben nyissa meg a hello [Azure-portálon][portal].
2. Az alkalmazás paneljén kattintson **összes beállítás**, és kattintson a **vertikális Felskálázás**.
   
    ![Lépjen be az Azure alkalmazást tooscale.][ChooseWHP]
3. Válassza ki a réteg, és kattintson a **válasszon**.
   
    Hello **értesítések** lapon villogjon, egy zöld **sikeres** hello művelet befejeződése után.

<a name="ScalingSQLServer"></a>

## <a name="scale-related-resources"></a>Kapcsolódó erőforrások méretezése
Ha az alkalmazás egyéb szolgáltatások, például az Azure SQL Database vagy az Azure Storage függ is méretezheti fel ezeket az erőforrásokat a igények alapján. Ezeket az erőforrásokat az App Service-csomag hello nem méretezi, és külön-külön lehet méretezni.

1. A **Essentials**, kattintson a hello **erőforráscsoport** hivatkozásra.
   
    ![Vertikális felskálázás az Azure alkalmazás kapcsolódó erőforrások](./media/web-sites-scale/RGEssentialsLink.png)
2. A hello **összegzés** hello része **erőforráscsoport** paneljén kattintson egy erőforrás, amelyet az tooscale. a következő képernyőkép hello jeleníti meg, egy SQL-adatbázis és egy Azure Storage típusú erőforrást.
   
    ![Keresse meg a tooresource csoport panel tooscale be az Azure alkalmazást](./media/web-sites-scale/ResourceGroup.png)
3. Egy SQL-adatbázis erőforrás kattintson **beállítások** > **tarifacsomag** tooscale hello tarifacsomagra vált.
   
    ![Vertikális felskálázás az Azure alkalmazás hello SQL-adatbázis háttér](./media/web-sites-scale/ScaleDatabase.png)
   
    Is bekapcsolhatja a [georeplikáció](../sql-database/sql-database-geo-replication-overview.md) az SQL Database-példányt.
   
    Egy Azure Storage erőforrás kattintson **beállítások** > **konfigurációs** tooscale a tárolási beállítások konfigurálásával.
   
    ![Vertikális felskálázás az Azure-alkalmazás által használt hello Azure Storage-fiókban](./media/web-sites-scale/ScaleStorage.png)

<a name="devfeatures"></a>

## <a name="learn-about-developer-features"></a>Fejlesztői szolgáltatásainak megismerése
Attól függően, hogy a hello IP-címek hello következő fejlesztőknek készített funkciók érhetők el:

### <a name="bitness"></a>Bitszámának
* Hello **alapvető**, **szabványos**, és **prémium** rétegek támogatja a 64 bites és 32 bites alkalmazásokat.
* Hello **szabad** és **megosztott** terv rétegek csak a 32 bites alkalmazások támogatásához.

### <a name="debugger-support"></a>Hibakereső támogatás
* Hibakereső támogatás érhető el a hello **szabad**, **megosztott**, és **alapvető** módot, az App Service-csomag egy kapcsolattípust.
* Hibakereső támogatás érhető el a hello **szabványos** és **prémium** módot, az App Service-csomag öt egyidejű kapcsolatok.

<a name="OtherFeatures"></a>

## <a name="learn-about-other-features"></a>Egyéb funkciókkal kapcsolatos tudnivalók
* Részletes információ összes fennmaradó hello App Service szolgáltatások hello csomagokat, beleértve a díjszabás és érdeklődési tooall a felhasználók (beleértve a fejlesztők) szolgáltatások: [App Service díjszabás](https://azure.microsoft.com/pricing/details/web-sites/).

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépések tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/) ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Nincs bankkártyára szükséges találhatók és nem jár kötelezettségekkel.
> 
> 

<a name="Next Steps"></a>

## <a name="next-steps"></a>Következő lépések
* Azure-ban használatába tooget lásd [Microsoft Azure ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/).
* Információk a díjszabásról támogatást és szolgáltatásiszint-szerződés, látogasson el a következő hivatkozások hello.
  
    [Adatátvitel díjszabása](https://azure.microsoft.com/pricing/details/data-transfers/)
  
    [A Microsoft Azure-támogatás ügyfeleknek](https://azure.microsoft.com/support/plans/)
  
    [Szolgáltatói szerződések](https://azure.microsoft.com/support/legal/sla/)
  
    [SQL adatbázis díjszabása](https://azure.microsoft.com/pricing/details/sql-database/)
  
    [Virtuális gépek és Felhőszolgáltatások mérete a Microsoft Azure][vmsizes]
  
    [Az App Service díjszabás](https://azure.microsoft.com/pricing/details/app-service/)
  
    [Az App Service díjszabás részletei – SSL-kapcsolatok](https://azure.microsoft.com/pricing/details/web-sites/#ssl-connections)
* Azure App Service információ gyakorlati tanácsok felépítése méretezhető és rugalmas architektúra, beleértve: [gyakorlati tanácsok: Azure App Service Web Apps](http://blogs.msdn.com/b/windowsazure/archive/2014/02/10/best-practices-windows-azure-websites-waws.aspx).
* App Service apps méretezésével kapcsolatos videók tekintse meg a következő erőforrások hello:
  
  * [Ha Azure webhelyek - lengyel Schackow tooScale](https://azure.microsoft.com/resources/videos/azure-web-sites-free-vs-standard-scaling/)
  * [Az automatikus skálázás Azure-webhelyek, CPU vagy ütemezett - rendelkező lengyel Schackow](https://azure.microsoft.com/resources/videos/auto-scaling-azure-web-sites/)
  * [Az Azure webhelyek méretezéssel -, lengyel Schackow](https://azure.microsoft.com/resources/videos/how-azure-web-sites-scale/)

<!-- LINKS -->
[vmsizes]:/pricing/details/app-service/
[SQLaccountsbilling]:http://go.microsoft.com/fwlink/?LinkId=234930
[azuresubscriptions]:http://go.microsoft.com/fwlink/?LinkID=235288
[portal]: https://portal.azure.com/

<!-- IMAGES -->
[ChooseWHP]: ./media/web-sites-scale/scale1ChooseWHP.png
[ChooseBasicInstances]: ./media/web-sites-scale/scale2InstancesBasic.png
[SaveButton]: ./media/web-sites-scale/05SaveButton.png
[BasicComplete]: ./media/web-sites-scale/06BasicComplete.png
[ScaleStandard]: ./media/web-sites-scale/scale3InstancesStandard.png
[Autoscale]: ./media/web-sites-scale/scale4AutoScale.png
[SetTargetMetrics]: ./media/web-sites-scale/scale5AutoScaleTargetMetrics.png
[SetFirstRule]: ./media/web-sites-scale/scale6AutoScaleFirstRule.png
[SetSecondRule]: ./media/web-sites-scale/scale7AutoScaleSecondRule.png
[SetThirdRule]: ./media/web-sites-scale/scale8AutoScaleThirdRule.png
[SetRulesFinal]: ./media/web-sites-scale/scale9AutoScaleFinal.png
[ResourceGroup]: ./media/web-sites-scale/scale10ResourceGroup.png
[ScaleDatabase]: ./media/web-sites-scale/scale11SQLScale.png
[GeoReplication]: ./media/web-sites-scale/scale12SQLGeoReplication.png
