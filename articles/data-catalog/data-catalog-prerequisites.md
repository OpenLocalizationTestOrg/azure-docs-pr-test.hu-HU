---
title: "aaaAzure Data Catalog előfeltételei |} Microsoft Docs"
description: "További tudnivalók az Azure Data Catalog használatába tooget kell hello előfeltételek."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: ef497a54-dc4d-4820-b5bf-c361b64b964d
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 0c8e768e5846c61b542b746d7ad80121725a9ec7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-catalog-prerequisites"></a>Az Azure Data Catalog előfeltételei

Mielőtt Azure Data Catalog állíthat be kell tootake néhány dolgot kiszolgálásához. Ne aggódjon, a folyamat nem tart sokáig.

## <a name="azure-subscription"></a>Azure-előfizetés
tooset mentése a Data Catalog hello tulajdonosa vagy egy Azure-előfizetés társtulajdonos kell lennie.

Azure-előfizetések megkönnyítik például a Data Catalog toocloud-szolgáltatás erőforrások eléréséhez. Előfizetések is segíthetnek, erőforrás-használat jelentett, számlázva és kifizette vezérlésére. Minden előfizetés van külön számlázási és a fizetési telepítő, így előfizetések és a csomagok, amelyek módosítják a részleg, a projekt, a regionális office és az stb. Minden felhőalapú szolgáltatás tooa előfizetéshez tartozik, és meg kell toohave előfizetés, a Data Catalog beállítása előtt. több, lásd: toolearn [kezelhetők a fiókok, előfizetések és rendszergazdai szerepkörök](../active-directory/active-directory-assign-admin-roles.md).

## <a name="azure-active-directory"></a>Azure Active Directory
tooset mentése a Data Catalog, be kell jelentkeznie be egy Azure Active Directory (Azure AD) felhasználói fiókkal.

Az Azure AD az üzleti toomanage identitás és hozzáférés, egyszerre hello felhőben és helyszíni egyszerű módszert biztosít. Egyszeri bejelentkezés tooany felhő felhasználók használja egy egyetlen munkahelyi vagy iskolai fiókkal, és a helyszíni webes alkalmazás. A Data Catalog használja az Azure AD tooauthenticate bejelentkezés. több, lásd: toolearn [Mi az Azure Active Directory?](../active-directory/active-directory-whatis.md).

> [!NOTE]
> Hello segítségével [Azure-portálon](http://portal.azure.com/), jelentkezhet be személyes Microsoft-fiókkal vagy egy Azure Active Directory munkahelyi vagy iskolai fiók. használatával a Data Catalog mentése tooset vagy hello Azure-portálon vagy hello [Data Catalog-portál](http://www.azuredatacatalog.com), Azure Active Directory-fiókkal, nem egy személyes fiókkal kell bejelentkeznie.
>
>

## <a name="active-directory-policy-configuration"></a>Az Active Directory-házirend konfigurálása
Olyan helyzet ahol bejelentkezhet toohello Data Catalog-portált, de az adatforrás-regisztráló eszköz toohello toosign tett kísérlet során előforduló, észlel olyan hibaüzenetet, amely megakadályozza, hogy jelentkezik be. Ez a probléma a probléma akkor fordulhat elő, csak akkor, ha az Ön hello vállalati hálózaton található, vagy Ez akkor fordulhat elő, csak ha csatlakoztat külső hello vállalati hálózatról.

hello adatforrás-regisztráló eszköz használja az űrlapos hitelesítés toovalidate a felhasználói hitelesítő adatait az Active Directoryban. toohelp a bejelentkezés sikeres, az Active Directory-rendszergazda kell engedélyezheti az űrlapos hitelesítést a globális hitelesítési házirend hello.

Hitelesítési módszerek hello globális hitelesítési házirend, ahogy az alábbi képernyőfelvétel a hello is lehet külön-külön engedélyezni, az intranetes és extranetes kapcsolatok esetén. Bejelentkezési hiba is felléphet, ha a hello hálózati kapcsolat, amelyből nincs engedélyezve az űrlapos hitelesítés.

 ![Az Active Directory globális hitelesítési házirend](./media/data-catalog-prerequisites/global-auth-policy.png)

## <a name="next-steps"></a>Következő lépések
További információkért lásd: [hitelesítési házirendek konfigurálása](https://technet.microsoft.com/library/dn486781.aspx).
