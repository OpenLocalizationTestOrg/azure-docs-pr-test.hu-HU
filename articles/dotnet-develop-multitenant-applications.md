---
title: "aaaMulti-bérlő webes Alkalmazásminta |} Microsoft Docs"
description: "Az architektúra áttekintése és tervezési mintáról olvashat, amelyek ismertetik, hogyan tooimplement egy több-bérlős webalkalmazást az Azure-on található."
services: 
documentationcenter: .net
author: wadepickett
manager: wpickett
editor: 
ms.assetid: 4f0281d2-1555-42b0-a99d-1222fade0b0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/05/2015
ms.author: wpickett
ms.openlocfilehash: 3b7822c8ca4aa50d295a174973ae4746c230ba66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="multitenant-applications-in-azure"></a>Több-bérlős alkalmazásokhoz az Azure-ban
Egy több-bérlős alkalmazás, amely lehetővé teszi a különböző felhasználók számára, vagy "bérlők," tooview hello alkalmazást, mintha az volt a saját megosztott erőforrás. A jellemző forgatókönyv, amely több-bérlős tooa alkalmazás adatmodelljeinek egyike hello az összes felhasználók számára az alkalmazás toocustomize hello felhasználói élményt kívánja, de egyébként külön hello ugyanazon alapvető üzleti követelmények. A nagy több-bérlős alkalmazások többek között az Office 365, az Outlook.com-os és a visualstudio.com webhelyre.

Egy alkalmazás szolgáltató szempontjából a több vállalat kiszolgálása hello előnyei többnyire toooperational vonatkoznak, és hatékonyság költség. Az alkalmazás egy verzióját képes igényeinek hello sok bérlő vagy ügyfél, így a rendszer összevonása felügyeleti feladatokhoz, mint a figyelést, teljesítményhangolás, szoftverkarbantartást és az adatok biztonsági.

hello következő hello legjelentősebb célokat és követelményeket a szolgáltató szempontjából a listáját tartalmazza.

* **Kiépítés**: hello alkalmazás képes tooprovision új bérlők számára kell lennie.  A bérlők nagy mennyiségű, több-bérlős alkalmazások esetén általában szükséges tooautomate Ez a folyamat által az önkiszolgáló kiépítés engedélyezése.
* **Karbantartási követelmények**: képes tooupgrade hello alkalmazás és más karbantartási feladatokat végez, amíg a több bérlő-k használják.
* **Figyelési**: meg kell lennie minden alkalommal tooidentify képes toomonitor hello alkalmazást, a problémák és tootroubleshoot őket. Ez magában foglalja, figyelés, hogyan mindegyik bérlő hello alkalmazás használja.

A megfelelően megvalósított több-bérlős alkalmazás maga biztosítja hello következő előnyökkel jár a toousers.

* **Elkülönítési**: hello tevékenységeket az egyes bérlők nincsenek hatással a más bérlők hello alkalmazás hello használatát. Bérlők nem érhető el egymás adatokat. Toohello bérlő úgy tűnik, mintha rendelkeznek hello alkalmazás kizárólagos használatát.
* **Rendelkezésre állási**: egyes bérlői szeretnének hello alkalmazás toobe folyamatosan rendelkezésre álló, lehet, hogy a szolgáltatásiszint-szerződésben garantált definiált garanciát. Ebben az esetben a többi bérlő hello tevékenységek nem érintik hello hello alkalmazás rendelkezésre állásának.
* **Méretezhetőség**: hello alkalmazás méretezi toomeet hello igény szerint az egyes bérlők számára. hello jelenléte és a műveletek a többi bérlő nem érintik hello alkalmazás hello teljesítményét.
* **Költségek**: költségek alacsonyabbak dedikált, egyetlen-bérlő alkalmazást futtat, mert a több-bérlős lehetővé teszi az erőforrás-megosztás hello.
* **Testreszabhatóság miatt**. hello képességét toocustomize hello alkalmazás különböző módokon, például fel szolgáltatásokat, színek és az emblémát módosításával, vagy még a saját kód vagy parancsfájl hozzáadása egy adott bérlő számára.

Röviden közben számos szempontot figyelembe kell fiók tooprovide egy kiválóan méretezhető szolgáltatás számos is hello célokat és követelményeket, amelyek közös toomany több-bérlős alkalmazásokhoz. Néhány nem lehet megfelelő, az adott forgatókönyveket, és egyéni célokat hello fontosságát és követelmények eltérőek lehetnek az egyes esetekben. Több-bérlős alkalmazás hello szolgáltatóként ki is célokat és követelményeket, többek között hello bérlők célok és követelmények, jövedelmezőség, számlázási, több szolgáltatási szintek, kiépítés, karbantartási követelmények figyelését, és automatizálási.

A több-bérlős alkalmazás további kialakítási szempontokkal kapcsolatban további információkért lásd: [egy több-Bérlős alkalmazást az Azure-on][Hosting a Multi-Tenant Application on Azure]. A több bérlős szoftverszolgáltatás (SaaS) típusú adatbázis-alkalmazások általános adatarchitektúra-mintázataival kapcsolatos információk: [Tervminták több-bérlős SaaS-alkalmazásokhoz Azure SQL Database esetén](sql-database/sql-database-design-patterns-multi-tenancy-saas-applications.md). 

Azure biztosít számos olyan szolgáltatást, amelyek lehetővé teszik tooaddress hello kulcs a több-bérlős rendszerek tervezése során észlelt problémákat.

**Elkülönítés**

* Szegmens webhely bérlők által állomásfejléc vagy anélkül SSL-kommunikáció
* Szegmens webhely bérlők által lekérdezés-paraméterek
* A feldolgozói szerepkörök webszolgáltatások
  * Feldolgozói szerepköröket. amely általában hello háttérkiszolgáló az alkalmazás adatokat feldolgozó.
  * Webes szerepkörök, amelyek általában összekötőként hello előtér-alkalmazások.

**Tárolás**

Adatkezelés például az Azure SQL Database vagy az Azure Storage szolgáltatások, például hello Table storage nagy mennyiségű strukturálatlan adatok biztosító szolgáltatás és a Blob szolgáltatás, amely szolgáltatások toostore biztosít a nagy mennyiségű strukturálatlan szöveg hello vagy bináris adatok, például a video-, hang- és lemezképek.

* Az SQL-adatbázis megfelelő biztonságossá tétele több-Bérlős adatok / bérlői SQL Server bejelentkezési azonosítók.
* Azure táblák használata az alkalmazás erőforrások megadásával a tároló szintű hozzáférési házirend, anélkül, hogy tooissue új URL-CÍMEK védett megosztott hozzáférési aláírásokkal hello erőforrások képességét tooadjust engedélyeket is hello.
* Alkalmazás-erőforrások Azure várólisták Azure várólisták nevében bérlők feldolgozása gyakran használt toodrive, de is szükség lehet, használt toodistribute munkahelyi kiépítése, vagy a felügyelet.
* Service Bus-üzenetsorok alkalmazás-erőforrásokat, hogy leküldéses értesítések munkahelyi tooa megosztott egy szolgáltatást, használja egy adott sorba ahol mindegyik bérlő küldő csak van engedélyek (mivel az ACS-től kiadott jogcímeket származó) toopush toothat várólista, közben csak hello fogadók hello szolgáltatás engedély toopull adatokból hello várólista hello több bérlő érkező rendelkezik.

**Kapcsolati és biztonsági szolgáltatások**

* Az Azure Service Bus, egy üzenetkezelési infrastruktúra, amely az alkalmazások engedélyezi azok lazán összekapcsolt megoldást, hogy a javított méretezés és rugalmasság a tooexchange üzenetek között.

**Hálózati szolgáltatások**

Azure, amely támogatja a hitelesítést, és javíthatja a kezelhetőségi az alkalmazások több hálózati szolgáltatásokat biztosít. Ezek a szolgáltatások hello alábbiakat foglalja magába:

* Az Azure virtuális hálózat megadható, hogy Ön kiépítése és virtuális magánhálózatok (VPN) kezelése az Azure-ban, valamint biztonságos hivatkozás ezen a helyszíni informatikai infrastruktúrát.
* Virtuális hálózati Traffic Manager lehetővé teszi bejövő forgalmat több üzemeltetett Azure között szolgáltatások, hogy azok hello futtatja tooload elosztása ugyanabban az adatközpontban vagy hello világ különböző üzemeltetésében.
* Azure Active Directory (Azure AD) szolgáltatás modern REST-alapú szolgáltatás, amely identitás kezelése és hozzáférés-vezérlés képességeinek biztosít a felhőalapú alkalmazásokhoz. Használja az Azure AD alkalmazás-erőforrásokat az Azure AD tooprovides és felhasználók toogain hozzáférés tooyour webalkalmazások és szolgáltatások hitelesítésére miközben lehetővé teszi a hitelesítési és engedélyezési toobe kívüli beleszámítja hello szolgáltatásai egyszerűen a a kódot.
* Az Azure Service Bus egy biztonságos üzenetküldést biztosít és adatok folyamata funkció elosztott és hibrid alkalmazások, például az Azure közötti kommunikáció üzemeltetett alkalmazások és a helyszíni alkalmazásokhoz és szolgáltatásokhoz, anélkül, hogy bonyolult tűzfal- és biztonsági infrastruktúra. Service Bus Relay használatával az alkalmazás-erőforrásokat toohello szolgáltatásokról, amelyek ki vannak téve, mint végpontokat tartozhat toohello bérlői (például kívül hello rendszer, például a helyszínen üzemeltetett), vagy kifejezetten hello bérlői (kiépítve szolgáltatások lehetnek mivel bizalmas, bérlő-specifikus adatok áthaladó őket).

**Erőforrások kiépítése**

Azure számos módon rendelkezés új bérlők hello alkalmazás tartalmazza. A bérlők nagy mennyiségű, több-bérlős alkalmazások esetén általában szükséges tooautomate Ez a folyamat által az önkiszolgáló kiépítés engedélyezése.

* Feldolgozói szerepkörök tooprovision és deaktiválás rendelkezés / bérlői erőforrások (például amikor egy új bérlőt jelentkezik be, vagy megszakítja a) lehetővé teszik, gyűjtéséhez mérési használja, és bizonyos ütemezés kezelésére vagy a válasz toohello metsző kulcs küszöbértékek teljesítménymutatók. Az ugyanarra a szerepkörre is használt toopush, frissítések és verziófrissítések toohello megoldás.
* Azure-blobokat lehet használt tooprovision számítási vagy az új bérlők előre inicializált tárolási erőforrások ugyanakkor biztosítható a tároló hozzáférési házirendek tooprotect hello számítási szolgáltatás csomagok, a virtuális merevlemez képek és egyéb erőforrásokat.
* SQL adatbázis-erőforrások egy bérlő kialakítási lehetőségek a következők:
  
  * A parancsfájlok DDL vagy beágyazott erőforrásként szerelvények belül
  * SQL Server 2008 R2 DAC-csomagokat telepített programozott módon.
  * A fő referencia-adatbázis másolása
  * Adatbázis importálása és exportálása tooprovision új adatbázisok használatával fájlból.

<!--links-->

[Hosting a Multi-Tenant Application on Azure]: http://msdn.microsoft.com/library/hh534480.aspx
[Designing Multitenant Applications on Azure]: http://msdn.microsoft.com/library/windowsazure/hh689716
