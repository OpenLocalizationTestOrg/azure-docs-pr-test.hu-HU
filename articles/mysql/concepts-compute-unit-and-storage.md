---
title: "Ismertető kiszámítani az Azure-adatbázis egység a MySQL |} Microsoft Docs"
description: "Azure MySQL-adatbázis: Ez a cikk ismerteti a számítási egységeket, és mi történik, amikor eléri a számítási feladatok hello elveit hello maximális számítási egység."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/23/2017
ms.openlocfilehash: 751b4fff2760e55565e2bc80d49db17a57397779
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="explaining-compute-units-in-azure-database-for-mysql"></a>Azure MySQL-adatbázis elmagyarázza számítási egység
Ez a cikk ismerteti a számítási egység hello fogalmát, és mi történik, ha a munkaterhelés eléri maximális számítási egység hello.

## <a name="what-are-compute-units"></a>Mik azok a számítási egység
Számítási egység egy mérték, amely garantáltan toobe elérhető tooa Processzor feldolgozási átviteli Azure-adatbázis egy MySQL-kiszolgáló. A számítási egység mérőszáma kevert CPU és memória-erőforrásokat. 50 számítási egység általában az alapszintű toohalf egyenlő. 100 számítási egység tooone core egyenlő. 2000 számítási egység tootwenty magok garantált adatfeldolgozást átviteli elérhető tooyour kiszolgáló egyenlő.

hello memóriamennyiség számítási egységenként hello Basic és Standard tarifacsomag szükséges van optimalizálva. Így dupla hello számítási egység hello teljesítményszintet növelésével csatlakozás toodoubling hello készlete erőforrás elérhető toothat MySQL az Azure-adatbázis egy.

Például egy szabványos 800 számítási egység nyújt további 8 x CPU átviteli sebesség és a memóriát, mint egy normál 100 számítási egység konfigurációs. Azonban amíg szabványos 100 számítási egységek hello azonos CPU átviteli tooBasic 100 számítási egység képest, a hello memóriamennyiség, amely előre konfigurálva, a Standard tarifacsomag kettős hello mennyiségű memóriával konfigurált IP-címek Basic. Ezért a Standard tarifacsomag munkaterhelés jobb teljesítményt és kisebb tranzakció késést biztosít alapvető tarifacsomag azonos számítási egység kijelölt hello biztosít.

## <a name="how-can-i-determine-hello-number-of-compute-units-needed-for-my-workload"></a>Hogyan lehet eldönteni, hogy a munkaterhelés számára szükséges számítási egység hello száma?
Ha egy meglévő MySQL-kiszolgálót futtató helyszíni toomigrate vagy virtuális gépen, úgy határozhatja meg hello számítási egységek számának becslése feldolgozási átviteli hány magok a számítási feladatok kell. 

Ha a meglévő helyszíni vagy a virtuális gép kiszolgálón van jelenleg okhoz 4 mag (nélkül számbavételi CPU hyperthread), első lépésként 400 számítási egység az Azure-adatbázis MySQL-kiszolgáló konfigurálása. Számítási egység dinamikusan méretezhetők felfelé vagy lefelé gyakorlatilag alkalmazás állásidő nélkül munkaterhelés igényeitől függően. 

Az Azure portál vagy írási Azure CLI parancsok - toomeasure hello hello metrikák grafikont számítási egység. Kapcsolódó metrikák toomonitor hello számítási egység százalékos értékét és számítási egység korlátot.

>[!IMPORTANT]
> Ha tárolási IOPS teljesen még nem használt toohello maximális, fontolja meg a figyelés hello számítási egység kihasználtsága, valamint. Hello számítási egység előléptetése is engedélyezheti a magasabb IO átviteli sebesség eléréséhez csökkentését hello teljesítménybeli szűk keresztmetszetek toolimited CPU és memória miatt.

## <a name="what-happens-when-i-hit-my-maximum-compute-units"></a>Mi történik, ha szeretnék elérte a maximális számítási egység?
Teljesítményszintet kalibrálni vannak, és szabályozott tooprovide erőforrások toorun az adatbázis munkaterhelés mentése toohello maximális korlátozhatja a kijelölt tarifacsomag hello és teljesítményszint szükséges. 

Ha a számítási feladatok eléri a maximális méretre hello vagy hello számítási egység vagy kiosztott IOPS-korlátok vonatkoznak, folytatás tooutilize hello erőforrások hello maximális engedélyezett szinten, de a lekérdezések nincsenek nagyobb valószínűséggel toosee késések fordulnak elő. Ezek a korlátozások nem eredményeznek ki a hibákat, de ahelyett, hogy a hello munkaterhelés lassulást hacsak hello lassulást el súlyos, amely lekérdezi túllépi az időkorlátot. 

Ha a terhelést eléri hello kapcsolatok számának felső határai, explicit hibák aktiválódnak. Erőforrások korlátozások további információkért lásd: [MySQL az Azure Database korlátozásai](concepts-limits.md).

## <a name="next-steps"></a>Következő lépések
A tarifacsomagok további információkért lásd: [tarifacsomagok MySQL az Azure-adatbázis](./concepts-service-tiers.md).
