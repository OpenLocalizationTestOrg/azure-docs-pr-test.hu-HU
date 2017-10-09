---
title: az Azure App Service Apps aaaMonitor |} Microsoft Docs
description: "Ismerje meg, hogyan toomonitor alkalmazások az Azure App Service segítségével hello Azure portálon."
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: d273da4e-07de-48e0-b99d-4020d84a425e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2016
ms.author: byvinyal
ms.openlocfilehash: 80d5a466102a894a49d04ae35aa54cc1d05a58df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-to-monitor-apps-in-azure-app-service"></a>Útmutató: az Azure App Service-alkalmazások figyelése
[App Service](http://go.microsoft.com/fwlink/?LinkId=529714) hello a beépített felügyeleti funkciókat biztosítja [Azure Portal](https://portal.azure.com).
Ez magában foglalja a hello képességét tooreview **kvóták** és **metrikák** egy alkalmazást, valamint a hello beállítása App Service-csomag **riasztások** és még **Skálázás** alapján automatikusan metrikákat.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a>Understanding kvóták és metrikák
### <a name="quotas"></a>Kvóták
Az App Service-ben üzemeltetett alkalmazások tulajdonos toocertain *korlátok* használhatnak erőforrásait. hello korlátokat határoznak hello **App Service-csomag** hello alkalmazással társított alkalmazások.

Ha a hello alkalmazás egy **szabad** vagy **megosztott** tervezi, akkor hello vonatkozó korlátozások hello erőforrások hello alkalmazás által meghatározott **kvóták**.

Ha hello alkalmazás a egy **alapvető**, **szabványos** vagy **prémium** tervezi, akkor -beállításokat használhatnak hello erőforrások hello vonatkozó korlátozások hello **mérete** (Kis, közepes, nagy méretű) és **száma példány** (1, 2, 3,...) a hello **App Service-csomag**.

**Kvóták** a **szabad** vagy **megosztott** -alkalmazásokat:

* **CPU(Short)**
  * Az alkalmazás egy 5 perces időszakban engedélyezett CPU mennyisége. Ez a kvóta újra 5 percenként állítja be.
* **CPU(Day)**
  * Ehhez az alkalmazáshoz egy napon belül engedélyezett Processzor teljes mennyisége. Ez a kvóta újra beállítja az UTC idő szerint éjfélkor 24 óránként.
* **Memória**
  * Teljes memóriamennyiség engedélyezett ehhez az alkalmazáshoz.
* **Sávszélesség**
  * Kimenő sávszélesség engedélyezett ehhez az alkalmazáshoz egy nap teljes mennyisége.
    Ez a kvóta újra beállítja az UTC idő szerint éjfélkor 24 óránként.
* **Fájlrendszer**
  * Teljes méretű tárolóhely.

csak kvóta alkalmazható tooapps üzemeltetett hello **alapvető**, **szabványos** és **prémium** tervek van **fájlrendszer**.

További információ a hello adott kvóták, korlátok és funkciók érhetők el másik App Service termékváltozatok hello itt található: [Azure előfizetés szolgáltatásra vonatkozó korlátozások](../azure-subscription-service-limits.md#app-service-limits)

#### <a name="quota-enforcement"></a>Kvóta kényszerítése
Ha az alkalmazás a használatát meghaladja hello **Processzor (rövid)**, **Processzor (nap)**, vagy **sávszélesség** kvóta majd hello alkalmazás le lesz állítva, amíg újra beállítja hello kvóta. Ebben az időszakban az összes bejövő kérelmet eredményezi egy **HTTP 403**.
![][http403]

Ha alkalmazás hello **memória** kvóta túllépése, akkor a hello alkalmazás rendszer indulnak újra.

Ha hello **fájlrendszer** kvóta túllépése, akkor minden írási művelet sikertelen lesz, beleértve toologs írása.

Kvóták növelhető vagy eltávolította a alkalmazás az App Service-csomag frissítése.

### <a name="metrics"></a>Mérőszámok
**Metrikák** hello alkalmazást, illetve az App Service-csomag viselkedés ismertetik.

Az egy **alkalmazás**, hello elérhető mérőszámok vannak:

* **Átlagos válaszidő**
  * hello átlagos idő a hello app tooserve kéréseket az ms.
* **Munkakészlet átlagos memória**
  * a MIB hello alkalmazás által használt memória mennyisége átlagos hello.
* **CPU-idő**
  * hello összeg CPU hello alkalmazás által felhasznált másodpercben. További információ a metrika lásd: [CPU-időt és a CPU százaléka](#cpu-time-vs-cpu-percentage)
* **Az adatok**
  * Bejövő sávszélesség MIB hello alkalmazás által használt hello mennyiségét.
* **Kimenő adatforgalmat**
  * kimenő sávszélesség MIB hello alkalmazás által használt hello mennyiségét.
* **HTTP 2xx**
  * A http-állapotkódot eredményez kérelmek száma > = 200, de < 300.
* **HTTP 3xx**
  * A http-állapotkódot eredményez kérelmek száma > = < 400, de 300.
* **HTTP 401-es**
  * HTTP 401-es állapotkód eredményezve kérelmek száma.
* **A HTTP 403**
  * 403-as HTTP-állapotkódot eredményez kérelmek száma.
* **HTTP 404-es**
  * HTTP 404 állapotkódot eredményez kérelmek száma.
* **406 http**
  * HTTP 406-állapotkódot eredményez kérelmek száma.
* **HTTP 4xx**
  * A http-állapotkódot eredményez kérelmek száma > = 400, de a < 500.
* **HTTP-kiszolgáló hibák**
  * A http-állapotkódot eredményez kérelmek száma > = 500, de a < 600.
* **Memória munkakészlete**
  * A MIB hello alkalmazás által használt memória pillanatnyi mérete.
* **Kérések**
  * Az eredményül kapott HTTP-állapotkód függetlenül kérelmek teljes száma.

Az egy **App Service-csomag**, hello elérhető mérőszámok vannak:

> [!NOTE]
> App Service-csomag metrikái csak érhetők el a tervek **alapvető**, **szabványos** és **prémium** Termékváltozat.
> 
> 

* **Processzor**
  * hello átlagos CPU hello csomag összes példányán keresztül használt.
* **Memóriahasználat (%)**
  * hello átlagos memória hello csomag összes példányán keresztül használt.
* **Az adatok**
  * hello átlagos bejövő keresztül használt sávszélesség hello csomag összes példányán.
* **Kimenő adatforgalmat**
  * hello átlagos kimenő hello csomag összes példányán keresztül használt sávszélesség.
* **Lemezvárólista hossza**
  * hello átlagos száma olvasási és írási váró kérelmek tárolón. A magas Lemezvárólista hossza arra utal, hogy az alkalmazás tooexcessive lemez i/o-miatt előfordulhat, hogy lehet lelassult.
* **HTTP-várólista hossza**
  * hello toosit volt hello várólista előtt teljesítését HTTP-kérelmek átlagos száma. Egy magas vagy növekvő HTTP-várólista hossza a jelenség a terv túl nagy terhelés alatt.

### <a name="cpu-time-vs-cpu-percentage"></a>CPU-időt és a CPU százaléka
<!-- toodo: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

Nincsenek 2 metrikákat, amelyek CPU-használat tükrözik. **CPU-idő** és **processzor**

**CPU-idő** akkor hasznos, az üzemeltetett alkalmazások **szabad** vagy **megosztott** tervek, mert azok egyik hello alkalmazás által használt CPU percben értendő.

**Processzor** a hello ugyanakkor akkor hasznos, az üzemeltetett alkalmazások **alapvető**, **szabványos** és **prémium** tervek, mert azok kiterjeszthető, és ez a metrika egy jó hello oka minden példányára összesített használatát.

## <a name="metrics-granularity-and-retention-policy"></a>Metrikák Granularitási és az adatmegőrzési házirend
Az alkalmazás és az app service-csomag metrikáinak naplózza, és összesítve hello szolgáltatást, amely hello Granularitás van, és az adatmegőrzési házirendek a következő:

* **Perc** granularitási metrikák megmaradnak a **48 óra**
* **Óra** granularitási metrikák megmaradnak a **30 napban**
* **Nap** granularitási metrikák megmaradnak a **90 nap**

## <a name="monitoring-quotas-and-metrics-in-hello-azure-portal"></a>Hello Azure Portal ellenőrzése kvóták és metrikákat.
Megtekintheti a különböző hello hello állapotának **kvóták** és **metrikák** hello kérelmet érintő [Azure Portal](https://portal.azure.com).

![][quotas]
**Kvóták** található a beállítások >**kvóták**. hello UX lehetővé teszi, hogy tekintse át: (1) hello kvóták nevét, (2) a átállítási időközt, (3) a jelenlegi korlátozását és (4) aktuális értékét.

![][metrics]
**Metrikák** közvetlenül hello erőforráspanelen való hozzáférést lehet. Testre szabhatja hello diagram: (1) **kattintson** , és válassza ki (2) **diagram szerkesztése**.
Itt módosíthatja a hello (3) **időtartománynak**, (4) **diagramtípus**, és (5) **metrikák** toodisplay.  

Ön talál további információt itt metrikák: [szolgáltatás metrikát](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

## <a name="alerts-and-autoscale"></a>Riasztások és automatikus skálázás
Metrikák egy alkalmazás vagy az App Service-csomag tooalerts, kaphat további toolearn kell csatlakoztatnia a következő témakörben: [riasztási értesítéseket](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

App Service alkalmazás üzemeltetett a basic, standard vagy prémium szintű App Service-csomagok támogatja **automatikus skálázás**. Ez lehetővé teszi tooconfigure szabályokat, amelyek az App Service-csomag metrikái figyelése és növelhető és csökkenthető hello példányszám további erőforrások rendelkezésre bocsátása, igény szerint vagy mentése pénz túlzott rendelkezés hello alkalmazás esetén. Automatikus méretezésének Itt többet is megtudhat: [hogyan tooScale](../monitoring-and-diagnostics/insights-how-to-scale.md) és itt [ajánlott eljárások az Azure a figyelő automatikus skálázás](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="whats-changed"></a>A változások
* Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
