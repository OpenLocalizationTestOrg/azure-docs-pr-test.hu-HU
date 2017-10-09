---
title: "aaaCloud Cruiser és a Microsoft Azure számlázási API-integráció |} Microsoft Docs"
description: "Egy egyedi terv biztosít a Microsoft Azure számlázási partnerek felhő Cruiser a hello Azure számlázási API integrálása a termék tapasztalataikat.  Ez különösen fontos az Azure és a felhő Cruiser ügyfelek, amelyek segítségével/próbált felhő Cruiser a Microsoft Azure Pack érdekelt."
services: 
documentationcenter: 
author: BryanLa
manager: tonguyen
editor: 
tags: billing
ms.assetid: b65128cf-5d4d-4cbd-b81e-d3dceab44271
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 02/03/2017
ms.author: mobandyo;sirishap;bryanla
ms.openlocfilehash: 74cc19bdeed26c6684210736caa0cb365e8f8821
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-cruiser-and-microsoft-azure-billing-api-integration"></a>Felhő Cruiser és a Microsoft Azure számlázási API-integráció
Ez a cikk ismerteti, hogyan hello adatokat gyűjtött új Microsoft Azure számlázási API-k használható felhőalapú Cruiser munkafolyamat hello költség szimuláció és elemzése.

## <a name="azure-ratecard-api"></a>Az Azure RateCard API
hello RateCard API arány információkat nyújt az Azure-ból. Hello megfelelő hitelesítő adatokkal rendelkező hitelesítés után kérdezhetők le hello API toocollect metaadatainak hello szolgáltatások érhető el a Azure-ban együtt hello díjszabás társított a kínálnak azonosítóját.

hello az alábbiakban látható hello árakat hello A0 (Windows) megjelenítő hello API minta válaszára példány:

    {
        "MeterId": "0e59ad56-03e5-4c3d-90d4-6670874d7e29",
        "MeterName": "Compute Hours",
        "MeterCategory": "Virtual Machines",
        "MeterSubCategory": "A0 VM (Windows)",
        "Unit": "Hours",
        "MeterRates":
        {
            "0": 0.029
        },
        "EffectiveDate": "2014-08-01T00:00:00Z",
        "IncludedQuantity": 0.0,
        "MeterStatus": "Active"
    },

### <a name="cloud-cruisers-interface-tooazure-ratecard-api"></a>A felhő Cruiser tartozó felület tooAzure RateCard API
Felhő Cruiser hello RateCard API adatokat különböző módon használhatják fel. Ebben a cikkben bemutatjuk, hogyan használt toomake IaaS munkaterhelések költség szimuláció és elemzések lehet-e.

toodemonstrate ez használati eset, képzelhető el, a munkaterhelés több példánya fut a Microsoft Azure Pack (WAP). hello cél van toosimulate azonos munkaterhelés Azure és a becslés hello költségeit ilyen áttelepítés során. A rendezés toocreate a szimuláció, két fő feladatok toobe végre:

1. **Importálás és a folyamat hello szolgáltatás összegyűjtött adatokat a hello RateCard API.** Ez a feladat hello munkafüzetekhez, ahol hello RateCard API hello kivonatát az átalakított és közzétett tooa új arány tervet is történik. Az új arány csomag használni kívánt hello szimulációja tooestimate hello Azure árak.
2. **WAP szolgáltatások és az Azure-szolgáltatásokat a IaaS optimalizálására.** Alapértelmezés szerint WAP szolgáltatások alapján vannak az egyes erőforrások (Processzor, memória mérete, lemez méretét, stb.) amíg az Azure szolgáltatások alapján példányméretének (A0 A1, a2 méretű, stb.). Ez a feladat első is felhő Cruiser ETL-motor hajtja végre, nevű munkafüzetekhez, ha ezeket az erőforrásokat is telepíthet a példány mérete, hasonló tooAzure példány szolgáltatások.

### <a name="import-data-from-hello-ratecard-api"></a>Adatokat importálhat hello RateCard API
Felhő Cruiser munkafüzetek információkkal egy automatizált módon toocollect és a folyamat a hello RateCard API.  ETL (extract-átalakítási-betöltés) munkafüzetek lehetővé teszik tooconfigure hello gyűjtemény, átalakítás és hello felhő Cruiser adatbázis közzétételét az adatokat.

Minden egyes munkafüzet egy vagy több gyűjteményt, hogy lehetővé teszi a különböző forrásokból toocomplement toocorrelate adatait legyen, vagy kiegészítheti hello használati adatok is. a következő két képernyőképeket megjelenítése hogyan hello új toocreate *gyűjtemény* meglévő munkafüzetet, és adatokat importál hello *gyűjtemény* a hello RateCard API:

![1. ábra – egy új gyűjtemény létrehozása][1]

![2. ábra – hello új gyűjteményből adatok importálása][2]

Hello hello munkafüzetbe importálás után lehetséges toocreate több lépéseket és átalakítási folyamatok, toomodify és modell hello adatokat. Ebben a példában csak dolgozunk infrastruktúra,--szolgáltatás (IaaS) használjuk hello átalakítása lépéseket tooremove szükségtelen sorokat, vagy rekordokat iránt érdeklődik, mivel kapcsolatos tooservices IaaS eltérő.

hello alábbi képernyőfelvételen látható hello átalakítása lépéseket használt RateCard API gyűjtött tooprocess hello adatokat:

![3. ábra - átalakítási tooprocess gyűjtött adatok lépések RateCard API-n][3]

### <a name="defining-new-services-and-rate-plans"></a>Tervezi az új szolgáltatásokat és sebesség meghatározása
Nincs olyan különböző módokon toodefine szolgáltatások felhő Cruiser. Hello beállítások egyike tooimport hello szolgáltatások hello felhasználás adataiból. Ezt a módszert gyakran használják az nyilvános felhők, ahol hello szolgáltató már definiálva vannak hello szolgáltatás használatakor.

A sebesség megtervezése olyan díjszabás vagy árak, amelyek alkalmazott toodifferent szolgáltatások alapján életbelépési dátum vagy ügyfélcsoportot, többek között lehet. Sebesség terveket is használható a felhő Cruiser toocreate szimuláció vagy "Mi, ha" forgatókönyvek, toounderstand hogyan szolgáltatások változásainak hatással lehet a munkaterhelés hello összköltsége.

Ebben a példában a felhő Cruiser hello RateCard API toodefine új szolgáltatások hello szolgáltatás adatait használjuk. Hello azonos módon is használjuk tartozó hello díjszabás toohello szolgáltatások egy új arány tervezze meg a felhő Cruiser toocreate.

Hello átalakítási folyamat végén hello lehetséges toocreate új lépéssel és hello RateCard API hello adatait az új szolgáltatásokat és a díjszabás is közzé.

![4. ábra – hello RateCard API hello adatok közzététele új szolgáltatásokat és díjszabás][4]

### <a name="verify-azure-services-and-rates"></a>Ellenőrizze az Azure-szolgáltatások és díjszabás
Miután közzétette hello szolgáltatások és sebességét, ellenőrizheti a felhő Cruiser importált szolgáltatások listája hello *szolgáltatások* lapon:

![5. ábra – hello ellenőrzése a új szolgáltatások][5]

A hello *arány tervek* lapon hello "AzureSimulation" hello aránnyal importált hello RateCard API hívása új arány terv ellenőrizheti.

![6. ábra - ellenőrzése hello új arány tervezéséhez és a kapcsolódó díjakat][6]

### <a name="normalize-wap-and-azure-services"></a>Szabványosíthatja WAP és az Azure-szolgáltatások
Alapértelmezés szerint a WAP biztosítja a használati adatok alapján hello használata számítási, memória és hálózati erőforrásokat. Felhő Cruiser segítségével megadhatja a alapú szolgáltatások közvetlenül a hello foglalási vagy mért használatát ezeket az erőforrásokat. Egy alapszintű arány megadhatja például, a CPU-használat óránként vagy kell fizetni hello GB tooan példány lefoglalt memória.

Ebben a példában a rendelés toocompare költségek közötti WAP és az Azure, a tooaggregate hello erőforrás-használata WAP kötegbe, amelyből a csatlakoztatott tooAzure szolgáltatások kell. Ez a transzformáció könnyen megvalósítható hello munkafüzeteket:

![7. ábra – WAP toonormalize adatszolgáltatások átalakítása][7]

hello utolsó lépése a hello munkafüzet az toopublish hello adatai hello felhő Cruiser adatbázisba. Ezzel a lépéssel a hello használati adatok van most kötegelve-szolgáltatás (amelyek toohello Azure Services), és a kötött toodefault díjszabás toocreate hello díjakat.

Befejezési hello munkafüzet után automatizálhatja hello adatfeldolgozás hello, hello Feladatütemező ad hozzá egy feladatot, és hello gyakoriságát és hello munkafüzet toorun idejének megadása.

![8. ábra - munkafüzet ütemezése][8]

### <a name="create-reports-for-workload-cost-simulation-analysis"></a>Jelentések létrehozása munkaterhelés költség szimuláció elemzés
Miután hello használati gyűjt, és díjak töltődnek be a hello felhő Cruiser adatbázis, azt kihasználhatják a hello felhő Cruiser Insights modul toocreate hello munkaterhelés költség szimuláció, amely azt a felügyelni.

A sorrend toodemonstrate ebben az esetben létrehozott jelentést a következő hello:

![Költség összehasonlítása][9]

hello felső graph költség összehasonlítása szolgáltatások összehasonlítása hello ár futó hello munkaterhelés WAP (sötét kék) és az Azure (könnyű kék) közötti minden egyes konkrét szolgáltatás jeleníti meg.

hello alsó grafikon azt ábrázolja, hello ugyanazokat az adatokat azonban részleg által lebontva. Ez látható minden részleg toorun hello költségek a munkaterhelés WAP és az Azure, hello különbség hello megtakarítások sávon (zöld) együtt.

## <a name="azure-usage-api"></a>Az Azure használati API
### <a name="introduction"></a>Bevezetés
A Microsoft új hello Azure használati API-használati adatok toogain betekintést a felhasználás a tooprogrammatically lekéréses előfizetők lehetővé. Ez egy olyan felhőalapú Cruiser ügyfelek számára, kihasználhatja az API-n keresztül elérhető hello gazdagabb dataset kiváló híreket.

Felhő Cruiser hello integrációja hello használati API többféle módon használhatják fel. hello granularitási (óránkénti használati adatai) és a metaadatok erőforrásadatok hello keresztül elérhető API hello szükséges dataset toosupport rugalmas visszajelzés vagy jóváírási modellt biztosít. 

Az oktatóanyag azt jelent egy példa a felhő Cruiser hogyan vonatkozhat hello használati API információkat. Pontosabban azt fogja hozzon létre egy erőforráscsoportot az Azure-on, hello fiók struktúra címkéket társíthat, majd húzza és felhő Cruiser adatait hello címke feldolgozása hello folyamatát írják le.

hello végső célja toobe képes toocreate jelentések, például a következő egy hello, és képes tooanalyze költségeket, és felhasználási hello fiók struktúra által hello címkék alapján.

![10. ábra - címkék használatával bontása jelentés][10]

### <a name="microsoft-azure-tags"></a>A Microsoft Azure-címkék
hello Azure használati API keresztül elérhető hello adatok közé tartoznak, nem csak fogyasztási adatai, hanem erőforrás metaadatok, többek között a vele társított címkéket. Címkék tartalmaznak egy egyszerűen tooorganize az erőforrások, de a sorrendje toobe hatékony, tooensure kell, hogy:

* Címke található helyesen alkalmazott toohello erőforrások kiépítése során
* Címkék hello visszajelzési/jóváírás folyamat tootie hello használati toohello szervezeti fiók struktúra megfelelően használják.

Ezeket a követelményeket is lehet kihívást, különösen akkor, ha van hello létesítése vagy díjszabási ügyféloldali manuális folyamat. Helytelenül beírt, hibás vagy még nincs megadva címke található ügyfelek gyakran panaszkodnak, címkékkel és ezeket a hibákat is, hogy a élettartama a hello díjszabási ügyféloldali nagyon nehéz feladat.

Hello az új Azure használati API felhő Cruiser is információk címkézés erőforrás lekéréses és egy kifinomult ETL eszköz nevű munkafüzetekhez, javítsa ki a közös címkézési hibákat. Reguláris kifejezések és a megfelelési adatok átalakításában, a felhő Cruiser helytelenül címkézett erőforrások azonosítása és címkékkel hello helyes, hello megfelelő társítást hello erőforrások hello fogyasztói biztosítása.

Hello díjszabási oldalon, a felhő Cruiser hello visszajelzési/jóváírás folyamat automatizálja, és kihasználhatják a hello címke információk tootie hello használati toohello megfelelő fogyasztói (részleg, osztály, projekt, stb.). Ezt az automatizálást hatalmas fokozása biztosít, és meggyőződni arról, hogy egy egységes és naplózható díjszabási folyamat.

### <a name="creating-a-resource-group-with-tags-on-microsoft-azure"></a>Erőforráscsoport létrehozása a Microsoft Azure címkékkel
hello első lépés az oktatóanyag toocreate hello Azure-portálon az erőforráscsoport, akkor hozzon létre új címkék tooassociate toohello erőforrásokat. Ehhez a példához azt létrehozni a következő címkék hello: részleg, környezet tulajdonosa, projekt.

hello az alábbi képernyőfelvétel egy minta hello erőforráscsoport társított címkék.

![11. ábra - erőforráscsoportot az Azure-portál társított címkékkel][11]

hello következő lépés az toopull hello információ a használati API hello felhő Cruiser be. Használati API hello jelenleg JSON formátumban válaszokat biztosít. Itt látható egy minta hello beolvasott adat:

    {
      "id": "/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXX/providers/Microsoft.Commerce/UsageAggregates/Daily_BRSDT_20150623_0000",
      "name": "Daily_BRSDT_20150623_0000",
      "type": "Microsoft.Commerce/UsageAggregate",
      "properties":
      {
        "subscriptionId": "bb678b04-0e48-4b44-XXXX-XXXXXXXXX",
        "usageStartTime": "2015-06-22T00:00:00+00:00",
        "usageEndTime": "2015-06-23T00:00:00+00:00",
        "meterName": "Compute Hours",
        "meterRegion": "",
        "meterCategory": "Virtual Machines",
        "meterSubCategory": "Standard_D1 VM (Non-Windows)",
        "unit": "Hours",
        "instanceData": "{\"Microsoft.Resources\":{\"resourceUri\":\"/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXXX/resourceGroups/DEMOUSAGEAPI/providers/Microsoft.Compute/virtualMachines/MyDockerVM\",\"location\":\"eastus\",\"tags\":{\"Department\":\"Sales\",\"Project\":\"Demo Usage API\",\"Environment\":\"Test\",\"Owner\":\"RSE\"},\"additionalInfo\":{\"ImageType\":\"Canonical\",\"ServiceType\":\"Standard_D1\"}}}",
        "meterId": "e60caf26-9ba0-413d-a422-6141f58081d6",
        "infoFields": {},
        "quantity": 8

      },
    },


### <a name="import-data-from-hello-usage-api-into-cloud-cruiser"></a>Adatokat importálhat felhő Cruiser hello használati API
Felhő Cruiser munkafüzetek információkkal egy automatizált módon toocollect és a folyamat a hello használati API. Az ETL (extract-átalakítási-betöltés) munkafüzetek lehetővé teszi tooconfigure hello gyűjtemény, átalakítás és hello felhő Cruiser adatbázis közzétételét az adatokat.

Minden egyes munkafüzet rendelkezhet egy vagy több gyűjteményt. Ez lehetővé teszi különböző forrásokból toocomplement vagy bővítsék hello használati adatok toocorrelate adatait. Ehhez a példához létre fogunk hozni egy új lap hello Azure sablon munkafüzet (*UsageAPI)* , és egy új *gyűjtemény* hello használati API tooimport adatait.

![3. ábra - használati API adatokat importálni a hello UsageAPI lap][12]

Figyelje meg, hogy ez a munkafüzet már rendelkezik egyéb lapok tooimport szolgáltatások az Azure-ból (*ImportServices*), és feldolgozni a hello felhasználási adatokat a számlázási API hello (*PublishData*).

Ezután használjuk hello használati API toopopulate hello *UsageAPI* lap, és a összefüggésbe hello információk hello hello számlázási API adataival fogyasztás hello *PublishData* lap.

### <a name="processing-hello-tag-information-from-hello-usage-api"></a>Feldolgozási hello címke adatait hello használati API
Hello hello munkafüzetbe importálás után nem fog létrehozni átalakítása lépéseket hello *UsageAPI* rendelés tooprocess hello hello API adatait a lap. Első lépés egy "JSON vágási" processzor tooextract hello egy mező a címkéket, majd hozza létre a mezők az egyes egyik (részleg, projekt, tulajdonosa és környezet) toouse.

![4. ábra – hozzon létre új mezők hello címke információ][13]

Értesítés hello "Hálózat" szolgáltatás hiányzik hello címkeadatokat (sárga jelölését), de ellenőrizni tudja, hogy része hello ugyanabban az erőforráscsoportban hello megtekintésével *ResourceGroupName* mező. Tudunk hello hello címkék ezen erőforráscsoportból más erőforrások, mert az információk tooapply hello címkék toothis erőforrás hello folyamat későbbi részében hiányzik használhatjuk.

hello következő lépésre egy keresési tábla társítanak hello adatait hello címkék toohello toocreate *ResourceGroupName*. A keresési tábla használni kívánt hello tovább lépés tooenrich hello adatokkal együtt címkeinformációkat.

### <a name="adding-hello-tag-information-toohello-consumption-data"></a>Hello címke információk toohello adatokkal hozzáadása
Most azt is ugorhat toohello *PublishData* lap, mely folyamatok hello hello számlázási API felhasználási adatait, és adja hozzá a hello címkék kinyert hello mezőket. Ez a folyamat végzi el a hello előző lépésnél hello segítségével létrehozott hello keresési tábla megnézi *ResourceGroupName* hello keresések hello kulcsaként.

![5. ábra – hello fiók struktúra hello keresések hello adataival feltöltése][14]

Figyelje meg, hogy hello megfelelő fiók struktúra mezők hello "Hálózat" szolgáltatás alkalmazása megtörtént, a hiányzó címkék hello hello a probléma kijavítása. Azt a célként megadott erőforráscsoport "Egyéb" kívül is feltöltve hello fiók struktúra mezők erőforrások, sorrendben toodifferentiate hello a jelentéseket.

Most már csak ellenőriznünk kell tooadd egy lépés toopublish hello használati adatok. Ezzel a lépéssel a hello megfelelő díjszabás minden egyes definiálva az arány tervezze meg a szolgáltatás alkalmazott toohello használati adatainak hello eredményül kapott kell fizetni hello adatbázis betölti az lesz.

hello legjobb része, hogy csak rendelkezik ezzel a folyamattal toogo egyszer. Hello munkafüzet befejezése után csupán be kell tooadd azt toohello Feladatütemező és az óránkénti futásra lesz vagy hello naponkénti ütemezett időpontja. Akkor célszerű csak egy függetlenül attól, hogy új jelentések létrehozásakor, vagy meglévőket, a rendelés tooanalyze hello adatok tooget lekérdezhetik a fontos információkat kaphat a felhőhasználat testreszabása.

### <a name="next-steps"></a>Következő lépések
* A felhő Cruiser munkafüzeteket és jelentések létrehozása részletes információkra van szüksége, tekintse meg a tooCloud Cruiser tartozó online [dokumentáció](http://docs.cloudcruiser.com/) (érvényes bejelentkezési adatait).  Felhő Cruiser kapcsolatos további információkért forduljon [ info@cloudcruiser.com ](mailto:info@cloudcruiser.com).
* Lásd: [betekintést nyerhet a Microsoft Azure erőforrás-felhasználás](billing-usage-rate-card-overview.md) hello Azure erőforrás-használat és RateCard API-khoz áttekintését.
* Tekintse meg a hello [Azure számlázási REST API-referencia](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) mindkét API-k bővebben hello Azure Resource Manager által nyújtott API-kat hello készlete részét képező.
* Ha azt szeretné, hogy toodive hello mintakód be, tekintse meg a Microsoft Azure számlázási API Kódminták a [Azure mintakódok](https://azure.microsoft.com/documentation/samples/?term=billing).

### <a name="learn-more"></a>További információ
* Lásd: hello [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md) cikk toolearn hello Azure Resource Manager többet.

<!--Image references-->

[1]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Create-New-Workbook-Collection.png "1. ábra – egy új gyűjtemény létrehozása"
[2]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Import-Data-From-RateCard.png "2. ábra – hello új gyűjteményből adatok importálása"
[3]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transformation-Steps-Process-RateCard-Data.png "3. ábra - átalakítási tooprocess gyűjtött adatok lépések RateCard API-n"
[4]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Publish-RateCard-Data-New-Services-Rates.png "4. ábra – hello RateCard API hello adatok közzététele új szolgáltatásokat és díjszabás"
[5]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing1.png "5. ábra – hello ellenőrzése a új szolgáltatások"
[6]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing2.png "6. ábra - ellenőrzése hello új arány tervezéséhez és a kapcsolódó díjakat"
[7]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transforming-WAP-Normalize-Services.png "7. ábra – WAP toonormalize adatszolgáltatások átalakítása"
[8]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workbook-Scheduling.png "8. ábra - munkafüzet ütemezése"
[9]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workload-Cost-Simulation-Report.png "9. ábra - mintajelentés hello munkaterhelés költség összehasonlítása a forgatókönyvhöz"
[10]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/1_ReportWithTags.png "10. ábra - címkék használatával bontása jelentés"
[11]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/2_ResourceGroupsWithTags.png "11. ábra - erőforráscsoportot az Azure-portál társított címkékkel"
[12]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/3_ImportIntoUsageAPISheet.png "12. ábra - használati API adatokat importálni a hello UsageAPI lap"
[13]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/4_NewTagField.png "13. ábra – hozzon létre új mezők hello címke információ"
[14]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/5_PopulateAccountStructure.png "14. ábra – hello fiók struktúra hello keresések hello adataival feltöltése"
