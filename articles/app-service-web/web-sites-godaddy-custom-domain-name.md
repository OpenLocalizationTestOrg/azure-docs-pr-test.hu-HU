---
title: "az Azure App Service (GoDaddy) egy egyéni tartománynevet aaaConfigure"
description: "Ismerje meg, hogyan toouse a tartomány nevére az Azure Web Apps GoDaddy"
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
ms.openlocfilehash: 48158ee752f9833249bbf85adf80f572d1c68486
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-custom-domain-name-in-azure-app-service-purchased-directly-from-godaddy"></a>Egyéni (közvetlenül a GoDaddytől vásárolt) tartománynév konfigurálása az Azure App Service-ben
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro.md)]

Ha a tartomány Azure App Service Web Apps keresztül vásárolt, majd tekintse meg az utolsó lépésében toohello [tartomány vásárlása Web Apps](custom-dns-web-site-buydomains-web-app.md).

Ez a cikk útmutatás segítségével egy egyéni tartománynevet, közvetlenül a megvásárolt [GoDaddy](https://godaddy.com) rendelkező [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a>DNS-rekordok ismertetése
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-raw.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a>Az egyéni tartomány DNS-rekord hozzáadása
tooassociate egy webalkalmazást az App Service szolgáltatásban az egyéni tartományt, hozzá kell adnia egy új bejegyzést hello DNS-tábla az egyéni tartomány GoDaddy által biztosított eszközök segítségével. Következő lépések toolocate hello DNS-eszközök a GoDaddy.com hello használata

1. Jelentkezzen be GoDaddy.com tooyour fiókot, és válassza ki **saját fiók** , majd **a tartományok kezelése**. Jelölje be hello legördülő menü hello tartománynév toouse kívánja az Azure-webalkalmazásban a, és válassza ki a **kezelése DNS**.
   
    ![GoDaddy tartozó egyéni tartomány lap](./media/web-sites-godaddy-custom-domain-name/godaddy-customdomain.png)
2. A hello **tartományhoz részleteinek** párbeszédpanelen görgessen toohello **DNS-zónafájlját** fülre. Ez a hello szakasz hozzáadása és módosítása a tartománynévhez tartozó DNS-rekordokat.
   
    ![DNS-zónafájlját lap](./media/web-sites-godaddy-custom-domain-name/godaddy-zonetab.png)
   
    Válassza ki **rekord hozzáadása** tooadd meglévő bejegyzés.
   
    túl**szerkesztése** egy létező bejegyzést, jelölje be hello toll & papír ikon hello bejegyzés mellett.
   
   > [!NOTE]
   > Új rekord hozzáadása előtt vegye figyelembe, hogy a GoDaddy már hozott létre DNS-rekordjainak a népszerű altartományok (nevű **állomás** -szerkesztőben), mint **e-mail**, **fájlok**, **mail**, és mások számára. Ha hello neve toouse kívánja már létezik, módosítsa a hello meglévő rekord létrehozása egy új helyett.
   > 
   > 
3. A rekord felvételekor hello rekordtípus először ki kell választania.
   
    ![Válassza ki a rekord típusa](./media/web-sites-godaddy-custom-domain-name/godaddy-selectrecordtype.png)
   
    Ezután meg kell adnia hello **állomás** (egyéni tartomány és altartomány hello) és amit a rendszergazda **mutat**.
   
    ![zóna rekord hozzáadása](./media/web-sites-godaddy-custom-domain-name/godaddy-addzonerecord.png)
   
   * Hozzáadásakor egy **(állomás) rekord** -meg kell adni a hello **állomás** mező tooeither  **@**  (gyökértartomány neve, mint például jelképez  **a contoso.com**,) * (több altartományok egyeztetéséhez helyettesítő) vagy hello altartomány toouse kívánja (például **www**.) Meg kell adni a hello **mutat** toohello IP-címét az Azure-webalkalmazásban mezőben.
   * Hozzáadásakor egy **(aliast) CNAME rekord** -meg kell adni a hello **állomás** mező toohello altartomány toouse kívánja. Például **www**. Meg kell adni a hello **mutat** mező toohello **. azurewebsites.net** tartománynevét, az Azure-webalkalmazásban. Például **contoso.azurewebsites.net**.
4. Kattintson a **hozzáadása egy másik**.
5. Válassza ki **TXT** hello rekord típusa, majd adja meg a **állomás** értékének  **@**  és egy **mutat** értékének  **&lt;yourwebappname&gt;. azurewebsites.net**.
   
   > [!NOTE]
   > A TXT-rekord hello rekord vagy hello első TXT rekord szerint hello tartomány saját Azure toovalidate használják. Miután hello tartományhoz már csatlakoztatott toohello webalkalmazás hello Azure portálon, a TXT-rekord bejegyzés lehet eltávolítani.
   > 
   > 
6. Hozzáadása után, vagy ha a bejegyzések módosítása, kattintson a **Befejezés** toosave módosításokat.

<a name="enabledomain"></a>

## <a name="enable-hello-domain-name-on-your-web-app"></a>A webalkalmazásban hello tartománynév engedélyezése
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-web-site.md)]

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="whats-changed"></a>A változások
* Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

