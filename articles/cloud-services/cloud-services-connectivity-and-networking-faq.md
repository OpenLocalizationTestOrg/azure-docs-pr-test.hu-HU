---
title: "a Microsoft Azure Cloud Services – gyakori kérdések aaaConnectivity és a hálózati problémák |} Microsoft Docs"
description: "Ez a cikk a kapcsolat és hálózati kapcsolatok a Microsoft Azure Felhőszolgáltatások kapcsolatos gyakori kérdések hello sorolja fel."
services: cloud-services
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: e725dbbf585a76807362c59299d0a31f511afd3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connectivity-and-networking-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Az ügyfélkapcsolódási és hálózati problémák Azure-szolgáltatásokhoz: gyakran ismételt kérdések (GYIK)

Ez a cikk tartalmazza a kapcsolat és a hálózati hibákkal kapcsolatos gyakran ismételt kérdések [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Is további részleteket hello [felhőalapú szolgáltatások Virtuálisgép-méretet lap](cloud-services-sizes-specs.md) mérete információt.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>I multi-VIP felhőszolgáltatásban olyan IP-címet nem sikerült lefoglalni
Először is győződjön meg arról, hogy hello virtuálisgép-példányt próbált tooreserve hello IP-Címek be van kapcsolva. Második győződjön meg arról, hogy használata fenntartott IP-címek egyaránt hello átmeneti és üzemi központi telepítések számára. **Ne** hello beállításainak módosítása közben hello telepítési frissítését végzi.

## <a name="how-do-i-remote-desktop-when-i-have-an-nsg"></a>Hogyan készíthetek távoli asztal Ha egy NSG-t?
Adja hozzá a szabályok toohello, amelyek lehetővé teszik a forgalom a portokon NSG **3389-es** és **20000**.  A távoli asztal-portot használja **3389-es**.  Felhőalapú szolgáltatás példányainak kerül, így nem közvetlenül szabályozhatja, hogy melyik példányt tooconnect számára.  Hello *RemoteForwarder* és *RemoteAccess* ügynökök RDP-forgalom kezelésére és hello ügyfél toosend RDP cookie-k engedélyezése, és adjon meg egy egyedi példány tooconnect való.  Hello *RemoteForwarder* és *RemoteAccess* ügynökök szükséges, hogy a port **20000** nyitható meg, amely blokkolhatja, ha egy NSG.

## <a name="can-i-ping-a-cloud-service"></a>Megpingelheti a cloud Service szolgáltatásra?

Nem, nem hello normál "ping" használatával / ICMP protokollt. az ICMP protokoll hello hello Azure terheléselosztó keresztül nem engedélyezett.

tootest való kapcsolat esetén, azt javasoljuk, hogy egy port ping végezzen. Miközben Ping.exe ICMP használja, a más eszközök, például a PSPing Nmap vagy telnet lehetővé teszik tootest kapcsolat tooa adott TCP-portot.

További információkért lásd: [ICMP tootest Azure Virtuálisgép-kapcsolat helyett használja a port ping-üzenetek](https://blogs.msdn.microsoft.com/mast/2014/06/22/use-port-pings-instead-of-icmp-to-test-azure-vm-connectivity/).

## <a name="how-do-i-prevent-receiving-thousands-of-hits-from-unknown-ip-addresses-that-indicate-some-sort-of-malicious-attack-toohello-cloud-service"></a>Hogyan akadályozható meg fogadását, amelyek jelzik, rosszindulatú támadások toohello valamilyen ismeretlen IP-címekről találatok ezer felhőalapú szolgáltatás?
Azure valósítja meg a többrétegű hálózati biztonsági tooprotect a platformszolgáltatások elosztott-szolgáltatásmegtagadásos (DDoS-) támadások ellen. hello Azure DDoS védelmi rendszer Azure folyamatos megfigyelési folyamat, amely folyamatosan növelése a behatolás-tesztelés keresztül részét képezi. Ez DDoS szolgálhat a rendszer úgy van kialakítva toowithstand nem csak támadások hello kívül azonban is más Azure-bérlőt. További részletekért lásd: [Microsoft Azure hálózati biztonság](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf).

Egy indítási tevékenységhez tooselectively blokk néhány bizonyos IP-címeket is létrehozhat. További információkért lásd: [blokkolja egy adott IP-cím](cloud-services-startup-tasks-common.md#block-a-specific-ip-address).

## <a name="when-i-try-toordp-toomy-cloud-service-instance-i-get-hello-message-hello-user-account-has-expired"></a>Amikor megpróbálok tooRDP toomy cloud service-példány, hello üzenet jelenik meg, a "hello felhasználói fiók lejárt."
"Ez a felhasználói fiók lejárt" hello hibaüzenet jelenhet meg, amikor az RDP-beállítások között megadott hello lejárati dátum megkerülése. Hello lejárati dátum hello portálról lehet módosítani a következő lépések végrehajtásával:
1. Jelentkezzen be Azure Management Console (https://manage.windowsazure.com) toohello tooyour felhőszolgáltatás keresse meg és válassza ki a hello **konfigurálása** fülre.
2. Válassza ki **távoli**.
3. Hello "Lejárati dátuma" dátum módosítása, és mentse a hello konfigurációs.

Képes tooRDP tooyour gép most kell lennie.

## <a name="why-is-loadbalancer-not-balancing-traffic-equally"></a>Miért van terheléselosztó nem terheléselosztási forgalom egyaránt?
Hogyan belső load balancer works kapcsolatos információkért lásd: [Azure terheléselosztó új terjesztési mód](https://azure.microsoft.com/blog/azure-load-balancer-new-distribution-mode/).

hello elosztási algoritmust használja az 5-rekordot (forrás IP-címe, a forrásport, cél IP-címe, cél port protokolltípus) toomap forgalom tooavailable kiszolgálók kivonat. Csak egy átviteli munkamenet belül tölcsérútvonalak biztosít. Az azonos TCP vagy UDP-munkamenet hello csomagok azonos datacenter IP (DIP) példány mögött hello elosztott terhelésű végpont toohello irányítja. Hello ügyfél bezárja és újra megnyitja a hello kapcsolat vagy egy új munkamenet indítása a hello azonos forrás IP-címe, hello forrásport változik, és leállítja hello forgalom toogo tooa különböző DIP végpont.
