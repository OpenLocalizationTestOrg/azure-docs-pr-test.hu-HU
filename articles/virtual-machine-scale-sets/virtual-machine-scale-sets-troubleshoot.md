---
title: "Virtuálisgép-méretezési csoportok aaaTroubleshoot automatikusan skálázva |} Microsoft Docs"
description: "Hárítsa el a virtuálisgép-méretezési csoportok automatikusan skálázva. Észlelt gyakran előforduló problémák megértése, hogyan tooresolve őket."
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7d87b72-ee24-4e52-9377-a42f337f76fa
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: windows
ms.devlang: na
ms.topic: article
ms.date: 10/28/2016
ms.author: guybo
ms.openlocfilehash: 4c9a70992348d87fb43646421a90a027bf400a17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-autoscale-with-virtual-machine-scale-sets"></a>Virtuálisgép-méretezési csoportok automatikusan skálázva hibaelhárítása
**A probléma** – létrehozta az automatikus skálázás infrastruktúra az Azure Resource Manager használatával Virtuálisgép-méretezési készlet – például egy sablont ehhez hasonló telepítésével: https://github.com/Azure/azure-quickstart-templates/tree/master/201- a skálázási szabályok definiált vmss-bottle – automatikus skálázás – rendelkezik, és azt nagyszerű, azzal a különbséggel, hogy mekkora terhelés, akkor helyezhető el virtuális gépek hello, függetlenül azok automatikus skálázás nem fog.

## <a name="troubleshooting-steps"></a>Hibaelhárítási lépések
Néhány dolgot tooconsider a következők:

* Hány magok minden virtuális gép rendelkezik, és minden core betöltése? Példa hello Azure gyors üzembe helyezési fenti sablon do_work.php parancsfájl, amely betölti a egyetlen mag rendelkezik. Ha például Standard_A1 vagy D1, majd szükség lenne toorun Ez a betöltés többször használ, a virtuális gépek nagyobb, mint egy alapszintű Virtuálisgép-méretet. Ellenőrizze, hogy hány processzormag, a virtuális gépek megtekintésével [méretek a Windows virtuális gépek Azure-ban](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* Hello Virtuálisgép-méretezési csoportban, hány virtuális gépeinek módon, az egyes munkaelem?
  
    A bővített kapacitású esemény csak fog kerül sor, amikor hello átlagos Processzorhasználatának keresztül **összes** méretezési csoportban lévő virtuális gépek hello túllépi hello küszöbértékét, belső hello időbeli hello automatikus skálázási szabályok definiálva.
* Nem hagyott ki méretezési eseményeket?
  
    Ellenőrizze az auditnaplókat hello hello Azure-portál a skála események. Lehet, hogy terjedő skálán fel és le, amely egy méretezési kimaradt. Szűrhet "Méretezési"...
  
    ![Naplók][audit]
* Eltérőek a méretezési és a kibővített küszöbértékeket megfelelően?
  
    Tegyük fel, hogy beállított egy szabály tooscale, amikor átlagos CPU 50 %-nál nagyobb több mint 5 perc és tooscale átlagos CPU esetén 50 %-nál kisebb. Emiatt egy "flapping" probléma CPU-használat esetén Bezárás toothis küszöbértéket, a skálázási műveletek folyamatosan növeléséhez és csökkentéséhez hello hello készlet mérete. Ebből kifolyólag hello automatikus skálázás szolgáltatás megpróbál tooprevent "" ugrál "", is jegyzékfájl nem skálázás, amely. Ezért győződjön meg arról, hogy a kibővített és skálázási a küszöbértékeket megfelelően különböző tooallow fel Between skálázás helyet.
* Volt-e írási saját JSON-sablon?
  
    Könnyen toomake hibák, ezért a sablont, például egy, amely felett bizonyítása toowork hello kezdődnie, és kis növekményes módosításokat. 
* Ön manuálisan méretezhető bejövő vagy kimenő?
  
    Próbálja meg újratelepíteni hello virtuális gépek különböző "kapacitás" beállítás toochange hello több Virtuálisgép-méretezési csoportban erőforrás manuálisan. Egy példa sablon toodo, ez a következő helyen: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing – tooedit hello sablon toomake hello van szükség, használja a méretezési mérete azonos számítógépre. Ha sikeresen módosíthatja a virtuális gépek hello számát manuálisan, majd meg tudja hello probléma elkülönített tooautoscale.
* Ellenőrizze a Microsoft.Compute/virtualMachineScaleSet és hello Microsoft.Insights erőforrások [Azure erőforrás-kezelővel](https://resources.azure.com/)
  
    Ez az egy nélkülözhetetlen hibaelhárítási eszköz, amely jelzi, hogy hello az Azure Resource Manager erőforrások állapotát. Kattintson az előfizetését, és nézze meg hello hibaelhárítási erőforráscsoportot. Hello számítási erőforrás-szolgáltató a Virtuálisgép-méretezési csoportban létrehozott hello tekintse meg, és ellenőrizze a hello példányait tartalmazó nézetet, amely jelzi, hogy a központi telepítés állapotának hello. Hello példányait tartalmazó nézetet a virtuális gépek a Virtuálisgép-méretezési csoportban hello is ellenőrizheti. Majd állapotba hello Microsoft.Insights erőforrás-szolgáltató, és ellenőrizze a hello automatikus skálázási szabályok szépen.
* Diagnosztikai bővítmény hello használata és teljesítményadatokat kibocsátó?
  
    **Frissítés:** Azure automatikus skálázás továbbfejlesztett toouse állomás alapú metrikák csővezeték, amelyhez már nem szükséges egy diagnosztikai bővítmény toobe telepítve lett. Ez azt jelenti, hogy tovább néhány hello bekezdések már nem érvényes, ha létrehoz egy automatikus skálázás alkalmazást hello új kimenetátirányításának segítségével. Azure-sablonok, amelyek konvertált toouse hello állomás csővezeték például: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale. 
  
    A gazdagép alapú metrikák az automatikus skálázási használata jobb, ha a következő okok miatt hello:
  
  * Kevesebb mozgó részek nem diagnosztika kiterjesztéseket toobe telepíteni kell.
  * Egyszerűbb sablonok. Csak hozzáadása insights automatikus skálázási szabályok tooan meglévő méretezési sablont.
  * További megbízható reporting, és új virtuális gépek gyorsabb indítása.
    
    hello csak okok miatt érdemes diagnosztikai kiterjesztés tookeep lenne, ha szüksége memória diagnosztika reporting/skálázás. A gazdagép alapú metrikák nem jelentik a memória.
    
    Vele szem előtt csak ha kövesse az Ez a cikk többi hello diagnosztikai bővítmények továbbra is használja az automatikus skálázást.
    
    Az Azure Resource Manager automatikus skálázás együttműködhet (de már nem rendelkezik) protokollt a Virtuálisgép-bővítmény hello diagnosztika bővítmény neve. Azt a teljesítmény adatok tooa tárfiók hello sablon meghatározott bocsát ki. Ezek az adatok majd hello Azure figyelőszolgáltatás szerint van összesítve.
    
    Hello Insights szolgáltatás hello virtuális gépek adatai nem olvashatók, ha az toosend kellene egy e-mailt – például ha hello virtuális gépeken volt, ezért nézze meg leveleit (hello Azure-fiók létrehozásakor megadott hello).
    
    Is megnyithatja, és keresse meg hello adatait. Tekintse meg hello Azure storage-fiók egy cloud explorer használatával. Például a hello használatával [Visual Studio Cloud Explorer](https://visualstudiogallery.msdn.microsoft.com/aaef6e67-4d99-40bc-aacf-662237db85a2), jelentkezzen be, és válasszon hello használata Azure-előfizetés és hello diagnosztikai tárfiók neve hivatkozott hello diagnosztika bővítmény definíciójának a központi telepítésben sablon...
    
    ![Cloud Explorer][explorer]
    
    Itt látni fogja álló, lemezcsoport típusú egyes virtuális gépek hello adatokat tároló tábla. Linux- és CPU-metrika példaként hello tart, nézze meg hello utolsó sort. hello Visual Studio cloud explorer lekérdezésnyelvet támogatja, így például a lekérdezés futtatása "időbélyeg gt dátum és idő" 2016-02-02T21:20:00Z "" toomake arra, hogy hello megtartott legutóbbi események (feltételezik időpontja UTC szerint). Nem látható hiba tartozik toohello hello adatok méretezése szabályok beállítása? Hello az alábbi példában hello CPU gép 20 elindított too100 % növelése keresztül hello utolsó 5 percig...
    
    ![Tárolási táblák][tables]
    
    Ha hello adatok nem létezik, majd az azt jelenti, hello probléma hello virtuális gépeken futó hello diagnosztikai kiterjesztésű. Ha hello adatok ott, ez arra utalhat, vagy probléma a skálázási szabályok vagy hello Insights szolgáltatással. Ellenőrizze [Azure állapot](https://azure.microsoft.com/status/).
    
    Amennyiben azt, hogy a fenti lépéseket, ha továbbra is problémák automatikus skálázás hello fórumok megpróbálhatja [MSDN](https://social.msdn.microsoft.com/forums/azure/home?category=windowsazureplatform%2Cazuremarketplace%2Cwindowsazureplatformctp), vagy [veremtúlcsordulás](http://stackoverflow.com/questions/tagged/azure), vagy a támogatási hívás. Lehet, előkészített tooshare hello sablon és hello teljesítményadatokat nézetét.

[audit]: ./media/virtual-machine-scale-sets-troubleshoot/image3.png
[explorer]: ./media/virtual-machine-scale-sets-troubleshoot/image1.png
[tables]: ./media/virtual-machine-scale-sets-troubleshoot/image4.png
