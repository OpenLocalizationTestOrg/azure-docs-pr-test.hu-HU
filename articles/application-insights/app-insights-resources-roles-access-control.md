---
title: "Azure Application Insights aaaResources, szerepkörök és hozzáférés-vezérlés |} Microsoft Docs"
description: "Tulajdonosai, közreműködő szerepkörrel rendelkező személyek és a szervezet insights olvasói."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 49f736a5-67fe-4cc6-b1ef-51b993fb39bd
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: bwren
ms.openlocfilehash: a6f6ca0443b5f60239f094606e124f856967d8ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resources-roles-and-access-control-in-application-insights"></a>Erőforrások, szerepkörök és hozzáférés-vezérlés az Application Insightsban
Szabályozhatja, ki rendelkezik-e olvasási és Azure access tooyour adatok frissítése [Application Insights][start], használatával [Microsoft Azure szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md).

> [!IMPORTANT]
> Rendelje hozzá a hello hozzáférés toousers **erőforráscsoportba vagy előfizetésbe** toowhich az alkalmazás erőforrás tartozik - nem a hello erőforrás magát. Rendelje hozzá a hello **Application Insights-összetevővel kapcsolatos közreműködői** szerepkör. Ez biztosítja a hozzáférés tooweb tesztek és a riasztások mellett az alkalmazás erőforrás egységes irányítását. [További információk](#access).
> 
> 

## <a name="resources-groups-and-subscriptions"></a>Erőforrások, csoportok és előfizetések
Első, néhány definíciók:

* **Erőforrás** - egy Microsoft Azure service-példányhoz. Az Application Insights-erőforrás gyűjti, elemzi és az alkalmazásból küldött hello telemetriai adatokat jeleníti meg.  Más típusú Azure-erőforrások közé tartoznak a webalkalmazások, adatbázisok és virtuális gépek.
  
    toosee az erőforrásokat, nyissa meg a hello [Azure Portal][portal], jelentkezzen be, majd kattintson az összes erőforrást. toofind egy erőforrás típusa hello szűrőmező nevének egy részét.
  
    ![Azure-erőforrások listája](./media/app-insights-resources-roles-access-control/10-browse.png)

<a name="resource-group"></a>

* [**Erőforráscsoport** ] [ group] -minden erőforrás tooone csoport tartozik. Egy csoport egy kényelmes módot toomanage kapcsolódó erőforrások, különösen a hozzáférés-vezérléshez. Például egy erőforrást a csoport egy webalkalmazást, az Application Insights-erőforrás toomonitor hello alkalmazások és a Storage erőforrás tookeep ütemezésbe helyezheti exportált adatok.

    ![A Tallózás, erőforráscsoportok, majd válasszon ki egy csoportot](./media/app-insights-resources-roles-access-control/11-group.png)

* [**Előfizetés** ](https://manage.windowsazure.com) -toouse Application Insights vagy más Azure-erőforrások, jelentkezzen be Azure-előfizetés tooan. Minden erőforráscsoport tooone Azure-előfizetéssel, ahol az ár csomag kiválasztása és, ha egy szervezet előfizetés válasszon hello tagok és a hozzáférési engedélyeik tartozik.
* [**Microsoft-fiók** ] [ account] -felhasználónév és jelszó toosign használhatja a tooMicrosoft Azure hello előfizetések, XBox Live-, Outlook.com-os és más Microsoft-szolgáltatásokban.

## <a name="access"></a>Hello erőforráscsoportban elérés
Fontos, hogy hozzáadása toohello erőforrás létrehozta az alkalmazáshoz, nincsenek is külön riasztások és a webalkalmazás-tesztek rejtett erőforrások toounderstand. -E csatolt toohello azonos [erőforráscsoport](#resource-group) az alkalmazás. Előfordulhat, hogy is vezettek be más Azure-szolgáltatásokat az ott, például webhelyekhez vagy tároló.

![Az Application Insightsban erőforrások](./media/app-insights-resources-roles-access-control/00-resources.png)

toocontrol toothese erőforrások eléréséhez ezért javasoljuk, hogy:

* Hozzáférés: hello **erőforráscsoportba vagy előfizetésbe** szintjét.
* Rendelje hozzá a hello **Application Insights-összetevővel kapcsolatos közreműködői** szerepkör toousers. Ez lehetővé teszi őket tooedit webes tesztjeinek használatát, a riasztások és az Application Insights-erőforrások hozzáférés tooany más szolgáltatások hello csoport megadása nélkül.

## <a name="tooprovide-access-tooanother-user"></a>tooprovide hozzáférés tooanother felhasználó
Tulajdonosi jogok toohello előfizetés vagy hello erőforráscsoportban kell rendelkeznie.

hello felhasználónak rendelkeznie kell egy [Microsoft Account][account], vagy nem érhető el tootheir [szervezeti Microsoft Account](../active-directory/sign-up-organization.md). Megadhatja a hozzáférés tooindividuals, valamint az Azure Active Directory toouser csoportok.

#### <a name="navigate-toohello-resource-group"></a>Keresse meg a toohello erőforráscsoport
Hiba a hello felhasználó hozzáadása.

![Az alkalmazás erőforrás panelén Essentials megnyitásához nyisson hello erőforráscsoportban, és nincs válassza ki a beállítások/felhasználókat. Kattintson az Add (Hozzáadás) parancsra.](./media/app-insights-resources-roles-access-control/01-add-user.png)

Vagy lépjen be egy másik szolgáltatásiszint és hello felhasználói toohello előfizetés hozzáadása sikerült.

#### <a name="select-a-role"></a>Szerepkörválasztás
![Válasszon egy hello új felhasználói szerepet](./media/app-insights-resources-roles-access-control/03-role.png)

| Szerepkör | Hello erőforráscsoportban található |
| --- | --- |
| Tulajdonos |Módosíthatja semmit, beleértve a felhasználói hozzáférés |
| Közreműködő |Bármi, beleértve az összes erőforrás szerkesztése |
| Application Insights-összetevővel kapcsolatos közreműködői |Az Application Insights erőforrásokat, webes tesztjeinek használatát, és a riasztások szerkesztése |
| Olvasó |Megtekintheti, de nem bármit módosíthat |

"Szerkesztés" magában foglalja a létrehozása, törlése és frissítése:

* Erőforrások
* Webteszt
* Riasztások
* Folyamatos exportálás

#### <a name="select-hello-user"></a>Hello felhasználó kiválasztása

Ha azt szeretné, hello felhasználói nem hello könyvtárban, felajánlhatja bárki, aki a Microsoft-fiók.
(Például Outlook.com, OneDrive, Windows Phone vagy XBox Live services használata, ha rendelkeznek a Microsoft-fiók.)

## <a name="related-content"></a>Kapcsolódó tartalom

* [Szerepköralapú hozzáférés-vezérlés az Azure-ban](../active-directory/role-based-access-control-configure.md)

<!--Link references-->

[account]: https://account.microsoft.com
[group]: ../azure-resource-manager/resource-group-overview.md
[portal]: https://portal.azure.com/
[start]: app-insights-overview.md
