---
title: "Egyéni tartománynév beállítása az Azure App Service (GoDaddy)"
description: "A GoDaddy tartomány nevét az Azure Web Apps használata"
services: app-service
documentationcenter: 
author: erikre
manager: erikre
editor: jimbe
ms.assetid: 33233e30-5846-488f-83f3-b32e5c114564
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2016
ms.author: cephalin
ms.openlocfilehash: 158c5dc06f83e16633d3c2fbb4eb27d3e8af030c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-custom-domain-name-in-azure-app-service-purchased-directly-from-godaddy"></a>Egyéni (közvetlenül a GoDaddytől vásárolt) tartománynév konfigurálása az Azure App Service-ben
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro.md)]

Ha tartományi Azure App Service Web Apps keresztül vásárolt, akkor tekintse meg az utolsó lépésében [tartomány vásárlása Web Apps](custom-dns-web-site-buydomains-web-app.md).

Ez a cikk útmutatás segítségével egy egyéni tartománynevet, közvetlenül a megvásárolt [GoDaddy](https://godaddy.com) rendelkező [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a>DNS-rekordok ismertetése
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-raw.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a>Az egyéni tartomány DNS-rekord hozzáadása
Az egyéni tartomány társítandó egy webalkalmazást az App Service-ben, hozzá kell adnia egy új bejegyzést a DNS-tábla az egyéni tartomány GoDaddy által biztosított eszközök segítségével. Az alábbi lépések segítségével keresse meg a DNS-eszközök a GoDaddy.com

1. Jelentkezzen be GoDaddy.com fiókját, és válassza ki **saját fiók** , majd **a tartományok kezelése**. Válassza ki a tartománynév, amely az Azure-webalkalmazásban használ, és válassza ki a legördülő menü **kezelése DNS**.
   
    ![GoDaddy tartozó egyéni tartomány lap](./media/web-sites-godaddy-custom-domain-name/godaddy-customdomain.png)
2. Az a **tartományhoz részleteinek** lapon, görgessen a **DNS-zónafájlját** fülre. Ez az a és a tartománynévhez tartozó DNS-rekordok módosításának használt szakasz.
   
    ![DNS-zónafájlját lap](./media/web-sites-godaddy-custom-domain-name/godaddy-zonetab.png)
   
    Válassza ki **rekord hozzáadása** hozzáadni egy meglévő rekordot.
   
    A **szerkesztése** egy létező bejegyzést, válassza ki a toll & papír a bejegyzés mellett.
   
   > [!NOTE]
   > Új rekord hozzáadása előtt vegye figyelembe, hogy a GoDaddy már hozott létre DNS-rekordjainak a népszerű altartományok (nevű **állomás** -szerkesztőben), mint **e-mail**, **fájlok**, **mail**, és mások számára. Ha létezik-e már a használni kívánt nevet, módosítsa az új létrehozása helyett a meglévő rekord.
   > 
   > 
3. Amikor felvesz egy bejegyzést, először ki kell választani a rekord típusát.
   
    ![Válassza ki a rekord típusa](./media/web-sites-godaddy-custom-domain-name/godaddy-selectrecordtype.png)
   
    Ezután meg kell adnia a **állomás** (az egyéni tartomány vagy altartomány) és amit a rendszergazda **mutat**.
   
    ![zóna rekord hozzáadása](./media/web-sites-godaddy-custom-domain-name/godaddy-addzonerecord.png)
   
   * Hozzáadásakor egy **(állomás) erőforrásrekord** -meg kell adni a **állomás** mező vagy  **@**  (gyökértartomány neve, mint például jelképez **contoso.com**,) * (több altartományok egyeztetéséhez helyettesítő) vagy a altartomány szeretné használni (például **www**.) Meg kell adni a **mutat** mezőről az Azure-webalkalmazásban IP-címét.
   * Hozzáadásakor egy **(aliast) CNAME rekord** -meg kell adni a **állomás** mezőről az altartomány szeretné használni. Például **www**. Meg kell adni a **mutat** mezőről az **. azurewebsites.net** tartománynevét, az Azure-webalkalmazásban. Például **contoso.azurewebsites.net**.
4. Kattintson a **hozzáadása egy másik**.
5. Válassza ki **TXT** , a rekordtípust, majd adja meg a **állomás** értékének  **@**  és egy **mutat** értékének  **&lt;yourwebappname&gt;. azurewebsites.net**.
   
   > [!NOTE]
   > A TXT-rekord segítségével Azure ellenőrizze a tartomány az A rekord vagy az első TXT-rekord saját. Miután a tartomány lett leképezve: a webalkalmazás az Azure portálon, a TXT-rekord bejegyzés lehet eltávolítani.
   > 
   > 
6. Hozzáadása után, vagy ha a bejegyzések módosítása, kattintson a **Befejezés** menti a módosításokat.

<a name="enabledomain"></a>

## <a name="enable-the-domain-name-on-your-web-app"></a>A tartomány nevét a webalkalmazásban engedélyezése
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-web-site.md)]

> [!NOTE]
> Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="whats-changed"></a>A változások
* Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)

