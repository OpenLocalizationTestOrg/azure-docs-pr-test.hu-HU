---
title: "aaaManage kísérletezhet az ismétlés a Machine Learning Studióban |} Microsoft Docs"
description: "Hogyan toomanage kísérletezhet az Azure Machine Learning Studióban az ismétlés"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 6a53530f-20d5-40ae-9b49-7b499ccb44b7
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: bd30c048ce063811b1b2de8ce6d71e99ba975713
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-experiment-iterations-in-azure-machine-learning-studio"></a>Kísérlet ismétléseinek kezelése az Azure Machine Learning Studióban
A prediktív elemzési modellek fejlesztése iteratív folyamat -, addig módosítjuk hello különböző funkciók és a paraméterek a kísérlet, az eredményeket addig közelítjük, amíg úgy nem véljük, hogy rendelkezik-e a betanított, hatékony modellel rendelkezünk. Kulcs toothis folyamat van követési hello különböző ismétlési kísérlet paraméterek és konfigurációkat.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Tekintse át az előző fut a kísérletek rendelés toochallenge bármikor, le újra, és végső soron erősítse meg vagy pontosíthatja az előző feltételezéseket. Amikor futtatja a kísérlet, a Machine Learning Studio tartja hello futtatja, beleértve az adatkészletet, a modul, és a port kapcsolatok és a paraméterek előzményeit. Az előzményekben az eredményeket, például indítási és leállítási időpontok naplóüzenetek vagy végrehajtásának állapota futásidejű adatok is rögzíti. Vessen egy pillantást vissza ezen fut, minden alkalommal tooreview hello időrendje a kísérlet és a köztes eredmények bármelyikét. Még akkor is használhatja a kísérlet toolaunch korábbi futtatásakor be egy új szakasz lekérdezés és a felderítési az elérési út toocreating egyszerű, bonyolult vagy akár ensemble modellezési megoldások a.

> [!NOTE]
> A kísérlet korábbi futtatásakor megtekintésekor hello kísérlet verzió zárolva van, ezért nem szerkeszthető. Azonban mentheti egy példányát kattintva **SAVE AS** és egy új hello példány nevét. A Machine Learning Studio megnyitása hello új példányt, amely majd szerkesztésével és futtatásával. Ezt a példányt a kísérlet érhető el hello **kísérletek** együtt a más kísérletek a listában.
> 
> 

## <a name="viewing-hello-prior-run"></a>Megtekintés hello előzetes futtatása
Ha a kísérlet megnyitott, amelyek legalább egyszer futnak, megtekintheti a hello megelőző hello kísérlet futtatása gombra kattintva **előzetes futtatása** hello tulajdonságok panelen.

Tegyük fel például, a kísérlet létrehozása és futtatása a verziókat 11:23, a 11:42 és 11:55. Ha, nyissa meg a legutóbbi futtatás hello hello kísérlet (11:55), majd kattintson az **előzetes futtatása**, a 11:42 futtatta hello verzió már meg van nyitva.

## <a name="viewing-hello-run-history"></a>Megtekintés hello előzmények futtatása
Kattintva megtekintheti az összes hello előző fut egy kísérlet **futtatása előzményeinek megtekintése** egy megnyitott kísérletben.

Tegyük fel például, egy kísérlet létrehozásához hello [lineáris regressziós] [ linear-regression] modul, és azt szeretné, tooobserve hello hatásának hello értékének módosítása **tanulási rátát** a a kísérlet eredményeit. Lefuttatta a hello kísérlet többször különböző érték is ezt a paramétert, az alábbiak szerint:

| Learning sebesség értékének | Futtatás kezdő időpontja |
| --- | --- |
| 0.1 |9/11/2014 du. 4:18:58 |
| 0.2 |9/11/2014 du. 4:24:33 |
| 0.4 |9/11/2014 du. 4:28:36 |
| 0.5 |9/11/2014 du. 4:33:31 |

Ha **futtatása ELŐZMÉNYEINEK megtekintése**, ezek futtatása listájának megtekintéséhez:

![Példa futtatási előzményei][runhistory]

Kattintson bármelyik ezek futtatása tooview hello pillanatképe kísérletezhet hello időpontban futtatta azt. hello konfigurációs paraméter értéke, megjegyzések és eredmények az összes megőrzött toogive egy adott futtató kísérletbe teljes bejegyzést.

> [!TIP]
> toodocument a hello kísérlet ismétléseinek, módosíthatja hello cím minden alkalommal, amikor futtatja, frissítheti az hello **összegzés** hello kísérletezhet hello tulajdonságok panelen, és adja hozzá, vagy frissítse az egyéni modulok fűzött megjegyzések toorecord a módosításokat. minden egyes futtatásához hello kísérlet hello cím, összegzésének és modul megjegyzések menti.
> 
> 

a hello kísérletek hello listája **kísérletek** a Machine Learning Studióban mindig megjeleníti az a kísérlet hello legújabb verzióját. Hello kísérlet korábbi futtatásakor megnyitásakor (használatával **előzetes futtatása** vagy **futtatása ELŐZMÉNYEINEK megtekintése**), toohello vázlatként megjelölt verziót gombra kattintva térhet vissza **futtatása ELŐZMÉNYEINEK megtekintése** , majd válassza hello iterációs, amely rendelkezik egy **állapot** a **szerkeszthető**.

## <a name="iterating-on-a-previous-run"></a>Az előző léptetés
Amikor rákattint **előzetes futtatása** vagy **futtatása ELŐZMÉNYEINEK megtekintése** és nyissa meg az előző, akkor az egy végzett kísérlet csak olvasható módban.

Ha azt szeretné, hogy az ismétlések kísérletbe kezdődő, és az előző konfigurált hello módon toobegin, ehhez megnyitása hello futtatni, majd **SAVE AS**. Ez hoz létre egy új kísérlet egy új cím, egy üres futtatási előzményei, és minden hello összetevők és paraméter értékének előző hello futtatásához. Az új kísérlet hello szerepel **KÍSÉRLETEKET** lap hello Machine Learning Studio kezdőlapjára, és módosíthatja, és kezdeményezi egy új futtatáshoz, futtassa az ismétlések kísérletbe előzményeit. 

Tegyük fel például, hello kísérlet futtatási előzményei hello előző részben található. Mi történik, ha úgy állítja be a hello tooobserve kívánt **tanulási rátát** paraméter too0.4, és próbálja meg különböző érték is hello **képzési alapidőszakkal száma** paraméter.

1. Kattintson a **futtatása ELŐZMÉNYEINEK megtekintése** , és nyissa meg a hello iterációs hello kísérlet futtatott du. 4:28:36 (beállította hello paraméter értéke too0.4).
2. Kattintson a **SAVE AS**.
3. Adjon meg egy új címet, majd kattintson a hello **OK** be van jelölve. Új hello kísérlet hozza létre.
4. Módosítsa a hello **képzési alapidőszakkal száma** paraméter.
5. Kattintson a **futtatása**.

Most toomodify folytatja, és futtassa a jelen verziójában a kísérlet létrehozása egy új futtatási előzményei toorecord a munkáját.

<!-- Images -->
[runhistory]:./media/machine-learning-manage-experiment-iterations/viewrunhistory.jpg


<!-- Module References -->
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
