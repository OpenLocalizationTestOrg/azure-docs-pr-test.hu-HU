---
title: "aaaAzure Advisor magas rendelkezésre állású javaslatok |} Microsoft Docs"
description: "Használja az Azure Advisor tooimprove magas rendelkezésre állását az Azure-környezetekhez."
services: advisor
documentationcenter: NA
author: kumudd
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 3ac75ce401271f0212d198d7a7dc75ab702b6eda
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="advisor-high-availability-recommendations"></a>Magas rendelkezésre állású javaslatokat biztosít

Az Azure Advisor segítségével győződjön meg arról, és az üzleti szempontból kritikus fontosságú alkalmazások hello folytonosságának javításához. Magas rendelkezésre állású javaslatok Advisor letölthető hello **magas rendelkezésre állású** hello Advisor irányítópult lapja.

![Magas rendelkezésre állás gomb hello Advisor irányítópult](./media/advisor-high-availability-recommendations/advisor-high-availability-tab.png)


## <a name="ensure-virtual-machine-fault-tolerance"></a>Virtuális gép a hibatűrés érdekében

Az Advisor azonosítja a virtuális gépek, amelyek nem részei egy rendelkezésre állási csoportot és helyezi át a rendelkezésre állási csoportok javasolja. Tooyour alkalmazás redundanciájának biztosítása érdekében javasoljuk, hogy legalább két virtuális gép rendelkezésre állási csoportba. Ez a konfiguráció biztosítja, hogy vagy a tervezett vagy nem tervezett karbantartási események esetén legalább egy virtuális gép elérhető, és megfelel-e hello Azure virtuális gép SLA-t. Dönthet úgy, vagy toocreate rendelkezésre állási készlet hello virtuális gép vagy tooadd hello virtuális gép tooan meglévő rendelkezésre állási csoportot.

> [!NOTE]
> Ha úgy dönt, toocreate rendelkezésre állási beállítása, hozzá kell adnia legalább egy további virtuális gép bele. Azt javasoljuk, hogy két csoportba, vagy további virtuális gépek rendelkezésre állási beállítása tooensure, amely legalább egy gép érhető kimaradás során.

![Az Advisor javaslat: A virtuális gép redundancia, használja a rendelkezésre állási csoportok](./media/advisor-high-availability-recommendations/advisor-high-availability-create-availability-set.png)

## <a name="ensure-availability-set-fault-tolerance"></a>Ellenőrizze a rendelkezésre állási csoport hibatűrés 

Az Advisor tartalmazhat egyetlen virtuális gép rendelkezésre állási készleteket azonosítja, és azt javasolja, hogy egy vagy több virtuális gépek tooit hozzáadása. Tooyour alkalmazás redundanciájának biztosítása érdekében javasoljuk, hogy legalább két virtuális gép rendelkezésre állási csoportba. Ez a konfiguráció biztosítja, hogy vagy a tervezett vagy nem tervezett karbantartási események esetén legalább egy virtuális gép elérhető, és megfelel-e hello Azure virtuális gép SLA-t. Virtuális gép vagy egy meglévő virtuális gép toouse és tooadd vagy toocreate kiválaszthatja azt toohello rendelkezésre állási csoportot.  

![Az Advisor javaslat: egy vagy több virtuális gépek toothis rendelkezésre állási csoport hozzáadása](./media/advisor-high-availability-recommendations/advisor-high-availability-add-vm-to-availability-set.png)


## <a name="ensure-application-gateway-fault-tolerance"></a>Ellenőrizze az alkalmazás átjáró hibatűrés
tooensure hello üzleti folytonossági kritikus fontosságú alkalmazások alkalmazás-átjárók vannak kapcsolva, az Advisor azonosítja az alkalmazás példányai, amelyek nincsenek beállítva a hibatűrés érdekében, és javaslatot tesz, a szervizelési műveletek is . Az Advisor azonosítja közepes vagy nagy Egypéldányos alkalmazás átjárók, és hozzáadja legalább egy további példányt javasol. Egy vagy több instance kis alkalmazásátjárót azonosítja és a áttelepítése toomedium vagy nagy termékváltozatok javasolja. Az Advisor azt javasolja, hogy ezek a műveletek tooensure, hogy az átjáró alkalmazáspéldányok-e a konfigurált toosatisfy hello aktuális SLA követelményeinek ezeket az erőforrásokat.

![Az Advisor javaslat: két vagy több közepes vagy nagy méretű kérelem átjáró példányok telepítése](./media/advisor-high-availability-recommendations/advisor-high-availability-application-gateway.png)

## <a name="improve-hello-performance-and-reliability-of-virtual-machine-disks"></a>A virtuális gép lemezeinek hello teljesítményét és megbízhatóságát javítása

Az Advisor azonosítja a virtuális gépek a standard lemezek és toopremium lemezek frissítését javasolja.
 
Prémium szintű Storage nagy teljesítményű, alacsony késésű lemez I/O-igényes munkaterhelések futó virtuális gépek támogatása nyújt. Prémium szintű storage-fiókok használó virtuálisgép-lemezek SSD-meghajtót (SSD) adatait tárolják. Hello az alkalmazás legjobb teljesítmény érdekében azt javasoljuk, hogy az áttelepített a magas IOPS toopremium tárolási igénylő virtuális gép lemezei. 

A lemezek nem igényelnek magas iops értéket, ha szabványos Storage fenntartásuk korlátozhatja költségeket. Standard szintű tárolást virtuálisgép-lemez adatokat tárolja a (merevlemezes HDD) meghajtók SSD-k helyett. A virtuális gép lemezek toopremium lemezek választható toomigrate. A legtöbb virtuális gép termékváltozatok Premium lemezek támogatottak. Azonban bizonyos esetekben, ha azt szeretné, hogy toouse premium lemezek esetén szükség lehet tooupgrade a virtuális gép termékváltozatok is.

![Az Advisor javaslat: a virtuális gép lemezek hello megbízhatóság javításához a toopremium lemezek frissítésével](./media/advisor-high-availability-recommendations/advisor-high-availability-upgrade-to-premium-disks.png)

## <a name="protect-your-virtual-machine-data-from-accidental-deletion"></a>A virtuális gép adatainak véletlen törlés elleni védelme
Az Advisor azonosítja a virtuális gépek, ahol a biztonsági mentés nem engedélyezett, és azt a biztonsági mentés engedélyezését javasolja. Virtuális gép biztonsági mentése beállítása hello az üzleti szempontból kritikus fontosságú adatok rendelkezésre állását biztosítja, és véletlen törlés vagy -sérülés elleni védelmet nyújt.

![Az Advisor javaslat: virtuális gép biztonsági mentési tooprotect a kritikus fontosságú adatok konfigurálása](./media/advisor-high-availability-recommendations/advisor-high-availability-virtual-machine-backup.png)

## <a name="access-high-availability-recommendations-in-advisor"></a>Hozzáférés magas rendelkezésre állású javaslatok Advisor

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).

2. Hello bal oldali ablaktáblában kattintson **további szolgáltatások**.

3. A hello szolgáltatás menü ablaktáblán, a **figyelés és felügyelet**, kattintson a **Azure Advisor**.  
 hello Advisor irányítópult jelenik meg.

4. Az hello Advisor irányítópultján kattintson a hello **magas rendelkezésre állású** lapra, majd válassza ki a kívánt tooreceive javaslatok hello előfizetés.

> [!NOTE]
> Advisor-javaslatokra tooaccess, először *az előfizetés regisztrálása* az Advisor szolgáltatásban. Egy előfizetés regisztrálva amikor egy *előfizetés tulajdonosának* elindítja az Advisor-irányítópult és az kattintással hello hello **javaslatok beszerzése** gombra. Ez egy *egyszeri művelet*. Hello előfizetés regisztrálása után érheti el, az Advisor-javaslatokra *tulajdonos*, *közreműködő*, vagy *olvasó* -előfizetéssel, egy erőforráscsoport vagy egy adott erőforrás.

## <a name="next-steps"></a>Következő lépések

Advisor-javaslatokra kapcsolatos további információkért lásd:
* [Bevezetés tooAzure Advisor](advisor-overview.md)
* [Bevezetés az Advisor használatába](advisor-get-started.md)
* [Költség javaslatokat biztosít](advisor-performance-recommendations.md)
* [Teljesítmény javaslatokat biztosít](advisor-performance-recommendations.md)
* [Biztonsági javaslatokat biztosít](advisor-security-recommendations.md)

