---
title: "az Azure-szolgáltatások aaaUsing terheléselosztás |} Microsoft Docs"
description: "Az oktatóanyag bemutatja, hogyan toocreate használatával egy olyan forgatókönyvet hello Azure terheléselosztás portfóliót: Traffic Manager, az alkalmazás átjárót és a terheléselosztó."
services: traffic-manager
documentationcenter: 
author: liumichelle
manager: vitinnan
editor: 
ms.assetid: f89be3be-a16f-4d47-bcae-db2ab72ade17
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/27/2016
ms.author: limichel
ms.openlocfilehash: d217047102d8c4828250dc0733e8ec9ee1e84b3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-load-balancing-services-in-azure"></a>Terheléselosztás szolgáltatások használata az Azure-ban

## <a name="introduction"></a>Bevezetés

A Microsoft Azure több kezelése a hálózati adatforgalom elosztása milyen szolgáltatásokat és elosztott terhelésű biztosít. Ezek a szolgáltatások külön-külön használhatja, vagy a módszerek kombinálásával igényeinek toobuild hello optimális megoldást.

Ez az oktatóanyag azt először meg kell határozni egy ügyfél-használati eset és hogyan el kell végezni robusztusabb és performant lásd a következő Azure terheléselosztás portfóliót hello segítségével: Traffic Manager, az alkalmazás átjárót és a terheléselosztó. Azt adja meg a központi telepítés, amely földrajzilag redundáns, forgalom tooVMs továbbítja, és segít létrehozásának részletes leírása kezelése különböző típusú kérelmet.

Egy elméleti szinten ezen szolgáltatások hello terheléselosztás hierarchiában különböző szerepet játszik.

* **A TRAFFIC Manager** globális DNS terheléselosztást biztosít. Megvizsgálja a bejövő DNS-kérésekre, és a kifogástalan állapotú végpontok válaszol, megfelelően hello útválasztási házirend hello ügyfél van kijelölve. Az útválasztási metódusait lehetőségek közül választhat:
  * Teljesítmény útválasztási toosend hello kérelmező toohello legközelebbi végpont késés tekintetében.
  * Útválasztási toodirect összes forgalom tooan végpont, más tartalékként végpontok prioritása.
  * Súlyozott ciklikus multiplexelés útválasztási, amely osztja el a forgalmat a hozzárendelt tooeach végpont hello súlyozás alapján.

  hello ügyfél közvetlenül kapcsolódik toothat végpont. Az Azure Traffic Manager észleli, ha a végpont nem kifogástalan, és majd átirányítások hello ügyfelek tooanother kifogástalan állapotú példány. Tekintse meg a túl[Azure Traffic Manager dokumentációs](traffic-manager-overview.md) hello szolgáltatással kapcsolatos további toolearn.
* **Alkalmazásátjáró** alkalmazás kézbesítési vezérlő (LÉPETT) biztosít az alkalmazás különböző réteg 7 terheléselosztás képességeket nyújtó szolgáltatás. Ez lehetővé teszi az ügyfelek toooptimize webkiszolgáló farm termelékenység kiszervezésével processzorigényes SSL-lezárást toohello Alkalmazásátjáró. Más réteg 7 útválasztási lehetőségek közé tartozik a ciklikus multiplexelés bejövő forgalmat, a munkamenet cookie-alapú kapcsolat, a URL-cím elérési út-alapú útválasztási és hello képességét toohost mögött egyetlen alkalmazás átjáró több webhelyek. Alkalmazásátjáró konfigurálhat egy internetes átjárót, egy csak belső-átjáró vagy mindkettőt. Alkalmazásátjáró teljesen Azure felügyelt, méretezhető és magas rendelkezésre állású. Diagnosztikai és naplózási képességek széles skáláját biztosítja a jobb kezelhetőség érdekében.
* **Terheléselosztó** hello Azure SDN-vermet, nagy teljesítményű, alacsony késésű réteg 4 terheléselosztás szolgáltatásokat nyújtó UDP és a TCP protokoll szerves része. Bejövő és kimenő kapcsolatok kezeli. Nyilvános és belső elosztott terhelésű végpont konfigurálhatja, és adja meg a szabályok toomap kapcsolatok tooback-a befejezési készlet célok bejövő TCP- és HTTP állapot-ellenőrzés beállítások toomanage szolgáltatás rendelkezésre állása használatával.

## <a name="scenario"></a>Forgatókönyv

A példában egy egyszerű webhely kétféle típusú tartalom látja, hogy használjuk: lemezképek és dinamikusan megjelenített weblapjain. hello webhely földrajzilag redundáns kell lennie, és azt a felhasználók hello legközelebbi (legkisebb mértékű késleltetést) szolgálhat hely toothem. hello alkalmazásfejlesztő úgy döntött, hogy a megfelelő URL-címek hello mintát/képek / * szolgáltatott hello webfarm hello részeinek eltérő virtuális gépek dedikált készletből.

Emellett a hello Virtuálisgép-készlet alapértelmezett hello dinamikus tartalom tootalk tooa háttér-adatbázis, amely egy magas rendelkezésre állású fürt kell. hello teljes telepítése Azure Resource Manageren keresztül állítható be.

A Traffic Manager, az alkalmazás átjárót és a terheléselosztó lehetővé teszi a webhely tooachieve ezek célok tervezése:

* **Multi-georedundancia**: egy régió tartozik leáll, ha a Traffic Manager útvonalak forgalom zökkenőmentesen toohello legközelebbi régiót hello alkalmazás tulajdonosa beavatkozása nélkül.
* **Kisebb késleltetést**: mivel a Traffic Manager automatikusan arra utasítja a hello ügyfél toohello legközelebbi régiót, hello felhasználói élmény kisebb késést biztosít hello weblap tartalma kérésekor.
* **Független méretezhetősége**: hello webes alkalmazás munkaterhelés tartalomtípushoz választják el, mert hello alkalmazás tulajdonosa méretezhető hello kérelem munkaterhelések egymástól független. Alkalmazásátjáró biztosítja, hogy hello forgalom irányított toohello jobb készletek megadott hello alapján szabályok és hello alkalmazás hello állapotáról.
* **Belső terheléselosztás**: mivel terheléselosztó van elé hello magas rendelkezésre állású fürt, csak egy adatbázis aktív és a megfelelő végpont hello kitett toohello alkalmazás. Emellett az adatbázis-rendszergazda is optimalizálhatja az hello munkaterhelés független hello előtér-alkalmazás hello fürtön aktív és passzív replikák elosztásával. Terheléselosztó kapcsolatok toohello magas rendelkezésre állású fürt továbbítja, és biztosítja, hogy csak a megfelelő adatbázisok fogadják.

hello alábbi ábrán ez a forgatókönyv hello architektúrájának:

![Terheléselosztás ábrája architektúrája](./media/traffic-manager-load-balancing-azure/scenario-diagram.png)

> [!NOTE]
> Ebben a példában csak az egyik hello terheléselosztás szolgáltatások, Azure biztosít sok lehetséges konfiguráció. A TRAFFIC Manager, az alkalmazás átjárót és a terheléselosztó használható vegyesen, és megfelelő toobest a terheléselosztás saját igényeinek megfelelően. Például ha SSL-kiszervezés, vagy a réteg 7 feldolgozási nincs szükség, Load Balancer az Alkalmazásátjáró helyett használható.

## <a name="setting-up-hello-load-balancing-stack"></a>Terheléselosztás verem hello beállítása

### <a name="step-1-create-a-traffic-manager-profile"></a>1. lépés: A Traffic Manager-profil létrehozása

1. Hello Azure-portálon, kattintson **új**, és ezután hello a piactéren "Traffic Manager-profil."
2. A hello **hozzon létre Traffic Manager-profil** panelen adja meg a következő alapvető információ hello:

  * **Név**: Adjon a Traffic Manager-profil egy DNS-előtag neve.
  * **Útválasztási módszer**: válassza ki a forgalom-útválasztási módszer házirend hello. Hello módszerekkel kapcsolatos további információkért lásd: [a Traffic Manager forgalom-útválasztási módszerei](traffic-manager-routing-methods.md).
  * **Előfizetés**: válassza ki, amely tartalmazza a hello-profil hello előfizetést.
  * **Erőforráscsoport**: hello-profil tartalmazó Select hello erőforráscsoportot. Egy új vagy meglévő erőforráscsoportot lehet.
  * **Erőforráscsoport helye**: a Traffic Manager szolgáltatás globális, és nem kötött tooa hely. Azonban meg kell adnia egy régiót hello Traffic Manager-profil társított hello metaadatokat tartalmazó hello csoport. Ezen a helyen nincs hatással van a hello profil futásidejű rendelkezésre állását hello.

3. Kattintson a **létrehozása** toogenerate hello Traffic Manager-profil.

  !["Létrehozása a Traffic Manager" panel](./media/traffic-manager-load-balancing-azure/s1-create-tm-blade.png)

### <a name="step-2-create-hello-application-gateways"></a>2. lépés: Hozzon létre hello alkalmazásátjárót

1. Hello Azure-portálon, hello bal oldali ablaktáblában kattintson **új** > **hálózati** > **Alkalmazásátjáró**.
2. Adja meg a következő alapvető információkat hello Alkalmazásátjáró hello:

  * **Név**: hello Alkalmazásátjáró hello nevét.
  * **Termékváltozat-méretét**: hello hello Alkalmazásátjáró, elérhető kis, közepes vagy nagy mérete.
  * **Count példány**: hello példányok száma, 2 és 10 közötti értéket.
  * **Erőforráscsoport**: hello erőforráscsoport hello Alkalmazásátjáró kapcsolnak. Lehet, hogy egy meglévő erőforráscsoportot, vagy egy újat.
  * **Hely**: hello Alkalmazásátjáró, amelyek azonos hello hello régió hely hello erőforráscsoportként működnek. hello hely fontos, mert hello virtuális hálózat és a nyilvános IP-címet kell hello hello átjáró ugyanazon a helyen.
3. Kattintson az **OK** gombra.
4. Hello virtuális hálózat, alhálózati, előtér-IP és hello Alkalmazásátjáró figyelő konfigurációi megadása. Ebben a forgatókönyvben hello előtérbeli IP-cím van **nyilvános**, amely fel van véve egy végpont toohello Traffic Manager-profil később toobe lehetővé teszi.
5. Konfigurálja a hello figyelő hello alábbi beállítások egyikét:
    * Ha a HTTP használata esetén nincs szükség tooconfigure. Kattintson az **OK** gombra.
    * Ha a HTTPS PROTOKOLLT használja, további konfigurációs szükség. Tekintse meg a túl[Alkalmazásátjáró létrehozása](../application-gateway/application-gateway-create-gateway-portal.md), induló 9. lépés. Amikor befejezte a hello konfigurálását, kattintson **OK**.

#### <a name="configure-url-routing-for-application-gateways"></a>URL-cím alkalmazás átjárók útválasztás konfigurálása

Ha úgy dönt, hogy a háttér-készlet, elérési út alapú szabállyal konfigurált Alkalmazásátjáró egy hello kérelem URL-címének elérési út mintája hozzáadása tooround multiplexelés veszi. Ebben a forgatókönyvben azt ad hozzá egy elérési utat alapú szabály toodirect bármely URL-cím elé "/images/\*" toohello kép kiszolgálókészlethez. URL-cím konfigurálásával kapcsolatos további információkat az Alkalmazásátjáró, elérési út-alapú útválasztási tekintse meg a túl[Alkalmazásátjáró elérési alapú szabály létrehozásához](../application-gateway/application-gateway-create-url-route-portal.md).

![Átjáró webes szintű diagramja](./media/traffic-manager-load-balancing-azure/web-tier-diagram.png)

1. A erőforráscsoportból nyissa meg az előző szakaszban hello létrehozott hello Alkalmazásátjáró toohello példányát.
2. A **beállítások**, jelölje be **háttérkészletek**, majd válassza ki **Hozzáadás** tooadd hello tooassociate hello webes szintű háttér-készletek a kívánt virtuális gépek.
3. A hello **háttérkészlet hozzáadása** panelen hello háttér-készlet neve hello és hello gépek hello készletben található összes hello IP-címét adja meg. Ebben a forgatókönyvben azt kapcsolódik a virtuális gépek két háttér-kiszolgálófiók rendelkezik.

  ![Application Gateway "Háttérkészlet hozzáadása" panel](./media/traffic-manager-load-balancing-azure/s2-appgw-add-bepool.png)

4. A **beállítások** hello Alkalmazásátjáró, válassza ki a **szabályok**, és kattintson a hello **elérési utat** gomb tooadd szabály.

  ![Alkalmazás átjáró szabályok "Elérési útja alapján" gombra](./media/traffic-manager-load-balancing-azure/s2-appgw-add-pathrule.png)

5. A hello **Hozzáadás elérési alapú szabály** panelen, adja meg a következő információ hello hello szabály konfigurálása.

   Alapvető beállítások:

   + **Név**: hello szabály, amely elérhető a portál hello hello rövid nevét.
   + **Figyelő**: hello figyelő, amely hello szabály szolgál.
   + **Alapértelmezett háttérkészlet**: hello háttér-készlet toobe hello alapértelmezett szabály együtt.
   + **Alapértelmezett beállítások HTTP**: hello HTTP beállítások toobe hello alapértelmezett szabály együtt.

   Elérési út-alapú szabályok:

   + **Név**: hello elérési alapú szabály hello rövid nevét.
   + **Elérési utak**: hello elérésiút-szabályt, amely forgalmat szolgál.
   + **Háttérkészlet**: Ez a szabály használt háttér-készlet toobe hello.
   + **HTTP-beállítása**: hello HTTP beállítások toobe együtt ez a szabály.

   > [!IMPORTANT]
   > Útvonalak megadása: Érvényes elérési kezdődhet "/". helyettesítő karakteres hello "\*" csak hello végén megengedett. Érvényes többek között az /xyz, /xyz\*, vagy /xyz/\*.

   ![Application Gateway "Elérési út alapú szabály hozzáadása" panel](./media/traffic-manager-load-balancing-azure/s2-appgw-pathrule-blade.png)

### <a name="step-3-add-application-gateways-toohello-traffic-manager-endpoints"></a>3. lépés: Alkalmazás átjárók toohello Traffic Manager-végpont-hozzáadáshoz

Ebben a forgatókönyvben a Traffic Manager csatlakoztatott tooapplication átjárók (mivel az előző lépésekben hello konfigurált) különböző régiókban található. Most, hogy hello alkalmazás átjáró van beállítva, hello következő lépésre-e tooconnect őket tooyour Traffic Manager-profil.

1. Nyissa meg a Traffic Manager-profil. Igen, az erőforráscsoport vagy keresse meg a Traffic Manager-profil hello hello neve hely toodo **összes erőforrás**.
2. Hello bal oldali ablaktáblában jelöljön ki **végpontok**, és kattintson a **Hozzáadás** tooadd egy végpontot.

  ![A TRAFFIC Manager végpontok "Hozzáadás" gombra](./media/traffic-manager-load-balancing-azure/s3-tm-add-endpoint.png)

3. A hello **végpont hozzáadása** panelen, hozzon létre egy végpontot hello a következő információk megadásával:

  * **Típus**: válassza ki, hello típusú végpont tooload elosztani a kéréseket. Ebben az esetben válassza **Azure-végpont** , mert jelenleg kapcsolódik az toohello átjáró alkalmazáspéldányok korábban beállított.
  * **Név**: hello végpont hello nevét adja meg.
  * **Erőforrás céltípust**: válasszon **nyilvános IP-cím** majd az **célerőforrás**, válassza ki a hello nyilvános IP-címe hello Alkalmazásátjáró korábban konfigurált.

   ![A TRAFFIC Manager "Végpont hozzáadása" panel](./media/traffic-manager-load-balancing-azure/s3-tm-add-endpoint-blade.png)

4. Most tesztelheti a telepítő a Traffic Manager-profil DNS hello elérésével (ebben a példában: TrafficManagerScenario.trafficmanager.net). Küldje el újból a kérelmeket, elindítani vagy állítsa le a virtuális gépek és a webkiszolgálók esetében, amelyek különböző régiókban létrejöttek, és módosítsa a hello Traffic Manager-profil beállításainak tootest a telepítő.

### <a name="step-4-create-a-load-balancer"></a>4. lépés:, Hozzon létre egy adott terheléselosztóhoz

Ebben a forgatókönyvben a Load Balancer osztja el a hello webes réteg toohello adatbázisok magas rendelkezésre állású fürt közötti kapcsolatok.

Ha a magas rendelkezésre állási adatbázis fürt által használt SQL Server AlwaysOn, tekintse meg a túl[konfigurálása egy vagy több mindig a rendelkezésre állási csoport figyelői](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) részletes útmutatásait.

A belső terheléselosztók konfigurálásával kapcsolatos további információkért lásd: [hello Azure-portálon hozzon létre egy belső elosztott terhelésű](../load-balancer/load-balancer-get-started-ilb-arm-portal.md).

1. Hello Azure-portálon, hello bal oldali ablaktáblában kattintson **új** > **hálózati** > **terheléselosztó**.
2. A hello **létrehozás terheléselosztó** panelen válassza ki a terheléselosztó nevét.
3. Set hello **típus** túl**belső**, és válassza ki a megfelelő virtuális hálózati hello és hello load balancer tooreside az alhálózatot.
4. A **IP-cím hozzárendelése**, válassza **dinamikus** vagy **statikus**.
5. A **erőforráscsoport**, válassza ki a hello erőforráscsoport hello terheléselosztóhoz.
6. A **hely**, válassza ki a megfelelő régiót hello hello terheléselosztóhoz.
7. Kattintson a **létrehozása** toogenerate hello terheléselosztóhoz.

#### <a name="connect-a-back-end-database-tier-toohello-load-balancer"></a>Csatlakozás a háttéradatbázis réteg toohello terheléselosztó

1. A erőforráscsoportból hello terheléselosztó hello előző lépésekben létrehozott található.
2. A **beállítások**, kattintson a **háttérkészletek**, és kattintson a **hozzáadása** tooadd egy háttér címkészletet.

  ![Terheléselosztó "Háttérkészlet hozzáadása" panel betöltése](./media/traffic-manager-load-balancing-azure/s4-ilb-add-bepool.png)

3. A hello **háttérkészlet hozzáadása** panelen adja meg a hello hello háttér-készlet neve.
4. Adja hozzá az egyes gépek vagy egy rendelkezésre állási készlet toohello háttér címkészletet.

#### <a name="configure-a-probe"></a>A mintavétel

1. A terheléselosztó a alatt **beállítások**, jelölje be **vizsgálat**, és kattintson a **Hozzáadás** tooadd vizsgálatok.

 ![Terheléselosztó "Add mintavételi" panel betöltése](./media/traffic-manager-load-balancing-azure/s4-ilb-add-probe.png)

2. A hello **Hozzáadás mintavételi** panelen adja meg hello mintavétel hello nevét.
3. Jelölje be hello **protokoll** hello mintavételhez. -Adatbázis érdemes lehet a HTTP-vizsgálatot, hanem egy TCP-Hálózatfigyelővel. toolearn terheléselosztó mintavételek menüpontban, kapcsolatos további információkért tekintse meg a túl[megértése load balancer mintavételt](../load-balancer/load-balancer-custom-probe-overview.md).
4. Adja meg a hello **Port** , az adatbázis toobe hello mintavételi eléréséhez használt.
5. A **időköz**, adja meg, hogy milyen gyakran tooprobe hello alkalmazás.
6. A **sérült küszöbérték**, adja meg a hello hello háttér-VM toobe megfelelő állapotúnak számít bekövetkező sikertelen a folyamatos mintavételek száma.
7. Kattintson a **OK** toocreate hello mintavétel.

#### <a name="configure-hello-load-balancing-rules"></a>Hello terheléselosztási szabályok konfigurálása

1. A **beállítások** válassza ki a terheléselosztó **terheléselosztási szabályok**, és kattintson a **Hozzáadás** toocreate szabály.
2. A hello **Hozzáadás terheléselosztási szabály** panelen adja meg a hello **neve** hello terheléselosztási szabály.
3. Válassza ki a hello **előtérbeli IP-cím** hello a terheléselosztót, **protokoll**, és **Port**.
4. A **háttérportot**, adja meg a hello port toobe hello háttér-készletben használt.
5. Jelölje be hello **háttérkészlet** és hello **mintavételi** hello előző lépéseket tooapply hello szabály, amely a hozott létre.
6. A **munkamenet megőrzését**, adja meg, hogyan hello munkamenetek toopersist.
7. A **az üresjárati időkorlát**, adja meg az üresjárati időkorlátot percet hello száma.
8. A **fix IP-Címek**, válassza **letiltott** vagy **engedélyezve**.
9. Kattintson a **OK** toocreate hello szabály.

### <a name="step-5-connect-web-tier-vms-toohello-load-balancer"></a>5. lépés: Csatlakozás webes rétegbeli virtuális gépek toohello terheléselosztó

Most azt adja meg az adatbázis-kapcsolat a webalkalmazás-réteg virtuális gépeken futó alkalmazások hello hello IP-cím és a terheléselosztó előtér-port. Ez a konfiguráció nincs meghatározott toohello virtuális gépeken futó alkalmazások. tooconfigure hello cél IP-cím és port, tekintse meg a toohello alkalmazás dokumentációját. toofind hello IP-címe hello előtér, az Azure-portálon hello toohello előtér-IP-készlet keresse fel a hello **terheléselosztó beállításai** panelen.

![Terheléselosztó "Előtér-IP-készlet" navigációs ablaktábla betöltése](./media/traffic-manager-load-balancing-azure/s5-ilb-frontend-ippool.png)

## <a name="next-steps"></a>Következő lépések

* [A Traffic Manager áttekintése](traffic-manager-overview.md)
* [Átjáró – áttekintés](../application-gateway/application-gateway-introduction.md)
* [Az Azure Load Balancer áttekintése](../load-balancer/load-balancer-overview.md)
