---
title: "aaaComparing egyéni lemezképek és a formulák a DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Egyéni lemezképek és a formulák hello különbségei ismerje meg, a virtuális gép alapjait, eldöntheti, melyik legjobban megfelel a környezetben."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: a3cb259a-7d80-40ec-8ee8-45105704d589
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: tarcher
ms.openlocfilehash: 3c1d88dfe0ff94b8e825bb7a0b4aca3341c9330d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="comparing-custom-images-and-formulas-in-devtest-labs"></a>Az egyéni lemezképek és a DevTest Labs szolgáltatásban képletek összehasonlítása
Mindkét [egyéni lemezképek](devtest-lab-create-template.md) és [képletek](devtest-lab-manage-formulas.md) alapjait használható [létrehozott új virtuális gépek](devtest-lab-add-vm-with-artifacts.md). Azonban hello a fő különbség a között egyéni lemezképek és a formulák az, hogy egy egyéni lemezképet egyszerűen csak egy virtuális merevlemez alapján, amíg egy képlete virtuális merevlemez alapuló rendszerképet kép *kívül* előre konfigurált beállítások – például a Virtuálisgép-méretet, a virtuális hálózat alhálózat, és az összetevők. Ezek előre konfigurált beállítások vannak beállítva, hogy a virtuális gép létrehozása hello időpontjában felülbírálható az alapértelmezett értékekkel. Ez a cikk ismerteti azokat hello előnyeit (szakemberek számára), és tartózókat (hátrányait) toousing egyéni lemezképek és képletek alapján.

## <a name="custom-image-pros-and-cons"></a>Kép: egyéni előnyei és hátrányai
Egyéni lemezképek adjon meg egy statikus és nem módosítható módja a toocreate virtuális gépek a kívánt környezetből. 

**Informatikai szakemberek**

* Virtuálisgép-létrehozásnál egyéni lemezképéről esetén gyors, mert semmi módosítása után a virtuális gép van hoz létre hello hello lemezképből. Ez azt jelenti amelyeket nem beállítások tooapply hello egyéni lemezkép csak a kép vonatkozó beállítások kihagyásával. 
* Egyetlen egyéni lemezkép alapján létrehozott virtuális gépek esetén azonosak.

**Hátrányait**

* Ha tooupdate kell bizonyos elemeinek hello egyéni lemezképet, hello lemezképet kell újból létre kell hozni.  

## <a name="formula-pros-and-cons"></a>Képletadat előnyei és hátrányai
Képletek számára egy dinamikus módon toocreate virtuális gépek hello szükséges beállításokat.

**Informatikai szakemberek**

* Hello környezet változásait a hello parancsprogramok keresztül összetevők rögzíthetők. Például, ha szeretné, hogy a virtuális gépek hello legújabb bitként a kiadási láncból telepítve, vagy besorolás hello legújabb kód a tárházban lévő, egyszerűen megadhatja az összetevő telepíti a legújabb bits hello vagy hello legújabb kód együtt hello képletben vesz egy cél alapjául szolgáló lemezképhez. Ha ez a képlet használt toocreate virtuális gépeket, hello legújabb bit/kód telepített/bejegyezve toohello virtuális gép. 
* Képletek alapértelmezett beállításokat, az egyéni lemezképek nem adhatók meg – például a Virtuálisgép-méretek és a virtuális hálózati beállításokat adhat meg. 
* a képlet mentett hello beállításokat alapértelmezett értékei láthatók, de hello virtuális gép létrehozásakor módosítható. 

**Hátrányait**

* Virtuális gép létrehozása egy képletet a virtuális gép létrehozása egy egyéni lemezképből több időt is igénybe vehet.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Kapcsolódó blogbejegyzések
* [Egyéni lemezképek vagy képletek?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a>Következő lépések
- [DevTest Labs – gyakori kérdések](devtest-lab-faq.md)