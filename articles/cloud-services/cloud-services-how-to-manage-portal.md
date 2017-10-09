---
title: "aaaCommon felhőalapú szolgáltatás felügyeleti feladatai |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage cloud services – a hello Azure-portálon. Ezekben a példákban hello Azure-portálon."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: cb218ad9-77d4-4149-83db-71159c00767e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: ade8a18a7754edbaae4903230c26c009fef63ed7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-cloud-services"></a>TooManage Cloud Services
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-manage-portal.md)
> * [klasszikus Azure portál](cloud-services-how-to-manage.md)
>
>

A hello **Felhőszolgáltatások (klasszikus)** területén hello Azure portál, lehet frissíteni a szerepkör-szolgáltatás vagy a központi telepítés, előléptetni egy szakaszos telepítés tooproduction, hivatkozás erőforrások tooyour felhőalapú szolgáltatás, így megtekintheti a hello erőforrás függőségek méretezési erőforrások hello együtt és egy felhőalapú szolgáltatás, vagy a központi telepítés törlése.

További információ arról, hogyan tooscale a felhőalapú szolgáltatás elérhető [Itt](cloud-services-how-to-scale-portal.md).

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Hogyan: Felhő szerepkör-szolgáltatás vagy a központi telepítés
Ha a felhőszolgáltatás tooupdate hello alkalmazáskód kell, akkor **frissítés** hello cloud service panelen. Egyetlen szerepkör vagy az összes szerepkör frissítheti. tooupdate, feltöltheti egy új service-csomag vagy a szolgáltatás konfigurációs fájljában.

1. A hello [Azure-portálon][Azure portal], válassza ki a kívánt tooupdate hello felhőalapú szolgáltatás. Ez a lépés hello cloud service példány paneljének megnyitása.
2. Hello paneljén kattintson hello **frissítés** gombra.

    ![Frissítés gomb](./media/cloud-services-how-to-manage-portal/update-button.png)

3. Hello üzemelő példány frissítése egy új felhőszolgáltatás csomagfájlját (.cspkg) és a szolgáltatás konfigurációs fájlját (.cscfg).

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. **Opcionálisan** hello üzemelő példány címkéje és hello storage-fiók frissítéséhez.
5. Ha a szerepkörök csak egy szerepkör példánya, válassza ki a hello **üzembe helyezés akkor is, ha egy vagy több szerepkör egyetlen példányt tartalmaz** tooenable hello frissítési tooproceed.

    Azure is csak 99,95 % szolgáltatás rendelkezésre állásának biztosítása a felhőalapú szolgáltatás frissítése közben. Ha szerepkörönként legalább két szerepkörpéldányokat (virtuális gépek). Két szerepkör osztályt egy virtuális gép ügyfél kérelmeket dolgozza fel más hello frissítése közben.

6. Ellenőrizze **indítsa el a központi telepítési** toohave hello frissítés alkalmazása hello csomag hello feltöltés befejeződése után.
7. Kattintson a **OK** toobegin hello szolgáltatás frissítése.

## <a name="how-to-swap-deployments-toopromote-a-staged-deployment-tooproduction"></a>Hogyan: felcserélése központi telepítések toopromote egy szakaszos telepítés tooproduction
Ha egy felhőalapú szolgáltatás, a szakasz az új kiadási toodeploy döntse el, és tesztelje az új kiadási a felhőalapú szolgáltatás tesztelési környezetben. Használjon **felcserélése** tooswitch hello URL-címek mely hello két központi telepítések tárgyalja, és lépteti elő egy új kiadási tooproduction.

Hello telepítéseit kicserélheti **Felhőszolgáltatások** lap vagy hello irányítópult.

1. A hello [Azure-portálon][Azure portal], válassza ki a kívánt tooupdate hello felhőalapú szolgáltatás. Ez a lépés hello cloud service példány paneljének megnyitása.
2. Hello paneljén kattintson hello **felcserélése** gombra.

    ![Cloud Services lapozófájl-kapacitás](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. hello következő megerősítési kérés nyílik meg.

    ![Cloud Services lapozófájl-kapacitás](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. Miután meggyőződött hello információkat, kattintson a **OK** tooswap hello központi telepítéseket.

    hello telepítési swap gyorsan mert megváltoztató egyedül hello hello virtuális IP-címek (VIP) hello telepítésekhez.

    toosave számítási költségek, a központi telepítés átmeneti, miután ellenőrizte, hogy a várt módon működik-e az éles telepítési hello törölheti.

### <a name="common-questions-about-swapping-deployments"></a>Központi telepítések csere kapcsolatos gyakori kérdésekre

**Mik azok a telepítések csere hello előfeltételei**

Nincsenek a sikeres telepítés swap két fő előfeltételei:

- Ha azt szeretné, hogy egy statikus IP-cím toouse a termelési aljzat, kell lefoglalni egy a, valamint az átmeneti helyet. Ellenkező esetben hello swap meghiúsul.

- A szerepkörök az összes példányát kell futnia, hello swap elvégzése előtt. Ellenőrizheti a hello Azure-portálon hello áttekintése panel példányai hello állapotát. Másik lehetőségként használhatja a hello [Get-AzureRole](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0) Windows PowerShell parancsot.

Vegye figyelembe, hogy a vendég operációs rendszer frissítése és a szolgáltatás javító műveleteket is eredményezheti, hogy központi telepítési toofail cseréje. További információkért lásd: [felhőalapú szolgáltatás központi telepítési problémák elhárítása](cloud-services-troubleshoot-deployment-problems.md).

**A lapozófájl-kapacitás negatívan befolyásolja az alkalmazáshoz állásidő? Hogyan tudom kezelje azt?**

Hello utolsó szakaszban leírt módon egy központi telepítési felcserélés, általában gyors, mert csak egy hello Azure terheléselosztó a konfiguráció megváltozott. Néhány esetben azonban képes tíz vagy több másodpercre és átmeneti kapcsolati hibákat eredményezhet. toolimit hatás tooyour ügyfelek, vegye fontolóra [ügyfél újrapróbálkozási logika](../best-practices-retry-general.md).

## <a name="how-to-link-a-resource-tooa-cloud-service"></a>Útmutató: egy erőforrás tooa felhőalapú szolgáltatás csatolása
hello Azure-portálon csatolja erőforrások együtt, például does hello aktuális klasszikus Azure portálon. Ehelyett a további erőforrások toohello telepíteni ugyanabban az hello felhőalapú szolgáltatás által használt erőforráscsoportban.

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Útmutató: egy felhőalapú szolgáltatás és a központi telepítései törlése
Egy felhőalapú szolgáltatás törlése előtt törölni kell minden meglévő telepítés.

toosave számítási költségek, a központi telepítés átmeneti, miután ellenőrizte, hogy a várt módon működik-e az éles telepítési hello törölheti. Le lett állítva telepített szerepkör-példányok számítási költségek kell fizetni.

A következő eljárás toodelete hello használja, a központi telepítés vagy a felhőalapú szolgáltatás.

1. A hello [Azure-portálon][Azure portal], válassza ki a kívánt toodelete hello felhőalapú szolgáltatás. Ez a lépés hello cloud service példány paneljének megnyitása.
2. Hello paneljén kattintson hello **törlése** gombra.

    ![Cloud Services lapozófájl-kapacitás](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. Törölheti a hello teljes felhőalapú szolgáltatás ellenőrzésével **felhőalapú szolgáltatás, és a központi telepítései** , vagy válasszon vagy hello **éles környezet** vagy hello **történő üzembe helyezése**.

    ![Cloud Services lapozófájl-kapacitás](./media/cloud-services-how-to-manage-portal/delete-blade.png)

4. Kattintson a hello **törlése** hello alsó gombra.
5. toodelete hello felhőalapú szolgáltatás, kattintson a **törlése a felhőalapú szolgáltatás**. Ezután hello megerősítő kérdés, kattintson **Igen**.

> [!NOTE]
> A felhőszolgáltatást törölték, és részletes figyelési van beállítva, akkor törölni kell hello adatok manuálisan a tárfiók. Ha toofind hello metrikák táblák kapcsolatos információkért lásd: [ez](cloud-services-how-to-monitor.md) cikk.


## <a name="how-to-find-more-information-about-failed-deployments"></a>Hogyan: sikertelen központi telepítéssel kapcsolatban további információt
Hello **áttekintése** panel rendelkezik állapotjelző hello tetején. Ha hello sáv gombra kattint, egy új panel megnyílik, és bármely hibaadatokat jelenít meg. Ha hello központi telepítési tartalmaz ki a hibákat, hello információk panel nem üres.

![Cloud Services lapozófájl-kapacitás](./media/cloud-services-how-to-manage-portal/status-info.png)



[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a>Következő lépések
* [A felhőalapú szolgáltatás általános konfigurációs](cloud-services-how-to-configure-portal.md).
* Ismerje meg, hogyan túl[felhőalapú szolgáltatás telepítése](cloud-services-how-to-create-deploy-portal.md).
* Konfigurálja a [egyéni tartománynév](cloud-services-custom-domain-name-portal.md).
* Konfigurálása [ssl-tanúsítványok](cloud-services-configure-ssl-certificate-portal.md).
