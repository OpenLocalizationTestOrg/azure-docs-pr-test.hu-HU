---
title: "az OMS célzó aaaSolution |} Microsoft Docs"
description: "Megoldás célcsoportkezelést egy olyan szolgáltatás, az Operations Management Suite (OMS), amely lehetővé teszi toolimit felügyeleti megoldások tooa meghatározott készletének ügynökök.  Ez a cikk ismerteti, hogyan toocreate a hatókör konfigurációját, és alkalmazza azt tooa megoldás."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1f054a4e-6243-4a66-a62a-0031adb750d8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2017
ms.author: bwren
ms.openlocfilehash: 6f8c8109e0d9e282e18724bf8b673b10de8e498a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-solution-targeting-in-operations-management-suite-oms-tooscope-management-solutions-toospecific-agents-preview"></a>Megoldással célzó Operations Management Suite (OMS) tooscope megoldások toospecific kezelőügynökök (előzetes verzió)
A megoldás tooOMS hozzáadásakor a alapértelmezett tooall Windows és Linux ügynökök csatlakoztatott tooyour Naplóelemzési munkaterület automatikusan telepíti.  Érdemes lehet a költségeket és a limit hello adatmennyiség gyűjtött toomanage megoldás ügynökök különösen készlete tooa korlátozásával.  Ez a cikk ismerteti, hogyan toouse **célcsoport-kezelési megoldás** Ez az az OMS-szolgáltatása, amely lehetővé teszi a hatókör tooapply tooyour megoldásokat.

## <a name="how-tootarget-a-solution"></a>Hogyan tootarget megoldás
Nincsenek három lépést tootargeting megoldás hello a következő részekben leírtak szerint.  Vegye figyelembe, hogy szüksége lesz hello OMS-portálon és a hello Azure-portálon más lépéseket.


### <a name="1-create-a-computer-group"></a>1. Hozzon létre egy számítógépcsoportot
Akkor adja meg, amelyet az adott hatókörben tooinclude létrehozásával hello számítógépek egy [számítógépcsoport](../log-analytics/log-analytics-computer-groups.md) a Naplóelemzési.  hello számítógépcsoport napló keresés alapján, vagy más forrásokból, például az Active Directory vagy a WSUS-csoportok importálása. Mint [az alábbiakban](#solutions-and-agents-that-cant-be-targeted), csak a számítógépek, amelyek közvetlenül csatlakoztatott tooLog Analytics hello hatókör fog szerepelni.

Ha már létre a munkaterületén hello számítógépcsoportot, majd is tartalmazni fogja az alkalmazott tooone vagy további megoldások hatókör konfigurációban.
 
 
 ### <a name="2-create-a-scope-configuration"></a>2. Hatókör-konfiguráció létrehozása
 A **hatókör konfigurációjának** egy vagy több számítógép csoportot tartalmaz, és lehet alkalmazott tooone vagy további megoldásokat. 
 
 Hozzon létre egy hatókör konfigurációját, a folyamatot követve hello segítségével.  

 1. Az Azure-portálon hello, keresse meg a túl**Naplóelemzési** válassza ki a munkaterületen.
 2. Hello munkaterület hello tulajdonságaiban **munkaterület adatforrások** válasszon **hatókör konfigurációk**.
 3. Kattintson a **Hozzáadás** toocreate egy új hatókör konfigurációját.
 4. Adjon meg egy **neve** hello hatókör-konfigurációhoz.
 5. Kattintson a **válassza ki a számítógépcsoportokat**.
 6. Válassza ki a létrehozott hello számítógépcsoport és opcionálisan más csoportok tooadd toohello beállításokat.  Kattintson a **Kiválasztás** gombra.  
 6. Kattintson a **OK** toocreate hello hatókör konfigurációját. 


 ### <a name="3-apply-hello-scope-configuration-tooa-solution"></a>3. Hello hatókör konfigurációs tooa megoldás alkalmazni.
Miután a hatókör konfigurációját, majd alkalmazhatja tooone vagy további megoldásokat.  Vegye figyelembe, hogy egy egyetlen hatókör konfigurációjának több megoldás is használható, amíg minden megoldás csak használhatja egy hatókör-konfigurációt.

A hatókör konfigurációját, a folyamatot követve hello segítségével alkalmazni.  

 1. Az Azure-portálon hello, keresse meg a túl**Naplóelemzési** válassza ki a munkaterületen.
 2. Hello tulajdonságaiban hello munkaterületen válassza ki a **megoldások**.
 3. Kattintson a hello megoldás tooscope szeretné.
 4. Hello megoldás alatt hello tulajdonságaiban **munkaterület adatforrások** válasszon **célcsoport-kezelési megoldás**.  Ha nem érhető el hello beállítás majd [Ez a megoldás nem tudja megcélozni](#solutions-and-agents-that-cant-be-targeted).
 5. Kattintson a **Hozzáadás hatókör konfigurációjának**.  Ha már van egy alkalmazott konfiguráció toothis megoldás ezt a beállítást elérhetetlenné válik.  Meglévő konfigurációs hello felvétele előtt el kell távolítania.
 6. Kattintson a létrehozott hello hatókör konfigurációját.
 7. A Watch hello **állapot** hello konfigurációs tooensure, amely azt mutatja, **sikeres**.  Ha hello állapot hibát jelez, majd kattintson a hello ellipszis toohello jobb hello konfigurációs, és válassza a **Szerkesztés hatókör konfigurációjának** toomake módosításokat.

## <a name="solutions-and-agents-that-cant-be-targeted"></a>Megoldások és az ügynököket, amelyeket nem telepíthető
Az alábbiakban az ügynök és a megoldások, amelyek nem használható a célcsoport-kezelési megoldás hello feltételeket.

- Csak célcsoport-kezelési megoldás tooagents telepítendő toosolutions vonatkozik.
- Csak célcsoport-kezelési megoldás a Microsoft által biztosított toosolutions vonatkozik.  Nem alkalmazható toosolutions [partnerek vagy saját maga által létrehozott](operations-management-suite-solutions-creating.md).
- Ügynökök tooLog Analytics közvetlenül csatlakozó csak szűrésére.  Megoldás-e azokat a hatókör beállításait tartalmazza-e a most csatlakoztatott Operations Manager felügyeleti csoport részét képező ügynökök tooany automatikusan telepíteni fogja.

### <a name="exceptions"></a>Kivételek
Célcsoport-kezelési megoldás nem használható hello annak ellenére, hogy azok hello fér el a következő megoldások megadott feltételeknek.

- Ügynök állapotfigyelő értékelése

## <a name="next-steps"></a>Következő lépések
- További tudnivalók a megoldások többek között a környezetében, a rendelkezésre álló tooinstall hello megoldásokat [hozzáadása Azure Naplóelemzés felügyeleti megoldások tooyour munkaterület](../log-analytics/log-analytics-add-solutions.md).
- További információ a számítógépcsoportok létrehozása [számítógépcsoportokat a Log Analyticshez jelentkezzen keresések](../log-analytics/log-analytics-computer-groups.md).