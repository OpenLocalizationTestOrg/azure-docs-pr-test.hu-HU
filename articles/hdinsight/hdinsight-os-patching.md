---
title: "Linux-alapú HDInsight-fürtök - Azure aaaConfigure az operációs rendszer javítási ütemezését |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure az operációs rendszer javítási ütemezését Linux-alapú HDInsight-fürtök."
services: hdinsight
documentationcenter: 
author: bprakash
manager: asadk
editor: bprakash
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/21/2017
ms.author: bhanupr
ms.openlocfilehash: 1598d64e594d7e8a68573fc63dd86051a5a9d025
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="os-patching-for-hdinsight"></a>Az operációs rendszer a HDInsight javítását 
A Hadoop felügyelt szolgáltatásként HDInsight gondoskodik az alapul szolgáló virtuális gépek a HDInsight-fürtök által használt hello OS hello javítását. 2016 augusztusától 1-től hello vendég operációs rendszer javítási házirend a Linux-alapú HDInsight-fürtök (3.4 vagy újabb verziója) módosítottuk. hello hello új házirend célja toosignificantly hello újraindítások számának csökkentése miatt toopatching. Új házirend hello továbbra is toopatch virtuális gépek (VM) Linux clusters minden hétfőn vagy csütörtök 12 óra UTC lépcsőzetes módon kezdődő bármely adott fürt csomópontjai között. A megadott virtuális gép azonban csak újraindul, legfeljebb 30 naponta egyszer miatt az operációs rendszer tooguest javítását. Ezenkívül hello első újraindítás, újonnan létrehozott fürt nem történik meg gyakoribb 30 napon hello fürt létrehozását. Javítások hatékony lesz, ha hello virtuális gépek újraindítása van.

## <a name="how-tooconfigure-hello-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Hogyan tooconfigure hello Linux-alapú HDInsight-fürtök az operációs rendszer javítási ütemezését
a HDInsight-fürtök hello virtuális gépek kell toobe alkalmanként indítani, hogy fontos biztonsági javításokat is telepíthető. 2016 augusztusától 1-től új Linux-alapú HDInsight-fürtök (3.4 vagy újabb verzió) újraindítása van, a következő ütemezés hello használata:

1. A virtuális gép hello fürt is csak a újra javítások legfeljebb egyszer egy 30 napos időtartamon belül.
2. hello újraindítás 12 óra UTC induló következik be.
3. hello újraindítás folyamatban van lépcsőzetes elrendezésben hello fürtön lévő virtuális gépek között, így hello újraindítás során hello fürt továbbra is elérhető.
4. hello első újraindítás, újonnan létrehozott fürt nem számított 30 napon belül hello fürt létrehozásának dátuma hamarabb történjen.

Ebben a cikkben leírt hello parancsfájlművelet használva módosíthatja hello az operációs rendszer javítási ütemezés az alábbiak szerint:
1. Engedélyezi vagy letiltja az automatikus újraindulását
2. Set hello gyakorisága újraindul (újraindításnál nap)
3. Ha újraindítás történik hello hét napja hello beállítása

> [!NOTE]
> A parancsfájl művelet csak Linux-alapú HDInsight-fürtökkel 2016 augusztusától 1. után létrehozott fog működni. Javítások hatékony lesz, csak akkor, ha a virtuális gép újraindítása van. 
>

## <a name="how-toouse-hello-script"></a>Hogyan toouse hello parancsfájl 

Ha ezt a parancsfájlt a használatához a következő információ hello:
1. parancsfájl helyét hello: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv01/os-patching-reboot-config.sh.  HDInsight a URI toofind használ, és hello parancsprogrammal hello hello fürt összes virtuális gépeken.
  
2. fürt csomóponttípusok hello parancsfájl alkalmazott hello: headnode, workernode, zookeeper. Ez a parancsfájl alkalmazott tooall csomóponttípusok hello fürt kell lennie. Ha még nem alkalmazott tooa csomóponttípus, majd hello virtuális gépek az adott csomópont toouse hello korábbi javítási ütemezés továbbra is.


3.  A paraméter: Ez a parancsfájl három numerikus paramétert fogad:

    | Paraméter | Meghatározás |
    | --- | --- |
    | Engedélyezi/letiltja az automatikus újraindulását |0 vagy 1. A 0 érték letiltja az automatikus újraindulását 1 lehetővé teszi az automatikus újraindulását. |
    | gyakoriság |7 too90 (a határokat is beleértve). virtuális gépek hello javítások, amely a számítógép újraindítása szükséges újraindítása előtt (nap) toowait hello száma. |
    | Hét napja |1 too7 (a határokat is beleértve). 1 érték azt jelzi, hétfőn hello újraindítás megtörténik, a 7 pedig egy Sunday.For példában paraméterek használatával 1-60 2 eredmények automatikus újraindítás csak 60 naponta (legfeljebb) keddjén jelennek meg. |
    | Adatmegőrzés |Egy parancsfájl művelet tooan meglévő fürt alkalmazásakor a hello parancsfájl jelölheti, megőrzött is. Új workernodes toohello fürt műveletek a méretezés során hozzáadott megőrzött parancsfájlok lépnek érvénybe. |

> [!NOTE]
> Ezt a parancsfájlt, megőrzött tooan meglévő fürt alkalmazásakor kell megjelölni. Ellenkező esetben a méretezés műveletek során létrehozott új csomópontok hello alapértelmezett ütemezés javítását fogja használni.
Ha hello parancsfájl hello Fürtlétrehozási folyamatának részeként, állandó automatikusan.
>

## <a name="next-steps"></a>Következő lépések

Hello parancsfájlművelet, az adott lépéseket lásd a következő szakaszok a hello hello [testreszabása Linuz-alapú HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md):

* [Egy parancsfájlművelettel fürt létrehozása során](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [A parancsfájl művelet tooa futtató fürt alkalmazása](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
