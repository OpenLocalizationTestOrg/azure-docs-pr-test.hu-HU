---
title: "aaaAzure Mobile Engagement hibaelhárítási útmutatója - szolgáltatás"
description: "Hibaelhárítás az Azure Mobile Engagement útmutatók"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 8b4275da-c0b4-4690-824a-48e9d7a1fc6e
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: cf48db323a873ccef95946f7bb26e8d7473c002f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-service-issues"></a>A szolgáltatásokkal kapcsolatos problémákról hibaelhárítási útmutatója
Az alábbiakban hello lehetséges problémák merülhetnek fel az Azure Mobile Engagement működésével.

## <a name="service-outages"></a>Szolgáltatás-kimaradások számát
### <a name="issue"></a>Probléma
* Azure Mobile Engagement szolgáltatáskimaradások által okozott toobe megjelenő problémákat.

### <a name="causes"></a>Okok
* Azure Mobile Engagement szolgáltatáskimaradások által okozott toobe megjelenő problémák számos különböző probléma okozhatja:
  * Az Azure Mobile Engagement rendszeres tooall eredetileg megjelenő elkülönített problémák
  * Ismert problémák okozta a kiszolgáló valamilyen okból kimaradás (mindig nem jeleníti meg a kiszolgáló állapota):
  * Ütemezés késések célcsoportkezelést, jelvény a frissítéssel kapcsolatos problémák, hibák statisztika stop gyűjtése, leküldéses nem működik, API leáll, új alkalmazásokat, vagy a felhasználók nem hozható létre, DNS-hibák, és időtúllépést hello felhasználói felület, API vagy alkalmazásokat az eszközön.
  * Függőség kimaradások cloud [Azure szolgáltatás állapota](http://status.azure.com/)
  * Leküldéses értesítési szolgáltatások (PNS) függőségi leállások esetén
  * Alkalmazás-áruház leállások esetén

1) Ha hello probléma rendszeres tootest toosee olyan funkciókat, a különböző hello tesztelheti

* Az Azure Mobile Engagement integrált alkalmazás
* Vizsgálati eszköz
* Vizsgálati eszköz operációs rendszer verziója
* Kampány
* Rendszergazdai felhasználói fiókkal
* Böngésző (IE, Firefox Chrome, stb.)
* Computer

2) Ha a probléma hello csak hatással van a hello felhasználói felületén vagy a hello API tootest:

* Teszt hello azonos mindkét hello Azure Mobile Engagement felhasználói felületén a funkciót, és Azure Mobile Engagement API hello.

3) Ha hello probléma van a mobiltelefon hálózati tootest:

* Teszt során csatlakoztatott toohello interneten keresztül Wi-Fi és a 3G mobiltelefon hálózaton keresztül csatlakozik.
* Győződjön meg arról, hogy a tűzfal nem blokkolja hello Azure Mobile Engagement IP-címek és portok.

4) Ha az eszköz hello probléma tootest:

* Ha az eszköz képes tooconnect tooAzure a Mobile Engagement egy másik integrált Azure Mobile Engagement-alkalmazás tesztelése.
* Tesztelje, hogy eseményeket, a feladatok és az összeomlások hozhat létre, amely hello Azure Mobile Engagement felhasználói felülete látható a telefonjáról. 
* Ha az eszköz azonosító alapján hello Azure Mobile Engagement felhasználói felület tooyour eszközről küldhet leküldéses értesítést tesztelése 

5) Ha hello probléma van az alkalmazás tootest:

* Telepítse, és tesztelje az alkalmazás az emulátor helyett egy fizikai eszközről:

6) Ha hello probléma van az operációs rendszer frissítései tooend felhasználói eszközök, az SDK frissítési tooresolve igénylő tootest:

* Hello az operációs rendszer különböző verziói különböző eszközökön az alkalmazás teszteléséhez.
* Ellenőrizze, hogy hello hello SDK legújabb verzióját használja-e.

## <a name="connectivity-and-incorrect-information-issues"></a>Kapcsolat és a megfelelő információkat problémák
### <a name="issue"></a>Probléma
* Bejelentkezés problémák hello Azure Mobile Engagement felhasználói felületén.
* Hello Azure Mobile Engagement API kapcsolati hibák.
* App-Info címkék feltöltésével keresztül hello Device API-val kapcsolatos problémák.
* A problémák naplók és exportált adatok letöltése az Azure Mobile engagement szolgáltatásból.
* Helytelen információt hello Azure Mobile Engagement felhasználói felülete látható.
* Azure Mobile Engagement naplók látható tájékoztatás helytelen.

### <a name="causes"></a>Okok
* Győződjön meg arról, a felhasználói fiók rendelkezik a megfelelő engedélyek tooperform hello feladatot.
* Győződjön meg arról, hogy hello probléma nem elszigetelt tooone számítógép vagy a helyi hálózaton.
* Győződjön meg róla, hogy adott hello Azure Mobile Engagement-szolgáltatás nem jelentett üzemkimaradások.
* Győződjön meg arról, hogy az App-Info címke fájlok hajtsa végre az összes szabályt:
  * Használjon csak hello UTF8 karakterkészlet (hello ANSI karakterkészlet nem támogatott).
  * Használjon vesszőt "," hello elválasztó karakterként (a vesszőt nyithatja meg a szolgáltatási kérelem toorequest toochange hello .csv elválasztó karaktert pontosvesszővel tooanother karakter "," ";").
  * Minden kisbetű használható logikai értékek a "true" és "false".
  * Maximális fájlméret hello 35MB-nál kisebb fájlt használja.

