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
# <a name="connecting-your-security-products-toohello-operations-management-suite-oms-security-and-audit-solution"></a><span data-ttu-id="48adf-103">Csatlakozás a biztonsági termékek toohello Operations Management Suite (OMS) biztonsági és naplózási megoldás</span><span class="sxs-lookup"><span data-stu-id="48adf-103">Connecting your security products toohello Operations Management Suite (OMS) Security and Audit Solution</span></span> 
<span data-ttu-id="48adf-104">Ez a dokumentum segít a biztonsági termékek hello OMS biztonsági és naplózási megoldás.</span><span class="sxs-lookup"><span data-stu-id="48adf-104">This document helps you connect your security products into hello OMS Security and Audit Solution.</span></span> <span data-ttu-id="48adf-105">a következő források hello támogatottak:</span><span class="sxs-lookup"><span data-stu-id="48adf-105">hello following sources are supported:</span></span>

- <span data-ttu-id="48adf-106">Common Event Format- (CEF-) események;</span><span class="sxs-lookup"><span data-stu-id="48adf-106">Common Event Format (CEF) events</span></span>
- <span data-ttu-id="48adf-107">Cisco ASA-események.</span><span class="sxs-lookup"><span data-stu-id="48adf-107">Cisco ASA events</span></span>


## <a name="what-is-cef"></a><span data-ttu-id="48adf-108">Mi az a CEF?</span><span class="sxs-lookup"><span data-stu-id="48adf-108">What is CEF?</span></span>
<span data-ttu-id="48adf-109">Közös esemény formátum (CEF) egy iparági szabványos formátumban Syslog-üzeneteket, számos biztonsági szállítók tooallow esemény együttműködési között különböző platformokon által használt felett.</span><span class="sxs-lookup"><span data-stu-id="48adf-109">Common Event Format (CEF) is an industry standard format on top of Syslog messages, used by many security vendors tooallow event interoperability among different platforms.</span></span> <span data-ttu-id="48adf-110">OMS biztonsági és naplózási megoldás támogatja az adatfeldolgozást az OMS biztonsági a biztonsági termékek CEF, ami lehetővé teszi a tooconnect használata.</span><span class="sxs-lookup"><span data-stu-id="48adf-110">OMS Security and Audit Solution support data ingestion using CEF, which enables you tooconnect your security products with OMS Security.</span></span> 

<span data-ttu-id="48adf-111">A data source tooOMS összekötésével a következő funkciókat nyújtanak, amelyek ehhez a platformhoz tartozó hello képes tootake előnye:</span><span class="sxs-lookup"><span data-stu-id="48adf-111">By connecting your data source tooOMS, you are able tootake advantage of hello following capabilities that are part of this platform:</span></span>

- <span data-ttu-id="48adf-112">Keresés és korreláció</span><span class="sxs-lookup"><span data-stu-id="48adf-112">Search & Correlation</span></span>
- <span data-ttu-id="48adf-113">Naplózás</span><span class="sxs-lookup"><span data-stu-id="48adf-113">Auditing</span></span>
- <span data-ttu-id="48adf-114">Riasztás</span><span class="sxs-lookup"><span data-stu-id="48adf-114">Alert</span></span>
- <span data-ttu-id="48adf-115">Fenyegetések felderítése</span><span class="sxs-lookup"><span data-stu-id="48adf-115">Threat Intelligence</span></span>
- <span data-ttu-id="48adf-116">Jelentős problémák</span><span class="sxs-lookup"><span data-stu-id="48adf-116">Notable Issues</span></span>

## <a name="collection-of-security-solution-logs"></a><span data-ttu-id="48adf-117">Biztonsági megoldások naplóinak gyűjtése</span><span class="sxs-lookup"><span data-stu-id="48adf-117">Collection of security solution logs</span></span>

<span data-ttu-id="48adf-118">Az OMS biztonsági megoldás a Syslog-alapú CEF-naplók és [Cisco ASA](https://blogs.technet.microsoft.com/msoms/2016/08/25/add-your-cisco-asa-logs-to-oms-security/)-naplók használatával támogatja a naplók gyűjtését.</span><span class="sxs-lookup"><span data-stu-id="48adf-118">OMS Security supports collection of logs using CEF over Syslogs and [Cisco ASA](https://blogs.technet.microsoft.com/msoms/2016/08/25/add-your-cisco-asa-logs-to-oms-security/) logs.</span></span> <span data-ttu-id="48adf-119">Ebben a példában hello forrás (a számítógép által generált hello naplók) Linux számítógép syslog-ng démon fut, és hello célja OMS biztonsági.</span><span class="sxs-lookup"><span data-stu-id="48adf-119">In this example, hello source (computer that generates hello logs) is a Linux computer running syslog-ng daemon and hello target is OMS Security.</span></span> <span data-ttu-id="48adf-120">feladatok tooprepare hello Linux számítógépre következő tooperform hello szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="48adf-120">tooprepare hello Linux computer you will need tooperform hello following tasks:</span></span>

- <span data-ttu-id="48adf-121">Töltse le a hello OMS-ügynököt, Linux, 1.2.0-25 verzió vagy újabb operációs rendszerre.</span><span class="sxs-lookup"><span data-stu-id="48adf-121">Download hello OMS Agent for Linux, version 1.2.0-25 or above.</span></span>
- <span data-ttu-id="48adf-122">Hajtsa végre a hello szakasz **telepítése útmutató** a [Ez a cikk](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux) tooinstall és előkészítésére hello ügynök tooyour munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="48adf-122">Follow hello section **Quick Install Guide** from [this article](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux) tooinstall and onboard hello agent tooyour workspace.</span></span>

<span data-ttu-id="48adf-123">Általában hello ügynök telepítve van egy mely hello naplók jönnek létre hello eltérő számítógépre.</span><span class="sxs-lookup"><span data-stu-id="48adf-123">Typically, hello agent is installed on a different computer from hello one on which hello logs are generated.</span></span> <span data-ttu-id="48adf-124">Továbbító hello naplók toohello ügynökszámítógépet általában szükséges lépések hello:</span><span class="sxs-lookup"><span data-stu-id="48adf-124">Forwarding hello logs toohello agent machine will usually require hello following steps:</span></span>

- <span data-ttu-id="48adf-125">Termék/gép tooforward hello szükséges események toohello a syslog démon, (rsyslog vagy a syslog-ng) naplózás hello konfigurálása hello ügynökszámítógépet.</span><span class="sxs-lookup"><span data-stu-id="48adf-125">Configure hello logging product/machine tooforward hello required events toohello syslog daemon (rsyslog or syslog-ng) on hello agent machine.</span></span>
- <span data-ttu-id="48adf-126">Engedélyezze a syslog démon hello ügynök gép tooreceive köszönőüzenetei egy távoli rendszerből.</span><span class="sxs-lookup"><span data-stu-id="48adf-126">Enable hello syslog daemon on hello agent machine tooreceive messages from a remote system.</span></span>

<span data-ttu-id="48adf-127">Hello ügynök gépen hello események hello syslog démon toolocal UDP-port 25226 küldött toobe kell.</span><span class="sxs-lookup"><span data-stu-id="48adf-127">On hello agent machine, hello events need toobe sent from hello syslog daemon toolocal UDP port 25226.</span></span> <span data-ttu-id="48adf-128">Ezt a portot a bejövő események hello ügynök figyel.</span><span class="sxs-lookup"><span data-stu-id="48adf-128">hello agent is listening for incoming events on this port.</span></span> <span data-ttu-id="48adf-129">hello az alábbiakban olvashat egy példa konfiguráció az összes esemény hello helyi rendszer toohello ügynöktől (hello konfigurációs toofit a helyi beállításokat módosíthatja):</span><span class="sxs-lookup"><span data-stu-id="48adf-129">hello following is an example configuration for sending all events from hello local system toohello agent (you can modify hello configuration toofit your local settings):</span></span>

1. <span data-ttu-id="48adf-130">Nyissa meg hello terminálablakot, és lépjen toohello directory */etc/syslog-ng /*</span><span class="sxs-lookup"><span data-stu-id="48adf-130">Open hello terminal window, and go toohello directory */etc/syslog-ng/*</span></span> 
2. <span data-ttu-id="48adf-131">Hozzon létre egy új fájlt *biztonsági-config-omsagent.conf* , és adja hozzá a tartalom a következő hello: OMS_facility = local4</span><span class="sxs-lookup"><span data-stu-id="48adf-131">Create a new file *security-config-omsagent.conf* and add hello following content:  OMS_facility = local4</span></span>
    
    <span data-ttu-id="48adf-132">filter f_local4_oms { facility(local4); };</span><span class="sxs-lookup"><span data-stu-id="48adf-132">filter f_local4_oms { facility(local4); };</span></span>

    <span data-ttu-id="48adf-133">destination security_oms { tcp("127.0.0.1" port(25226)); };</span><span class="sxs-lookup"><span data-stu-id="48adf-133">destination security_oms { tcp("127.0.0.1" port(25226)); };</span></span>

    <span data-ttu-id="48adf-134">log { source(src); filter(f_local4_oms); destination(security_oms); };</span><span class="sxs-lookup"><span data-stu-id="48adf-134">log { source(src); filter(f_local4_oms); destination(security_oms); };</span></span>
    
3. <span data-ttu-id="48adf-135">Töltse le a hello fájl *security_events.conf* , majd helyezze */etc/opt/microsoft/omsagent/conf/omsagent.d/* hello OMS-ügynököt a számítógépben.</span><span class="sxs-lookup"><span data-stu-id="48adf-135">Download hello file *security_events.conf* and place at */etc/opt/microsoft/omsagent/conf/omsagent.d/* in hello OMS Agent computer.</span></span>
4. <span data-ttu-id="48adf-136">Írja be alább toorestart hello a syslog démon hello parancsot: *földgáznál syslog-futtatása:*</span><span class="sxs-lookup"><span data-stu-id="48adf-136">Type hello command below toorestart hello syslog daemon:  *For syslog-ng run:*</span></span>
    
    ```
    sudo service rsyslog restart
    ```

    <span data-ttu-id="48adf-137">*Az rsyslog futtatása esetén:*</span><span class="sxs-lookup"><span data-stu-id="48adf-137">*For rsyslog run:*</span></span>
    
    ```
    /etc/init.d/syslog-ng restart
    ```
5. <span data-ttu-id="48adf-138">Írja be alább toorestart hello OMS-ügynököt hello parancsot:</span><span class="sxs-lookup"><span data-stu-id="48adf-138">Type hello command below toorestart hello OMS Agent:</span></span>

    <span data-ttu-id="48adf-139">*A syslog-ng futtatása esetén:*</span><span class="sxs-lookup"><span data-stu-id="48adf-139">*For syslog-ng run:*</span></span>
    
    ```
    sudo service omsagent restart
    ```

    <span data-ttu-id="48adf-140">*Az rsyslog futtatása esetén:*</span><span class="sxs-lookup"><span data-stu-id="48adf-140">*For rsyslog run:*</span></span>
    
    ```
    systemctl restart omsagent
    ```
6. <span data-ttu-id="48adf-141">Írja be az alábbi hello parancsot, és ellenőrizze, hogy nincsenek-e hibák hello OMS-ügynököt napló hello eredmény tooconfirm:</span><span class="sxs-lookup"><span data-stu-id="48adf-141">Type hello command below and review hello result tooconfirm that there are no errors in hello OMS Agent log:</span></span>

    ``` 
    tail /var/opt/microsoft/omsagent/log/omsagent.log
    ```

## <a name="reviewing-collected-security-events"></a><span data-ttu-id="48adf-142">Az összegyűjtött biztonsági események áttekintése</span><span class="sxs-lookup"><span data-stu-id="48adf-142">Reviewing collected security events</span></span>

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

<span data-ttu-id="48adf-143">Követően hello konfigurációs, hello biztonsági esemény OMS biztonsági okozhatnak toobe indul el.</span><span class="sxs-lookup"><span data-stu-id="48adf-143">After hello configuration is over, hello security event will start toobe ingested by OMS Security.</span></span> <span data-ttu-id="48adf-144">toovisualize ezeket az eseményeket, nyissa meg hello napló keresése hello paranccsal *típus = CommonSecurityLog* a hello keresési mező, és nyomja le az ENTER BILLENTYŰT.</span><span class="sxs-lookup"><span data-stu-id="48adf-144">toovisualize those events, open hello Log Search, type hello command *Type=CommonSecurityLog* in hello search field and press ENTER.</span></span> <span data-ttu-id="48adf-145">hello következő példa bemutatja, ez a parancs eredménye hello figyelje meg, hogy ebben az esetben OMS biztonsági már okozhatnak a biztonsági naplók több szállítóktól származó:</span><span class="sxs-lookup"><span data-stu-id="48adf-145">hello following example shows hello result of this command, notice that in this case OMS Security already ingested security logs from multiple vendors:</span></span>
   
![Az OMS biztonsági és auditálási alapkonfiguráció értékelése](./media/oms-security-connect-products/oms-security-connect-products-fig1.png)

<span data-ttu-id="48adf-147">Ez a keresés egy adott forgalmazótól, például toovisualize online Cisco naplózza, írja be a pontosíthatja: *típus CommonSecurityLog DeviceVendor = = Cisco*.</span><span class="sxs-lookup"><span data-stu-id="48adf-147">You can refine this search for one single vendor, for example, toovisualize online Cisco logs, type: *Type=CommonSecurityLog DeviceVendor=Cisco*.</span></span> <span data-ttu-id="48adf-148">hello "CommonSecurityLog" mezők előre meghatározott tartozó bármely CEF fejléc hello alapvető extensios, minden más bővítmény közben beleértve, hogy "Egyéni kiterjesztés", vagy nem, program beszúrja a "AdditionalExtensions" mező.</span><span class="sxs-lookup"><span data-stu-id="48adf-148">hello “CommonSecurityLog” has predefined fields for any CEF header including hello basic extensios, while any other extension whether it’s “Custom Extension” or not, will be inserted into "AdditionalExtensions" field.</span></span> <span data-ttu-id="48adf-149">Hello egyéni mezők szolgáltatás dedikált tooget mezők származó is használhatja.</span><span class="sxs-lookup"><span data-stu-id="48adf-149">You can use hello Custom Fields feature tooget dedicated fields from it.</span></span> 

### <a name="accessing-computers-missing-baseline-assessment"></a><span data-ttu-id="48adf-150">Az alapkonfiguráció értékeléséből kimaradt számítógépek elérése</span><span class="sxs-lookup"><span data-stu-id="48adf-150">Accessing computers missing baseline assessment</span></span>
<span data-ttu-id="48adf-151">OMS hello tartományi tag alapterv profil Windows Server 2008 R2 rendszerben legfeljebb tooWindows Server 2012 R2 támogat.</span><span class="sxs-lookup"><span data-stu-id="48adf-151">OMS supports hello domain member baseline profile on Windows Server 2008 R2 up tooWindows Server 2012 R2.</span></span> <span data-ttu-id="48adf-152">A Windows Server 2016 alapkonfigurációja még nem végleges, és közzététele után azonnal megtörténik a hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="48adf-152">Windows Server 2016 baseline isn’t final yet and will be added as soon as it is published.</span></span> <span data-ttu-id="48adf-153">Minden más operációs rendszerek OMS biztonsági és naplózási alapterv értékeléssel beolvasott hello alatt szerepelhet **hiányzik az alapkonfiguráció értékelése számítógépek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="48adf-153">All other operating systems scanned via OMS Security and Audit baseline assessment appear under hello **Computers missing baseline assessment** section.</span></span>

## <a name="see-also"></a><span data-ttu-id="48adf-154">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="48adf-154">See also</span></span>
<span data-ttu-id="48adf-155">Ebben a dokumentumban, megtudta, hogyan tooconnect a CEF megoldás tooOMS.</span><span class="sxs-lookup"><span data-stu-id="48adf-155">In this document, you learned how tooconnect your CEF solution tooOMS.</span></span> <span data-ttu-id="48adf-156">További információ az OMS biztonsági toolearn tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="48adf-156">toolearn more about OMS Security, see hello following articles:</span></span>

* [<span data-ttu-id="48adf-157">Az Operations Management Suite (OMS) áttekintése</span><span class="sxs-lookup"><span data-stu-id="48adf-157">Operations Management Suite (OMS) overview</span></span>](operations-management-suite-overview.md)
* [<span data-ttu-id="48adf-158">Figyelés és a válaszoló tooSecurity riasztásait az Operations Management Suite biztonsági és naplózási megoldás</span><span class="sxs-lookup"><span data-stu-id="48adf-158">Monitoring and Responding tooSecurity Alerts in Operations Management Suite Security and Audit Solution</span></span>](oms-security-responding-alerts.md)
* [<span data-ttu-id="48adf-159">Az erőforrások figyelése az Operations Management Suite biztonsági és auditálási megoldásban</span><span class="sxs-lookup"><span data-stu-id="48adf-159">Monitoring Resources in Operations Management Suite Security and Audit Solution</span></span>](oms-security-monitoring-resources.md)

