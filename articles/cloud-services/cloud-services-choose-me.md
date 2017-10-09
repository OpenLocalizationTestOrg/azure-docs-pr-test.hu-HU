---
title: "aaaAzure számítási lehetőségek - Felhőszolgáltatások |} Microsoft Docs"
description: "További tudnivalók az Azure compute beállítások és a Leírás: az App Service, a Cloud Services és a virtuális gépek"
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
ms.assetid: ed7ad348-6018-41bb-a27d-523accd90305
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: e7f109a54c61cc2f37644d39a61d2d932a374587
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="should-i-choose-cloud-services-or-something-else"></a>Cloud Services szolgáltatásokból vagy más kell választanom?
Azure Cloud Services hello választási lehetőség, hogy a felhasználó? Azure biztosít a különböző üzemeltetési modell-alkalmazások futtatására. Mindegyik számos különböző szolgáltatások, így melyiket választja, attól függ, pontosan dolog, amit toodo.

[!INCLUDE [compute-table](../../includes/compute-options-table.md)]

<a name="tellmecs"></a>

## <a name="tell-me-about-cloud-services"></a>További részletek a cloud services csomag
Cloud Services csomag példája [Platform,--szolgáltatás](https://azure.microsoft.com/overview/what-is-paas/) (PaaS). Például [App Service](../app-service-web/app-service-web-overview.md), ez a technológia méretezhető, megbízható tervezett toosupport alkalmazásokat, és olcsó toooperate. Ahogy az App Service található virtuális gépek, így túl a Felhőszolgáltatások, amelyek azonban rendelkezik a virtuális gépek hello teljesebb körű vezérlése. A Cloud Service virtuális gépeken telepítheti a saját szoftvereit, és a távoli is be őket.

![cs_diagram](./media/cloud-services-choose-me/diagram.png)

Több vezérlő is jelenti, hogy az kisebb könnyű használat. Ha nem kell további hello beállításokat, általában gyorsabb és könnyebb tooget webalkalmazás és az App Service Web Apps futó tooCloud szolgáltatások képest.

A felhőalapú szolgáltatás szerepkörök két típusa van. hello csak két hello közötti különbség hogyan üzemelteti a szerepkör hello virtuális gép.

* **Webes szerepkör**  
Automatikusan telepíti és futtatja az alkalmazás IIS-en keresztül.

* **Feldolgozói szerepkör**  
Nem használják az IIS szolgáltatást, és az alkalmazás önálló futtatja.

Például egy egyszerű alkalmazást használhatja csak egyetlen webes szerepkör, egy webhely szolgál. Olyan összetettebb alkalmazást egy webes szerepkör toohandle használhatja a bejövő kérelmeket a felhasználók számára, akkor továbbítja ezeket a kérelmeket, a feldolgozói szerepkör tooa feldolgozásra. (Ez a kommunikáció használhatja [Service Bus](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md) vagy [Azure várólisták](../storage/common/storage-introduction.md).)

Előző ábra javasol, hello, minden virtuális gépek hello hello futtatni egy alkalmazással azonos felhőalapú szolgáltatás. Felhasználók hozzáférési hello alkalmazást egy egyetlen nyilvános IP-cím, kérelmek keresztül automatikusan terhelésű hello alkalmazás virtuális gépek között. hello platform [méretezi, és telepíti a](cloud-services-how-to-scale.md) virtuális gépek hello oly módon, hogy ezzel elkerülheti a hardver hibaérzékeny pontot Felhőszolgáltatások alkalmazásokban.

Annak ellenére, hogy az alkalmazások virtuális gépek futnak, akkor fontos, hogy a Cloud Services nyújt PaaS, nem IaaS toounderstand. Itt található információk egyirányú toothink: az infrastruktúra-szolgáltatási, például az Azure virtuális gépek, akkor először hozzon létre az alkalmazás fut, a hello környezet konfigurálása, majd erre a környezetre az alkalmazás központi telepítése. Ön a világ számos kezeléséért műveleteket, mint az új javított hello-verzióval működik az egyes virtuális gépek telepítése során. PaaS ezzel szemben, hogy a rendszer, mintha hello környezet már létezik. Mindössze meg kell toodo, az alkalmazás központi telepítése. Az fut, beleértve az új verziók hello operációs rendszerének telepítése hello platform kezelik a kulcsokat meg.

## <a name="scaling-and-management"></a>Méretezés és kezelése
A Cloud Serviceshez ne hozzon létre virtuális gépeket. Ehelyett adja meg, amely közli Azure hány kíváncsi, például a konfigurációs fájl **három webalkalmazás-szerepkörpéldányokat** és **két szerepkör feldolgozópéldányok**, és hello platform létrehozza azt.  Úgy is dönt [milyen méretű](cloud-services-sizes-specs.md) azokat a virtuális gépek biztonsági kell lennie, de nincs explicit módon létrehozásuk magát. Ha az alkalmazásnak toohandle nagyobb terhelést, további virtuális gépek esetén kérje meg, és azokat a példányokat az Azure létrehoz. Ha hello terhelés csökkenése, állítsa le ezeken a példányokon, és állítsa le a számukra fizet.

A Cloud Services általában kérelmet egy kétlépéses folyamat keresztül elérhető toousers. Egy fejlesztő első [feltöltések hello alkalmazás](cloud-services-how-to-create-deploy.md) toohello platform tartozó átmeneti terület. Amikor készen áll a hello fejlesztői toomake hello alkalmazás live, az éles hello Azure portál tooswap átmeneti használata. Ez [átmeneti és üzemi közötti váltás](cloud-services-nodejs-stage-application.md) állásidő nélkül, amely lehetővé teszi, hogy a futó alkalmazások új verzióra frissített tooa a felhasználók megzavarása nélkül kell végezhető.

## <a name="monitoring"></a>Figyelés
Cloud Services csomag figyelést is biztosít. Például Azure virtuális gépek, azt észleli a hibás fizikai kiszolgálóra, és újraindítja a hello egy új gépen az adott kiszolgálón futó virtuális gépek. De a Felhőszolgáltatások is észleli a sikertelen virtuális gépeket és alkalmazásokat, nem csak a hardver meghibásodása. Ellentétben a virtuális gépek, az ügynök belül minden webes és feldolgozói szerepkör rendelkezik, amely ezért tudja toostart új virtuális gépek és a hibák bekövetkezésekor alkalmazáspéldányok.

hello Felhőszolgáltatások PaaS jellege más hatással, túl van. Hello legfontosabb egyike, hogy ez a technológia épülő alkalmazások kell írható toorun megfelelően Ha bármely webes vagy feldolgozói szerepkör példánya nem sikerült. Ez egy alkalmazás karbantartása nem szabad Felhőszolgáltatások nyújtaniuk tooachieve hello fájlrendszer a saját virtuális gépek. A virtuális gépek létrehozott Azure virtuális gépekkel, eltérően tooCloud szolgáltatások virtuális gépeken végrehajtott írási műveletek nem állandó; nem hasonlít a virtuális gépek adatlemez van. Ehelyett a Cloud Services alkalmazás explicit módon kell írni minden állapot tooSQL adatbázis, blobok, táblák, vagy valamilyen egyéb külső tárhelyen. Alkalmazások létrehozásához, így azokat könnyebben tooscale és teszi ellenállóbbá toofailure mindkét fontos célok a Felhőszolgáltatások.

## <a name="next-steps"></a>Következő lépések
[Hozzon létre egy cloud service-alkalmazást a .NET](cloud-services-dotnet-get-started.md)  
[Cloud service-alkalmazás létrehozása a node.js](cloud-services-nodejs-develop-deploy-app.md)  
[Cloud service-alkalmazás létrehozása a PHP](../cloud-services-php-create-web-role.md)  
[Cloud service-alkalmazás létrehozása a Python](cloud-services-python-ptvs.md)

