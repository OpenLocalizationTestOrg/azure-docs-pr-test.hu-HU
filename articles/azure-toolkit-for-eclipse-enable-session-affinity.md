---
title: "aaaEnable munkamenet affinitás használatával hello Eclipse Azure eszköztára"
description: "Ismerje meg, hogyan hello Azure eszköztára Eclipse tooenable munkamenet kapcsolat használatával."
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
ms.openlocfilehash: 523e728c58bda95e7af4b242e831694eb6d75cb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-session-affinity"></a>Munkamenet-kapcsolat engedélyezése
Belül hello Azure eszköztára Eclipse engedélyezheti a HTTP-munkamenetben affinitás, munkamenetből vagy annak "kapcsolódó", a szerepkörök. hello következő kép bemutatja hello **terheléselosztás** tulajdonságai párbeszédpanelen használt tooenable hello munkamenet affinitási szolgáltatás:

![][ic719492]

## <a name="tooenable-session-affinity-for-your-role"></a>tooenable munkamenet-kapcsolatot az adott szerepkörhöz
1. Kattintson a jobb gombbal a Eclipse Project Explorer hello szerepkör, kattintson a **Azure**, és kattintson a **terheléselosztás**.

2. A hello **WorkerRole1 terheléselosztás tulajdonságok** párbeszédpanel:

   a. Ellenőrizze **engedélyezése HTTP munkamenet affinitás (a kapcsolódó munkamenetek) ehhez a szerepkörhöz.**

   b. A **bemeneti végpont toouse**, válassza ki például egy bemeneti végpont toouse **http (nyilvános: 80-as, saját: 8080)**. Az alkalmazás a HTTP-végpontként kell használni ezt a végpontot. Több végpont engedélyezheti az adott szerepkörhöz, ugyanakkor kiválaszthatja csak az egyiket toosupport állandóságát munkamenetek.

   c. Építse újra az alkalmazást.

Miután engedélyezte őket, ha egynél több szerepkörpéldányt, egy adott ügyfél érkező HTTP-kérelmek továbbra is kezeli hello azonos szerepkörpéldányt.

hello Eclipse eszközkészlet lehetővé teszi, hogy ez egy speciális, a szerepkörpéldányok mindegyik alkalmazás alkalmazáskérelmek útválasztása (ARR) nevű IIS modul telepítésével. Az ARR reroutes HTTP kérelmeket toohello megfelelő szerepkörpéldányt. hello eszközkészlet automatikusan ismét beállítja a kiválasztott hello végpont, úgy, hogy hello bejövő HTTP-forgalom első irányított toohello ARR szoftver. hello eszközkészlet is létrehoz egy új belső végpont, amely a Java-kiszolgáló számára konfigurált toolisten. Ez az ARR tooreroute hello HTTP forgalom toohello megfelelő szerepkör példánya által használt hello végpontnak. Ezzel a módszerrel a többpéldányos környezetben minden szerepkörpéldányt, egy fordított proxy összes hello más esetekben a kapcsolódó munkamenetek engedélyezése szolgálja ki.

## <a name="notes-about-session-affinity"></a>Munkamenet-kapcsolatra vonatkozó megjegyzések
* Munkamenet-kapcsolat nem működik a hello compute emulator. hello-beállítások hello compute emulator a felépítési folyamat zavarása nélkül lehet alkalmazni, vagy az emulátor végrehajtási számítási, de hello szolgáltatást hello compute emulator belül nem működik.

* Munkamenet-affinitás lemezterület az Azure, a telepítés során, további szoftvereket a rendszer letölti és telepíti a szerepkörpéldányok hello Azure-felhőbe a szolgáltatás indításakor hello mennyisége növekedését eredményezi.

* hello idő tooinitialize szerepkörönként hosszabb ideig tart.

* Belső végpont, mint a forgalom rerouter, fent említett toofunction lesz hozzáadva.


## <a name="see-also"></a>Lásd még:
[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]

[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]

[Hello Azure eszköztára Eclipse telepítése][Installing hello Azure Toolkit for Eclipse] 

Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[How tooMaintain Session Data with Session Affinity]: http://go.microsoft.com/fwlink/?LinkID=699539
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->
