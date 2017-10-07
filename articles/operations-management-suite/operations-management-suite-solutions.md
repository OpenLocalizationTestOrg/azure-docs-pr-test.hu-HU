---
title: az Operations Management Suite (OMS) aaaSolutions |} Microsoft Docs
description: "Megoldások bővíthetők hello Operations Management Suite (OMS), adja meg a csomagolt felügyeleti lehetőségeket, hogy az ügyfelek tootheir OMS-munkaterület adhat hozzá.  Ez a cikk ügyfelek és partnerek által létrehozott hogyan egyéni megoldások részleteit."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 1f054a4e-6243-4a66-a62a-0031adb750d8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5b5a538f1bc4b5577bec94db08bd43668bc6584a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-management-solutions-in-operations-management-suite-oms-preview"></a>Felügyeleti megoldások Operations Management Suite (OMS) (előzetes verzió) használatával
> [!NOTE]
> Ez a felügyeleti megoldásokra az OMS Szolgáltatáshoz, amely jelenleg előzetes verzióban érhetők előzetes dokumentáció.    
> 
> 

Megoldások bővíthetők hello Operations Management Suite (OMS), adja meg a csomagolt felügyeleti lehetőségeket, hogy az ügyfelek tootheir környezet adhat hozzá.  Ezenkívül túl[Microsoft által biztosított megoldások](../log-analytics/log-analytics-add-solutions.md), partnereinknek és ügyfeleinknek felügyeleti megoldások toobe a saját környezetben használt, illetve mikor hello közösségi keresztül elérhető toocustomers hozhat létre.

## <a name="finding-and-installing-management-solutions"></a>Keresése és telepítése a megoldások
Több módon keresése és telepítése a megoldások hello a következő részekben leírtak szerint.

### <a name="azure-marketplace"></a>Azure Piactér
A Microsoft által biztosított megoldások, és megbízható partnerek a hello Azure-portálon az Azure piactér hello telepíthetők.

1. Jelentkezzen be toohello Azure-portálon.
2. Hello bal oldali ablaktáblában jelöljön ki **további szolgáltatások**.
3. Vagy görgessen lefelé túl**megoldások** vagy típus *megoldások* történő hello **szűrő** párbeszédpanel.
4. Kattintson a hello **+ Hozzáadás** gombra.
5. Érdekli, vagy keresse meg azt, megoldások kereséséhez kattintson hello **szűrő** gombra, vagy írja be a hello **keresési Everthing** mezőbe.
6. Kattintson a Piactéri elemet tooview azok részletes információt.
7. Kattintson a **létrehozása** tooopen hello **megoldás hozzáadása** ablaktáblán.
8. Fel a kért toorequired információk, például hello [OMS munkaterületet, és az Automation-fiók](#oms-workspace-and-automation-account) ezenkívül azokat a paramétereket a toovalues hello megoldás.
9. Kattintson a **létrehozása** tooinstall hello megoldás.

### <a name="oms-portal"></a>OMS-portálon
A Microsoft által biztosított megoldások hello megoldások gyűjtemény az OMS-portálon hello is kell telepíteni.

1. Jelentkezzen be toohello OMS-portálon.
2. Kattintson a hello **megoldások gyűjtemény** csempére.
3. Az oldalon hello OMS megoldások gyűjteménye ismerje meg minden elérhető megoldás. Kattintson a megjeleníteni kívánt tooadd tooOMS hello megoldás hello nevét.
4. Hello megoldáshoz választott hello oldalon hello megoldás részletes információkat jelenít meg. Kattintson az **Add** (Hozzáadás) parancsra.
5. Egy új csempe jelenik meg, hello az OMS – áttekintés oldalra, és elkezdi használni, azt követően hello OMS-szolgáltatás feldolgozza az adatok hozzáadott hello megoldás.

### <a name="azure-quickstart-templates"></a>Azure-gyorssablonok
Hello Közösség tagjai is elküldhetik felügyeleti megoldások tooAzure gyorsindítási sablonok.  Töltse le a későbbi telepítési ezeket a sablonokat vagy vizsgálhatja azokat hogyan túl toolearn[hozzon létre egy saját megoldások](#creating-a-solution).

1. Kövesse az ismertetett folyamatot hello [OMS munkaterületet, és az Automation-fiók](#oms-workspace-and-automation-account) toolink egy munkaterület és a fiókot.
2. Nyissa meg túl[Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/documentation/templates/).  
3. Keressen olyan megoldás, amely kíváncsiak vagyunk.
4. Válasszon hello megoldás hello eredmények tooview hozzá tartozó részletek.
5. Kattintson a hello **tooAzure telepítése** gombra.
6. Rákérdezéses tooprovide információkat, például hello erőforráscsoportot és helyet a hozzáadása toovalues azokat a paramétereket hello megoldásban lesz.
7. Kattintson a **beszerzési** tooinstall hello megoldás.

### <a name="deploy-azure-resource-manager-template"></a>Azure Resource Manager-sablon üzembe helyezése
Hello közösségi vagy, hogy kapott megoldások [saját](#creating-a-solution) valósíthatók meg a Resource Manager sablonként és hello szabványos módszerek bármelyikét használhatja [a sablonok telepítésével](../azure-resource-manager/resource-group-template-deploy-portal.md).  Vegye figyelembe, hogy hello megoldás a telepítés előtt kell létrehozni, és hello [OMS munkaterületet, és az Automation-fiók](#oms-workspace-and-automation-account).

## <a name="oms-workspace-and-automation-account"></a>OMS-munkaterület és Automation-fiók
A legtöbb felügyeleti megoldás szükséges egy [OMS-munkaterület](../log-analytics/log-analytics-manage-access.md) toocontain nézetek és egy [Automation-fiók](../automation/automation-security-overview.md#automation-account-overview) toocontain runbookok és a kapcsolódó erőforrások. hello munkaterület, és a fiók hello követelményeknek kell megfelelnie.

* A megoldás csak akkor tudja használni, egy OMS-munkaterület és egy Automation-fiók.  
* hello OMS-munkaterület és a megoldás által használt Automation-fiók nem lehet csatolt tooone egy másik. Az OMS-munkaterület csak akkor csatolt tooone Automation-fiók, és Automation-fiók csak akkor lehetséges, hogy a csatolt tooone OMS-munkaterület.
* toobe kapcsolódó, hello OMS-munkaterület, és automatizálási fióknak kell lennie a hello ugyanazt az erőforráscsoportot és régióban.  hello kivétel: USA keleti régiójában az OMS-munkaterület és és az Automation-fiók az USA keleti régiója 2.

### <a name="creating-a-link-between-an-oms-workspace-and-automation-account"></a>Automation-fiók és az OMS-munkaterület közötti kapcsolat létrehozása
Hogyan hello OMS-munkaterület ad meg, és a megoldás hello telepítési módszer függ Automation-fiók.

* A Microsoft-megoldás hello OMS-portálon keresztül történő telepítésekor hello aktuális OMS-munkaterület van telepítve, és nem Automation-fiók szükséges.
* A megoldás hello Azure piactéren keresztül történő telepítésekor az OMS-munkaterület és Automation-fiók kéri, és közöttük hello hivatkozás jön létre.  
* A megoldások kívül hello Azure piactérről hozzá kell rendelnie hello OMS-munkaterület és Automation-fiók hello megoldás telepítése előtt.  Ehhez jelölje ki a megoldás hello Azure piactér és hello OMS-munkaterület és Automation-fiók kiválasztása.  Nincs tooactually hello megoldás telepíteni, mert hello hivatkozás létrejön, amint hello OMS-munkaterület Automation-fiók ki van választva.  Miután hello kapcsolat létrejött, majd használhatja, hogy OMS-munkaterület és Automation-fiók minden megoldás. 

### <a name="verifying-hello-link-between-an-oms-workspace-and-automation-account"></a>Automation-fiók és az OMS-munkaterület közötti hello hivatkozás ellenőrzése
Ellenőrizheti a hello hivatkozás az OMS-munkaterület és a használatával a következő eljárás hello Automation-fiók között.

1. Jelölje ki hello Automation-fiók hello Azure-portálon.
2. Görgessen toohello alsó részén hello **beállítások** ablaktáblán.
3. Van-e nevű szakaszban **OMS-erőforrások** a hello **beállítások** ablaktáblán, majd ezt a fiókot az OMS-munkaterület csatolt tooan.
4. Válassza ki **munkaterület** tooview hello részleteit hello OMS-munkaterület toothis az Automation-fiókhoz csatolva.

## <a name="listing-management-solutions"></a>Megoldások listázása
A következő eljárás tootooview hello felügyeleti megoldásokat hello munkaterületek csatolt tooyour Azure-előfizetés hello használata.

1. Jelentkezzen be toohello Azure-portálon.
2. Hello bal oldali ablaktáblában jelöljön ki **további szolgáltatások**.
3. Vagy görgessen lefelé túl**megoldások** vagy típus *megoldások* történő hello **szűrő** párbeszédpanel.
4. A munkaterületek telepített megoldások jelenik meg.

Vegye figyelembe, hogy csak a telepített hello aktuális munkaterületen hello OMS-portálon hello Microsoft megoldások megtekintése.

## <a name="removing-a-management-solution"></a>A felügyeleti megoldás eltávolítása
A felügyeleti megoldás eltávolítása után hello megoldás az összes erőforrást is eltávolítja.  

1. Keresse meg a hello megoldás hello Azure portálon hello eljárás használatával [megoldások listázása](#listing-solutions).
2. Jelölje ki a kívánt tooremove hello megoldást.
3. Kattintson a hello **törlése** gombra.

## <a name="creating-a-management-solution"></a>A felügyeleti megoldás létrehozása
Teljes útmutatás létrehozása kezelési megoldások érhetők el [megoldások létrehozása az Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md). 

## <a name="next-steps"></a>Következő lépések
* Keresési [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/documentation/templates) példákért különböző Resource Manager-sablonok.
* Hozza létre a sajátját [megoldások](operations-management-suite-solutions-creating.md).

