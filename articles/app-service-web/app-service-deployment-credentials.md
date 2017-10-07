---
title: "App Service üzembe helyezési hitelesítő adatok aaaAzure |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure App Service üzembe helyezési hitelesítő adatokat."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/05/2016
ms.author: dariagrigoriu
ms.openlocfilehash: d6f9f5cc1b62a17c42643266f4c9490f827c63f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a>Telepítési hitelesítő adatok beállítása az Azure App Service
[Az Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) támogatja a hitelesítő adatok kétféle [helyi Git-telepítésének](app-service-deploy-local-git.md) és [FTP/S telepítési](app-service-deploy-ftp.md). A rendszer nem hello ugyanaz, mint az Azure Active Directorybeli hitelesítő adatokat.

* **Felhasználói szintű hitelesítő adatokat**: egy készletét hello teljes Azure-fiók hitelesítő adatait. A bármely alkalmazás bármely előfizetés, amely az Azure-fiók hello van engedélye tooaccess használt toodeploy tooApp szolgáltatás lehet. Ezek a hello alapértelmezett hitelesítő adatok beállítása, amelyet megadtak **alkalmazásszolgáltatások** > **&lt;alkalmazás_neve >** > **üzembe helyezési hitelesítő adatok**. Ez az alapértelmezés szerint, amely az illesztett hello portálon grafikus felhasználói Felülettel is hello (például hello **áttekintése** és **tulajdonságok** az alkalmazás [erőforráspanelen](../azure-resource-manager/resource-group-portal.md#manage-resources)).

    > [!NOTE]
    > Delegálja a szerepköralapú hozzáférés vezérlés (RBAC) vagy társadminisztrátornak engedélyek tooAzure erőforrások eléréséhez, ha minden Azure felhasználói, amely megkapja a hozzáférési tooan alkalmazás képes személyes felhasználói szintű hitelesítő adatokat használhatna, amíg hozzáférését visszavonja. A központi telepítési hitelesítő adatokat nem lehet megosztva Azure másokkal.
    >
    >

* **Alkalmazási szintű hitelesítő adatokat**: hitelesítő adatok az egyes alkalmazásokhoz egy készletét. Lehet, hogy a használt toodeploy toothat alkalmazás hátterére van hatással. hello hitelesítő adatokat az egyes alkalmazások a alkalmazás létrehozásakor automatikusan létrejön, és hello alkalmazásban található közzétételi profil. Nem konfigurálhatja manuálisan hello hitelesítő adatokat, de visszaállíthatja őket egy olyan alkalmazáshoz bármikor.

    > [!NOTE]
    > A sorrend toogive valaki hozzáférési toothese hitelesítő adatokat keresztül szerepköralapú hozzáférés vezérlés (RBAC), toomake kell őket közreműködő vagy nagyobb hello Web App. Olvasók toopublish nem engedélyezett, és ezért nem tudja elérni ezeket a hitelesítő adatokat.
    >
    >

## <a name="userscope"></a>Állítsa be, és a felhasználói szintű hitelesítő adatok alaphelyzetbe állítása

Konfigurálhatja a felhasználói szintű hitelesítő adatokat a bármely alkalmazás [erőforráspanelen](../azure-resource-manager/resource-group-portal.md#manage-resources). Attól függetlenül történik melyik alkalmazás konfigurálja ezeket a hitelesítő adatokat, tooall alkalmazások vonatkozik, és az Azure-előfizetések minden fiókot. 

tooconfigure a felhasználói szintű hitelesítő adatait:

1. A hello [Azure-portálon](https://portal.azure.com), kattintson az App Service >  **&lt;any_app >** > **üzembe helyezési hitelesítő adatok**.

    > [!NOTE]
    > Hello portálon rendelkeznie kell legalább egy alkalmazás eléréséhez hello telepítési hitelesítő adatok panelt. Azonban a hello [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md), konfigurálhatja a felhasználói szintű hitelesítő adatok nélkül egy meglévő alkalmazást.

2. Konfigurálja a hello felhasználónevét és jelszavát, és kattintson **mentése**.

    ![](./media/app-service-deployment-credentials/deployment_credentials_configure.png)

Az üzembe helyezési hitelesítő adatok megadása után hello található *Git* az alkalmazás központi telepítési felhasználónév **áttekintése**,

![](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

és és *FTP* az alkalmazás központi telepítési felhasználónév **tulajdonságok**.

![](./media/app-service-deployment-credentials/deployment_credentials_properties.png)

> [!NOTE]
> Azure nem jeleníti meg a felhasználói szintű telepítési jelszavát. Ha hello jelszavát, nem lehet beolvasni. Azonban, alaphelyzetbe állíthatja a hitelesítő adatok hello ebben a szakaszban ismertetett lépéseket követve.
>
>  

## <a name="appscope"></a>Első és az alkalmazási szintű hitelesítő adatok alaphelyzetbe állítása
Minden alkalmazás az App Service-ben, az alkalmazási szintű tárolt hitelesítő adatok vannak hello XML közzétételi profil.

tooget hello alkalmazási szintű hitelesítő adatokat:

1. A hello [Azure-portálon](https://portal.azure.com), kattintson az App Service >  **&lt;any_app >** > **áttekintése**.

2. Kattintson a **... További** > **Get közzétételi profil**, és elindítja a letöltési egy. PublishSettings-fájl.

    ![](./media/app-service-deployment-credentials/publish_profile_get.png)

3. Nyissa meg hello. PublishSettings fájlt, és megkeresi hello `<publishProfile>` hello attribútummal rendelkező címke `publishMethod="FTP"`. Ezt követően az beszerzése a `userName` és `password` attribútumok.
Ezek a hello alkalmazási szintű hitelesítő adatokat.

    ![](./media/app-service-deployment-credentials/publish_profile_editor.png)

    Hasonló toohello felhasználói szintű hitelesítő adatokat, hello FTP telepítési felhasználónév hello formátumban kell `<app_name>\<username>`, és csak akkor lesz hello Git telepítési felhasználónév `<username>` nélkül előző hello `<app_name>\`.

tooreset hello alkalmazási szintű hitelesítő adatokat:

1. A hello [Azure-portálon](https://portal.azure.com), kattintson az App Service >  **&lt;any_app >** > **áttekintése**.

2. Kattintson a **... További** > **a közzétételi profil alaphelyzetbe állítása**. Kattintson a **Igen** tooconfirm hello alaphelyzetbe állítása.

    a korábban letöltött hello a reset művelet érvénytelenné válik. PublishSettings-fájlok.

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogyan toouse ezen hitelesítő adatok toodeploy az alkalmazását [helyi Git](app-service-deploy-local-git.md) vagy [FTP/S](app-service-deploy-ftp.md).
