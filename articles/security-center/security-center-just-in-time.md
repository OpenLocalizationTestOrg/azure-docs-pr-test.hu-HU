---
title: "idő a virtuális gépen aaaJust elérni az Azure Security Centerben |} Microsoft Docs"
description: "A dokumentumból megtudhatja, hogyan igény szerint VM hozzáférés az Azure Security Center segítségével szabályozhatja a hozzáférést tooyour Azure virtuális gépek."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: terrylan
ms.openlocfilehash: e6b58dd2c686cb227392b294e85914df5a546016
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-virtual-machine-access-using-just-in-time"></a>JIT-virtuális gép hozzáférés kezelése

Csak az idő a virtuális gép (VM) hozzáférést lehet használt toolock bejövő forgalom tooyour Azure virtuális gépeken, csökkenti a kitettség tooattacks egyszerű a hozzáférés tooconnect tooVMs szükség esetén adja meg.

> [!NOTE]
> hello csak az idő a szolgáltatás jelenleg előzetes és hello a Security Center Standard csomagban érhető el.  Lásd: [árazás](security-center-pricing.md) toolearn bővebben a Security Center által tarifacsomag szükséges.
>
>

## <a name="attack-scenario"></a>Támadás

Találgatásos támadások gyakran felügyeleti célportjainak, azt jelenti, hogy toogain hozzáférés tooa virtuális gép. Ha sikeres, a támadó ellenőrzése alatt tartja a virtuális gép hello igénybe, és egy foothold létesíteni a környezetbe.

Egyirányú tooreduce kitettség tooa találgatásos támadás toolimit hello, hogy mennyi ideig legyen egy nyitott port. Szolgáltatásfelügyelet portjai nem szükséges toobe nyílt mindig tegye. Szükségük toobe csak akkor nyitva vannak csatlakoztatott toohello VM, például tooperform felügyeleti vagy karbantartási feladatok. Amikor a JIT engedélyezve van, a Security Center az [hálózati biztonsági csoport](../virtual-network/virtual-networks-nsg.md) (NSG) szabályok, amelyek korlátozzák a hozzáférési toomanagement portok, így nem tudja megcélozni a támadók.

![Csak az idő forgatókönyv][1]

## <a name="how-does-just-in-time-access-work"></a>Hogyan csak az idő access működik?

Ha JIT engedélyezve van, a Security Center zárolja bejövő forgalom tooyour Azure virtuális gépek egy NSG-szabály létrehozásával. Kiválaszthatja hello hello VM toowhich a bejövő forgalom lesz zárolva. Csak az idő-megoldások hello ezeket a portokat szabályozzák.

Ha a felhasználó hozzáférési tooa VM kér, a Security Center ellenőrzi hello felhasználó [szerepköralapú hozzáférés-vezérlést (RBAC)](../active-directory/role-based-access-control-configure.md) írási hozzáférést biztosítson az Azure-erőforrás hello engedélyeket. Ha írási engedélyekkel rendelkeznek, a hello kérelem jóváhagyása és a Security Center automatikusan konfigurálja a hálózati biztonsági csoportokkal (NSG-k) tooallow hello bejövő forgalom toohello kezelőportjai megadott hello időt. Hello időszak lejárta után a Security Center visszaállítja hello NSG-k tootheir korábbi állapotába.

> [!NOTE]
> Biztonsági központ csak a virtuális gép elérhető jelenleg csak virtuális gépek Azure Resource Manager használatával telepített. hello klasszikus és Resource Manager üzembe helyezési modellel kapcsolatos információkért tekintse meg toolearn [Azure Resource Manager és klasszikus üzembe helyezési](../azure-resource-manager/resource-manager-deployment-model.md).
>
>

## <a name="using-just-in-time-access"></a>Csak az idő access használatával

Hello **közvetlenül a virtuális gép elérhető** hello csempét **Security Center** panel virtuális gépek csak az idő hozzáférési és hello engedélyezett hozzáférési kérelmek száma a hello múlt héten konfigurálva hello számát jeleníti meg.

Jelölje be hello **közvetlenül a virtuális gép elérhető** csempe és hello **közvetlenül a virtuális gép elérhető** panel nyílik meg.

![Virtuális gép hozzáférés JIT csempéje][2]

Hello **közvetlenül a virtuális gép elérhető** panel információt nyújt a virtuális gépek hello állapotát:

- **Konfigurált** -virtuális gépek, amelyek csak a virtuális gép elérhető konfigurált toosupport lettek. hello adatainak hello múlt héten és az egyes virtuális gép hello száma jóváhagyott kéréseket, legutóbbi hozzáférés dátuma és időpontja és utolsó felhasználó tartalmazza.
- **Ajánlott** -csak a virtuális gép elérhető támogatható, de nincs beállítva a virtuális gépeket. Javasoljuk, hogy engedélyezze a csak az idő VM hozzáférés-vezérlés a virtuális gépeken. Lásd: [engedélyezése csak a virtuális gép elérhető](#enable-just-in-time-vm-access).
- **Nincs javaslat** -egy virtuális gép nem ajánlott toobe okozó okai:
  - Hiányzik az NSG - hello csak az idő-megoldások szükséges egy NSG toobe helyen.
  - Klasszikus VM - VM elérhető csak a Security Center jelenleg csak virtuális gépek Azure Resource Manager használatával telepített. A klasszikus üzembe helyezési hello csak az idő-megoldás által nem támogatott.
  - Egyéb - egy virtuális gép esetén ebbe a kategóriába tartozó hello biztonsági házirendje hello előfizetéshez vagy erőforráscsoporthoz hello ki van kapcsolva a megoldás JIT hello, vagy adott VM hello hiányzik egy nyilvános IP-cím, és nem rendelkezik egy NSG.

## <a name="configuring-a-just-in-time-access-policy"></a>Beállítása csak hozzáférési házirendben idő

tooselect hello tooenable kívánt virtuális gépek:

1. A hello **közvetlenül a virtuális gép elérhető** panelen, jelölje be hello **ajánlott** fülre.

  ![Csak az idő hozzáférés engedélyezése][3]

2. A **virtuális gépek**, válassza ki a megjeleníteni kívánt tooenable hello virtuális gépeket. A pipa következő tooa VM ez helyezi.
3. Válassza ki **engedélyezése virtuális gépeken JIT**.
4. Kattintson a **Mentés** gombra.

### <a name="default-ports"></a>Alapértelmezett port

A Security Center JIT engedélyezését javasolja hello alapértelmezett portokat tekintheti meg.

1. A hello **közvetlenül a virtuális gép elérhető** panelen, jelölje be hello **ajánlott** fülre.

  ![Megjeleníti az alapértelmezett portok][6]

2. A **virtuális gépek**, jelöljön ki egy virtuális Gépet. Ez egy pipa következő toohello virtuális Gépet, és megnyílik hello helyezi **JIT VM konfiguráció** panelen. Ezen a panelen hello alapértelmezett portok jeleníti meg.

### <a name="add-ports"></a>Portok hozzáadása

A hello **JIT VM konfiguráció** panelen is hozzáadhat és kívánja tooenable hello csak az idő-megoldások új port konfigurálása.

1. A hello **JIT VM konfiguráció** panelen válassza **Hozzáadás**. Ekkor megnyílik a hello **Hozzáadás portkonfigurációjának** panelen.

  ![Port konfigurálása][7]

2. A **Hozzáadás portkonfigurációjának** panelen azonosíthatja a kérelem ideje forrás IP-címek és maximális megengedett hello port protokolltípus,.

  Forrás IP-címek engedélyezett vannak hello IP-címtartományok engedélyezett tooget elérést egy jóváhagyott kérésre.

  Kérések maximális ideje hello maximális időszak, hogy egy adott portot lehet megnyitni.

3. Kattintson az **OK** gombra.

## <a name="requesting-access-tooa-vm"></a>A kért hozzáférés tooa méretű VM

toorequest hozzáférés tooa VM:

1. A hello **közvetlenül a virtuális gép elérhető** panelen, jelölje be hello **beállított** fülre.
2. A **virtuális gépek**, válassza ki a megjeleníteni kívánt tooenable hozzáférés hello virtuális gépeket. A pipa következő tooa VM ez helyezi.
3. Válassza ki **hozzáférés kérése**. Ekkor megnyílik a hello **hozzáférés kérése** panelen.

  ![Kérelem hozzáférés tooa méretű VM][4]

4. A hello **hozzáférés kérése** panelen konfigurálhatja az egyes VM hello portok tooopen együtt hello forrás IP-címe, amely hello port megnyitott hello időkerete tooand mely hello a port meg van nyitva. Hello csak a idő házirendben beállított hozzáférés csak toohello portok is igényelhet. Minden porthoz tartozik egy maximális engedélyezett idő, csak az idő házirend hello származik.
5. Válassza ki **portok megnyitása**.

## <a name="editing-a-just-in-time-access-policy"></a>Szerkesztés csak hozzáférési házirendben idő

Módosíthatja a virtuális gép létezik csak az idő házirend hozzáadásával és konfigurálásával egy új port tooopen ezt a virtuális gépet, vagy bármely más paraméter módosításával kapcsolódó tooan már védett port.

A rendezés tooedit egy meglévő csak az idő házirendet a virtuális gépek hello **beállított** lapon:

1. A **virtuális gépek**, válassza ki a virtuális gép tooadd egy port tooby, kattintson a három hello pontokból hello soron belüli ezt a virtuális gépet. Ekkor megnyílik egy menüben.
2. Válassza ki **szerkesztése** hello menüben. Ekkor megnyílik a hello **JIT VM konfiguráció** panelen.

  ![Házirend szerkesztése][8]

3. A hello **JIT VM konfiguráció** panelen módosíthatja hello meglévő beállítások port már védett gombra kattintva a porton, vagy választhat **Hozzáadás**. Ekkor megnyílik a hello **Hozzáadás portkonfigurációjának** panelen.

  ![Adjon hozzá egy portot][7]

4. A **Hozzáadás portkonfigurációjának** panel hello port, a protokoll típusát, a forrás IP-címek és a kérelem maximális idő azonosításához.
5. Kattintson az **OK** gombra.
6. Kattintson a **Mentés** gombra.

## <a name="auditing-just-in-time-access-activity"></a>Csak az idő-hozzáférési tevékenységet naplózás

Akkor is kaphat a naplófájl-keresési VM tevékenységeket. tooview naplói:

1. A hello **közvetlenül a virtuális gép elérhető** panelen, jelölje be hello **beállított** fülre.
2. A **virtuális gépek**, ezt a virtuális gépet a hello soron belüli hello három pont kattintva válassza ki a Virtuálisgép-tooview adatok kapcsolatos. Ekkor megnyílik egy menüben.
3. Válassza ki **tevékenységnapló** hello menüben. Ekkor megnyílik a hello **tevékenységnapló** panelen.

![Válassza ki a tevékenység naplója][9]

Hello **tevékenységnapló** panel ezt a virtuális gépet és idő, dátum és előfizetés korábbi műveletek szűrt nézetét jeleníti meg.

![Tevékenység napló megtekintése][5]

Hello naplóadatok kiválasztásával letöltheti **ide toodownload minden hello elem CSV-ként**.

Hello szűrők módosítása, és válassza ki **alkalmaz** toocreate a Keresés és a napló.

## <a name="using-just-in-time-vm-access-via-powershell"></a>Igény szerint VM hozzáférés PowerShell használatával

Rendelés toouse hello csak a PowerShell idő megoldás, győződjön meg arról, hogy hello [legújabb](/powershell/azure/install-azurerm-ps) Azure PowerShell-verzió.
Ha így tesz, meg kell-e tooinstall hello [legújabb](https://www.powershellgallery.com/packages/Azure-Security-Center/0.0.12) az Azure Security Center hello PowerShell gyűjteményből.

### <a name="configuring-a-just-in-time-policy-for-a-vm"></a>Konfigurálása csak a virtuális gépek ideje házirend

tooconfigure csak egy adott VM-idő szabályzatok kell toorun ezt a parancsot a PowerShell-munkamenetben: Set-ASCJITAccessPolicy.
Hajtsa végre a hello parancsmag dokumentáció toolearn további.

### <a name="requesting-access-tooa-vm"></a>A kért hozzáférés tooa méretű VM

csak az idő-megoldások hello tooaccess egy adott virtuális gép által védett, akkor kell toorun ezt a parancsot a PowerShell-munkamenetben: meghívása ASCJITAccess.
Hajtsa végre a hello parancsmag dokumentáció toolearn további.

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta, hogyan igény szerint VM hozzáférés a Security Center segítségével szabályozhatja a hozzáférést tooyour Azure virtuális gépek.

További információ a Security Center toolearn hello következő lásd:

- [Biztonsági szabályzatok beállítása](security-center-policies.md) – további hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.
- [Biztonsági javaslatok kezelése](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.
- [Biztonsági állapotfigyelés](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.
- [Kezelése és reagálás toosecurity riasztások](security-center-managing-and-responding-alerts.md) – további hogyan toomanage és válaszoljon toosecurity riasztásokat.
- [Partnermegoldások figyelése](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.
- [Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
- [Azure Security blog](https://blogs.msdn.microsoft.com/azuresecurity/) – Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.


<!--Image references-->
[1]: ./media/security-center-just-in-time/just-in-time-scenario.png
[2]: ./media/security-center-just-in-time/just-in-time.png
[3]: ./media/security-center-just-in-time/enable-just-in-time-access.png
[4]: ./media/security-center-just-in-time/request-access-to-a-vm.png
[5]: ./media/security-center-just-in-time/activity-log.png
[6]: ./media/security-center-just-in-time/default-ports.png
[7]: ./media/security-center-just-in-time/add-a-port.png
[8]: ./media/security-center-just-in-time/edit-policy.png
[9]: ./media/security-center-just-in-time/select-activity-log.png
