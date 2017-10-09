---
title: "aaaManaging Azure Cloud Explorer erőforrások |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Cloud Explorer toobrowse és a Visual Studio Azure-erőforrások kezeléséhez."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 6347dc53-f497-49d5-b29b-e8b9f0e939d7
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/25/2017
ms.author: kraigb
ms.openlocfilehash: 8a81660074d5d04be063df9e25076b7a97586514
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hello-resources-associated-with-your-azure-accounts-in-visual-studio-cloud-explorer"></a>A Visual Studio Cloud Explorer Azure fiókokhoz tartozó hello erőforrások kezelése
Cloud Explorer lehetővé teszi, hogy Ön tooview az Azure-erőforrások és csoportok, vizsgálja meg az tulajdonságait, és a műveleteket kulcs fejlesztői diagnosztika a Visual Studio. 

Például a hello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040), Cloud Explorer épül hello Azure Resource Manager-készletben. Ezért Cloud Explorer megértette erőforrások, például Azure erőforráscsoport-sablonok és a Logic Apps alkalmazásokat és API-alkalmazások például az Azure-szolgáltatásokat, valamint a [szerepköralapú hozzáférés-vezérlés](active-directory/role-based-access-control-configure.md) (RBAC). 

## <a name="prerequisites"></a>Előfeltételek
- [Visual Studio 2017](https://www.visualstudio.com/downloads/) a hello **Azure számítási** kiválasztva, vagy a Visual Studio korábbi verzióját a hello [Microsoft Azure SDK for .NET 2.9](https://www.microsoft.com/en-us/download/details.aspx?id=51657).
- A Microsoft Azure-fiók – Ha nincs fiókja, akkor [regisztráljon egy ingyenes próbaverzióra](http://go.microsoft.com/fwlink/?LinkId=623901) vagy [aktiválhatja a Visual Studio előfizetői előnyeit](http://go.microsoft.com/fwlink/?LinkId=623901).

> [!NOTE]
> Válassza ki a Cloud Explorer tooview **nézet** > **Cloud Explorer** hello menüsávjában.   
> 
> 

## <a name="add-an-azure-account-toocloud-explorer"></a>Egy Azure-fiók tooCloud Explorer hozzáadása
tooview hello erőforrások társított Azure-fiókot, először hozzá kell hello fiók tooCloud Explorer. 

1. A **Cloud Explorer**, jelölje be **Azure-fiók beállításai**.

    ![Cloud Explorer Azure fiók beállításainak ikonja](media/vs-azure-tools-resources-managing-with-cloud-explorer/azure-account-settings.png)

1. Válassza ki **új fiókot felveszi**. 

    ![Cloud Explorer-fiók hozzáadása hivatkozás](media/vs-azure-tools-resources-managing-with-cloud-explorer/add-account-link.png)

1. Jelentkezzen be Azure-fiók azt szeretné, amelynek erőforrásait toohello toobrowse. 

1. Miután bejelentkezett az Azure-fiók tooan, azzal a fiókkal társított hello előfizetések jeleníti meg. Válassza ki a kívánt toobrowse, és válassza ki hello fiók-előfizetések jelölőnégyzeteit hello **alkalmaz**. 
 
    ![Cloud Explorer: válassza ki az Azure-előfizetések toodisplay](media/vs-azure-tools-resources-managing-with-cloud-explorer/select-subscriptions.png)

1. Azt szeretné, amelynek erőforrásait hello előfizetések kiválasztása után toobrowse, ezen előfizetések és az erőforrások megjelenítése hello Cloud Explorer.

    ![Cloud Explorer erőforrás Azure-fiókot listázása](media/vs-azure-tools-resources-managing-with-cloud-explorer/resources-listed.png)

## <a name="remove-an-azure-account-from-cloud-explorer"></a>Távolítsa el az Azure-fiók Cloud Explorer 

1. A **Cloud Explorer**, jelölje be **Azure-fiók beállításai**.

    ![Cloud Explorer Azure fiók beállításainak ikonja](media/vs-azure-tools-resources-managing-with-cloud-explorer/azure-account-settings.png)

1. Válassza ki a következő toohello fiók tooremove, **eltávolítása**.

    ![Cloud Explorer Azure fiók beállításainak ikonja](media/vs-azure-tools-resources-managing-with-cloud-explorer/remove-account.png)

## <a name="view-resource-types-or-resource-groups"></a>Erőforrástípus vagy erőforráscsoportok megtekintésére
tooview az Azure-erőforrások, dönthet úgy, vagy **erőforrástípusok** vagy **erőforráscsoportok** nézet.

1. A **Cloud Explorer**, válassza ki az erőforrás nézet legördülő lista hello.

    ![Cloud Explorer legördülő lista tooselect hello szükséges erőforrások megtekintése](media/vs-azure-tools-resources-managing-with-cloud-explorer/resources-view-dropdown.png)

1. Hello helyi menüből válassza ki a kívánt hello nézetet: 

    - **Erőforrástípusok** nézet – hello közös nézet hello használt [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040), az Azure-erőforrások kategóriába sorolt, például webes alkalmazásokat, a storage-fiókok és a virtuális gépek típus szerint jeleníti meg. 
    - **Erőforráscsoportok** - hello Azure erőforráscsoport, amellyel fontosságúak társított szerint kategorizálja Azure-erőforrások megtekintése. Egy erőforráscsoportot az Azure-erőforrások, általában egy adott alkalmazás által használt csomag egyik gyermekszoftver. További információ az Azure erőforráscsoport-sablonok, toolearn lásd [Azure Resource Manager áttekintése](./azure-resource-manager/resource-group-overview.md).

    hello következő kép bemutatja hello összehasonlítása két erőforrás nézeteinek társítása:

    ![Cloud Explorer erőforrás nézetek összehasonlítása](media/vs-azure-tools-resources-managing-with-cloud-explorer/resource-views-comparison.png)

## <a name="view-and-navigate-resources-in-cloud-explorer"></a>Megtekintheti, és keresse meg a Cloud Explorer erőforrások
Azure-erőforrás toonavigate tooan és megtekintse az adataikat a Cloud Explorer, bontsa ki a hello elem típusa vagy tartozó erőforráscsoport, és válassza a hello erőforrás. Erőforrás kiválasztásakor információk megjelennek-e a két hello lapok - **műveletek** és **tulajdonságok** - Cloud Explorer hello alján. 

- **Műveletek** lapon - listák hello elvégezhető műveletekhez nyújtanak a Cloud Explorer hello kiválasztott erőforrás. Is megtekintheti ezeket a beállításokat kattintson a jobb gombbal a hello erőforrás tooview a helyi menüből.

- **Tulajdonságok** lapon - hello tulajdonságainak megjelenítése hello erőforrás, például a típusa, területi beállítás és az erőforrás csoport, amelyhez társítva.

hello alábbi ábrán láthatók az egyes lapokon az egy App Service egy példa összehasonlítása:

![](./media/vs-azure-tools-resources-managing-with-cloud-explorer/actions-and-properties.png)

Minden erőforrás van hello művelet **nyissa meg a portál**. Amikor úgy dönt, hogy ez a művelet, Cloud Explorer jeleníti meg hello kiválasztott erőforrás hello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040). Hello **nyissa meg a portál** szolgáltatás toodeeply beágyazott erőforrások navigáláshoz lesz szüksége.

További műveletek és a tulajdonságértékek is megjelenhetnek hello Azure erőforráscsoport alapján. Például webes alkalmazásokat és a logic apps is hello műveletek **nyissa meg böngészőben** és **Hibakereső csatlakoztatása** továbbá túl**nyissa meg a portál**. Műveletek tooopen szerkesztők jelennek meg, ha úgy dönt, hogy a tárolási fiók blob, üzenetsor vagy tábla. Az Azure-alkalmazások is rendelkeznek **URL-cím** és **állapot** tulajdonságok, míg a tárolási erőforrások kulcs és a kapcsolati karakterlánc tulajdonságait.

## <a name="find-resources-in-cloud-explorer"></a>A Cloud Explorer erőforrások keresése
a megadott név megadásával az Azure-fiók előfizetések toolocate erőforrások adja meg a hello nevét, a hello **keresési** Cloud Explorer párbeszédpanel.

![A Cloud Explorer erőforrások keresése](./media/vs-azure-tools-resources-managing-with-cloud-explorer/search-for-resources.png)

Hello karakter beírásakor **keresési** mezőbe csak azok a karakterek megfelelő erőforrások jelennek meg a hello erőforrások fájában.
