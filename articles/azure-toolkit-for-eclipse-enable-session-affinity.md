---
title: "Az Azure-eszközkészlet használata az Eclipse munkamenet-kapcsolat engedélyezése"
description: "Megtudhatja, hogyan engedélyezze a munkamenet-kapcsolatot az Azure-eszközkészlet használata az eclipse-ben."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 88c595ec-7d85-40bd-9078-8d6be7b3f0fa
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: ab8623d6f9751ed6d71d9a5b1c0d5e939c442862
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="enable-session-affinity"></a>Munkamenet-kapcsolat engedélyezése
Belül az Eclipse Azure eszközkészlet engedélyezheti a HTTP-munkamenetben affinitás, munkamenetből vagy annak "kapcsolódó", a szerepkörök. Az alábbi képen látható a **terheléselosztás** tulajdonságainak párbeszédpanelét jeleníti meg a munkamenet affinitási szolgáltatás lehetővé teszi, hogy:

![][ic719492]

## <a name="to-enable-session-affinity-for-your-role"></a>Munkamenet-kapcsolatot az adott szerepkörhöz engedélyezése
1. Kattintson a jobb gombbal a szerepkör által Eclipse Project Explorer parancsra **Azure**, és kattintson a **terheléselosztás**.

2. Az a **WorkerRole1 terheléselosztás tulajdonságok** párbeszédpanel:

   a. Ellenőrizze **engedélyezése HTTP munkamenet affinitás (a kapcsolódó munkamenetek) ehhez a szerepkörhöz.**

   b. A **bemeneti végpont használandó**, válassza ki például használatához bemeneti végpontja **http (nyilvános: 80-as, saját: 8080)**. Az alkalmazás a HTTP-végpontként kell használni ezt a végpontot. Több végpont engedélyezheti az adott szerepkörhöz, ugyanakkor kiválaszthatja, csak az egyiket a kapcsolódó munkamenetek támogatásához.

   c. Építse újra az alkalmazást.

Miután engedélyezte őket, ha egynél több szerepkörpéldányt, egy adott ügyfél érkező HTTP-kérelmek továbbra is megegyező szerepkörpéldányon kezeli.

Az Eclipse eszközkészlet lehetővé teszi, hogy ez egy speciális, a szerepkörpéldányok mindegyik alkalmazás alkalmazáskérelmek útválasztása (ARR) nevű IIS modul telepítésével. Az ARR reroutes HTTP-kéréseket a megfelelő szerepkör példánya. Az eszközkészlet automatikusan újrakonfigurálja a kijelölt végponttanúsítvány, úgy, hogy a bejövő HTTP-forgalom először csatlakoznak az ARR szoftver. Az eszközkészlet is létrehoz egy új belső végpontot, amely a Java kiszolgáló figyelésére van konfigurálva. Ez a végpont a megfelelő szerepkör-példányhoz a HTTP-forgalom átirányítása ARR segítségével. Ezzel a módszerrel a többpéldányos környezetben minden szerepkörpéldányt fordított proxykiszolgálójaként a többi példányát, a kapcsolódó munkamenetek engedélyezése funkcionál.

## <a name="notes-about-session-affinity"></a>Munkamenet-kapcsolatra vonatkozó megjegyzések
* Munkamenet-kapcsolat nem működik a compute emulator. A beállítások a felépítési folyamat zavarása nélkül lehet alkalmazni a compute emulator vagy compute emulator végrehajtási, de maga a szolgáltatás nem működik a compute emulator belül.

* Munkamenet-affinitás lemezterület az Azure, a telepítés során, további szoftvereket a rendszer letölti és telepíti a szerepkörpéldányok a szolgáltatás indításakor Azure felhőben mennyiségének olyan növekedése eredményez.

* Minden egyes szerepkör inicializálása idő hosszabb ideig tart.

* A forgalom rerouter fog működni, fent említett belső végpont lesz hozzáadva.


## <a name="see-also"></a>Lásd még:
[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]

[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]

[Az eclipse-ben az Azure eszközkészlet telepítése][Installing the Azure Toolkit for Eclipse] 

Azure Java használatával kapcsolatos további információkért lásd: a [Azure Java fejlesztői központból][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[How to Maintain Session Data with Session Affinity]: http://go.microsoft.com/fwlink/?LinkID=699539
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->
