---
title: "aaaCreate munkafolyamatok a sablonok – Azure Logic Apps |} Microsoft Docs"
description: "Első lépések – Azure Logic Apps sablonok tooconnect apps használatával gyorsan hozzon létre a munkafolyamatok és adatok integrálására."
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 3656acfb-eefd-4e75-b5d2-73da56c424c9
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: LADocs; klam
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0127523662a12076772b52968f1e2f9cbad72a1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-workflow-using-a-pre-built-template-or-pattern-tooget-started-quickly"></a>A munkafolyamat egy előre elkészített sablon vagy a minta tooget gyorsan konfigurálása

## <a name="what-are-logic-app-templates"></a>Mik azok a logic app sablonok
A logic app-sablon egy előre létrehozott logikai alkalmazás használható tooquickly Ismerkedés a saját munkafolyamat létrehozása. 

Ezek a sablonok egy jó módszer toodiscover vannak, amelyek használatával logic apps építhetők különböző minták. Használhatja ezeket a sablonokat, mint-van, illetve szerkesztésükhöz toofit a forgatókönyvéhez.

## <a name="overview-of-available-templates"></a>Az elérhető sablonok – áttekintés
Nincsenek közzétett hello logic app platform számos elérhető sablonok. Néhány példa kategóriák, valamint a őket, összekötői hello típusú alább láthatók.

### <a name="enterprise-cloud-templates"></a>Vállalati felhő sablonok
Sablonok, amelyek integrálják a Dynamics CRM-hez, a Salesforce, a mezőbe, a Azure Blob és a többi összekötőt, a vállalati felhő igényeinek. Néhány példa arra, mi ezekkel végezhető-sablonjai tartalmazzák a érdeklődők rendszerezése és a vállalati fájladatok biztonsági mentésekor.

### <a name="enterprise-integration-pack-templates"></a>Vállalati integrációs felügyeleticsomag-sablonok
VETER beállításait (ellenőrzése, kinyerési, átalakítási, kiegészítése, útvonal-) folyamatok, egy X12 EDI keresztül AS2-dokumentum, és átalakítás az tooXML, valamint X12 és AS2 üzenet kezelési fogadásakor.

### <a name="protocol-pattern-templates"></a>Protokoll mintát sablonok
Ezeket a sablonokat a logic apps tartalmazó protokoll kombinációját, például a kérés-válasz HTTP, valamint a Integrációk keresztül FTP és az SFTP áll. A ezek léteznek vagy alapjaként protokoll összetettebb létrehozásához.  

### <a name="personal-productivity-templates"></a>Egyéni hatékonyságnövelő sablonok
Minták toohelp személyes termelékenység növelése közé tartozik a sablonokat, napi Emlékeztető beállítása, fontos munkaelemek kapcsolja be a feladatlisták és lefelé tooa egyetlen felhasználói jóváhagyás lépés hosszadalmas feladatok automatizálásához.

### <a name="consumer-cloud-templates"></a>Fogyasztó felhő sablonok
Egyszerű sablonokat, amelyekbe beépül az közösségi szolgáltatások, például a Twitter, Slackhez és e-mailek, végső soron képes a közösségi média kezdeményezések marketing megerősítése. Ide tartoznak például zavaros másolja, sablonok, amelyek segíthetnek a termelékenység növelése által hagyományosan ismétlődő feladatok mentik a fordított időt is. 

## <a name="how-toocreate-a-logic-app-using-a-template"></a>Hogyan toocreate egy logic app sablon segítségével
megkezdődött a logic app sablon segítségével tooget hello logic app designer kísérhet. Ha hello designer meg egy meglévő logikai alkalmazás megnyitásával, hello logikai alkalmazás automatikusan betölti a Tervező nézetben. Azonban ha egy új logikai alkalmazást hoz létre, látható az alábbi üdvözlő képernyőt.  
 ![](../../includes/media/app-service-logic-templates/template7.png)  

Ezen a képernyőn megadhatja egy üres logikai alkalmazást vagy egy előre elkészített sablon toostart. Ha hello sablonok valamelyikét választja, további információkkal szolgálnak. A jelen példában használjuk hello *Dropbox egy új fájl jön létre, amikor másolja tooOneDrive* sablont.  
 ![](../../includes/media/app-service-logic-templates/template2.png)  

Ha úgy dönt, hogy toouse hello sablon, csak kiválaszthatnak egy hello *ezzel a sablonnal* gombra. Meg kell adnia toosign a tooyour fiókok alapján melyik összekötők hello sablont használja. Vagy, ha korábban már létrehozta a kapcsolatot az összekötők, válassza alább látható módon folytatódik.  
 ![](../../includes/media/app-service-logic-templates/template3.png)  

Hello kapcsolat létrehozása és kijelölése után *továbbra is*, hello logikai alkalmazás Tervező nézetben nyílik meg.  
 ![](../../includes/media/app-service-logic-templates/template4.png)  

Hello a fenti példában a hello helyzet sok sablonokkal néhány hello kötelező tulajdonság mező előfordulhat, hogy lehet a program belül hello összekötők; azonban néhány továbbra is szükség lehet egy érték mielőtt tooproperly hello logikai alkalmazás központi telepítése. Ha néhány hello hiányzó mezők megadása nélkül próbálja toodeploy, értesítést is, egy hibaüzenet.

Ha a tooreturn toohello sablon megjelenítő, válassza ki a hello *sablonok* hello felső navigációs sáv gombjára. Váltás hátsó toohello sablon megjelenítő, elvesznek a nem mentett végrehajtási. Előzetes tooswitching sablon viewer programba, megjelenik egy figyelmeztető üzenet, amely értesíti erről.  
 ![](../../includes/media/app-service-logic-templates/template5.png)  

## <a name="how-toodeploy-a-logic-app-created-from-a-template"></a>Hogyan toodeploy logikai alkalmazás létrehozása sablonból
Amennyiben az a sablon betöltése, és a kívánt módosításokat végzett, jelölje ki mentési gomb hello hello bal felső sarokban. Ez menti, és teszi közzé a Logic Apps alkalmazást.  
 ![](../../includes/media/app-service-logic-templates/template6.png)  

Ha hogyan tooadd további lépések sablonná meglévő logic app további információkat, például volna, vagy hogy általában módosítást kell végrehajtania, további információ: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).

