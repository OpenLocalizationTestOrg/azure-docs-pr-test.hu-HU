---
title: "aaaAzure IoT Hub IP-kapcsolat szűrők |} Microsoft Docs"
description: "Hogyan toouse az adott IP-címről tooblock kapcsolatok szűrési IP-címeket tooyour Azure IoT-központ. Letilthatja egyes érkező kapcsolatokat vagy IP-címek tartományát."
services: iot-hub
documentationcenter: 
author: BeatriceOltean
manager: timlt
editor: 
ms.assetid: f833eac3-5b5f-46a7-a47b-f4f6fc927f3f
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2017
ms.author: boltean
ms.openlocfilehash: 45e5906a494561b6108895d86d6a04fc3b52b8fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-ip-filters"></a>IP-szűrők használata

A biztonság az Azure IoT Hub alapján IoT megoldások fontos eleme. Néha szüksége tooexplicitly hello IP-címet, amelyből az eszközök csatlakozni tud megadni a biztonsági beállítások részeként. Hello _IP-szűrő_ funkciója lehetővé teszi elutasítása, vagy meghatározott IPv4-címeket érkező forgalmat fogad tooconfigure szabályait.

## <a name="when-toouse"></a>Ha toouse

Nincsenek két adott használati esetek hasznos tooblock hello IoT-központok végpontjai bizonyos IP-címek esetén:

- Az IoT hub kell forgalom fogadására csak a megadott IP-címről, és minden más elutasítása. Az IoT hub való használata esetén például [Azure Express Route] toocreate magánhálózati kapcsolatok az IoT-központ és a helyszíni infrastruktúra között.
- Hello IoT hub-rendszergazda által azonosított, gyanús IP-címekről tooreject forgalom van szüksége.

## <a name="how-filter-rules-are-applied"></a>Állapotszűrő szabályok alkalmazása

IoT Hub szolgáltatási szint hello hello IP Állapotszűrő szabályok érvényesek. Ezért hello IP Állapotszűrő szabályok alkalmazása tooall kapcsolatok az eszközökről és a háttér-alkalmazások bármely támogatott protokoll használatával.

A kapcsolódási kísérlet egy IP-címről, amely megfelel egy rejecting IP szabályt az IoT hub kap egy jogosulatlan 401-es állapotkód és a leírást. hello válaszüzenetet nem említi hello IP szabály.

## <a name="default-setting"></a>Alapértelmezett beállítás

Alapértelmezés szerint hello **IP-szűrő** hello portálon az IoT-központ a rács értéke üres. Ez az alapértelmezett beállítás azt jelenti, hogy a központ kapcsolatokat fogad IP-címeket. Az alapértelmezett érték egyenértékű tooa szabály, amely fogadja a hello 0.0.0.0/0 IP-címtartományt.

![Az IoT-központ alapértelmezett IP-szűrési beállítások][img-ip-filter-default]

## <a name="add-or-edit-an-ip-filter-rule"></a>Az IP-szűrési szabály felvétele vagy szerkesztése

Amikor egy IP-szűrő szabályt, a következő értékek hello kéri:

- Egy **IP-szűrés szabálynév** , amely másolatot too128 karakter egyedi, nem betűérzékeny, alfanumerikus karakterláncnak kell lennie. Csak ASCII 7 bites hello alfanumerikus karakterek plusz `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` elfogadottak.
- Válassza ki a **elutasítása** vagy **fogadja el** , hello **művelet** hello IP-szűrési szabály.
- Adjon meg egy IPv4-cím vagy IP-címek a CIDR jelölésrendszer blokkban. Például a CIDR jelölésrendszer 192.168.100.0/22 jelöli hello 1024 IPv4-címek 192.168.100.0 too192.168.103.255.

![Adja hozzá az IP szűrési szabály tooan IoT-központ][img-ip-filter-add-rule]

Hello szabály mentése után megjelenik egy figyelmeztetés értesíti, hogy hello frissítése folyamatban van.

![Értesítés egy IP-szűrési szabály mentése][img-ip-filter-save-new-rule]

Hello **Hozzáadás** lehetőség le van tiltva, ha eléri hello legfeljebb 10 IP-szűrő szabályokban.

Meglévő szabály hello sor hello szabályt tartalmazó duplán kattintva módosíthatja.

> [!NOTE]
> Elutasítása IP címeket így előfordulhat, hogy más Azure-szolgáltatások (például az Azure Stream Analytics, Azure virtuális gépek vagy hello eszköz Explorer hello portálon) hello IoT-központ való interakció.

> [!WARNING]
> IP-szűrés engedélyezve van az IoT-központ tooread üzeneteit Azure Stream Analytics (ASA) használja, ha használja hello Event Hub-kompatibilis nevét és az IoT-központ végpontjának a hello ASA kapcsolati karakterláncot.

## <a name="delete-an-ip-filter-rule"></a>Az IP-szűrési szabály törlése

toodelete egy IP-szűrő szabályt, jelöljön ki egy vagy több szabály hello rács kattintson **törlése**.

![Az IoT-központ IP-szűrési szabály törlése][img-ip-filter-delete-rule]

## <a name="ip-filter-rule-evaluation"></a>IP-szűrési szabály értékelése

IP-szűrési szabályokat a rendszer sorrendben alkalmazza, és hello első szabály, hogy megfelel hello IP-címét határozza meg a hello fogadja el vagy utasítsa el a műveletet.

Például ha szeretné, hogy hello tartomány 192.168.100.0/22 tooaccept címeit, és utasítsa el minden más, hello első szabály hello rácsban hello cím tartomány 192.168.100.0/22 kell fogadnia. hello közvetkező szabályának hello tartomány 0.0.0.0/0 használatával kell utasítania összes címet.

Az IP-Állapotszűrő szabályok hello rácsban hello sorrendjének módosításához kattintson a hello függőleges három pont sor hello elején, majd húzza használatával, és dobja el.

toosave az új IP-szűrés rendelés szabály, kattintson a **mentése**.

![Az IoT-központ IP-szűrési szabályokat hello sorrendjének módosítása][img-ip-filter-rule-order]

## <a name="next-steps"></a>Következő lépések

toofurther megismerkedhet az IoT-központ hello képességeit, lásd:

- [Figyelési műveletek][lnk-monitor]
- [Az IoT-központ metrikák][lnk-metrics]

<!-- Images -->
[img-ip-filter-default]: ./media/iot-hub-ip-filtering/ip-filter-default.png
[img-ip-filter-add-rule]: ./media/iot-hub-ip-filtering/ip-filter-add-rule.png
[img-ip-filter-save-new-rule]: ./media/iot-hub-ip-filtering/ip-filter-save-new-rule.png
[img-ip-filter-delete-rule]: ./media/iot-hub-ip-filtering/ip-filter-delete-rule.png
[img-ip-filter-rule-order]: ./media/iot-hub-ip-filtering/ip-filter-rule-order.png


<!-- Links -->

[IoT Hub developer guide]: iot-hub-devguide.md
[Azure Express Route]:  https://azure.microsoft.com/en-us/documentation/articles/expressroute-faqs/#supported-services

[lnk-monitor]: iot-hub-operations-monitoring.md
[lnk-metrics]: iot-hub-metrics.md