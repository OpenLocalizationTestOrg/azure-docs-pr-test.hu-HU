---
title: "aaaAzure App Service, a virtuális gépek, a Service Fabric és a Cloud Services összehasonlítása |} Microsoft Docs"
description: "Megtudhatja, hogyan toochoose közötti Azure App Service, a virtuális gépek, a Service Fabric és a Felhőszolgáltatások tárolására szolgáló webes alkalmazások."
services: app-service\web, virtual-machines, cloud-services
documentationcenter: 
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: 7d346a23-532a-42a9-98a8-23b7286d32a8
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 07/07/2016
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 7577332ed049df66178c7b2cd5c440a7f93a7865
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-virtual-machines-service-fabric-and-cloud-services-comparison"></a>Az Azure App Service, a virtuális gépek, a Service Fabric és a Cloud Services összehasonlítása
## <a name="overview"></a>Áttekintés
Azure számos lehetőséget kínál a webhelyek toohost: [Azure App Service][Azure App Service], [virtuális gépek][Virtual Machines], [Service Fabric ] [ Service Fabric], és [a felhőalapú szolgáltatások][Cloud Services]. Ez a cikk segít hello-beállítások megértéséhez, valamint hogy hello megfelelő választás a webalkalmazás.

Az Azure App Service hello a legjobb választás a legtöbb web Apps. Üzembe helyezési és kezelési integrált hello platform, helyek gyorsan toohandle nagy forgalomterhelés méretezhető és hello beépített terhelés és a traffic manager magas rendelkezésre állás biztosításához. Meglévő App Service tooAzure webhelyek áthelyezheti könnyen rendelkező egy [online áttelepítési eszköz](https://www.migratetoazure.net/), Web Application Gallery hello nyílt forráskódú alkalmazás használja, vagy hozzon létre egy új helyet hello keretrendszer és a kiválasztott eszközök használatával. Hello [WebJobs] [ WebJobs] szolgáltatás révén könnyen tooadd háttér feladat feldolgozási tooyour App Service webalkalmazás.

A Service Fabric akkor hasznos, ha létrehoz egy új alkalmazást vagy újraírása egy meglévő alkalmazást toouse egy mikroszolgáltatási architektúra. Alkalmazásokat, egy megosztott készletéhez gépeket futtathatnak, kezdje kis lépésekkel, és növekedjen toomassive méretezéssel, több száz vagy ezer gépek igény szerint. Állapotalapú szolgáltatások révén könnyen tooconsistently és megbízhatóan tárolja az alkalmazás állapota, és automatikusan kezeli a Service Fabric szolgáltatás particionálás, a méretezés és a rendelkezésre állási meg.  A Service Fabric is támogatja WebAPI az Open Web Interface .NET (OWIN) és az ASP.NET Core.  Összehasonlított tooApp szolgáltatás, a Service Fabric is biztosít a további szabályozhatják, és közvetlen hozzáférést, hello alapul szolgáló infrastruktúra. Azokat a kiszolgálók távoli is, vagy konfigurálja a kiszolgáló indítása feladatokat. Felhőszolgáltatások hasonló tooService szabályozására könnyű használat, és a háló, de most már egy régebbi szolgáltatás és az új fejlesztési érdemes a Service Fabric.

Ha egy meglévő alkalmazást az App Service vagy a Service Fabric jelentős módosításokat toorun újraindexelést igénylő, sorrendben toosimplify toohello felhő áttelepítése a jelenítette meg a virtuális gépek. Azonban megfelelően állítja be, biztonságossá tétele és karbantartása a virtuális gépek jóval több időt igényel, és informatikai szakértői tooAzure App Service és a Service Fabric képest. Ha tervezi az Azure virtuális gépek, győződjön meg arról, figyelembe fiók hello folyamatos karbantartási szükséges erőfeszítés toopatch frissítése, és a virtuális gép környezete kezeléséhez. Az Azure virtuális gépek infrastruktúra,--szolgáltatás (IaaS), míg az App Service és a Service Fabric Platform,--szolgáltatás (Paas). 

## <a name="features"></a>Szolgáltatások összehasonlítása
hello a következő táblázat összehasonlítja az App Service hello képességeit, a Felhőszolgáltatások, virtuális gépek és a Service Fabric toohelp ellenőrizze hello legjobb választás. Az egyes lehetőségek SLA hello kapcsolatos aktuális információkért lásd: [Azure szolgáltatói szerződések](https://azure.microsoft.com/support/legal/sla/).

| Szolgáltatás | App Service (webalkalmazások) | Cloud Services (webes szerepkörök) | Virtuális gépek | Service Fabric | Megjegyzések |
| --- | --- | --- | --- | --- | --- |
| Szinte azonnali telepítés |X | | |X |E alkalmazást vagy egy alkalmazás frissítés tooa felhőalapú szolgáltatás, vagy egy virtuális gép létrehozása több percig legalább; egy olyan alkalmazás tooa webes alkalmazást másodpercet vesz igénybe. |
| Vertikális felskálázás toolarger gépek helyezze üzembe újra nélkül |X | | |X | |
| Web server-példány osztozik az tartalmát és beállításait, ami azt jelenti, hogy nem tooredeploy van, vagy konfigurálja újra, mert méretezni. |X | | |X | |
| Több központi telepítési környezetekben (üzemi és átmeneti) |X |X | |X |A Service Fabric toohave lehetővé teszi több környezet az alkalmazások vagy az alkalmazás-párhuzamos toodeploy különböző verziói. |
| Az operációs rendszer automatikus frissítéseit |X |X | | |Automatikus frissítések szolgáltatás az operációs rendszer egy jövőbeli Service Fabric vonatkozó tervezett. |
| Váltás zökkenőmentes platform (32 bites és 64 bites közötti egyszerű váltást) |X |X | | | |
| Kód a GIT-, FTP telepítése |X | |X | | |
| Kód a Web Deploy telepítése |X | |X | |Cloud Services támogatja a Web Deploy toodeploy frissítések tooindividual szerepkörpéldányokat hello használatát. Azonban nem használható egy szerepkör kezdeti telepítését, és ha használja a Web Deploy frissítési rendelkezik toodeploy külön-külön tooeach szerepkör példánya. Rendelés tooqualify hello Cloud Service SLA éles környezetekben a több példányt kell megadni. |
| WebMatrix támogatása |X | |X | | |
| Hozzáférés tooservices, például a Service Bus, a tárolás, az SQL-adatbázis |X |X |X |X | |
| Állomás web vagy webes szolgáltatások réteg többrétegű-architektúra |X |X |X |X | |
| Állomás középső réteg többrétegű-architektúra |X |X |X |X |App Service web apps könnyen üzemeltetéséhez egy REST API középső réteggel, és hello [WebJobs](http://go.microsoft.com/fwlink/?linkid=390226) szolgáltatás tárolhatja, háttérben történő feldolgozási feladatot. Webjobs-feladatok a egy külön webhelyen tooachieve független méretezhetősége hello szinthez futtatása. hello preview [API-alkalmazások](../app-service-api/app-service-api-apps-why-best-platform.md) szolgáltatás REST szolgáltatások üzemeltetéséhez szükséges további funkciókat biztosít. |
| Integrált MySQL,--szolgáltatás támogatása |X |X |X | |Felhőszolgáltatások integrálhatja MySQL,--szolgáltatás ClearDB tartozó ajánlatok keresztül, de nem hello Azure Portal munkafolyamat részeként. |
| Támogatja az ASP.NET, a klasszikus ASP, Node.js, PHP, Python |X |X |X |X |A Service Fabric támogatja hello létrehozását a webes előtér-használatával [ASP.NET 5](../service-fabric/service-fabric-add-a-web-frontend.md) vagy bármilyen (Node.js, Java, stb.) telepítheti egy [Vendég végrehajtható](../service-fabric/service-fabric-deploy-existing-app.md). |
| Horizontális felskálázás toomultiple példányok helyezze üzembe újra nélkül |X |X |X |X |Virtuális gépek toomultiple példányt lehet horizontálisan, de a rajtuk futó hello szolgáltatásokat úgy kell megírni, a kibővített toohandle. Egy terheléselosztó tooroute terhelésigényét tooconfigure rendelkezik hello számítógépeken, és hozzon létre egy Affinitáscsoporthoz tooprevent miatt toomaintenance vagy hardverhibákat összes példány egyidejű újraindul. |
| SSL támogatása |X |X |X |X |App Service web Apps az egyéni tartománynevek SSL csak Basic és Standard módban támogatott. A webalkalmazásokkal SSL használatával kapcsolatos információkért lásd: [konfigurálása az Azure-webhely SSL-tanúsítvány](app-service-web-tutorial-custom-ssl.md). |
| Visual Studio-integráció |X |X |X |X | |
| Távoli hibakeresés |X |X |X | | |
| A TFS-sel kód telepítése |X |X |X |X | |
| A hálózatelkülönítés [Azure-beli virtuális hálózathoz](/azure/virtual-network/) |X |X |X |X |Lásd még: [Azure-webhelyek virtuális hálózati integráció](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/) |
| Támogatja az [az Azure Traffic Manager](/azure/traffic-manager/) |X |X |X |X | |
| Integrált Végpontmonitoring kijelző |X |X |X | | |
| Távoli asztal eléréséhez tooservers | |X |X |X | |
| Minden egyéni MSI telepítése | |X |X |X |A Service Fabric lehetővé teszi a végrehajtható fájlok tárolási toohost egy [Vendég végrehajtható](../service-fabric/service-fabric-deploy-existing-app.md) vagy bármely alkalmazásba hello virtuális gépeken telepítheti. |
| Képes toodefine végrehajtása kezdeti feladatok | |X |X |X | |
| Figyelheti tooETW események | |X |X |X | |

## <a name="scenarios"></a>Forgatókönyvek és javaslatok
Toowhich Azure web üzemeltetési beállítás akkor lehet leginkább megfelelő minden, az alábbiakban néhány alkalmazás szabhatják a javaslatokat.

* [Kell egy előtér-webkiszolgáló és a háttérben történő feldolgozás adatbázis háttér toorun üzleti alkalmazások integrálva a helyszíni eszközök.](#onprem)
* [Kell egy megbízható módot toohost elérni a vállalati webhely, amelyet a méretezi well és globális ajánlatokat.](#corp)
* [Windows Server 2003 rendszeren futó IIS 6 alkalmazás van.](#iis6)
* [A kisméretű vállalkozások tulajdonosa vagyok, és van szükség az alacsony költségű módja toohost helyemről, de vegye figyelembe a jövőbeli növekedésre.](#smallbusiness)
* [Egy webes vagy egy grafikus tervező vagyok, és szeretnék toodesign kívánt és a webhelyek létrehozása a felhasználó.](#designer)
* [A többrétegű alkalmazást egy webes előtér-toohello felhő az I vagyok áttelepítése.](#multitier)
* [Saját alkalmazás függ a nagy mértékben testre szabott Windows vagy Linux környezetek és I szeretné toomove azt toohello felhő.](#custom)
* [A hely nyílt forráskódú szoftvert használ, és toohost kívánt azt az Azure-ban.](#oss)
* [Van egy üzleti alkalmazást, amelyet a tooconnect toohello vállalati hálózathoz.](#lob)
* [Szeretném toohost a mobil ügyfelek REST API vagy webes szolgáltatás.](#mobile)

### <a id="onprem"></a>Kell egy előtér-webkiszolgáló és a háttérben történő feldolgozás adatbázis háttér toorun üzleti alkalmazások integrálva a helyszíni eszközök.
Az Azure App Service egy olyan kiváló megoldás az összetett üzleti alkalmazások. Lehetővé teszi, hogy automatikusan méretezése terhelés kiegyensúlyozott platformon, és az Active Directory védett és csatlakozás tooyour a helyszíni erőforrások alkalmazások fejlesztéséhez. Ezek az alkalmazások könnyen egy világszínvonalú portál és API-k kezelése, és az hogyan használják az ügyfelek a őket az alkalmazás insight eszközök toogain betekintést lehetővé teszi. Hello [Webjobs] [ Webjobs] funkció lehetővé teszi a háttérfolyamatot futtatni, és a webes réteg közben a hibrid kapcsolat és a virtuális hálózat szolgáltatások részeként feladatok révén könnyen tooconnect hátsó tooon helyszíni erőforrások. Az Azure App Service web Apps három 9 SLA biztosít, és lehetővé teszi:

* Az alkalmazások futtatásához megbízhatóan öngyógyító, az automatikus javítás felhőalapú platformot.
* Méretezhető automatikusan adatközpontok globális hálózaton keresztül.
* Készítsen biztonsági másolatot, és állítsa vissza az adatok helyreállítását.
* Lehet, ISO SOC2 és PCI szabványoknak.
* Integráció az Active Directoryval

### <a id="corp"></a>Kell egy megbízható módot toohost elérni a vállalati webhely, amelyet a méretezi well és globális ajánlatokat.
Az Azure App Service kiválóan alkalmas a vállalati webhely üzemeltetéséhez. Lehetővé teszi a web apps tooscale gyorsan és könnyen toomeet igény adatközpontok globális hálózaton keresztül. Helyi reach hibatűrést és intelligens forgalom kezeléséhez biztosít. Minden platformon, amely világszínvonalú felügyeleti eszközöket biztosít így hely állapotának és a webhelyek forgalmát toogain betekintést gyorsan és egyszerűen. Az Azure App Service web Apps három 9 SLA biztosít, és lehetővé teszi:

* A webhelyek futtatható megbízhatóan öngyógyító, az automatikus javítás felhő platform.
* Méretezhető automatikusan adatközpontok globális hálózaton keresztül.
* Készítsen biztonsági másolatot, és állítsa vissza az adatok helyreállítását.
* Naplóinak kezeléséhez és a forgalom integrált eszközökkel.
* Lehet, ISO SOC2 és PCI szabványoknak.
* Integráció az Active Directoryval

### <a id="iis6"></a>Windows Server 2003 rendszeren futó IIS 6 alkalmazás van.
Az Azure App Service segítségével könnyen tooavoid hello áttelepítése régebbi IIS6 alkalmazások társított infrastruktúra fenntartási költségeit. A Microsoft létrehozott [részletes áttelepítési útmutató és könnyen toouse áttelepítési eszközök](https://www.movemetowebsites.net/) , amelyek lehetővé teszik a toocheck kompatibilitási, és azonosíthatja a kell toobe végrehajtott módosításokat. Integráció a Visual Studio, a TFS és a közös CMS eszközök segítségével könnyen toodeploy IIS6 alkalmazások közvetlen toohello a felhő. Amennyiben telepített, hello Azure Portal robusztus felügyeletet olyan eszközöket biztosít, amelyek lehetővé teszik tooscale toomanage költségek le, illetve a hierarchiában felfelé toomeet igény szerinti szükség szerint. Hello áttelepítési eszköz segítségével:

* Gyorsan és egyszerűen telepítse át a régebbi Windows Server 2003 web application toohello felhő.
* Részt vesz a tooleave a csatolt SQL adatbázis helyszíni toocreate egy hibrid alkalmazást.
* Automatikusan helyezze át az SQL-adatbázis és az örökölt alkalmazást.

### <a id="smallbusiness"></a>A kisméretű vállalkozások tulajdonosa vagyok, és van szükség az alacsony költségű módja toohost helyemről, de vegye figyelembe a jövőbeli növekedésre.
Az Azure App Service nem kiválóan alkalmas ebben a forgatókönyvben, mert használatba szabad, és hozzáadhatja további képességeket, ha szüksége van rájuk. Minden egyes szabad webalkalmazás tartalmaz egy Azure által biztosított tartományt (*your_company*. azurewebsites.net), és hello platform integrált üzembe helyezési és kezelési eszközök, valamint az alkalmazás gyűjteménye, melyek könnyen tooget elindult. Nem találhatók sok más szolgáltatások és méretezési beállításokat, amelyek lehetővé teszik a hello hely tooevolve az nagyobb felhasználói igényeknek. Az Azure App Service a következőket teheti:

* Ingyenes szint hello kezdődnie, és szükség szerint majd növelheti.
* Állítsa be a népszerű webes alkalmazásokhoz, például a WordPress hello Alkalmazáskatalógusában tooquickly használja.
* További Azure-szolgáltatások és funkciók tooyour alkalmazás hozzáadása igény szerint.
* Tegye biztonságossá webalkalmazását, a HTTPS.

### <a id="designer"></a>Egy webes vagy egy grafikus tervező vagyok, és I toodesign kívánt és a webhelyek a felhasználó létrehozása
Webes fejlesztők és tervezők körében Azure App Service számos keretrendszerek és eszközök könnyen integrálható, központi telepítés támogatja a Git és az FTP és eszközöket és szolgáltatásokat, például a Visual Studio és az SQL-adatbázis szoros integrációt nyújt. Az App Service-szel a következőket teheti:

* A parancssori eszközök [automatizált feladatokat][scripting].
* Például együttműködve népszerű nyelvhez [.Net][dotnet], [PHP][PHP], [Node.js][nodejs], és [Python][Python].
* Jelölje ki a toovery magas kapacitások vertikális felskálázásával három különböző méretezési szintjét.
* Más Azure-szolgáltatásokkal, például a integrálása [SQL-adatbázis][sqldatabase], [Service Bus] [ servicebus] és [tárolási] [ Storage], vagy a hello ajánlatok partnerek [Azure Store][azurestore], például a MySQL és a mongodb-Protokolltámogatással.
* Olyan eszközöket, például a Visual Studio, a Git, a WebMatrix, a WebDeploy, a TFS és az FTP integrálja.

### <a id="multitier"></a>A többrétegű alkalmazást egy webes előtér-toohello felhő az I vagyok áttelepítése
Ha a többrétegű alkalmazások, például webkiszolgálót, amely a tooa adatbázis fut Azure App Service, amely az Azure SQL Database szoros integrációt nyújt jó választás. És futó háttéralkalmazás folyamatokhoz hello WebJobs szolgáltatást is használhatja.

Egy vagy több, hogy a rétegek Service Fabric válassza, ha több kell hello kiszolgálói környezetben, például az a kiszolgálóra hello képességét tooremote vezérlése, vagy konfigurálja a kiszolgáló indítása feladatokat.

Válassza a virtuális gépek egy vagy több, hogy a rétegek Ha azt szeretné, hogy toouse a saját gép lemezképére, vagy futtassa a szoftver vagy szolgáltatásokhoz, amelyek nem lehet konfigurálni a Service Fabric.

### <a id="custom"></a>Saját alkalmazás függ a nagy mértékben testre szabott Windows vagy Linux környezetek és I szeretné toomove azt toohello felhő.
Ha az alkalmazás által igényelt összetett telepítést vagy a konfiguráció szoftver- és hello operációs rendszer, virtuális gépek valószínűleg hello legjobban megfelelő megoldást. A virtuális gépek a következőket teheti:

* Hello virtuális gép gyűjtemény toostart használja az operációs rendszert, például a Windows vagy Linux, és azután testre szabhatják alkalmazás igényeinek.
* Hozzon létre, és töltse fel az Azure virtuális gépen egy meglévő helyszíni kiszolgáló toorun egyéni képe.

### <a id="oss"></a>A hely nyílt forráskódú szoftvert használ, és toohost kívánt azt az Azure-ban
Ha a nyílt forráskódú keretrendszer App Service-ben hello nyelvek esetén támogatott, és az alkalmazás által igényelt keretrendszerek, automatikusan megtörténik. App Service lehetővé teszi:

* Számos népszerű nyitott forrás nyelven, használjon például [.NET][dotnet], [PHP][PHP], [Node.js][nodejs], és [Python][Python].
* Állítsa be a WordPress, Drupal, Umbraco, DNN és sok más külső webes alkalmazásokhoz.
* Telepítse át egy meglévő alkalmazást, vagy hozzon létre egy újat a hello Application Gallery.

Ha a nyílt forráskódú keretrendszer nem támogatott az App Service, futtatható hello egyik más Azure webszolgáltatási beállítások. Virtuális gépekkel, telepítése és konfigurálása hello szoftver a hello gép lemezképére, amely lehet a Windows vagy Linux-alapú.

### <a id="lob"></a>Van egy üzleti alkalmazást, amelyet a tooconnect toohello vállalati hálózat
Ha azt szeretné, hogy a sor üzleti alkalmazás toocreate, a webhely közvetlen hozzáférést tooservices vagy hello vállalati hálózaton lévő adatok lehet szükség. Ez az App Service, a Service Fabric és a hello használata virtuális gépek lehetséges [Azure Virtual Network szolgáltatás](/azure/virtual-network/). Az App Service hello használható [VNET integrációs szolgáltatás](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/), ami lehetővé teszi az Azure-alkalmazások toorun, mintha a vállalati hálózaton.

### <a id="mobile"></a>Szeretném toohost egy REST API vagy webes szolgáltatás mobil ügyfelek számára
HTTP-alapú webes szolgáltatások lehetővé teszik a toosupport rendszerűek, beleértve a mobil ügyfelek széles választékát. Például az ASP.NET Web API keretrendszerek integrálva a Visual Studio toomake könnyebb toocreate, és REST szolgáltatásait.  Ezek a szolgáltatások érhetők el a egy webszolgáltatási végpontot, ezért lehetséges toouse a webalkalmazás-üzemeltetési az eljárást a Azure toosupport ebben a forgatókönyvben. App Service azonban REST API-k tárolásához kiváló választás. Az App Service-szel a következőket teheti:

* Gyorsan létrehozhat egy [mobilalkalmazás](../app-service-mobile/app-service-mobile-value-prop.md) vagy [API-alkalmazás](../app-service-api/app-service-api-apps-why-best-platform.md) toohost hello HTTP webszolgáltatás egy Azure globálisan elosztott adatközpontokban.
* Telepítse át a meglévő szolgáltatások, vagy hozzon létre újakat.
* Szolgáltatásiszint-szerződés megvalósítása a rendelkezésre állás érdekében, ha a egyetlen példánya, vagy dedikált toomultiple gépek kiterjesztése.
* Használjon hello hely tooprovide REST API-k tooany HTTP-ügyfelek, beleértve a mobil ügyfelek közzé.

> [!NOTE]
> Ha azt szeretné, hogy az Azure App Service egy olyan fiók regisztrálása előtt lépései tooget, nyissa meg túl<a href="https://trywebsites.azurewebsites.net/">https://trywebsites.azurewebsites.net</a>, ahol azonnal létrehozhat egy rövid élettartamú alapszintű alkalmazást az Azure App Service szabad. Nincs szükség bankkártyára, nem jár kötelezettségekkel.
> 
> 

## <a id="nextsteps"></a>További lépések
Hello három webszolgáltatási beállítások kapcsolatos további információkért lásd: [Introducing Azure](../fundamentals-introduction-to-azure.md).

az alkalmazáshoz kiválasztott hello-beállítások használatába tooget tekintse meg a következő erőforrások hello:

* [Azure App Service](/azure/app-service/)
* [Azure Cloud Services](/azure/cloud-services/)
* [Az Azure virtuális gépek](/azure/virtual-machines/)
* [Service Fabric](/azure/service-fabric/)

<!-- URL List -->

[Azure App Service]: /azure/app-service/
[Cloud Services]: /azure/cloud-services/
[Virtual Machines]: /azure/virtual-machines/
[Service Fabric]: /azure/service-fabric/
[ClearDB]: http://www.cleardb.com/
[WebJobs]: http://go.microsoft.com/fwlink/?linkid=390226&clcid=0x409
[Configuring an SSL certificate for an Azure Website]: app-service-web-tutorial-custom-ssl.md
[azurestore]: https://azuremarketplace.microsoft.com/en-us/marketplace/apps
[scripting]: https://azure.microsoft.com/documentation/scripts/?services=web-sites
[dotnet]: https://azure.microsoft.com/develop/net/
[nodejs]: https://azure.microsoft.com/develop/nodejs/
[PHP]: https://azure.microsoft.com/develop/php/
[Python]: https://azure.microsoft.com/develop/python/
[servicebus]: /azure/service-bus/
[sqldatabase]: /azure/sql-database/
[Storage]: /azure/storage/

<!-- IMG List -->

[ChoicesDiagram]: ./media/choose-web-site-cloud-service-vm/Websites_CloudServices_VMs_3.png
