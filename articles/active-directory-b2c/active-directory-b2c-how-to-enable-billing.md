---
title: "egy Azure-előfizetés tooAzure AD B2C aaaHow tooLink |} Microsoft Docs"
description: "Részletes útmutató tooenable számlázási az Azure AD B2C bérlő számára az Azure-előfizetéssel."
services: active-directory-b2c
documentationcenter: dev-center-name
author: rojasja
manager: mbaldwin
ms.service: active-directory-b2c
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/05/2016
ms.author: joroja
ms.openlocfilehash: 07b2ed5f7f5f543c9cbb8e9a35107462448e9440
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="linking-an-azure-subscription-tooan-azure-b2c-tenant-toopay-for-usage-charges"></a>Egy Azure-előfizetés tooan Azure B2C bérlő toopay használati díjak csatolása

A folyamatban lévő használati díjak, az Azure Active Directory B2C (vagy az Azure AD B2C) számlázott tooan Azure-előfizetéshez. A hello Bérlői rendszergazda tooexplicitly hivatkozás hello Azure AD B2C bérlő tooan Azure-előfizetés maga hello B2C bérlő létrehozása után szükség.  Ez a hivatkozás érhető el, hozzon létre egy Azure AD "B2C-bérlő" erőforrás hello cél Azure-előfizetés. Sok B2C bérlő csatolt tooa egyetlen Azure-előfizetése más Azure-erőforrások (például virtuális gépeket, adatokat tároló, LogicApps) együtt is lehet.


> [!IMPORTANT]
> számlázási és a B2C jelenleg a következő lap hello tarifacsomag használati szolgáltatáscsomagjai hello: [az Azure AD B2C árazás](
https://azure.microsoft.com/pricing/details/active-directory-b2c/)

## <a name="step-1---create-an-azure-ad-b2c-tenant"></a>1. lépés – az Azure AD B2C bérlő létrehozása
B2C bérlő létrehozásakor először el kell végezni. Hagyja ki ezt a lépést, ha már létrehozta a cél B2C-bérlő. [Ismerkedés az Azure AD B2C-vel](active-directory-b2c-get-started.md)

## <a name="step-2---open-azure-portal-in-hello-azure-ad-tenant-that-shows-your-azure-subscription"></a>2. lépés – nyissa meg Azure portálra az Azure AD-bérlő bemutató az Azure-előfizetéshez hello
Keresse meg a toohello [Azure-portálon](https://portal.azure.com). Azure AD-bérlő hello Azure-előfizetéssel szeretné toouse megjelenítő toohello váltani. Az Azure AD-bérlő eltér hello B2C-bérlő. Belül hello Azure-portálon kattintson a hello fiók nevét a hello hello irányítópult tooselect hello Azure AD-bérlő jobb felső sarkában. Azure-előfizetés szükséges tooproceed. [Azure-előfizetés igénylése](https://account.windowsazure.com/signup?showCatalog=True)

![Azure AD-bérlő tooyour Váltás](./media/active-directory-b2c-how-to-enable-billing/SelectAzureADTenant.png)

## <a name="step-3---create-a-b2c-tenant-resource-in-azure-marketplace"></a>3. lépés - a B2C-bérlő erőforrás létrehozása az Azure piactéren
Piactér hello piactér ikonra kattintva, vagy jelöljön ki hello zöld "+" hello bal felső sarokban hello irányítópult megnyitásához.  Keresse meg és jelölje ki az Azure Active Directory B2C. Válassza ki a létrehozása.

![Válassza ki a piactér](./media/active-directory-b2c-how-to-enable-billing/marketplace.png)

![Keresési AD B2C](./media/active-directory-b2c-how-to-enable-billing/searchb2c.png)

hello Azure AD B2C-erőforrás létrehozása párbeszédpanel magában foglalja az hello a következő paramétereket:

1. Az Azure AD B2C-bérlő – hello legördülő listából válassza ki az Azure AD B2C-bérlő.  Csak jogosult az Azure AD B2C-bérlők megjelenítése.  Jogosult B2C-bérlők megfeleljen ezeknek a feltételeknek: hello hello B2C bérlő globális rendszergazdai jogosultságokkal, és hello B2C bérlő nincs jelenleg társított tooan Azure-előfizetés

2. Az Azure AD B2C erőforrásnév - paraméter előre kiválasztott toomatch hello hello B2C-bérlő tartományneve

3. Előfizetés - rendszergazdaként vagy társadminisztrátorának aktív Azure-előfizetéssel.  Lehetséges, hogy a több Azure AD B2C bérlő tooone Azure-előfizetéshez hozzáadva

4. Erőforráscsoport és az erőforráscsoport helye – Ez az összetevő segítségével több Azure-erőforrások.  Ez a beállítás nincs hatással van a B2C bérlő helyét, a teljesítmény vagy a számlázási állapota

5. PIN-kód legegyszerűbb hozzáférés tooyour B2C bérlő számlázási információkat és hello B2C bérlő beállítások toodashboard ![B2C erőforrás létrehozása](./media/active-directory-b2c-how-to-enable-billing/createresourceb2c.png)

## <a name="step-4---manage-your-b2c-tenant-resources-optional"></a>4. lépés - a B2C-bérlő (nem kötelező) erőforrások kezelése
Központi telepítés befejezése után, a "B2C-bérlő" új erőforrás hello cél erőforráscsoportban jön létre, és a kapcsolódó Azure-előfizetés.  Meg kell jelennie egy új erőforrást "B2C-bérlő" hozzáadott típusú mellett az egyéb Azure-erőforrások.

![B2C-erőforrás létrehozása](./media/active-directory-b2c-how-to-enable-billing/b2cresourcedashboard.png)

Hello B2C bérlő erőforrás kattint, le is tudja
- Kattintson az előfizetés neve tooreview számlázási adatokat. Tekintse meg a számlázási és használat.
- Kattintson az Azure AD B2C beállítások tooopen egy új böngészőlapon közvetlenül a tooyour B2C bérlő beállítások panel
- Támogatási kérés küldése
- Helyezze át a B2C bérlő erőforrás tooanother Azure-előfizetést, vagy tooanother erőforráscsoportot.  A választott módosításokat, mely Azure-előfizetés kap hálózathasználati költségeket.

![B2C erőforrás-beállítások](./media/active-directory-b2c-how-to-enable-billing/b2cresourcesettings.png)

## <a name="known-issues"></a>Ismert problémák
- B2C-bérlő törlését. A B2C-bérlő jön létre, ha törölni, és újra létrehozza hello ugyanazon a néven, is törölje és hozza létre újból "Hivatkozási" erőforrás hello hello ugyanazon a néven.  Megtalálja a "Linking" alatt az "Összes erőforrás" hello Azure-portálon keresztül hello előfizetés-bérlőben.
- Önálló megállapított korlátozás regionális erőforrás helye.  Ritka esetekben a felhasználók létrehozott Azure-erőforrás létrehozása regionális korlátozás.  Előfordulhat, hogy ez a korlátozás hivatkozás hello Azure-előfizetés és a B2C-bérlő közötti létrehozásának hello. toomitigate, adjon enyhíteni ezt a korlátozást.

## <a name="next-steps"></a>Következő lépések
Ha ezeket a lépéseket az egyes a B2C-bérlők befejeződött, az Azure-előfizetéshez az Azure közvetlen vagy a nagyvállalati szerződés részletei megfelelően lesz számlázva.
- Tekintse át a használati és elszámolási belül a kijelölt Azure-előfizetés
- Tekintse át a részletes--napi használati jelentések használata hello [használati Reporting API](active-directory-b2c-reference-usage-reporting-api.md)
