---
title: "Térkép integráció a System Center Operations Manager aaaService |} Microsoft Docs"
description: "Szolgáltatástérkép Operations Management Suite megoldás, amely automatikusan felderíti az alkalmazás-összetevők a Windows és Linux rendszerek és a maps hello szolgáltatások közötti kommunikáció. Ez a cikk ismerteti a Szolgáltatástérkép használatával tooautomatically elosztott alkalmazás diagramokat hozhat létre az Operations Manager programban."
services: operations-management-suite
documentationcenter: 
author: daveirwin1
manager: jwhit
editor: tysonn
ms.assetid: e8614a5a-9cf8-4c81-8931-896d358ad2cb
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2017
ms.author: bwren;dairwin
ms.openlocfilehash: cff9cce2559448ec3a5fd14087b867f314716560
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-map-integration-with-system-center-operations-manager"></a>Szolgáltatástérkép integráció a System Center Operations Manager
  > [!NOTE]
  > A funkció jelenleg nyilvános előzetes verziójához.
  > 
  
Operations Management Suite Szolgáltatástérkép automatikusan észleli a Windows és Linux rendszerek alkalmazás-összetevők és társít hello kommunikációs szolgáltatások között. Szolgáltatástérkép lehetővé teszi a kiszolgálók hello módon úgy gondolja, hogy azok összekapcsolt rendszerekhez, hogy a kritikus szolgáltatások tooview. Szolgáltatástérkép kiszolgálók, folyamatok és portok közötti kapcsolatok között bármely TCP-csatlakoztatott architektúra, nem szükséges ügynököt telepíteni hello mellett konfigurációval jeleníti meg. További információkért lásd: hello [Szolgáltatástérkép dokumentáció](operations-management-suite-service-map.md).

Ez az integráció Szolgáltatástérkép és a System Center Operations Manager között automatikusan az Operations Manager programban, a Service Map hello dinamikus függőségi maps alapuló hozhat létre elosztott alkalmazás diagramokat.

## <a name="prerequisites"></a>Előfeltételek
* Az Operations Manager felügyeleti csoport, amely a kiszolgálók egy csoportja kezel.
* Az Operations Management Suite munkaterület hello Szolgáltatástérkép megoldás.
* Kiszolgálók egy csoportja (legalább egyet), amely az Operations Manager és a küldő adatok tooService térkép felügyeli. A Windows és Linux-kiszolgálókon támogatott.
* A szolgáltatás egyszerű hozzáférés toohello Azure-előfizetéssel társított hello Operations Management Suite-munkaterülettel. További információ: túl[szolgáltatásnevet létrehozni](#creating-a-service-principal).

## <a name="install-hello-service-map-management-pack"></a>Hello Szolgáltatástérkép felügyeleti csomag telepítéséhez
Hello Microsoft.SystemCenter.ServiceMap felügyeleticsomag-nyaláb (Microsoft.SystemCenter.ServiceMap.mpb) importálása az Operations Manager és a Service Map hello integrációjával engedélyezése. hello köteg hello a következő felügyeleti csomagokat tartalmazza:
* A Microsoft szolgáltatás térkép alkalmazás nézetek
* A Microsoft System Center Service Map belső
* A Microsoft System Center Service Map felülbírálások
* A Microsoft System Center Service Map

## <a name="configure-hello-service-map-integration"></a>Hello Szolgáltatástérkép-integráció konfigurálása
Hello Szolgáltatástérkép felügyeleti csomagot, egy új csomópont telepítése után **Szolgáltatástérkép**, jelenik meg **Operations Management Suite** a hello **felügyeleti** ablaktáblán. 

Szolgáltatástérkép integrációs tooconfigure hello a következő:

1. tooopen hello konfigurációs varázsló hello **szolgáltatás – áttekintés** ablaktáblán kattintson a **munkaterület hozzáadása**.  

    ![Szolgáltatás – áttekintés ablaktáblán](media/oms-service-map/scom-configuration.png)

2. A hello **kapcsolat konfigurációs** ablak, írja be a hello bérlő neve vagy azonosítója, azonosítója (más néven a hello felhasználónév vagy a clientID) és hello szolgáltatás egyszerű jelszót, és kattintson **következő**. További információ: túl[szolgáltatásnevet létrehozni](#creating-a-service-principal).

    ![hello kapcsolat konfigurációs ablak](media/oms-service-map/scom-config-spn.png)

3. A hello **előfizetés kijelölés** ablakban jelölje ki a hello Azure-előfizetéssel, Azure erőforráscsoport (hello Operations Management Suite-munkaterülettel tartalmazó hello) és az Operations Management Suite-munkaterület, és kattintson **Következő**.

    ![Operations Manager konfigurációs munkaterület hello](media/oms-service-map/scom-config-workspace.png)

4. A hello **gép Csoportkijelölés** melyik szolgáltatás térkép gép csoportok kiválasztása ablakban azt szeretné, hogy toosync tooOperations Manager. Kattintson a **hozzáadása gép csoportok**, hello listájából válassza ki a csoportok **elérhető számítógép-csoportok**, és kattintson a **hozzáadása**.  Ha befejezte a csoportok kiválasztásával, kattintson a **Ok** toofinish.
    
    ![az Operations Manager konfigurációs gép csoportok hello](media/oms-service-map/scom-config-machine-groups.png)
    
5. A hello **kiszolgáló kiválasztása** ablakban konfigurálnia hello szolgáltatás térkép kiszolgálók csoport, amelyet az Operations Manager és a Service Map közötti toosync hello kiszolgálókkal. Kattintson a **kiszolgálók hozzáadása**.   
    
    Hello integrációs toobuild egy elosztott alkalmazás diagram kiszolgáló, a hello kiszolgálónak kell lennie:

    * Operations Manager által felügyelt
    * Szolgáltatástérkép kezeli
    * Hello szolgáltatás térkép kiszolgálók csoport szerepel

    ![Operations Manager konfigurációs csoport hello](media/oms-service-map/scom-config-group.png)

6. Választható lehetőség: Hello felügyeleti kiszolgáló erőforrás készlet toocommunicate Operations Management Suite válassza ki, és kattintson **hozzáadása munkaterület**.

    ![az Operations Manager konfigurációs erőforráskészlet hello](media/oms-service-map/scom-config-pool.png)

    Előfordulhat, hogy igénybe vehet egy perc tooconfigure és regisztrálása hello Operations Management Suite-munkaterülettel. Beállítások konfigurálása után az Operations Manager indít el az Operations Management Suite hello első Szolgáltatástérkép szinkronizálás.

    ![az Operations Manager konfigurációs erőforráskészlet hello](media/oms-service-map/scom-config-success.png)


## <a name="monitor-service-map"></a>A figyelő Szolgáltatástérkép
Az Operations Management Suite-munkaterülettel hello csatlakoztatása után jelenik meg egy új mappát, a Service Map hello **figyelés** hello Operations Manager konzol ablaktáblájában.

![hello az Operations Manager figyelés ablaktáblán](media/oms-service-map/scom-monitoring.png)

hello Szolgáltatástérkép mappa négy csomópont rendelkezik:
* **Aktív riasztások**: minden hello aktív kapcsolatos riasztásokat az Operations Manager és a Service Map hello kommunikációját sorolja fel.  Ne feledje, hogy ezek a riasztások nem szinkronizált tooOperations Manager alatt Operations Management Suite-riasztásokat. 

* **Kiszolgálók**: listák hello figyelt kiszolgálók, amelyek a Service Map toosync konfigurálva.

    ![hello Operations Manager figyelési kiszolgálók panel](media/oms-service-map/scom-monitoring-servers.png)

* **Számítógép-függőség Csoportnézeteket**: a Service Map szinkronizált összes számítógép-csoportok listája. Bármely csoport tooview az elosztott alkalmazás diagram gombra.

    ![hello Operations Manager elosztott alkalmazás diagramja](media/oms-service-map/scom-group-dad.png)

* **Kiszolgáló függőségi nézetek**: minden olyan kiszolgáló, a Service Map szinkronizált sorolja fel. Bármely kiszolgáló tooview az elosztott alkalmazás diagram gombra.

    ![hello Operations Manager elosztott alkalmazás diagramja](media/oms-service-map/scom-dad.png)

## <a name="edit-or-delete-hello-workspace"></a>Szerkesztheti és törölheti a hello munkaterület
Ön módosíthatja és törölheti a konfigurált hello munkaterületen hello **szolgáltatás – áttekintés** ablaktáblán (**felügyeleti** ablaktábla > **Operations Management Suite**  >  **Térkép szolgáltatás**). Csak egy Operations Management Suite-munkaterület most konfigurálhatja.

![hello Operations Manager szerkesztése munkaterület ablaktáblán](media/oms-service-map/scom-edit-workspace.png)

## <a name="configure-rules-and-overrides"></a>Szabályok és felülbírálások konfigurálása
A szabály _Microsoft.SystemCenter.ServiceMapImport.Rule_, Szolgáltatástérkép tooperiodically fetch információk készült. toochange szinkronizálási időzítést, a felülbírálások hello szabály konfigurálhatja (**szerzői műveletek** ablaktábla > **szabályok** > **Microsoft.SystemCenter.ServiceMapImport.Rule**).

![hello Operations Manager felülbírálások tulajdonságai ablakban](media/oms-service-map/scom-overrides.png)

* **Engedélyezett**: engedélyezheti vagy tilthatja le az automatikus frissítések. 
* **IntervalMinutes**: hello idő közötti frissítések alaphelyzetbe. hello alapértelmezett program óránként. Ha gyakrabban toosync server maps, hello érték módosítására képes.
* **TimeoutSeconds**: alaphelyzetbe hello mennyi idő után hello kérelem időkorlátja lejár. 
* **TimeWindowMinutes**: hello idő az ablak-visszaállítási adatok lekérdezése. Alapértelmezett érték 60 perc ablak. hello maximális Szolgáltatástérkép által engedélyezett értéke 60 perc.

## <a name="known-issues-and-limitations"></a>Ismert problémák és korlátozások

hello aktuális terv megadja hello következő problémák és korlátozások:
* Csak kapcsolódhatnak tooa egyetlen Operations Management Suite-munkaterülettel.
* Bár hozzáadhatja a kiszolgálók toohello térkép kiszolgálók szolgáltatáscsoport manuálisan keresztül hello **szerzői műveletek** ablaktáblán, az ezeken a kiszolgálókon hello maps azonnal nem lett szinkronizálva.  Ezek lesznek szinkronizálva a Szolgáltatástérkép hello következő szinkronizálási ciklusban során.
* Ha végzett módosításokat toohello elosztott alkalmazás diagramok hello felügyeleti csomag által létrehozott, ezeket a módosításokat valószínűleg felülírja a Szolgáltatástérkép hello következő szinkronban.

## <a name="create-a-service-principal"></a>Egyszerű szolgáltatás létrehozása
Hivatalos Azure dokumentációjában az egyszerű szolgáltatás létrehozása lásd:
* [Egy egyszerű szolgáltatás létrehozása a PowerShell használatával](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Hozzon létre egy egyszerű Azure parancssori felület használatával](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)
* [Hozzon létre egy egyszerű hello Azure-portál használatával](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal)

### <a name="feedback"></a>Visszajelzés
Van olyan visszajelzést az USA Szolgáltatástérkép és a jelen dokumentáció? Látogasson el a [felhasználói véleményekkel foglalkozó weblapunkra](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), ahol felajánlja a szolgáltatások és arra való szavazás a meglévő javaslatok.
