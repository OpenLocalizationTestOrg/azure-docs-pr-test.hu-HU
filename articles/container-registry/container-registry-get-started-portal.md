---
title: "aaaCreate titkos Docker beállításjegyzék - Azure-portál |} Microsoft Docs"
description: "Első lépések, létrehozását és kezelését a személyes Docker-tároló nyilvántartó, hello Azure-portálon"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: 53a3b3cb-ab4b-4560-bc00-366e2759f1a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 40f3ce44fea26e5fbeca865c9da6df55c2df9511
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-private-docker-container-registry-using-hello-azure-portal"></a>Hozza létre a titkos Docker tároló beállításkulcs hello Azure-portál használatával
Az Azure portál toocreate tároló beállításjegyzékbeli hello használatát, és a beállítások kezelését. Is hozhat létre, és hello segítségével kezeléséhez tároló [Azure CLI 2.0 parancsok](container-registry-get-started-azure-cli.md), [Azure PowerShell](container-registry-get-started-powershell.md) vagy programozott módon hello tároló beállításjegyzék [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).

Háttér és fogalmak: [áttekintése hello](container-registry-intro.md).

## <a name="create-a-container-registry"></a>Tároló-beállításjegyzék létrehozása
1. A hello [Azure-portálon](https://portal.azure.com), kattintson a **+ új**.
2. Hello a piactéren **Azure tároló beállításjegyzék**.
3. Válassza ki az **Azure Container Registry-t**, amelynek közzétevője a **Microsoft**.
    ![Container Registry szolgáltatás az Azure Marketplace-en](./media/container-registry-get-started-portal/container-registry-marketplace.png)
4. Kattintson a **Create** (Létrehozás) gombra. Hello **Azure tároló beállításjegyzék** panel jelenik meg.

    ![A tároló-beállításjegyzékek beállításai](./media/container-registry-get-started-portal/container-registry-settings.png)
5. A hello **Azure tároló beállításjegyzék** panelen adja meg a következő információ hello. Amikor elkészült, kattintson a **Létrehozás** gombra.

    a. **Beállításjegyzék neve**: egy globálisan egyedi legfelső szintű tartománynév az adott beállításjegyzékhez. Ebben a példában hello beállításkulcs értéke *myRegistry01*, de helyettesítse a saját, egyedi nevét. hello név csak betűket és számokat tartalmazhat.

    b. **Erőforráscsoport**: Válasszon ki egy létező [erőforráscsoport](../azure-resource-manager/resource-group-overview.md#resource-groups) vagy egy új hello típusnév.

    c. **Hely**: válassza ki az Azure-adatközpontban helyet, ahol hello szolgáltatás [elérhető](https://azure.microsoft.com/regions/services/), például a **déli középső Régiójában**.

    d. **A rendszergazdai jogú felhasználó**: Ha azt szeretné, egy rendszergazda felhasználó tooaccess hello beállításjegyzék engedélyezése. Ez a beállítás hello beállításkulcs létrehozása után módosíthatja.

      > [!IMPORTANT]
      > Ezenkívül tooproviding rendszergazdai felhasználói fiókkal keresztül férnek hozzá, a tároló nyilvántartó támogatja az Azure Active Directory szolgáltatás rendszerbiztonsági tagok által támogatott hitelesítési. További információkat és szempontokat [a tároló-beállításjegyzékkel való hitelesítéssel kapcsolatos cikkben](container-registry-authentication.md) találhat.
      >

    e. **A tárfiók**: hello alapértelmezett beállítás toocreate használja egy [tárfiók](../storage/common/storage-introduction.md), vagy jelöljön ki egy meglévő tárfiók hello ugyanazon a helyen. A Premium Storage jelenleg nem támogatott.

## <a name="manage-registry-settings"></a>Beállításjegyzék beállításainak kezelése
Miután létrehozta a hello beállításjegyzék, hello beállításjegyzék-beállítások szerinti keresése hello kezdődő **tároló nyilvántartó** hello portál panel. Például előfordulhat, hogy hello beállítások toolog tooyour beállításjegyzékben kell, vagy előfordulhat, hogy szeretné, hogy tooenable, vagy tiltsa le a hello rendszergazdai jogú felhasználó.

1. A hello **tároló nyilvántartó** panelen kattintson a beállításjegyzék hello nevére.

    ![Tároló-beállításjegyzék panel](./media/container-registry-get-started-portal/container-registry-blade.png)
2. toomanage hozzáférési beállításait, kattintson a **hozzáférési kulcs**.

    ![Hozzáférés a tároló-beállításjegyzékhez](./media/container-registry-get-started-portal/container-registry-access.png)
3. Vegye figyelembe a következő beállítások hello:

   * **Bejelentkezési kiszolgáló** -hello teljesen minősített név toolog toohello beállításjegyzékben használja. Ebben a példában ez `myregistry01.azurecr.io`.
   * **A rendszergazdai jogú felhasználó** - tooenable bekapcsolására, vagy tiltsa le a hello beállításjegyzék rendszergazda felhasználói fiókot.
   * **Felhasználónév** és **jelszó** -hello hello rendszergazdai felhasználói fiók hitelesítő adatait (Ha engedélyezve van) toolog toohello beállításjegyzék használhatja. Opcionálisan újragenerálás hello jelszavakat. Két jelszavak jönnek létre, így fenntarthatja a kapcsolatokat toohello beállításjegyzék hello újragenerálja egy jelszó segítségével más jelszót. az egyszerű szolgáltatás tooauthenticate helyett, lásd: [hitelesítés egy titkos Docker-tároló rendszerleíró](container-registry-authentication.md).

## <a name="next-steps"></a>Következő lépések
* [Leküldéses az első kép hello Docker parancssori felület használatával](container-registry-get-started-docker-cli.md)
