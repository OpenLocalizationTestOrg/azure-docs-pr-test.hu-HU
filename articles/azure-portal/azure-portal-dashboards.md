---
title: "aaaCreate és az Azure portál irányítópultok megosztása |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan toocreate és Szerkesztés irányítópultok a hello Azure-portálon."
services: azure-portal
documentationcenter: 
author: sewatson
manager: timlt
editor: tysonn
ms.assetid: ff422f36-47d2-409b-8a19-02e24b03ffe7
ms.service: multiple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 09/06/2016
ms.author: sewatson
ms.openlocfilehash: 0facd10fe3284d7ad9a9e29791e5af5b5b95c97f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-share-dashboards-in-hello-azure-portal"></a>Hozzon létre és osszon irányítópultok a hello Azure-portálon
Több irányítópulton létrehozhat és megoszthatja őket másokkal, akiknek hozzáférést tooyour Azure előfizetések.  Ez a cikk végighalad létrehozása, szerkesztése, közzététele és kezelése, hozzáférés toodashboards hello alapjait.

## <a name="create-a-dashboard"></a>Irányítópult létrehozása
toocreate egy irányítópultot, válassza hello **új irányítópult** következő toohello aktuális irányítópult neve gombra.  

![irányítópult létrehozása](./media/azure-portal-dashboards/new-dashboard.png)

Ez a művelet létrehoz egy új, üres, személyes irányítópultot, és elhelyezi, amelyen az irányítópult neve, és adja hozzá vagy átrendezheti a csempéket testreszabási módban.  Ebben a módban a hello összecsukható hello bal oldali navigációs menü csempére a gyűjtemény vesz igénybe.  hello csempe gyűjtemény lehetővé teszi, hogy az Azure-erőforrások különböző módokon csempék találhatók: által tallózással [erőforráscsoport](../azure-resource-manager/resource-group-overview.md#resource-groups), az erőforrás írja be, ha [címke](../azure-resource-manager/resource-group-using-tags.md), vagy az erőforrás neve alapján keres.  

![testre szabhatja az irányítópultot](./media/azure-portal-dashboards/customize-dashboard.png)

Húzással őket hello irányítópult felület, ahol azt szeretné, hozzáadhatja a csempéket.

Van egy új kategóriába **általános** csempék, amelyek nincsenek társítva van egy adott erőforráshoz.  Ebben a példában azt PIN-kód hello Markdown csempe.  A csempe tooadd egyéni tartalom tooyour irányítópult használja.  hello csempe támogatja egyszerű szöveges [Markdown-szintaxis](https://daringfireball.net/projects/markdown/syntax), és a HTML egy korlátozott körét.  (A biztonsági, mint szúrjon nem hajtható végre `<script>` címkéket, vagy bizonyos stílusbeállításokat elemmel, amely hello portal CSS.) 

![markdown hozzáadása](./media/azure-portal-dashboards/add-markdown.png)

## <a name="edit-a-dashboard"></a>Irányítópult szerkesztése
Miután létrehozta az irányítópulton, PIN-kód hello csempe vagy a paneleken hello csempe ábrázolását csempét. Most PIN-kód az erőforráscsoport hello ábrázolását. Hello böngészésekor, elem vagy hello erőforráscsoport panel vagy rögzíti. Mindkét megközelítés eredményez hello erőforráscsoport ábrázolását hello csempe rögzítését.

![PIN-kód toodashboard](./media/azure-portal-dashboards/pin-to-dashboard.png)

Miután hello elem, megjelenik az irányítópulton.

![Irányítópult nézet](./media/azure-portal-dashboards/view-dashboard.png)

Most, hogy a Markdown csempe- és erőforrás csoport rögzített toohello irányítópultot, azt átméretezése, és megfelelő elrendezésekben hello csempék átrendezését.

Rámutató és válassza a "...", vagy kattintson a jobb gombbal a mozaikokra összes hello környezetfüggő parancsok mozaik tekintheti meg. Alapértelmezés szerint nincsenek két elemek:

1. **Az irányítópult rögzítését** – hello irányítópultról eltávolítja hello csempe
2. **Testre szabhatja** – kerül üzemmód testreszabása

![Mozaik elrendezés testreszabása](./media/azure-portal-dashboards/customize-tile.png)

Kiválasztásával testreszabása, méretezze át, és újra sorrendbe állítja a csempéket. tooresize csempére, válassza hello hello környezetfüggő menüből, új méret, ahogy az a következő kép hello.

![Automatikus oszlopszélesség csempe](./media/azure-portal-dashboards/resize-tile.png)

Vagy ha hello csempe bármilyen méretű támogatja, húzva hello alsó jobb sarkában szükséges toohello méretét.

![Automatikus oszlopszélesség csempe](./media/azure-portal-dashboards/resize-corner.png)

Csempék átméretezése, után hello irányítópult megtekintéséhez.

![csempéje](./media/azure-portal-dashboards/view-tile.png)

Miután befejezte a testreszabása egy irányítópultot, egyszerűen válassza hello **végzett Testreszabás** tooexit testreszabási módban, vagy kattintson a jobb gombbal, és válassza ki **végzett Testreszabás** hello helyi menüből.

## <a name="publish-a-dashboard-and-manage-access-control"></a>Irányítópult közzétételét és kezelését a hozzáférés-vezérlés
Amikor létrehoz egy irányítópultot, saját alapértelmezés szerint azt jelenti, hogy hello csak ő is.  toomake azt látható tooothers hello használata **megosztás** mellett megjelenő gombra hello más irányítópult parancsok.

![Irányítópult megosztása](./media/azure-portal-dashboards/share-dashboard.png)

Az irányítópult toobe előfizetésbe és erőforráscsoportba csoport közzétett toochoose kell adnia. tooseamlessly Irányítópultok integrálása hello ökoszisztéma, azt korábban megosztott irányítópultok, megvalósítva Azure-erőforrások (tehát nem lehet megosztani, írja be az e-mail címet).  Hello csempék hello portálon többsége által megjelenített adatok eléréséhez toohello irányadók [Azure szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md). Access control szempontból megosztott irányítópultok ugyanazok a virtuális gép vagy egy tárfiókot.  

Tegyük fel, Azure-előfizetéssel rendelkezik, és a csoport tagjai hozzárendelt hello szerepkörök **tulajdonos**, **közreműködő**, vagy **olvasó** hello előfizetés.  Tulajdonos vagy közreműködő szerepkörrel rendelkező személyek felhasználók képes toolist, nézet, létrehozása, módosítása, vagy törölje az adott előfizetésen belül irányítópultok.  Felhasználók, akik olvasók képes toolist és nézet irányítópultokat, amelyek nem, de módosíthatja vagy törölheti őket is.  Olvasási joggal rendelkező felhasználók képes toomake helyi szerkesztést tooa megosztott irányítópult, azonban nem tudja toopublish e módosítások hátsó toohello server találhatók.  Azonban tudják hello irányítópult saját használatra titkos másolatát.  Mivel egyes csempék hello irányítópult mindig, saját alapján megfelelnek hello erőforrások hozzáférés-vezérlési szabályok kényszerítéséhez.  

Kényelmi célokat szolgál, hello portal erőforráscsoportban irányítópultok helyétől minta felé hívott élmény útmutatók a közzétételi **irányítópultok**.  

![Irányítópult közzététele](./media/azure-portal-dashboards/publish-dashboard.png)

Másik lehetőségként toopublish irányítópult tooa adott erőforráscsoportot.  Az irányítópult hello hozzáférés-vezérlés hello hozzáférés-vezérlés hello erőforráscsoport megegyezik.  Felhasználók által kezelhető az erőforráscsoport hello erőforrások hozzáférés toohello irányítópultok is.

![Irányítópult tooresource csoport közzététele](./media/azure-portal-dashboards/publish-to-resource-group.png)

Az irányítópult közzététele után hello **megosztás + hozzáférés** Vezérlőpulton frissítse, és azt hello közzétett irányítópulton, beleértve a hivatkozás toomanage felhasználói hozzáférés toohello irányítópult információkat jelenít meg.  Ez a hivatkozás indít hello szabványos szerepköralapú hozzáférés-vezérlés használt panel toomanage hozzáférést az Azure-erőforrásokkal.  Mindig kaphat vissza toothis nézet kijelölése **megosztás**.

![hozzáférés-vezérlés kezelése](./media/azure-portal-dashboards/manage-access.png)

## <a name="next-steps"></a>Következő lépések
* toomanage erőforrások, lásd: [kezelése Azure-erőforrások portálon keresztül](../azure-resource-manager/resource-group-portal.md).
* toodeploy erőforrások, lásd: [erőforrások a Resource Manager-sablonok és az Azure-portál telepítése](../azure-resource-manager/resource-group-template-deploy-portal.md).

