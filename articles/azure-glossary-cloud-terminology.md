---
title: "aaaAzure szószedet - Azure szótár |} Microsoft Docs"
description: "Használjon hello Azure szószedet toounderstand felhő terminológia hello Azure platformon. A rövid Azure szótár biztosít jelentésdefiníciókat közös felhő feltételek az Azure-bA."
keywords: "Azure szótár, felhőalapú terminológia, Azure szószedet, terminológiai definíciók, felhőalapú feltételek"
services: na
documentationcenter: na
author: MonicaRush
manager: jhubbard
editor: 
ms.assetid: d7ac12f7-24b5-4bcd-9e4d-3d76fbd8d297
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: monicar
ms.openlocfilehash: 486bbbfc71a48a6ebc39b14f7ab71f8604b7a6b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-azure-glossary-a-dictionary-of-cloud-terminology-on-hello-azure-platform"></a>A Microsoft Azure szószedet: dictionary hello Azure platformon a felhő-terminológia

hello Microsoft Azure szószedete rövid dictionary felhő terminológia a hello Azure platformon. Lásd még:

* [A Microsoft Azure és az Amazon Web Services](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/) -definíciók az Azure-szolgáltatások és AWS másolataik.<!-- I propose toolink toohttps://azure.microsoft.com/en-us/services/ instead of this -->
* [A felhő számítási feltételek](https://azure.microsoft.com/overview/cloud-computing-dictionary/) -általános iparági felhő feltételeket.

## <a name="account"></a>Fiók
Egy fiók, amely tooaccess használatban van, és az Azure-előfizetés kezeléséhez. Gyakran van hivatkozott tooas egy Azure fiók, bár egy fiók is lehet ezek egyikét sem: egy meglévő munkahelyi, iskolai, vagy személyes Microsoft-fiók, vagy egy Office 365 felhasználói nevét és jelszavát. Is létrehozhat egy fiók toomanage Azure-előfizetés hello a Feliratkozás [ingyenes próbaverzió](https://azure.microsoft.com).  
Lásd: [Azure-előfizetéssel az Office 365-fiókkal regisztrálhat](billing/billing-use-existing-office-365-account-azure-subscription.md) és [fiókokat is használhatja a toosign](active-directory/active-directory-how-subscriptions-associated-directory.md).

## <a name="api-app"></a>API-alkalmazás
Egy másik nevet [App Service alkalmazás](#app-service-app).

## <a name="app-service-app"></a>App Service alkalmazás
hello számítási erőforrásokat, amelyek [Azure App Service](app-service/app-service-value-prop-what-is.md) nyújt üzemeltetéséhez egy [webhelybet vagy webalkalmazást](app-service-web/app-service-web-overview.md), [webes API-t](app-service-api/app-service-api-apps-why-best-platform.md), vagy [mobil-háttéralkalmazás](app-service-mobile/app-service-mobile-value-prop.md). App Service alkalmazások is hivatkozott tooas *alkalmazásszolgáltatások*, *webalkalmazások*, *API-alkalmazások*, és *mobilalkalmazások*.

## <a name="availability-set"></a>A rendelkezésre állási csoport
Virtuális gépek, amelyek gyűjteménye felügyelete együtt, tooprovide alkalmazás redundanciát és megbízhatóságát. rendelkezésre állási csoportok hello használata biztosítja, hogy tervezett vagy nem tervezett karbantartási esemény során legalább egy virtuális gép elérhető.  
Lásd: [hello Windows virtuális gépek rendelkezésre állásának kezelése](virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) és [hello Linux virtuális gépek rendelkezésre állásának kezelése](virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="classic-model"></a>Az Azure klasszikus telepítési modell
Egy két [üzembe helyezési modellel](resource-manager-deployment-model.md) toodeploy erőforrások használt Azure-ban (hello új modelljének az Azure Resource Manager). Egyes Azure-szolgáltatásokhoz csak hello Resource Manager üzembe helyezési modellben támogatja, néhány csak hello klasszikus üzembe helyezési modellt támogatja, és néhány támogatja. minden Azure szolgáltatás hello dokumentációja határozza meg, mely támogatják-e a modellek.

## <a name="cli"></a>Azure parancssori felület (CLI)
Egy parancssori felületet, amely használt toomanage Azure Linux, Windows és macOS szolgáltatásokat.  Egyes szolgáltatásokat vagy a szolgáltatások csak a PowerShell vagy a hello CLI keresztül kezelheti. Lásd: [Azure CLI 2.0](/cli/azure/overview)

## <a name="powershell"></a>Az Azure PowerShell
A parancssori felület toomanage Azure szolgáltatások a Windows rendszerű számítógépeken a parancssorból. Egyes szolgáltatásokat vagy a szolgáltatások csak a PowerShell vagy a hello CLI keresztül kezelheti.
Lásd: [hogyan tooinstall Azure PowerShell és konfigurálása](/powershell/azure/overview)

## <a name="arm-model"></a>Az Azure Resource Manager telepítési modell
Egy két [üzembe helyezési modellel](resource-manager-deployment-model.md) használt toodeploy erőforrásokat (más hello hello klasszikus üzembe helyezési modellel) a Microsoft Azure-ban. Egyes Azure-szolgáltatásokhoz csak hello Resource Manager üzembe helyezési modellben támogatja, néhány csak hello klasszikus üzembe helyezési modellt támogatja, és néhány támogatja. minden Azure szolgáltatás hello dokumentációja határozza meg, mely támogatják-e a modellek.

## <a name="fault-domain"></a>Hibatartomány
virtuális gépek, amelyek esetleg meghiúsulhat: hello azonos rendelkezésre állási csoportba hello idő. Példa: gépcsoport szekrényben, amelyek egy közös power forrás- és a hálózati kapcsolóhoz. Az Azure hello virtuális gépek rendelkezésre állási csoportba automatikusan egymástól több tartalék tartományokban.  
Lásd: [hello Windows virtuális gépek rendelkezésre állásának kezelése](virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) vagy [hello Linux virtuális gépek rendelkezésre állásának kezelése](virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)  

## <a name="geo"></a>földrajzi
A két vagy több régió általában tartalmazó adatok rezidens meghatározott határ. hello határain belül vagy kívül nemzeti szegélyek, és adó szabályozás befolyásolják. Minden földrajzi legalább egy régió tartozik. Geos példák Ázsia Csendes-óceáni és japán. Más néven *geográfiai*.  
Lásd: [Azure-régiók](best-practices-availability-paired-regions.md)

## <a name="geo-replication"></a>a georeplikáció
hello folyamat automatikusan replikálása tartalmának, például a blobot, táblát és üzenetsort, egy regionális pár belül.  
Lásd: [aktív Georeplikáció Azure SQL-adatbázis](sql-database/sql-database-geo-replication-overview.md)
<!-- hello meaning of "geo" in this term seems toobe different than hello meaning provided in hello "geo" entry -->

## <a name="image"></a>Kép
Tartalmazó fájl hello operációs rendszer és az alkalmazás konfigurációját, amelyek használt toocreate lehetnek a virtuális gépek száma. Az Azure-ban lemezképek két típusa van: virtuális gép lemezképét és az operációsrendszer-lemezképek. A Virtuálisgép-lemezkép az operációs rendszer tartalmazza, és az összes lemez tooa virtuális gép csatlakoztatva, hello kép létrehozásakor. Az operációsrendszer-lemezképek operációs rendszer nélküli adatok lemezkonfigurációkkal valóüzemeltetése csak egy általánosított tartalmazza.  
Lásd: [keresse meg és jelölje be a Windows virtuális gép képfájljait az Azure PowerShell vagy a hello CLI](virtual-machines/windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="limits"></a>Korlátok
erőforrások hozhatók létre, vagy a teljesítmény teljesítményteszt elérhető hello hello száma. Korlátok általában társított előfizetéseket, szolgáltatások és ajánlatokat.  
Lásd: [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](azure-subscription-service-limits.md)

## <a name="load-balancer"></a>Terheléselosztó
Ez az erőforrás osztja el a bejövő forgalmat a hálózaton lévő számítógépek között. Az Azure terheléselosztó osztja el a forgalmat toovirtual gépek definiált egy terheléselosztó készlet. A [terheléselosztó](load-balancer/load-balancer-overview.md) internetre is lehetnek, vagy belső lehet.  

## <a name="mobile-app"></a>mobilalkalmazás
Egy másik nevet [App Service-alkalmazást](#app-service-app).

## <a name="offer"></a>az ajánlat
hello árképzési, kreditek és a kapcsolódó fogalmak alkalmazható tooan Azure-előfizetés.  
Lásd: hello [Azure-ajánlat a Részletek lap](https://azure.microsoft.com/support/legal/offer-details/)

## <a name="portal"></a>portal
hello biztonságos webes portál toodeploy használt, és kezelése az Azure-szolgáltatásokhoz.  Nincsenek a két portál: hello [Azure-portálon](http://portal.azure.com/) és hello [klasszikus portál](http://manage.windowsazure.com/). Egyes szolgáltatások érhetők el mindkét portálok, míg mások csak érhetők el az egyik, vagy más hello. Hello [az Azure portál elérhetőségi diagram](https://azure.microsoft.com/features/azure-portal/availability/) által mely portálon elérhető szolgáltatások listája.

## <a name="region"></a>Régió
Olyan, amely közötti nemzeti szegélyek, és egy vagy több adatközpontok tartalmaz egy földrajzi területen. Árképzési regionális szolgáltatások és típusú hello régió szinten érhetők el. A régió általában párosítani egy másik régióban, amely tooseveral száz miles azonnal fel lehet. Regionális párok alkalmas mechanizmusaként vész-helyreállítási és magas rendelkezésre állás elérésére. Más néven tooas *hely*.  
Lásd: [Azure-régiók](best-practices-availability-paired-regions.md)

## <a name="resource"></a>Erőforrás
Az Azure-megoldás részét képező cikk. Minden Azure szolgáltatás lehetővé teszi toodeploy különböző típusú erőforrások, például adatbázisok vagy a virtuális gépek.   
Lásd: [Azure Resource Manager áttekintése](azure-resource-manager/resource-group-overview.md)

## <a name="resource-group"></a>erőforráscsoport
A tároló az erőforrás-kezelőben, amely az alkalmazáshoz kapcsolódó erőforrásokat tárol. hello erőforráscsoport tartalmazhatnak valamennyi hello erőforrások az alkalmazáshoz, és csak azokat az erőforrásokat, amelyek logikailag egy csoportba tartoznak. Eldöntheti, hogyan tooallocate erőforrások tooresource csoportok alapján hasznossá hello a legtöbb, a szervezet számára legjobb.  
Lásd: [Azure Resource Manager áttekintése](azure-resource-manager/resource-group-overview.md)

## <a name="arm-template"></a>Resource Manager-sablon
A JSON-fájl egy vagy több Azure-erőforrások deklarációval meghatározó és meghatározó hello közötti függőségek telepített erőforrásokhoz. hello sablon használt toodeploy hello erőforrások csak következetesen és ismételten.  
Lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md)

## <a name="resource-provider"></a>Erőforrás-szolgáltató
Egy szolgáltatás, amely megadja az erőforrások hello telepítheti és kezelheti a Resource Manager használatával. Mindegyik erőforrás-szolgáltató telepített hello erőforrásaival kapcsolatos műveletek kínál. Erőforrás-szolgáltatók hello Azure-portálon az Azure PowerShell, és több programozási SDK is elérhetőek.  
Lásd: [Azure Resource Manager áttekintése](azure-resource-manager/resource-group-overview.md)

## <a name="role"></a>Szerepkör
Eszköz, amely hozzárendelhető toousers, a csoportok és a szolgáltatások hozzáférés szabályozása. Szerepkörök képes tooperform műveletek, mint például létrehozása, kezelése, és olvassa el az Azure-erőforrások.  
Lásd: [RBAC: beépített szerepkörök](active-directory/role-based-access-built-in-roles.md)

## <a name="sla"></a>Szolgáltatásiszint-szerződéssel (SLA)
Hasznos üzemidő és a kapcsolatot a Microsoft felé vállalt kötelezettségeinket leíró hello szerződést. Minden Azure szolgáltatásnak van egy adott SLA-t.  
Lásd: [szolgáltatási szintek](https://azure.microsoft.com/support/legal/sla/)

## <a name="sas"></a>közös hozzáférésű jogosultságkód (SAS)
Olyan aláírása, amely lehetővé teszi anélkül, hogy a fiókkulcs toogrant korlátozott hozzáférés tooa erőforrás. Például [Azure Storage használt SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) toogrant client access tooobjects például blobokat. [Az IoT-központ által használt SAS](iot-hub/iot-hub-devguide-security.md#security-tokens) toogrant eszközök engedély toosend telemetriai adatokat.

## <a name="storage-account"></a>Tárfiók
Amely lehetővé teszi az Azure Storage Azure Blob, a várólista, a tábla és a fájl szolgáltatások toohello eléréséhez. a tárfiók neve hello hello egyedi névteret Azure Storage-adatobjektumok határozza meg.  
Lásd: [tudnivalók az Azure storage-fiókok](storage/common/storage-create-storage-account.md)

## <a name="subscription"></a>előfizetést
Az ügyfél áll-e a Microsoft tooobtain Azure lehetővé tevő szolgáltatás. hello előfizetés árak és a kapcsolódó fogalmak hello előfizetés a kiválasztott hello ajánlat szabályozzák.
Lásd: [Microsoft Online előfizetői szerződés](https://azure.microsoft.com/support/legal/subscription-agreement/) és [kapcsolódnak hogyan Azure-előfizetések az Azure Active Directoryval](active-directory/active-directory-how-subscriptions-associated-directory.md)

## <a name="tag"></a>Címke
Az indexelő kifejezés, amely lehetővé teszi toocategorize erőforrások tooyour felügyeleti vagy számlázási követelményeinek megfelelően. Ha rendelkezik olyan összetett erőforrások gyűjteménye, használható címkék toovisualize azok az eszközök hello lehető legcélszerűbb hello módon. Például elláthat címkével olyan erőforrásokat, amelyek hasonló szerepet töltenek be a szervezet vagy toohello tartozik részleghez.  
Lásd: [címkéket tooorganize az Azure-erőforrások használata](resource-group-using-tags.md)

## <a name="update-domain"></a>Tartomány frissítése
virtuális gépek rendelkezésre állási csoportba: hello frissített hello ugyanannyi időt vesz igénybe. A tervezett karbantartások hello azonos frissítési tartományban együtt újraindítása közben a virtuális gépek. Azure soha nem indít egynél több frissítési tartományt egyszerre. Más néven tooas frissítési tartományokhoz.  
Lásd: [hello Windows virtuális gépek rendelkezésre állásának kezelése](virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) és [hello Linux virtuális gépek rendelkezésre állásának kezelése](virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="vm"></a>virtuális gép
hello szoftver végrehajtása egy operációs rendszert futtató fizikai számítógépre. Több virtuális gép az egyidejűleg futtatható hello ugyanazt a hardvert. Az Azure virtuális gépek elérhetők a különböző méretű.  
Lásd: [Virtual Machines – dokumentáció](https://azure.microsoft.com/documentation/services/virtual-machines/)

## <a name="vm-extension"></a>Virtuálisgép-bővítmény
Viselkedéshez vagy szolgáltatásokat vagy más programok működik, vagy adjon meg hello képességét toointeract futó számítógép megvalósító erőforrás. Például használhatja hello VM hozzáférés bővítmény tooreset vagy módosíthatja a távelérés értékek Azure virtuális géphez.
<!-- This definition seems obscure toome; maybe a list of examples would work better than a conceptual definition? -->
Lásd: [virtuálisgép-bővítmények és szolgáltatásokról (Windows)](virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) vagy [virtuálisgép-bővítmények és szolgáltatásokról (Linux)](virtual-machines/linux/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="vnet"></a>virtuális hálózat
A hálózat, amely kapcsolatot biztosít az Azure-erőforrások, hogy el van különítve a többi Azure bérlők között. Egy [Azure VPN Gateway](vpn-gateway/vpn-gateway-about-vpngateways.md) lehetővé teszi, hogy a virtuális hálózatok közötti kapcsolatokat létesíteni és [egy virtuális és egy a helyszíni hálózat között](vpn-gateway/vpn-gateway-plan-design.md). Hello IP-címblokkok, a DNS-beállítások, a biztonsági házirendek és a virtuális hálózaton belül az útvonaltáblák teljes mértékben irányíthatja.  
Lásd: [Virtual Network áttekintése](virtual-network/virtual-networks-overview.md)  

## <a name="web-app"></a>Webalkalmazás
Egy másik nevet [App Service-alkalmazást](#app-service-app).

## <a name="see-also"></a>Lásd még:

* [Ismerkedés az Azure-ral](https://azure.microsoft.com/get-started/)
* [Felhő erőforrás center](https://azure.microsoft.com/resources/)  
* [Az üzleti alkalmazás az Azure-ral](https://azure.microsoft.com/overview/business-apps-on-azure/)
* [Az Adatközpont Azure](https://azure.microsoft.com/overview/business-apps-on-azure/)

