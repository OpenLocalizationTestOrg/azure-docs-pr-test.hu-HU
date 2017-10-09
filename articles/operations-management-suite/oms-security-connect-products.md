---
title: "aaaConnecting a biztonsági termékek toohello Operations Management Suite (OMS) biztonsági és a naplózási megoldás |} Microsoft Docs"
description: "A dokumentum segítséget nyújt, tooconnect a biztonsági termékek tooOperations felügyeleti csomag biztonsági és naplózási megoldás közös esemény formátumban."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 46eee484-e078-4bad-8c89-c88a3508f6aa
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: yurid
ms.openlocfilehash: 0f4b372d0379987c4e249628a3c8d52733be65c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-your-security-products-toohello-operations-management-suite-oms-security-and-audit-solution"></a>Csatlakozás a biztonsági termékek toohello Operations Management Suite (OMS) biztonsági és naplózási megoldás 
Ez a dokumentum segít a biztonsági termékek hello OMS biztonsági és naplózási megoldás. a következő források hello támogatottak:

- Common Event Format- (CEF-) események;
- Cisco ASA-események.


## <a name="what-is-cef"></a>Mi az a CEF?
Közös esemény formátum (CEF) egy iparági szabványos formátumban Syslog-üzeneteket, számos biztonsági szállítók tooallow esemény együttműködési között különböző platformokon által használt felett. OMS biztonsági és naplózási megoldás támogatja az adatfeldolgozást az OMS biztonsági a biztonsági termékek CEF, ami lehetővé teszi a tooconnect használata. 

A data source tooOMS összekötésével a következő funkciókat nyújtanak, amelyek ehhez a platformhoz tartozó hello képes tootake előnye:

- Keresés és korreláció
- Naplózás
- Riasztás
- Fenyegetések felderítése
- Jelentős problémák

## <a name="collection-of-security-solution-logs"></a>Biztonsági megoldások naplóinak gyűjtése

Az OMS biztonsági megoldás a Syslog-alapú CEF-naplók és [Cisco ASA](https://blogs.technet.microsoft.com/msoms/2016/08/25/add-your-cisco-asa-logs-to-oms-security/)-naplók használatával támogatja a naplók gyűjtését. Ebben a példában hello forrás (a számítógép által generált hello naplók) Linux számítógép syslog-ng démon fut, és hello célja OMS biztonsági. feladatok tooprepare hello Linux számítógépre következő tooperform hello szüksége lesz:

- Töltse le a hello OMS-ügynököt, Linux, 1.2.0-25 verzió vagy újabb operációs rendszerre.
- Hajtsa végre a hello szakasz **telepítése útmutató** a [Ez a cikk](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux) tooinstall és előkészítésére hello ügynök tooyour munkaterületen.

Általában hello ügynök telepítve van egy mely hello naplók jönnek létre hello eltérő számítógépre. Továbbító hello naplók toohello ügynökszámítógépet általában szükséges lépések hello:

- Termék/gép tooforward hello szükséges események toohello a syslog démon, (rsyslog vagy a syslog-ng) naplózás hello konfigurálása hello ügynökszámítógépet.
- Engedélyezze a syslog démon hello ügynök gép tooreceive köszönőüzenetei egy távoli rendszerből.

Hello ügynök gépen hello események hello syslog démon toolocal UDP-port 25226 küldött toobe kell. Ezt a portot a bejövő események hello ügynök figyel. hello az alábbiakban olvashat egy példa konfiguráció az összes esemény hello helyi rendszer toohello ügynöktől (hello konfigurációs toofit a helyi beállításokat módosíthatja):

1. Nyissa meg hello terminálablakot, és lépjen toohello directory */etc/syslog-ng /* 
2. Hozzon létre egy új fájlt *biztonsági-config-omsagent.conf* , és adja hozzá a tartalom a következő hello: OMS_facility = local4
    
    filter f_local4_oms { facility(local4); };

    destination security_oms { tcp("127.0.0.1" port(25226)); };

    log { source(src); filter(f_local4_oms); destination(security_oms); };
    
3. Töltse le a hello fájl *security_events.conf* , majd helyezze */etc/opt/microsoft/omsagent/conf/omsagent.d/* hello OMS-ügynököt a számítógépben.
4. Írja be alább toorestart hello a syslog démon hello parancsot: *földgáznál syslog-futtatása:*
    
    ```
    sudo service rsyslog restart
    ```

    *Az rsyslog futtatása esetén:*
    
    ```
    /etc/init.d/syslog-ng restart
    ```
5. Írja be alább toorestart hello OMS-ügynököt hello parancsot:

    *A syslog-ng futtatása esetén:*
    
    ```
    sudo service omsagent restart
    ```

    *Az rsyslog futtatása esetén:*
    
    ```
    systemctl restart omsagent
    ```
6. Írja be az alábbi hello parancsot, és ellenőrizze, hogy nincsenek-e hibák hello OMS-ügynököt napló hello eredmény tooconfirm:

    ``` 
    tail /var/opt/microsoft/omsagent/log/omsagent.log
    ```

## <a name="reviewing-collected-security-events"></a>Az összegyűjtött biztonsági események áttekintése

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

Követően hello konfigurációs, hello biztonsági esemény OMS biztonsági okozhatnak toobe indul el. toovisualize ezeket az eseményeket, nyissa meg hello napló keresése hello paranccsal *típus = CommonSecurityLog* a hello keresési mező, és nyomja le az ENTER BILLENTYŰT. hello következő példa bemutatja, ez a parancs eredménye hello figyelje meg, hogy ebben az esetben OMS biztonsági már okozhatnak a biztonsági naplók több szállítóktól származó:
   
![Az OMS biztonsági és auditálási alapkonfiguráció értékelése](./media/oms-security-connect-products/oms-security-connect-products-fig1.png)

Ez a keresés egy adott forgalmazótól, például toovisualize online Cisco naplózza, írja be a pontosíthatja: *típus CommonSecurityLog DeviceVendor = = Cisco*. hello "CommonSecurityLog" mezők előre meghatározott tartozó bármely CEF fejléc hello alapvető extensios, minden más bővítmény közben beleértve, hogy "Egyéni kiterjesztés", vagy nem, program beszúrja a "AdditionalExtensions" mező. Hello egyéni mezők szolgáltatás dedikált tooget mezők származó is használhatja. 

### <a name="accessing-computers-missing-baseline-assessment"></a>Az alapkonfiguráció értékeléséből kimaradt számítógépek elérése
OMS hello tartományi tag alapterv profil Windows Server 2008 R2 rendszerben legfeljebb tooWindows Server 2012 R2 támogat. A Windows Server 2016 alapkonfigurációja még nem végleges, és közzététele után azonnal megtörténik a hozzáadása. Minden más operációs rendszerek OMS biztonsági és naplózási alapterv értékeléssel beolvasott hello alatt szerepelhet **hiányzik az alapkonfiguráció értékelése számítógépek** szakasz.

## <a name="see-also"></a>Lásd még:
Ebben a dokumentumban, megtudta, hogyan tooconnect a CEF megoldás tooOMS. További információ az OMS biztonsági toolearn tekintse meg a következő cikkek hello:

* [Az Operations Management Suite (OMS) áttekintése](operations-management-suite-overview.md)
* [Figyelés és a válaszoló tooSecurity riasztásait az Operations Management Suite biztonsági és naplózási megoldás](oms-security-responding-alerts.md)
* [Az erőforrások figyelése az Operations Management Suite biztonsági és auditálási megoldásban](oms-security-monitoring-resources.md)

