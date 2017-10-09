---
title: "aaaUse Azure DevTest Labs szolgáltatásban a fejlesztők számára |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Azure DevTest Labs fejlesztői forgatókönyvek esetén."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 22e070e5-3d1a-49fe-9d4c-5e07cb0b7fe2
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: tarcher
ms.openlocfilehash: 16a3ef47c9fcbca3050dd50db5b472a9a1e3c62c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-devtest-labs-for-developers"></a>Használja az Azure DevTest Labs szolgáltatásban a fejlesztők számára
Az Azure DevTest Labs szolgáltatásban használt tooimplement sok főbb forgatókönyvek, de hello elsődleges forgatókönyvek közül magában foglalja a DevTest Labs toohost fejlesztési gépek segítségével a fejlesztők számára lehet. Ebben a forgatókönyvben a DevTest Labs alábbi előnyöket nyújtja:

- A fejlesztők gyorsan építhető ki az igény szerinti fejlesztési gépeik.
- A fejlesztők könnyen teste szabhatja a fejlesztési gépeikhez, amikor erre szükség van.
- A rendszergazdák szabályozhatják a költségek biztosításával, hogy:
  - A fejlesztők további virtuális gépek fejlesztési van szükségük, mint nem olvasható be.
  - Virtuális gépek leállnak le, ha nincsenek használatban. 

![DevTest Labs használatát képzés](./media/devtest-lab-developer-lab/devtest-lab-developer-lab.png)

Ebből a cikkből megismerheti használt toomeet fejlesztői követelmények és részletes lépéseket, hogy másolatot labor tooset követheti hello Azure DevTest Labs szolgáltatásokra vonatkozó.

## <a name="implementing-developer-environments-with-azure-devtest-labs"></a>Azure DevTest Labs végrehajtási fejlesztői környezetek
1. **Hello labor létrehozása** 
   
    Labs hello Azure DevTest Labs szolgáltatásban a kiindulási pontjaként. Labor létrehozása után a feladatokat, mint a felhasználók (fejlesztők) toohello labor, a beállítás házirendek toocontrol költségek, gyorsan, hozhat létre virtuális gép lemezképek meghatározása és a több hozzáadását végezheti el.  
   
    További hello hivatkozásokat a következő táblázat hello parancsával:
   
   | Tevékenység | Ismertetett témák |
   | --- | --- |
   | [Labor létrehozása a Azure DevTest Labs szolgáltatásban](devtest-lab-create-lab.md) |Ismerje meg, hogyan toocreate egy Azure DevTest Labs labor hello Azure-portálon. |
2. **Hozzon létre a virtuális gépek beépített piactéren elérhető rendszerkép és az egyéni lemezképek használatával percek alatt** 
   
    Válassza ki az előre elkészített képek széles képek hello Azure piactér a, és hello, amikor elérhetővé teszi azokat. Ha hello előre elkészített lemezképek nem megfelelnek az elvárásainak, hozzon létre egy virtuális gép hello laborban egyéni lemezképként az Azure piactér van szüksége, minden hello szoftver telepítése és a virtuális gép mentése hello előre elkészített lemezkép használatával, amikor egyéni lemezkép is létrehozhat.

    Ha egyéni lemezképek fog használni, fontolja meg egy kép gyári toocreate használatát, és a lemezképek terjesztése. Egy kép gyári egy olyan konfigurációs, kód megoldás, rendszeresen készítésére és a beállított képek automatikusan továbbítja. Ez menti hello szükséges idő toomanually hello rendszer konfigurálása után a virtuális gépek hello jött létre az operációs rendszer alapjául.
  
    További hello hivatkozásokat a következő táblázat hello parancsával:
   
   | Tevékenység | Ismertetett témák |
   | --- | --- |
   | [Azure piactéren elérhető rendszerkép konfigurálása](devtest-lab-configure-marketplace-images.md) |Megtudhatja, hogyan engedélyezett Azure piactéren elérhető rendszerkép zajlik, a kijelölés csak hello lemezképek elérhetővé kívánja hello fejlesztők számára.|
   | [Egyéni lemezkép létrehozása](devtest-lab-create-template.md) |Hozzon létre egy egyéni lemezképet kell, hogy a fejlesztők gyorsan létre tud hozni egy virtuális gép hello egyéni lemezkép használatával hello szoftver előtti telepítésével.|
   | [Kép gyári megismerése](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/) |Bemutató videó ismerteti, hogyan tooset össze, és egy kép gyári használja.|

3. **A fejlesztői gépek újrafelhasználható sablonok létrehozása** 
   
    A Azure DevTest Labs szolgáltatásban képlet alapértelmezett tulajdonság értékek listájának használt toocreate egy virtuális Gépet. Létrehozhat egy képletet hello laborkörnyezetben ki kép, a Virtuálisgép-méretet (kombinációja Processzor és memória szempontjából) és a virtuális hálózati. Mindegyik fejlesztő látható hello képlet hello laborban és toocreate egy virtuális gép használja. 
   
    További hello hivatkozásokat a következő táblázat hello parancsával:
   
   | Tevékenység | Ismertetett témák |
   | --- | --- |
   | [DevTest Labs képletek toocreate virtuális gépek kezelése](devtest-lab-manage-formulas.md) |Ismerje meg, hogyan is létrehozhat egy képletet fel egy lemezképet, a Virtuálisgép-méretet (Processzor és memória szempontjából kombinációja) és a virtuális hálózat.|

4. **Az összetevők tooenable rugalmas VM testreszabási létrehozása**

   Az összetevők használt toodeploy, és állítsa be az alkalmazását, a virtuális gép kiépítése után. Az összetevők lehetnek:

   - Az eszközök, amelyet a Virtuálisgép - hello például ügynökök, a Fiddler és a Visual Studio tooinstall.
   - A Virtuálisgép - hello toorun kívánt például egy tárház klónozása műveleteket.
   - Az alkalmazások, amelyet az tootest.

   Sok az összetevők még elérhető, a-kész. A saját egyéni összetevők hozhat létre, ha azt szeretné, további testreszabási saját igényeinek.

   További hello hivatkozásokat a következő táblázat hello parancsával:
   
   | Tevékenység | Ismertetett témák |
   | --- | --- |
   | [Egyéni összetevők létrehozása a DevTest Labs szolgáltatásban virtuális Géphez](devtest-lab-artifact-author.md) |A labor létrehozása a saját egyéni összetevők hello virtuális gépekhez.|
   | [Adja hozzá a Git tárház toostore egyéni összetevők és az Azure Resource Manager sablonokban használható Azure DevTest Labs szolgáltatásban](devtest-lab-add-artifact-repo.md) |Megtudhatja, hogyan toostore az egyéni összetevők a saját privát Git-tárház.|

5. **Költségek szabályozása**
   
    Az Azure DevTest Labs lehetővé teszi tooset házirend hello labor toospecify hello maximálisan megengedett számú hello, amikor a fejlesztők által létrehozott virtuális gépek. 
   
    Ha a fejlesztői csapat tartozik egy ütemezés működik, és szeretné, hogy toostop hello virtuális gépeinek hello nap adott időpontban, és automatikusan indítsa újra őket követő hello, könnyen elvégezhető, amely beállítása automatikus leállítás be- és az automatikus indítási szabályzatok hello tesztkörnyezetben. 
   
    Végül alkalmazásfejlesztés befejeződése után törölheti hello virtuális gépeinek egyszerre egy PowerShell-parancsfájl futtatásával. 
   
    További hello hivatkozásokat a következő táblázat hello parancsával:
   
   | Tevékenység | Ismertetett témák |
   | --- | --- |
   | [Laborszabályzatok definiálása](devtest-lab-set-lab-policy.md) |Hello laborban házirendek beállításával kapcsolatos költségek szabályozását. |
   | [Törölje az összes hello labor virtuális gépeken, egy PowerShell-parancsfájl használatával](devtest-lab-faq.md#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |Törli az összes hello labs egyetlen művelettel, ha fejlesztési befejeződött.|

1. **A virtuális gép virtuális hálózati tooa hozzáadása** 
   
    DevTest Labs hoz létre egy új virtuális hálózatot (VNET), ha a labor létrehozása. Ha konfigurálta a saját virtuális Hálózatot – például használatával expressroute-on vagy a telephelyek közötti VPN – a virtuális hálózat tooyour labor virtuális hálózati beállításait úgy, hogy az elérhető virtuális gépek létrehozásakor is hozzáadhat.

    Emellett nincs Azure Active Directory tartományi csatlakozási összetevő érhető el, amely virtuális gép tooa tartományhoz fog csatlakozni, hello virtuális gép létrehozásakor. 
   
    További hello hivatkozásokat a következő táblázat hello parancsával:
   
   | Tevékenység | Ismertetett témák |
   | --- | --- |
   | [A Azure DevTest Labs szolgáltatásban virtuális hálózat konfigurálása](devtest-lab-configure-vnet.md) |Ismerje meg, hogyan tooconfigure Azure DevTest Labs segítségével a virtuális hálózat hello Azure-portálon.|

6. **Minden egyes fejlesztői hello labor megosztása**
   
    Labs közvetlenül elérhető a fejlesztők a megosztott kapcsolaton keresztül. Még nincs toohave az Azure-fiók, mindaddig, amíg azok rendelkeznek egy [Microsoft-fiók](devtest-lab-faq.md#what-is-a-microsoft-account). A fejlesztők más fejlesztők által létrehozott virtuális gépek nem látható.  
   
    További hello hivatkozásokat a következő táblázat hello parancsával:
   
   | Tevékenység | Ismertetett témák |
   | --- | --- |
   | [Fejlesztői tooa labor hozzáadása a Azure DevTest Labs szolgáltatásban](devtest-lab-add-devtest-user.md) |Hello Azure portál tooadd fejlesztők tooyour labor használja.|
   | [Adja hozzá a fejlesztők toohello tesztlabor egy PowerShell-parancsfájl használatával](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |A fejlesztők tooyour labor hozzáadása PowerShell tooautomate használja. |
   | [Hivatkozás toohello labor beolvasása](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |Ismerje meg, hogyan fejlesztők közvetlenül hozzáférhetnek a labor hivatkozáson keresztül.|

7. **Labor létrehozása a további csapatokra automatizálása** 
   
    Labor létrehozása, egyéni beállításait, például úgy, hogy a Resource Manager-sablon létrehozása és használata azt újra és újra toocreate azonos labs automatizálható. 
   
    További hello hivatkozásokat a következő táblázat hello parancsával:
   
   | Tevékenység | Ismertetett témák |
   | --- | --- |
   | [A Resource Manager sablonnal labor létrehozása](devtest-lab-faq.md#how-do-i-create-a-lab-from-an-azure-resource-manager-template) |Hozzon létre labs Azure DevTest Labs Resource Manager-sablonok használatával. |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

