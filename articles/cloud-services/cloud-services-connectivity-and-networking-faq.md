---
title: "Az ügyfélkapcsolódási és hálózati problémák a Microsoft Azure Cloud Services – gyakori kérdések |} Microsoft Docs"
description: "Ez a cikk felsorolja a kapcsolat és hálózati kapcsolatok a Microsoft Azure Felhőszolgáltatások kapcsolatos gyakori kérdésekre."
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
ms.openlocfilehash: 55d5b692930e273af29c3de9d2b96b6d012b3415
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="connectivity-and-networking-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Az ügyfélkapcsolódási és hálózati problémák Azure-szolgáltatásokhoz: gyakran ismételt kérdések (GYIK)

Ez a cikk tartalmazza a kapcsolat és a hálózati hibákkal kapcsolatos gyakran ismételt kérdések [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Is tud kezelni a [felhőalapú szolgáltatások Virtuálisgép-méretet lap](cloud-services-sizes-specs.md) mérete információt.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>I multi-VIP felhőszolgáltatásban olyan IP-címet nem sikerült lefoglalni
Először is győződjön meg arról, hogy a virtuálisgép-példányt, amely az IP-Címek lefoglalása próbál be van-e kapcsolva. Ezután ellenőrizze, hogy az átmeneti és üzemi telepítések foglalt IP-cím használja. **Ne** módosíthatja a beállításokat, amíg a telepítés frissítését végzi.

## <a name="how-do-i-remote-desktop-when-i-have-an-nsg"></a>Hogyan készíthetek távoli asztal Ha egy NSG-t?
Szabályok hozzáadása az NSG-t, amelyek lehetővé teszik a forgalom a portokon **3389-es** és **20000**.  A távoli asztal-portot használja **3389-es**.  Felhőalapú szolgáltatás példányainak kerül, így nem közvetlenül szabályozhatja, hogy melyik példányt való csatlakozáshoz.  A *RemoteForwarder* és *RemoteAccess* ügynökök kezelése RDP-forgalmát, és az ügyfél küldése az RDP cookie, és adjon meg egy egyedi példány való csatlakozáshoz.  A *RemoteForwarder* és *RemoteAccess* ügynökök szükséges, hogy a port **20000** nyitható meg, amely blokkolhatja, ha egy NSG.

## <a name="can-i-ping-a-cloud-service"></a>Megpingelheti a cloud Service szolgáltatásra?

Nem, nem a normál "ping" használatával / ICMP protokollt. Az ICMP protokoll nem engedélyezett az Azure load balancer keresztül.

Kapcsolat tesztelése, azt javasoljuk, hogy egy port ping végezzen. Ping.exe az ICMP, míg más eszközök, például a PSPing Nmap vagy telnet engedélyezi, hogy egy adott TCP-portot kapcsolat teszteléséhez.

További információkért lásd: [port ping-üzenetek használata helyett ICMP Azure Virtuálisgép-kapcsolat tesztelése](https://blogs.msdn.microsoft.com/mast/2014/06/22/use-port-pings-instead-of-icmp-to-test-azure-vm-connectivity/).

## <a name="how-do-i-prevent-receiving-thousands-of-hits-from-unknown-ip-addresses-that-indicate-some-sort-of-malicious-attack-to-the-cloud-service"></a>Hogyan akadályozható meg fogadását, amelyek jelzik a felhőszolgáltatás a rosszindulatú támadás valamilyen ismeretlen IP-címekről találatok ezer?
Azure bevezet egy többrétegű hálózati biztonság, a platform szolgáltatásaiból elosztott-szolgáltatásmegtagadásos (DDoS-) támadások elleni védelméhez. Az Azure DDoS védelmi rendszer Azure folyamatos figyelési folyamat része, amely folyamatosan növelése a behatolás-tesztelés keresztül. A DDoS védelmi rendszer célja, hogy nem csak a kívülről, hanem más Azure bérlő támadások ellen. További részletekért lásd: [Microsoft Azure hálózati biztonság](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf).

Egy indítási feladat szelektív letiltása az egyes IP-címek is létrehozhat. További információkért lásd: [blokkolja egy adott IP-cím](cloud-services-startup-tasks-common.md#block-a-specific-ip-address).

## <a name="when-i-try-to-rdp-to-my-cloud-service-instance-i-get-the-message-the-user-account-has-expired"></a>A cloud service-példány a távoli asztali meg, az üzenet jelenik meg, "a felhasználói fiók lejárt."
"Ez a felhasználói fiók lejárt" hibaüzenet jelenhet meg, ha a lejárati dátum az RDP-beállítások között megadott megkerülése. Módosíthatja a lejárati dátumot a portálról az alábbiak szerint:
1. Jelentkezzen be az Azure felügyeleti konzol (https://manage.windowsazure.com), keresse meg a felhőalapú szolgáltatás, és válassza ki a **konfigurálása** fülre.
2. Válassza ki **távoli**.
3. Módosítsa a "jár le" dátuma, és mentse a konfigurációt.

Most kell tudni az RDP a számítógépen.

## <a name="why-is-loadbalancer-not-balancing-traffic-equally"></a>Miért van terheléselosztó nem terheléselosztási forgalom egyaránt?
Hogyan belső load balancer works kapcsolatos információkért lásd: [Azure terheléselosztó új terjesztési mód](https://azure.microsoft.com/blog/azure-load-balancer-new-distribution-mode/).

A használt algoritmus egy 5 rekordos (forrás IP-címe, a forrásport, cél IP-címe, cél port protokolltípus) kivonatoló forgalom hozzárendelése elérhető kiszolgálón. Csak egy átviteli munkamenet belül tölcsérútvonalak biztosít. A datacenter IP-címet (DIP) példányt az elosztott terhelésű végpont mögött ugyanabban a TCP vagy UDP-munkamenetben csomagokat a rendszer kéri. Amikor az ügyfél bezárja és újra megnyitja a kapcsolatot, vagy új munkamenet indítása a azonos forrás IP-címről, a forrásport módosításai, és egy másik DIP végpont gomba a forgalmat.
