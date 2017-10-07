---
title: "RemoteApp hibrid gyűjtemények létrehozásáról a aaaTroubleshoot |} Microsoft Docs"
description: "Megtudhatja, hogyan tootroubleshoot RemoteApp hibrid gyűjtemény létrehozása sikertelen"
services: remoteapp
documentationcenter: 
author: vkbucha
manager: mbaldwin
ms.assetid: b32033ee-8d52-4e74-bb78-86ca873c34e2
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: cc426f24bd0c349a8862d54acbafa9cf84446f4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-creating-azure-remoteapp-hybrid-collections"></a>Hibaelhárítás Azure RemoteApp hibrid gyűjtemények létrehozásával
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Hibrid gyűjtemény üzemel, és tárolja az hello Azure felhőben, de is megadható, hogy felhasználók hozzáférést adatokhoz és erőforrásokhoz, a helyi hálózaton tárolt adatokat. A felhasználók az Azure Active Directoryval szinkronizált vagy összevont vállalati hitelesítő adataikkal jelentkezhetnek be, és érhetik el az alkalmazásokat. Telepíthet egy meglévő Azure virtuális hálózat használó hibrid gyűjteményt, vagy létrehozhat egy új virtuális hálózat. Azt javasoljuk, hogy hozzon létre, vagy használható egy virtuális hálózati alhálózat CIDR-tartomány elég nagy a várt jövőbeni növekedés Azure RemoteApp.

A gyűjtemény még nem hozott létre? Lásd: [hibrid gyűjtemény létrehozása](remoteapp-create-hybrid-deployment.md) hello lépéseket.

Ha problémája van a gyűjtemény létrehozása, vagy ha hello gyűjtemény nem működik hello módon úgy gondolja, hogy kell-e, tekintse meg a következő információk hello.

## <a name="your-image-is-invalid"></a>A kép érvénytelen.
Ha megjelenik egy üzenet, például "GoldImageInvalid", ha vár a Azure tooprovision a gyűjtemény, az azt jelenti, hogy a sablon lemezképe nem felel meg hello [kép követelmények definiált](remoteapp-imagereqs.md). Igen, nyissa meg olvasni azokat [követelmények](remoteapp-imagereqs.md), javítsa ki a lemezkép és toocreate próbálkozzon újra a gyűjteményben.

## <a name="does-your-vnet-have-network-security-groups-defined"></a>Rendelkezik a virtuális hálózat hálózati biztonsági csoportok definiált?
Hálózati biztonsági csoportokat használ a gyűjtemény hello alhálózaton definiált, győződjön meg arról, hogy ezek [URL-címek és portok](remoteapp-ports.md) érhető el az alhálózaton belül.

További hálózati biztonsági csoportok toohello telepített virtuális gépek által hello ad-alhálózatot adhat hozzá.

## <a name="are-you-using-your-own-dns-servers-and-are-they-accessible-from-your-vnet-subnet"></a>A saját DNS-kiszolgálók használata? És azok elérhető a virtuális hálózat alhálózatból?
> [!NOTE]
> Toomake meg arról, hogy hello a virtuális hálózat DNS-kiszolgálók mindig be, és mindig tudja tooresolve hello virtuális géphez a virtuális hálózat hello rendelkezik. Ez a Google DNS ne használjon.
> 
> 

A hibrid gyűjtemények használhatja a saját DNS-kiszolgálók. Azt adja meg azokat a hálózati konfigurációs séma vagy hello felügyeleti portálon keresztül a virtuális hálózat létrehozásakor. DNS-kiszolgálók hello ahhoz, hogy azok van megadva (megakadályozását tooround multiplexelés) feladatátvételi módon használt.  
Tekintse meg a túl[névfeloldás virtuális gépek és a Szerepkörpéldányok](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) arról, hogy a DNS-kiszolgálók konfigurált correcly toomake.

Győződjön meg arról a gyűjtemény hello DNS-kiszolgáló elérhető és érhetők el az ebben a gyűjteményben megadott hello VNET alhálózati.

Példa:

    <VirtualNetworkConfiguration>
    <Dns>
      <DnsServers>
        <DnsServer name="" IPAddress=""/>
      </DnsServers>
    </Dns>
    </VirtualNetworkConfiguration>

![A DNS-kiszolgáló megadása](./media/remoteapp-hybridtrouble/dnsvpn.png)

## <a name="are-you-using-an-active-directory-domain-controller-in-your-collection"></a>Használja az Active Directory-tartományvezérlő a gyűjtemény?
Jelenleg csak az Active Directory-tartomány egy Azure RemoteApp társítva. hello hibrid gyűjtemény csak Azure Active Directory-fiókok egy Windows Server Active Directory-telepítésből; DirSync eszközzel szinkronizált támogat pontosabban vagy szinkronizálása megtörtént-e a jelszó-szinkronizálási lehetőséggel hello szinkronizálása megtörtént-e az Active Directory összevonási szolgáltatások (AD FS) összevonásának konfigurálásával. Egyéni tartományt, amely megfelel a helyi tartomány hello UPN-tartomány utótag toocreate kell, és a címtár-integráció beállítása.

Lásd: [Active Directory konfigurálása az Azure RemoteApp](remoteapp-ad.md) további információt.

Ellenőrizze, hogy megadott hello tartomány adatok érvényesek, és hello tartományvezérlő hello Azure távoli App használt hello alhálózati létrehozott virtuális gép elérhető. Győződjön meg arról is hello szolgáltatás fiók a megadott hitelesítő adatok megadták az engedélyek tooadd számítógépek toohello tartományhoz, és hogy hello AD neve megadott hello DNS hello virtuális hálózat szerepel a feloldhatók legyenek.

## <a name="what-domain-name-did-you-specify-when-you-created-your-collection"></a>Milyen tartománynév adta meg a gyűjtemény létrehozásakor?
hello tartománynév létrehozott vagy fel lehet egy belső tartománynevet (nem az Azure AD tartományi neve), és feloldható DNS-formátumban (contoso.local) kell lennie. Például van egy Active Directory belső nevét (contoso.local), és az Active Directory egyszerű felhasználónév (contoso.com) - rendelkezik toouse hello belső nevét a gyűjtemény létrehozásakor.

