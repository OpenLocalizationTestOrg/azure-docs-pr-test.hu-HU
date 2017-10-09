---
title: "Azure DevTest Labs aaaUse képzési |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Azure DevTest Labs képzési forgatókönyvek esetén."
services: devtest-lab,virtual-machines
documentationcenter: na
author: steved0x
manager: douge
editor: 
ms.assetid: 57ff4e30-7e33-453f-9867-e19b3fdb9fe2
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2016
ms.author: sdanie
ms.openlocfilehash: 4a5f44a282d8f6a58849c730ff89237ccff39ca8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-devtest-labs-for-training"></a>Azure DevTest Labs használatát képzés
Az Azure DevTest Labs lehet használt tooimplement hozzáadása toodev/teszt sok főbb forgatókönyvek. Azokra egyik tooset képzési labor fel. Az Azure DevTest Labs toocreate egy tesztkörnyezetet, ahol megadhatja az egyéni sablonok lehetővé teszi, hogy minden tanuló képzési toocreate azonos és elkülönített környezetben használhatja. Házirendek tooensure, hogy a képzés környezetekben csak akkor, ha szüksége van rájuk, és tartalmaznak elég erőforrások – például a virtuális gépek - hello képzési szükséges elérhető tooeach tanuló is alkalmazhatja. Végül könnyedén megoszthatja hello labor képzésben, amelyek az egy kattintással eléréséhez.

![DevTest Labs használatát képzés](./media/devtest-lab-training-lab/devtest-lab-training.png)

Az Azure DevTest Labs megfelel a követelményeknek, amelyek minden virtuális környezetben szükséges tooconduct képzési hello: 

* Képzésben más képzésben által létrehozott virtuális gépek nem látható.
* Minden képzési gép azonosnak kell lennie.
* Képzésben gyorsan építhető ki a képzés környezetek
* Biztosítsa, hogy a képzésben nem olvasható be a további virtuális gépek van szükségük a hello képzési, valamint a virtuális gépek leállítása, ha azok nem használja őket, mint a költségek szabályozásához
* Egyszerűen megoszthatja hello képzési tesztlabor a minden tanuló
* Hello képzési labor újra és újra felhasználhatja

Ebből a cikkből megismerheti használható Azure DevTest Labs szolgáltatásokra vonatkozó toomeet hello korábban leírt képzési követelmények és mentése egy tesztkörnyezetet, hogy a képzés tooset követve részletes lépéseket.  

## <a name="implementing-training-with-azure-devtest-labs"></a>Az Azure DevTest Labs végrehajtási képzési
1. **Hello labor létrehozása** 
   
    Labs hello Azure DevTest Labs szolgáltatásban a kiindulási pontjaként. Labor létrehozása után végrehajthatja a tevékenységek, például hozzáadja a felhasználókat (képzésben) toohello labor set házirendek toocontrol költségek, adja meg, amely gyorsan hozhat létre Virtuálisgép-lemezképek.   
   
    További hello hivatkozásokat a következő táblázat hello parancsával:
   
   | Tevékenység | Ismertetett témák |
   | --- | --- |
   | [Labor létrehozása a Azure DevTest Labs szolgáltatásban](devtest-lab-create-lab.md) |Ismerje meg, hogyan toocreate egy Azure DevTest Labs labor hello Azure-portálon. |
2. **Előre elkészített piactéren elérhető rendszerkép és az egyéni lemezképek használatával percek alatt képzési virtuális gépek létrehozása** 
   
    Válassza ki a beépített képek széles képek a hello Azure piactér, és elérhetővé teszi azokat a hello képzésben hello tesztkörnyezetben. Hello előre elkészített lemezképek nem felelnek meg a követelményeknek, ha egyéni kép is létrehozhat egy tesztkörnyezetet az Azure piactérről, hello hello képzési és mentheti a virtuális gép hello hello laborban egyéni lemezképként kell minden szoftver telepítése egy előre elkészített lemezképet használó virtuális gépek létrehozásával. 
   
    További hello hivatkozásokat a következő táblázat hello parancsával:
   
   | Tevékenység | Ismertetett témák |
   | --- | --- |
   | [Azure piactéren elérhető rendszerkép konfigurálása](devtest-lab-configure-marketplace-images.md) |Megtudhatja, hogyan zajlik engedélyezett Azure piactéren elérhető rendszerkép; érdemes hello képzési elérhetővé teszi a kijelölés csak hello lemezképek. |
   | [Egyéni lemezkép létrehozása](devtest-lab-create-template.md) |Hozzon létre egy egyéni lemezképet ahhoz, hogy képzésben gyorsan létre tud hozni egy virtuális gép hello egyéni lemezképet használja az hello képzési szükség hello szoftver előtti telepítésével. |
3. **A képzési gépek újrafelhasználható sablonok létrehozása** 
   
    A Azure DevTest Labs szolgáltatásban képlet alapértelmezett tulajdonság értékek listájának használt toocreate egy virtuális Gépet. Létrehozhat egy képletet hello laborkörnyezetben ki kép, a Virtuálisgép-méretet (kombinációja Processzor és memória szempontjából) és a virtuális hálózati. Minden egyes tanuló hello képlet hello laborban látható és toocreate egy virtuális gép használja. 
   
    További hello hivatkozásokat a következő táblázat hello parancsával:
   
   | Tevékenység | Ismertetett témák |
   | --- | --- |
   | [DevTest Labs képletek toocreate virtuális gépek kezelése](devtest-lab-manage-formulas.md) |Ismerje meg, hogyan is létrehozhat egy képletet fel egy lemezképet, a Virtuálisgép-méretet (Processzor és memória szempontjából kombinációja) és a virtuális hálózat. |
4. **Költségek szabályozása**
   
    Azure DevTest Labs lehetővé teszi tooset házirend hello labor toospecify hello maximálisan megengedett számú hello laborban vizsgában hozhat létre virtuális gépeket. 
   
    Ha vannak folytatott többnapos képzés és szeretné, hogy toostop hello virtuális gépeinek hello nap adott időpontban, és automatikusan indítsa újra őket követő hello, egyszerűen elérheti, hogy beállítása automatikus leállítás be- és automatikusan elinduló házirendek hello tesztkörnyezetben. 
   
    Végezetül betanítás törölheti hello virtuális gépeinek egyszerre egy PowerShell-parancsfájl futtatásával. 
   
    További hello hivatkozásokat a következő táblázat hello parancsával:
   
   | Tevékenység | Ismertetett témák |
   | --- | --- |
   | [Laborszabályzatok definiálása](devtest-lab-set-lab-policy.md) |Hello laborban házirendek beállításával kapcsolatos költségek szabályozását. |
   | [Törölje az összes hello labor virtuális gépeken, egy PowerShell-parancsfájl használatával](devtest-lab-faq.md#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |Törli az összes hello labs egyetlen művelettel, ha hello képzési befejeződött. |
5. **Minden egyes tanuló hello labor megosztása**
   
    Labs közvetlenül elérhetők a képzésben közösen-kapcsolaton keresztül. A képzésben nem is kell toohave az Azure-fiók, mindaddig, amíg azok rendelkeznek egy [Microsoft-fiók](devtest-lab-faq.md#what-is-a-microsoft-account). Képzésben más képzésben által létrehozott virtuális gépek nem látható.  
   
    További hello hivatkozásokat a következő táblázat hello parancsával:
   
   | Tevékenység | Ismertetett témák |
   | --- | --- |
   | [Az Azure DevTest Labs tanuló tooa labor hozzáadása](devtest-lab-add-devtest-user.md) |Hello Azure portál tooadd képzésben tooyour képzési labor használja. |
   | [PowerShell parancsfájl használatával képzésben toohello labor hozzáadása](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |Használjon PowerShell tooautomate képzésben tooyour képzési labor hozzáadása. |
   | [Hivatkozás toohello labor beolvasása](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |Ismerje meg, hogyan labor közvetlenül elérhető hivatkozás segítségével. |
6. **Hello labor újra és újra felhasználhatja** 
   
    Labor létrehozása, egyéni beállításait, például úgy, hogy a Resource Manager-sablon létrehozása és használata azt újra és újra toocreate azonos labs automatizálható. 
   
    További hello hivatkozásokat a következő táblázat hello parancsával:
   
   | Tevékenység | Ismertetett témák |
   | --- | --- |
   | [A Resource Manager sablonnal labor létrehozása](devtest-lab-faq.md#how-do-i-create-a-lab-from-an-azure-resource-manager-template) |Hozzon létre labs Azure DevTest Labs Resource Manager-sablonok használatával. |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

