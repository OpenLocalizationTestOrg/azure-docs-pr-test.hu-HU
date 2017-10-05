---
title: "A telepítés során az Azure AD-val új eszköz beállítása |} Microsoft Docs"
description: "Ez a témakör azt ismerteti, hogyan felhasználók állíthat be az Azure AD Join a first run Experience összetevő során."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 4367f453ef7c794653dfa9f305b137a168e0d207
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="set-up-a-new-device-with-azure-ad-during-setup"></a>A telepítés során az Azure AD-val új eszköz beállítása
A Windows 10-es felhasználók is csatlakozott az eszközeiket az Azure Active Directory (Azure AD) a kezdőélmény (FRX). Ez lehetővé teszi a szervezetek számára, hogy az alkalmazottak vagy az diákok légmentes fóliacsomagolásúak eszközök terjesztése a, illetve hogy azok válassza ki a saját eszköz (CYOD).
Ha Windows 10 Professional vagy Windows 10 Enterprise kiadás telepítve van az eszközön, a felhasználói élmény alapértelmezés szerint a telepítési folyamat a vállalat által birtokolt eszközök.

## <a name="to-join-a-device-to-azure-ad"></a>Eszköz csatlakoztatása az Azure AD
1. Kapcsolja be az új eszközt, és a telepítés megkezdéséhez, láthatja a **készen első** üzenet. Kövesse az utasításokat, az eszköz beállításához.
2. Indítsa el a területi és nyelvi beállítások testreszabása. Majd fogadja el a Microsoft szoftverlicenc-szerződést.
   ![A régió testreszabása](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png)
3. Válassza ki a hálózatot, az internethez való kapcsolódáshoz használandó.
4. Adja meg, hogy egy személyes vagy vállalati tulajdonú eszköz használata. Ha a vállalat tulajdonában, kattintson a **saját szervezethez tartozik az eszköz**. Ezzel elindítja az Azure AD Join élmény. Az alábbiakban látható egy képernyőt, hogy látni fogja, ha a Windows 10 Professional használ.
   <center>
   ![Ez a számítógép képernyő tulajdonosára](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png)
5. Adja meg a hitelesítő adatokkal, amelyeket Ön számára a szervezet által.
   <center>
   ![Bejelentkezési képernyő](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)
6. Miután megadta a felhasználónevét, a megfelelő bérlő az Azure ad-ben található. Ha egy összevont tartományban vannak, a helyszíni Secure Token Service (STS) kiszolgálóhoz – például az Active Directory összevonási szolgáltatások (AD FS) irányítja.
7. Ha egy felhasználó egy nem összevont tartományban, adja meg a hitelesítő adatok közvetlenül az Azure AD által szolgáltatott lapon. Vállalati arculat megjelenítése be lett állítva, ha akkor tekintse meg a vállalati embléma és az is támogatja a szöveget.
8. A multi-factor authentication kihívást kéri. Ez a probléma a rendszergazdák által konfigurálható.
9. Az Azure AD ellenőrzi, hogy a felhasználó/eszköz igényli-e a mobileszköz-kezelés regisztrációs.
10. A Windows regisztrálja az eszközt a szervezet címtárához az Azure AD-ben, és regisztrálja a mobileszköz-kezelés, ha szükséges.
11. Ha egy felügyelt felhasználók Windows viszi az automatikus bejelentkezési folyamat során az asztalon.
12. Ha egy összevont felhasználó, a rendszer irányítja a Windows bejelentkezési képernyő, a hitelesítő adatok megadását.

> [!NOTE]
> A Windows-out-of-box élményt nyújt a helyi Windows Server Active Directory-tartományhoz való csatlakozás nem támogatott. Ezért, ha azt tervezi, a számítógép csatlakoztatása a tartományhoz, ki kell jelölni a hivatkozás **állítsa be a Windowst helyi fiókkal** helyette. Majd csatlakozhat a tartományhoz, a beállítások a számítógépen, mielőtt ezt.
> 
> 

## <a name="additional-information"></a>További információ
* [Vállalati használatú Windows 10: Az eszközök munkahelyi célú használata](active-directory-azureadjoin-windows10-devices-overview.md)
* [A felhőalapú képességek kiterjesztése a Windows 10-eszközökre az Azure Active Directory Joinon keresztül](active-directory-azureadjoin-user-upgrade.md)
* [Microsoft Passporton keresztül jelszó nélkül identitások hitelesítése](active-directory-azureadjoin-passport.md)
* [További információk az Azure AD Join használati forgatókönyveiről](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Tartományhoz csatlakoztatott eszközök csatlakoztatása az Azure AD-hez Windows 10-es környezetben](active-directory-azureadjoin-devices-group-policy.md)
* [Az Azure AD Join beállítása](active-directory-azureadjoin-setup.md)

