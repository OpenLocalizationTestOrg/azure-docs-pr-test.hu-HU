---
title: "aaaTroubleshoot Windows virtuális gép aktiválási problémák az Azure-ban |} Microsoft Docs"
description: "Hello biztosít hibáinak elhárítása Windows virtuális gép aktiválási problémák elhárítása az Azure-ban lépései"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: genlin
manager: willchen
editor: 
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 4ebdac66166e80dcd68cd9e2931b30a29ac01eef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-windows-virtual-machine-activation-problems"></a>Azure Windows virtuális gép aktiválással kapcsolatos problémák elhárítása

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Ha gondja támad, amikor aktiválja a Azure Windows virtuális gép (VM), amely egy egyéni lemezképből jön létre, a dokumentum tootroubleshoot hello probléma hello információk is használhatja. 

## <a name="symptom"></a>Jelenség

Amikor egy Windows Azure VM tooactivate, hibaüzenetet kap üzenet a következő minta hello hasonlít:

**Hiba: a szoftver LicensingService 0xC004F074 hello jelentett hello számítógép aktiválása sikertelen volt. Érhető el. nem kulcs ManagementService (KMS). Tekintse meg az alkalmazások eseménynaplójában hello további információt.**

## <a name="cause"></a>Ok
Azure virtuális gép aktiválási problémák általában fordulhat elő, hello Windows virtuális gép nem konfigurálható megfelelő KMS-ügyfél telepítési kulcsának használatával hello, vagy hello Windows virtuális gép rendelkezik a kapcsolati probléma toohello Azure KMS szolgáltatást (kms.core.windows.net, port 1668). 

## <a name="solution"></a>Megoldás

>[!NOTE]
>Ha a telephelyek közötti VPN használ, és a kényszerített bújtatás, lásd: [használata Azure egyéni útvonalak tooenable KMS-aktiválás kényszerített bújtatással](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx). 
>
>Ha ExpressRoute használ, és rendelkezik az alapértelmezett útvonal közzétett című [Azure virtuális gép tooactivate ExpressRoute előfordulhat, hogy feladatátvételt](http://blogs.msdn.com/b/mast/archive/2015/12/01/azure-vm-may-fail-to-activate-over-expressroute.aspx).

### <a name="step-1-configure-hello-appropriate-kms-client-setup-key-for-windows-server-2016-and-windows-server-2012-r2"></a>1. lépés konfigurálása hello megfelelő KMS-ügyfél telepítő kulcsát (a Windows Server 2016 és a Windows Server 2012 R2)

A virtuális Gépet, amely a Windows Server 2016 vagy a Windows Server 2012 R2 egyéni lemezkép készült hello hello megfelelő KMS-ügyfél telepítő kulcsát a virtuális gép hello kell konfigurálni.

Ez a lépés nem érvényes tooWindows 2012 vagy Windows 2008 R2. Hello Automation virtuális gép aktiválása (AVMA) szolgáltatás, amely csak a Windows Server 2012 R2 és Windows Server 2016 által támogatott használ.

1. Futtatás **slmgr.vbs/dlv** egy rendszergazda jogú parancssorba. Ellenőrizze a hello leírás hello kimeneti értéket, és ellenőrizze, hogy létrehozták, kereskedelmi (KISKERESKEDELMI csatorna) vagy (VOLUME_KMSCLIENT) mennyiségi licencelési adathordozóról:
  
    ```
    cscript c:\windows\system32\slmgr.vbs /dlv
    ```

2. Ha **slmgr.vbs/dlv** jeleníti meg a KISKERESKEDELMI csatorna, futtassa a következő parancsok tooset hello hello [KMS-ügyfél telepítési kulcsának](https://technet.microsoft.com/library/jj612867%28v=ws.11%29.aspx?f=255&MSPPError=-2147217396) Windows Server verziójának hello használ, és kényszeríti a tooretry aktiválási: 

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk <KMS client setup key>

    cscript c:\windows\system32\slmgr.vbs /ato
     ```

    Például Windows Server 2016 Datacenter, akkor futnak a következő parancs hello:

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk CB7KF-BWN84-R7R2Y-793K2-8XDDG
    ```

### <a name="step-2-verify-hello-connectivity-between-hello-vm-and-azure-kms-service"></a>2. lépés ellenőrizze, hogy hello kapcsolódik hello VM és Azure KMS szolgáltatás között

1. Letöltéséhez és kibontásához hello [Psping](http:/technet.microsoft.com/en-us/sysinternals/jj729731.aspx) eszköz tooa helyi mappa a virtuális Gépet, amely nem aktiválható hello. 

2. Nyissa meg tooStart, keresse meg a Windows PowerShell, kattintson a jobb gombbal a Windows PowerShell és válassza a Futtatás rendszergazdaként.

3. Győződjön meg arról, hogy a virtuális gép hello konfigurált toouse hello megfelelő Azure KMS-kiszolgálóhoz. toodo Ez a következő parancsot:
  
    ```
    iex “$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /skms
    kms.core.windows.net:1688
    ```
    Hello parancsot kell visszaadnia: kulcskezelő szolgáltatás számítógépnév tookms.core.windows.net:1688 beállítása sikerült.

4. Győződjön meg arról, hogy rendelkezik-e kapcsolat toohello KMS-kiszolgáló Psping használatával. Váltás toohello mappát, amelyikbe kibontotta hello Pstools.zip letöltése, és futtassa a következő hello:
  
    ```
    \psping.exe kms.core.windows.net:1688
    ```
  
  Utolsó előtti második sorában hello hello kimeneti, győződjön meg arról, hogy látni: küldött = 4, fogadott = 4, elveszett = 0 (0 % veszteség).

  Ha elveszett nagyobb, mint 0 (nulla), hello virtuális gép nincs kapcsolat toohello KMS-kiszolgálón. Ebben a helyzetben hello virtuális gép virtuális hálózatban, és van egy egyéni DNS-kiszolgáló megadva, meg kell győződnie arról, hogy a DNS-kiszolgáló esetén képes tooresolve kms.core.windows.net. Vagy hello DNS kiszolgáló tooone kms.core.windows.net megoldásához módosítsa.

  Figyelje meg, hogy ha eltávolítja az összes DNS-kiszolgáló egy virtuális hálózatot, virtuális gépek Azure belső DNS-szolgáltatás használatára. Ez a szolgáltatás kms.core.windows.net tudja oldani.
  
Azt is ellenőrizze, hogy a hello Vendég tűzfal nincs konfigurálva, hogy megakadályozza a aktiválási kísérletek.

5. Sikeres kapcsolat tookms.core.windows.net ellenőrzése után futtassa a következő parancsot, hogy emelt szintű Windows PowerShell-parancssorba hello. Ez a parancs megpróbál aktiválási több alkalommal.

    ```
    1..12 | % { iex “$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /ato” ; start-sleep 5 }
    ```

A sikeres aktiválási hello alábbihoz információt adhatja vissza:

**Windows(R), ServerDatacenter edition (12345678-1234-1234-1234-12345678) aktiválása... A termék aktiválása sikeres.**

## <a name="faq"></a>GYIK 

### <a name="i-created-hello-windows-server-2016-from-azure-marketplace-do-i-need-tooconfigure-kms-key-for-activating-hello-windows-server-2016"></a>Windows Server 2016 hello létrehozott Azure piactérről. Szükséges tooconfigure KMS-kulcs aktiválásához hello Windows Server 2016? 
 
Nem. hello lemezképként az Azure piactér hello megfelelő KMS ügyfél telepítési kulcsának már konfigurálva van. 

### <a name="does-windows-activation-work-hello-same-way-regardless-if-hello-vm-is-using-azure-hybrid-use-benefit-hub-or-not"></a>Nem a Windows folyamataktivációs munkahelyi hello azonos módon függetlenül attól, hogy ha hello virtuális gép által használt Azure hibrid használata juttatás (HUB), vagy nem? 
 
Igen. 
 
### <a name="what-happens-if-windows-activation-period-expires"></a>Mi történik, ha a Windows aktiválási időszak lejár? 
 
Ha még nincs aktiválva a Windows hello türelmi időszak lejárt, a Windows Server 2008 R2 és a Windows újabb verziói jelennek meg aktiválásával kapcsolatos további értesítések. hello tapéta fekete marad, és a Windows Update biztonsági és csak a kritikus frissítéseket, de nem választható frissítéseket telepíti. Hello értesítést hello hello alján szakasz [licencelési feltételek](http://technet.microsoft.com/en-us/library/ff793403.aspx) lap.   

## <a name="need-help-contact-support"></a>Segítség Forduljon a támogatási szolgálathoz.
Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma megoldódik gyorsan tooget.
