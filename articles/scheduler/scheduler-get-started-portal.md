---
title: "az Azure portál Azure Scheduler használatába aaaGet |} Microsoft Docs"
description: "Ismerkedés az Azure portál Azure Scheduler szolgáltatásával"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: e69542ec-d10f-4f17-9b7a-2ee441ee7d68
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/10/2016
ms.author: deli
ms.openlocfilehash: 58255c0ad19da65932f8b1d36cb8fef1ff6e651b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-scheduler-in-azure-portal"></a>Ismerkedés az Azure portál Azure Scheduler szolgáltatásával
Azure Scheduler könnyen toocreate ütemezett feladatok is. Ebben az oktatóanyagban megtudhatja, hogyan toocreate egy feladatot. Megismerkedhet a Scheduler megfigyelési és felügyeleti képességeivel is.

## <a name="create-a-job"></a>Feladat létrehozása
1. Jelentkezzen be a túl[Azure-portálon](https://portal.azure.com/).  
2. Kattintson a **+ új** > típus *Feladatütemező* hello keresőmezőbe > válassza **Feladatütemező** eredmények > kattintson **létrehozása**.
   
    ![][marketplace-create]
3. Hozzunk létre egy olyan feladatot, amely egy GET kéréssel egyszerűen rákeres a http://www.microsoft.com/ webhelyre. A hello **ütemezési feladat** képernyőn, írja be a következő információ hello:
   
   1. **Név:**`getmicrosoft`  
   2. **Előfizetés:** Az Ön Azure-előfizetése   
   3. **Feladatgyűjtemény:** Válasszon ki egy létező feladatgyűjteményt, vagy kattintson az **Új létrehozása** lehetőségre, és adjon meg egy nevet.
4. A következő **műveleti beállítások**, adja meg a következő értékek hello:
   
   1. **Művelettípus:**` HTTP`  
   2. **Módszer:**`GET`  
   3. **URL-cím:**` http://www.microsoft.com`  
      
      ![][action-settings]
5. Végül határozzon meg egy ütemezést. hello feladat egyszeri feladat sikerült kell definiálni, de most az ismétlődési ütemezés szerint válasszon:
   
   1. **Ismétlődés**: `Recurring`
   2. **Indítás**: A mai dátum
   3. **Ismétlődés**: `12 Hours`
   4. **Befejeződés**: A mai naptól számított két nap múlva  
      
      ![][recurrence-schedule]
6. Kattintson a  **Create** (Létrehozás) gombra

## <a name="manage-and-monitor-jobs"></a>Feladatok kezelése és figyelése
Ha egy feladat jön létre, hello fő Azure irányítópult jelenik meg. Kattintson a hello feladat és egy új ablakban nyílik meg a következő lapokon hello:

1. Tulajdonságok  
2. Művelet beállításai  
3. Ütemezés  
4. Előzmények
5. Felhasználók
   
   ![][job-overview]

### <a name="properties"></a>Tulajdonságok
A csak olvasható tulajdonságok hello felügyeleti metaadatok hello Feladatütemező feladat ismertetik.

   ![][job-properties]

### <a name="action-settings"></a>Művelet beállításai
Egy feladat hello kattintva **feladatok** képernyő segítségével beállíthatja, hogy a feladat tooconfigure. Ez lehetővé teszi a Speciális beállítások konfigurálása, ha nem adja meg azokat a hello gyors létrehozása varázsló.

Az összes művelet esetében hello újrapróbálkozási házirendje és hello hiba művelet módosíthatja.

A HTTP és HTTPS Feladatművelet típusú módosíthatja hello metódus tooany engedélyezett HTTP-műveletet. Előfordulhat, hogy is hozzáadhat, törlése, vagy módosítsa a hello fejlécek és alapszintű hitelesítési adatokat.

A tároló várólista művelet típusú hello tárfiók, üzenetsornév, SAS-jogkivonat és body módosíthatja.

A service bus művelet típusú hello névtér, témakör/várólista elérési útja, hitelesítési beállítások, átviteli típust, üzenet- és az üzenet törzse módosíthatja.

   ![][job-action-settings]

### <a name="schedule"></a>Ütemezés
Ez lehetővé teszi a hello ütemezést konfigurálja újra, ha azt szeretné, hogy toochange hello ütemezés hello létrehozott gyors létrehozása varázsló.

Ez az egy lehetőség toobuild [ütemezése összetett és speciális ismétlődési a feladatban](scheduler-advanced-complexity.md)

Módosíthatja a hello kezdő dátum és idő, illetve ismétlődő ütemezésekhez és hello lejárati dátuma és ideje (ha hello feladat ismétlődő.)

   ![][job-schedule]

### <a name="history"></a>Előzmények
Hello **előzmények** lap megjeleníti a minden feladat végrehajtása a kiválasztott metrikák hello rendszer hello kijelölt feladat. A metrikák a Feladatütemező hello állapotának valós idejű értékeit tartalmazza:

1. status  
2. Részletek  
3. Újrapróbálkozási kísérletek
4. Előfordulás: 1., 2., 3. stb.
5. A futtatás kezdési időpontja  
6. A futtatás befejezési időpontja
   
   ![][job-history]

Rákattinthat a futtatási tooview a **előzmények részletek**, beleértve a hello teljes válasz minden végrehajtásra. Ezen a párbeszédpanelen is lehetővé teszi toocopy hello válasz toohello vágólapra.

   ![][job-history-details]

### <a name="users"></a>Felhasználók
Az Azure Szerepköralapú hozzáférés-vezérlés (RBAC) részletesen beállítható hozzáférés-vezérlést biztosít az Azure Scheduler szolgáltatáshoz. Hogyan toouse hello felhasználók lapon tekintse meg a túl toolearn[átruházásához hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md)

## <a name="see-also"></a>Lásd még:
 [A Scheduler ismertetése](scheduler-intro.md)

 [A Scheduler alapfogalmai, terminológiája és entitáshierarchiája](scheduler-concepts-terms.md)

 [Csomagok és számlázás az Azure Schedulerben](scheduler-plans-billing.md)

 [Hogyan ütemezi a toobuild összetett és speciális ismétlődési és Azure-ütemező](scheduler-advanced-complexity.md)

 [A Scheduler REST API-jának leírása](https://msdn.microsoft.com/library/mt629143)

 [A Scheduler PowerShell-parancsmagjainak leírása](scheduler-powershell-reference.md)

 [Scheduler – magas fokú rendelkezésre állás és megbízhatóság](scheduler-high-availability-reliability.md)

 [Scheduler – korlátozások, alapértékek és hibakódok](scheduler-limits-defaults-errors.md)

 [Kimenő hitelesítés a Schedulerben](scheduler-outbound-authentication.md)

[marketplace-create]: ./media/scheduler-get-started-portal/scheduler-v2-portal-marketplace-create.png
[action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-action-settings.png
[recurrence-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-recurrence-schedule.png
[job-properties]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-properties.png
[job-overview]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-overview-1.png
[job-action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-action-settings.png
[job-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-schedule.png
[job-history]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history.png
[job-history-details]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history-details.png


[1]: ./media/scheduler-get-started-portal/scheduler-get-started-portal001.png
[2]: ./media/scheduler-get-started-portal/scheduler-get-started-portal002.png
[3]: ./media/scheduler-get-started-portal/scheduler-get-started-portal003.png
[4]: ./media/scheduler-get-started-portal/scheduler-get-started-portal004.png
[5]: ./media/scheduler-get-started-portal/scheduler-get-started-portal005.png
[6]: ./media/scheduler-get-started-portal/scheduler-get-started-portal006.png
[7]: ./media/scheduler-get-started-portal/scheduler-get-started-portal007.png
[8]: ./media/scheduler-get-started-portal/scheduler-get-started-portal008.png
[9]: ./media/scheduler-get-started-portal/scheduler-get-started-portal009.png
[10]: ./media/scheduler-get-started-portal/scheduler-get-started-portal010.png
[11]: ./media/scheduler-get-started-portal/scheduler-get-started-portal011.png
[12]: ./media/scheduler-get-started-portal/scheduler-get-started-portal012.png
[13]: ./media/scheduler-get-started-portal/scheduler-get-started-portal013.png
[14]: ./media/scheduler-get-started-portal/scheduler-get-started-portal014.png
[15]: ./media/scheduler-get-started-portal/scheduler-get-started-portal015.png
