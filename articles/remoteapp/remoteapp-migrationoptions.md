---
title: "az Azure RemoteApp kívüli áttelepítése aaaOptions |} Microsoft Docs"
description: "További tudnivalók az Azure RemoteApp kívüli áttelepítése hello-beállítások."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: c4e0e5bc-5c13-4487-b1b6-ebf2a5edc1f0
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 75324597881520d0c75939983b728ae9bbd7f436
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="options-for-migrating-out-of-azure-remoteapp"></a>Az Azure RemoteApp kívüli áttelepítése beállítások
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.


Ha az Azure RemoteApp segítségével hello miatt megszüntette [használatból való kivonást közlemény](https://go.microsoft.com/fwlink/?linkid=821148) vagy befejezte a próbaverzió, mert ki az Azure RemoteApp tooanother alkalmazásszolgáltatási toomigrate van szükség. Áttelepítés két különböző megközelítés: egy önállóan felügyelt (gyakran nevezik infrastruktúra szolgáltatásként [IaaS]) üzembe helyezése vagy egy teljes körűen felügyelt (más néven Platformszolgáltatást) vagy [PaaS/SaaS] szolgáltatott szoftver kínál. 

Önkiszolgáló IaaS saját munka központi telepítések, felügyelt, üzemeltetett, és a saját tulajdonában, közvetlenül telepített virtuális gépek (VM) vagy a fizikai rendszerek. Más hello: végén, egy teljes körűen felügyelt PaaS/SaaS kínál az Azure RemoteApp hasonlít - partner biztosít szolgáltatás szintű rétege egy távelérési megoldás, amely kezeli a működési és karbantartás közben, hello ügyfélként néhány lemezképet és az alkalmazás felügyeleti.

[Áttelepítési lehetőségek hello Azure RemoteApp előadások megtekintése](https://social.msdn.microsoft.com/Forums/azure/40557aaa-3e9f-403c-b221-ad3eac10dc56/migration-option-webinar-recordings?forum=AzureRemoteApp), vagy olvassa el a további tudnivalókat (többek között különböző beállításokat tartalmazó hello példát).

## <a name="self-managed-iaas-solutions"></a>Önálló felügyelt (IaaS) megoldások
### <a name="rds-on-iaas"></a>**Infrastruktúra-szolgáltatási a távoli asztali szolgáltatások**
Telepítése natív munkamenet-alapú távoli asztali szolgáltatások (a Windows Server) használó telepítés RemoteApp vagy asztali számítógépek helyszíni vagy üzemeltetett környezetben (hasonló Azure virtuális gépeken). Távoli asztali szolgáltatások IaaS üzemelő példányok esetében az ügyfelek már ismeri a legjobb és, amelyek a meglévő technikai segítséget az RDS központi telepítésével. 

> [!NOTE]
> Szükség van mennyiségi licencelési szoftverek megbízhatósági (SA) a távoli asztali ügyfél-hozzáférési licencek toouse ennek e telepítési beállításnak.

Az Azure virtuális gépeken futó távoli asztali szolgáltatások telepítése az egyszerűbb, mint legalább egyszer használatos üzembe helyezési és sablonok javítását (olvasni egy [áttekintése](https://blogs.technet.microsoft.com/enterprisemobility/2015/07/13/azure-resource-manager-template-for-rds-deployment/) , majd [nyissa meg azokat](https://aka.ms/rdautomation)). Kaphat hello Azure klasszikus üzembe helyezési modell erőforrások (nem az Azure erőforrás-modellje erőforrások) Azure RemoteApp belül az azonos rugalmas méretezési képességeket hello segítségével [automatikus skálázás parancsfájl](https://gallery.technet.microsoft.com/scriptcenter/Automatic-Scaling-of-9b4f5e76), bár vannak további testreszabásokat és beállításokat. Az Azure virtuális gépeken futó távoli asztali szolgáltatások telepítésekor támogatási keresztül valósul meg [Azure támogatja](https://azure.microsoft.com/support/plans/), azonos támogatási szakemberek számára, amelyet támogat, és az Azure RemoteApp hello. Akkor is beolvasása költségeket, a meglévő használati elvégezzék becslése [Azure támogatási](https://azure.microsoft.com/support/plans/), vagy saját kezűleg számítások hajthat végre használatával egy hamarosan toobe kiadott költség Számológép.  Emellett N sorozatú virtuális gépe (jelenleg magán előnézetben) hozzáadhat vGPU - túl hall több vGPU hozzáadásáról és arról, hogyan[hasznosítására távoli asztali szolgáltatások fejlesztések a Windows Server 2016](https://myignite.microsoft.com/videos/2794) az ignite-on munkamenetben.   

Részletes üzembe helyezési útmutatók a tudunk [Windows Server 2012 R2](http://aka.ms/rdsonazure) és [Windows Server 2016](http://aka.ms/rdsonazure2016) tooassist a telepítés. Tekintse meg a hello [távoli asztal blog](https://blogs.technet.microsoft.com/enterprisemobility/?product=windows-server-remote-desktop-services) a legújabb híreket hello.

### <a name="citrix-on-iaas"></a>**Az infrastruktúra-szolgáltatási Citrix**
Egy natív Citrix telepítési XenApp munkamenet-alapú vagy XenDesktop lehet telepíteni a helyszíni vagy üzemeltetett környezetben (például Azure virtuális gépeken). 

Tekintse meg a hello részletes telepítési útmutató, [Citrix XA 7.6 Azure](http://www.citrixandmicrosoft.com/Documents/Citrix-Azure Deployment Guide-v.1.0.docx), további információt. Tudjon meg többet az [Azure Citrix](http://www.citrixandmicrosoft.com/Solutions/AzureCloud.aspx), beleértve a ár Számológép. Is megkeresheti a [Citrix forduljon](http://citrix.com/English/contact/index.asp) toodiscuss a beállítások.

## <a name="fully-managed-paassaas-offerings"></a>Teljes körűen felügyelt (PaaS/SaaS) ajánlatok

### <a name="citrix-xenapp-essentials-released-april-2017"></a>Citrix XenApp Essentials (kiadott 2017. április)
Most már elérhető hello [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/Citrix.XenAppEssentials), Citrix XenApp Essentials hello új alkalmazás virtualizálási kiszolgáló, hello power kombinálásával és rugalmasságát hello Citrix felhőplatform hello egyszerű, előírásoknak megfelelő, és egyszerű felhasználásához látnia, Microsoft Azure RemoteApp. 

Meglévő Azure RemoteApp-ügyfeleknek is [regisztrálhat egy ingyenes próbaverzióra](https://www.citrix.com/products/citrix-cloud/form/xenapp-essentials-msft-trial/).  Megjegyzés: Csak a Citrix felhasználói szolgáltatást ingyenesen elérhető szabad, az Azure számítási és tárolási költségek alkalmazása

tudj meg többet:
- [Azure RemoteApp tooCitrix XenApp Essentials áttelepítése](remoteapp-migrate-citrix.md)
- [Citrix és a Microsoft](https://www.citrix.com/global-partners/microsoft/remote-app.html)
- [Citrix XenApp Essentials bemutató](https://www.youtube.com/watch?v=91Z7CCfQ-9k).  

### <a name="citrix-cloud-xenapp-service-and-xendesktop-service"></a>Citrix felhő XenApp és XenDesktop szolgáltatás 

[Citrix felhő XenApp és XenDesktop szolgáltatás](https://www.citrix.com/products/citrix-cloud/services.html) hello legjobb megoldás az alkalmazások és asztali számítógépek, valamint speciális kezelési és is figyelési képességek hello kézbesítését. 

#### <a name="conexlink-platform-name-mycloudit"></a>Conexlink (Platform nevének: MyCloudIT)
[MyCloudIT](https://mycloudit.com) használható automatizálási platform, az informatikai cégek toosimplify, optimalizálása, és a méret hello áttelepítési és a távoli asztali számítógépek, távoli alkalmazások és az infrastruktúra kézbesítését hello Microsoft Azure felhőbe. 

hello MyCloudIT platform telepítési idő csökkenti a költségeket, 30 %-kal Azure 95 %-kal, és az ügyfél teljes informatikai infrastruktúra helyezi a hangsúlyt hello felhő néhány billentyűparancsra be. Partnerek kezelheti az ügyfelek egy globális irányítópultról, szolgáltatás végfelhasználók hello világ tenni, mielőtt a soha nem, nő a bevételek többletterhelést vagy kiterjedt Azure képzési hozzáadása nélkül.  

> Elsődleges hely: Dallas, TX, USA
> 
> Művelet régió: világszerte
> 
> Partneri állapota: [arany](https://partnercenter.microsoft.com/pcv/solution-providers/conexlink_4298787366/843036_1?k=Conexlink)
> 
> Microsoft Felhőszolgáltató: Igen
> 
> Munkamenet-alapú RemoteApp- és asztali megoldásokat nyújtsanak: Igen, mindkettő
> 
> Az Azure RemoteApp áttelepítési megoldások: Igen, [további](https://mycloudit.com/remote-app-microsoft/)
> 
> Brian Garoutte, üzleti fejlesztését VP
> 
> Telefon: 972-218-0741
>   
> E-mail:[brian.garoutte@conexlink.com](mailto:brian.garoutte@conexlink.com)

### <a name="frame"></a>Keret

A vállalati és kormányzati, felügyelt szolgáltatók és vezető szoftvergyártók informatikai szervezetek keret toocreate kiválasztása, és a biztonságos-, szoftver-meghatározott munkaterületek hello felhőben kezelése. Kis toolarge vállalatoknak, keret segítségével nagyon egyszerűen toolet felhasználók férnek hozzá a Windows-alkalmazások bármelyik böngészőben bármilyen eszközről. hello keret platform tartalmaz mindent, ami egy rendszergazda igények toodeploy alkalmazások például hello Azure-infrastruktúra és a távoli asztali szolgáltatások licencek hello felhőből (a saját Azure-fiók és a licencek megadása nem kötelező). 

További információ [Azure keret](https://www.fra.me/ara). 

> Elsődleges hely: San Mateo, CA, USA
>
> Művelet régió: világszerte
>
> Microsoft-partnernek: Igen
> 
> Telefon: 1-480-269-4668

### <a name="awingu"></a>Awingu
Awingu örökölt alkalmazások, SaaS és dokumentumok fut egy html5 böngészőből egyszerű online munkaterület megoldást kínál. Ilyen elérhetővé alkalmazások biztonságosan az eszköz bármilyen típusú. A szolgáltatott szoftver szolgáltatásokat a beállítások széles köre op Single-Sign-On érhető el. Is sokoldalú (felhő) fájlok rendszerek mélyen integrálható a munkaterületre. Következő toofull mobilitási Awingu tartozó gazdag online munkaterület rendszerében optimális biztonsági részletes vezérlőkkel (pl. letöltése/feltöltése), teljes használati naplózást, a többtényezős hitelesítés (pl. Azure MFA), a munkamenet rögzítése és a további. Az a-kész, Awingu lehetővé teszi, hogy a dokumentum és az alkalmazás optimális és biztonságos együttműködési megosztásának munkamenet.
Awingu a megoldás, több-bérlős, több AD, és nyissa meg az API-t. Használják a kis és nagy vállalatok egyaránt Felhőbeli szolgáltatók és [ISV-k](http://www.isv2saas.com). Ezen ügyfelek különösen értékeljük hello egyszerű használat, alacsony és a könnyű telepítése teljes tulajdonlási költségek.

Awingu mindent tudó van [hello Azure piactéren elérhető](https://azuremarketplace.microsoft.com/marketplace/apps/awingu.awingu-arm) 2 beépített egyidejű felhasználókkal. További licencek keresztül érhetők el egy [számos forgalmazóknak és viszonteladók számára](http://www.awingu.com/reseller).

További információ [alternatív tooAzure RemoteApp, a Awingu](http://alternative-for-azure-remoteapp.awingu.com/).


> Elsődleges hely: Belgium
> 
> Régiókban működő: EMEA, Észak-Amerikából és Brazília
> 
> Munkamenet-alapú RemoteApp- és asztali megoldásokat nyújtsanak: Igen, mindkettő 
> 
> **Globális**:
> 
> Arnaud Marlière, a róluk szóló
> 
> E-mail:[arnaud@awingu.com](mailto:arnaud@awingu.com)
> 
> Telefon: +1 646 583 3025
> 
> **Belgium HQ**:
> 
> Ottergemsesteenweg-Zuid 808 B44
> 
> 9000 Gent
> 
> E-mail:[info@awingu.com](mailto:info@awingu.com) 
> 
> Telefon: +32 9 296 40 11
> 
> **USA**:
> 
> 7. emelet, 1177 Ave hello Americas, a
> 
> New York közt, NY 10036
> 
> E-mail:[info.us@awingu.com](mailto:info.us@awingu.com)

### <a name="microsoft-hosted-service-provider"></a>Microsoft-szolgáltatás szolgáltatót
Üzemeltetési partnerek az esetek többségében egy teljes körűen felügyelt Windows asztali üzemeltetett alkalmazásszolgáltatás, amelyek magukban foglalhatják kezelése hello Azure-erőforrások, operációs rendszerek, alkalmazások és segélyszolgálat hello partner használatával a Microsoft a licencszerződések és más szoftverszolgáltatóval együtt egy Szolgáltatóilicenc tooallow továbbértékesítése előfizető hozzáférési licenc (SAL) alatt. hello alábbi információkat jelenít meg adatokat és néhány hello infrastruktúrájáról szakosodott boltoknak az Azure RemoteApp áttelepítési rendelkező ügyfelek az elérhetőségeit. Tekintse meg [hello aktuális tárolt szolgáltatók listáját](http://aka.ms/rdsonazurecertified) hello elérési út és a frissítésfelmérő IaaS a távoli asztali szolgáltatások, amelyek végrehajtása.  

### <a name="citrix-service-provider-program"></a>Citrix Service Provider programot
Citrix Service Provider programot hello megkönnyíti a szolgáltatás szolgáltatók toodeliver hello egyszerűség virtuális a felhőalapú informatika tooSMBs, az egyszerű, használatalapú fizetési modell szeretnék hello szolgáltatásokat kínáló őket. Citrix szolgáltatók nő a Microsoft SPLA vállalatok számára, és bontsa ki a távoli asztali szolgáltatások platform beruházások bármely, helyfüggetlen hozzáférés, hello legszélesebb alkalmazások támogatása, a gazdag felhasználói élmény, a nagyobb biztonság és a jobb méretezhetőséget. Citrix szolgáltatók pedig további előfizetők vonzerőt, ügyfelek elégedettségének növelése, és a működési költségek csökkentése. [További](http://www.citrix.com/products/service-providers.html) vagy [partner keresése](https://www.citrix.com/buy/partnerlocator.html).

#### <a name="acuutech"></a>Acuutech
[Acuutech](http://www.acuutech.com) biztosítására szakosodott vállalattal teljes asztali és ISV alkalmazások kézbesítéséhez üzemeltetett asztali megoldásokat biztosító Microsoft technológia tooa globális ügyfél alap épülő Azure és a saját adatközpontját a feladatait.

> Elsődleges hely: London, UK; Jelen; Houston, TX
> 
> Művelet régió: világszerte
> 
> Partneri állapota: arany
> 
> Microsoft Felhőszolgáltató: Igen
> 
> Munkamenet-alapú RemoteApp- és asztali megoldásokat nyújtsanak: Igen, mindkettő
> 
> Az Azure RemoteApp áttelepítési megoldások: Igen, [további](http://www.acuutech.com/ara-migration/)
> 
> **Egyesült Királyság**:
>   
> 5/6 York House Langston közúti
>   
> Loughton, Essex IG10 3TQ
>   
> Telefon: +44 (0) 20 8502 2155
> 
> **Szingapúri**:
>   
> 100 Cecil utca, #09-02, 
>   
> Földgömb, szingapúri 069532 hello
> 
> Telefon: + 65 6709 4933
>   
> **Észak-Amerika**:
>   
> 3601 S. Sandman St.
>   
> Suite 200, Houston, TX 77098
>   
> Telefon: +1 713 691 0800

#### <a name="aspex"></a>ASPEX
[ASPEX](http://www.aspex.be/en) biztosítására szakosodott vállalattal tér át toohello felhő- és ISV ISV-k "toooptimize az aktuális felhő beállítások keresése. ASPEX azokat a felügyelt szolgáltatásokat, a devops és a tanácsadási szolgáltatásokat kínál.  

> Elsődleges hely: Antwerpen, Belgium
> 
> Művelet régió: Nyugat-Európa
> 
> Partneri állapota: [ezüst](https://partnercenter.microsoft.com/pcv/solution-providers/aspex_9397f5dd-ebdd-405b-b926-19a5bda61f7a/cfe00bac-ea36-4591-a60b-ec001c4c3dff)
> 
> Microsoft Felhőszolgáltató: Igen
> 
> Munkamenet-alapú RemoteApp- és asztali megoldásokat nyújtsanak: Igen, mindkettő
> 
> Az Azure RemoteApp áttelepítési megoldások: Igen, [további](https://www.aspex.be/en/azure-remote-apps)
> 
> Telefon: +3232202198
> 
> E-mail:[info@aspex.be](mailto:info@aspex.be)
> 
> Webes: [http://cloud.aspex.be/contact-ara-0](http://cloud.aspex.be/contact-ara-0)

#### <a name="caasecom"></a>Caase.com
[Caase.com](http://www.caase.com/) segítségével a vállalatok, helyi, nem állami szervek és kifizetni a Microsoft Cloud hello munka intelligensebb úgy útjuk egészségügyi intézmények. Bármely helyen bármely és alacsony informatikai költségekkel alatt álló hatékony és biztonságos. Caase.com Microsoft Office365, az Azure, a nagyvállalati mobilitás és a biztonsági és a Windows egy igaz specialistája. A tanácsadási, áttelepítési szolgáltatások, a bevezetési programok, az képzési kezelési és támogatási Caase.com hoz létre egy optimalizált és biztonságos platform együttműködésre is ügyfelek alkalmazottak, partnerek és szállítók számára.
Caase.com hello mastermind hello Azure távoli munkaterület (mobil munkahelyi) és hello digitális munkahelyi (közösségi Intranet). A két megoldás – valósítható meg bevezetésével – hello foundation, amely biztosítja, hogy ezek a megoldások hello felhasználók hello Kellemesen, sikeres és hatékony élmény legyenek az útvonal toohello Microsoft Cloud.
Holland fordítási ánd keresztül itt támogató film: http://caase.com/over-ons/

> Művelet régió: holland alapú, globális reach
> 
> Partneri állapota: [arany](https://partnercenter.microsoft.com/pcv/solution-providers/caasecom_4295593260/51159_3)
> 
> Microsoft Felhőszolgáltató: Igen
> 
> Munkamenet-alapú RemoteApp- és asztali megoldásokat nyújtsanak: Igen, mindkettő
> 
> Az Azure RemoteApp áttelepítési megoldások: Igen, [további](http://caase.com/diensten/microsoft-azure/).
> 
> 
> Hollandia:
> 
> Rigtersbleek-Zandvoort 10 (De Spinnerij)
> 
> 7521 BE, Enschede
> 
> Telefon: +31 (0) 88 4320 000


#### <a name="nerdio"></a>Nerdio
[Az Azure-Nerdio](http://getnerdio.com/nfa/) egy informatikai automatizálási platform ridiculously egyszerű kiépítés, felügyeleti és a teljes informatikai környezetek optimalizálási letölti a Microsoft cloud hello. Önálló virtuális asztalok, a távoli alkalmazások és a kiszolgálók a néhány óra múlva. Három kattintással hello környezet felügyeletéhez vagy kisebb Nerdio felügyeleti portálon. Használjon intelligens automatikus skálázást, és mentse too60 40 % Azure IaaS-erőforrásokra.

> Elsődleges hely: Chicago, Illinois művelet régió: világszerte Partner állapota: [arany](https://partnercenter.microsoft.com/en-us/pcv/solution-providers/adar-inc_341c9afa-f12c-46f5-8f7b-3f9ef59a66a5/3a7ae479-3ac2-42f6-84e2-d456dc7424e1) Microsoft Felhőszolgáltató: Igen
> 
> Munkamenet-alapú RemoteApp- és asztali megoldásokat nyújtsanak: Igen, mindkettő
> 
> Az Azure RemoteApp áttelepítési megoldások: Igen
> 
> 
> 8001 Lincoln Ave
> 
> Suite 212
> 
> Skokie, IL 60077
> 
> USA
> 
> (844) 4NERDIO kiegészítő 6
> 
> [sayhello@getnerdio.com](mailto:sayhello@getnerdio.com)

#### <a name="saasplaza"></a>**SaaSplaza**
[SaaSplaza](http://www.saasplaza.com/) kínál a teljes Microsoft Dynamics (NAV, AX, a csoportházirend, SA, CRM) kaphat privát és nyilvános Azure-felhőbe.

> Elsődleges hely: holland
> 
> Művelet régió: világszerte
> 
> Partneri állapota: [arany](https://partnercenter.microsoft.com/pcv/solution-providers/saasplaza_4295495801/791011_2?k=saasplaza)
> 
> Microsoft Felhőszolgáltató: Igen
> 
> Munkamenet-alapú RemoteApp- és asztali megoldásokat nyújtsanak: Igen, mindkettő
> 
> **EMEA**:
> 
> Prins Mauritslaan 29-35
> 
> 71 LP Badhoevedorp
> 
> hello Hollandia
> 
> Telefon: +31 20 547 8060 
> 
>  **Dél-Amerika**:
> 
> 171 Szászország közúti, Suite 105
> 
> Encinitas, CA 92024
> 
> San Diego
> 
> Egyesült Államok
> 
> Telefon: +1 858 385 8900 
> 
> **APAC**:
> 
> 105 Cecil utca.
>    
> \#11-08, hello nyolcszög
> 
> Szingapúri 069534
> 
> Szingapúr
>   
> Telefonkönyv - szingapúri: + 65 6222 6591
> 
> Telefonkönyv - Ausztrália: +61 2 8310 5568 
>    
> Telefonkönyv - Új-Zéland: + 64 4 488 0321
> 
## <a name="need-more-help"></a>További segítségre van szüksége?
Továbbra is súgó kiválasztása vagy további kérdései? A következő módszerek tooget súgó hello egyikét használhatja. 

1. E-mailben nekünk az [ arainfo@microsoft.com ](mailto:arainfo@microsoft.com).
2. Ügyfél [az Azure támogatási](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Első lépésként megnyitása egy [az Azure támogatási esetet](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
3. Hívjon fel minket. [Helyi értékesítési számos](https://azure.microsoft.com/overview/sales-number/).

