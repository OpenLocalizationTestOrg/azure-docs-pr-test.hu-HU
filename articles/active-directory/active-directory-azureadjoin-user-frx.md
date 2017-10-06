---
title: "az Azure AD a telepítés során egy új eszközt aaaSet |} Microsoft Docs"
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
ms.openlocfilehash: 6afce4be7f084f1956a6f9dbddaa8def0605956d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-new-device-with-azure-ad-during-setup"></a>A telepítés során az Azure AD-val új eszköz beállítása
A Windows 10 felhasználók úgy csatlakozhatnak a saját eszközök tooAzure Active Directory (Azure AD) a hello kezdőélmény (FRX). Ez lehetővé teszi a szervezetek toodistribute légmentes fóliacsomagolásúak eszközök tootheir az alkalmazottak vagy a diákok, vagy azokat válassza ki a saját eszköz (CYOD).
Ha Windows 10 Professional vagy Windows 10 Enterprise kiadás telepítve van az eszközön, a hello alapértelmezett toohello telepítési folyamat a vállalat által birtokolt eszközök felhasználói élmény.

## <a name="toojoin-a-device-tooazure-ad"></a>egy eszköz tooAzure AD toojoin
1. Kapcsolja be az új eszköz és a telepítési folyamat hello, megjelennie hello **készen első** üzenet. Hajtsa végre a hello kér tooset be az eszközt.
2. Indítsa el a területi és nyelvi beállítások testreszabása. Majd fogadja el a hello Microsoft szoftverlicenc-szerződést.
   ![A régió testreszabása](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png)
3. Válassza ki a hello hálózati toouse toohello Internet csatlakozni szeretne.
4. Adja meg, hogy egy személyes vagy vállalati tulajdonú eszköz használata. Ha a vállalat tulajdonában, kattintson a **az eszköz tartozik toomy szervezet**. Ekkor elindul a hello Azure AD Join élmény. Az alábbiakban látható egy képernyőt, hogy látni fogja, ha a Windows 10 Professional használ.
   <center>
   ![Ez a számítógép képernyő tulajdonosára](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png)
5. Adja meg a hello hitelesítő adatokkal, amelyeket tooyou a szervezet által.
   <center>
   ![Bejelentkezési képernyő](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)
6. Miután megadta a felhasználónevét, a megfelelő bérlő az Azure ad-ben található. Ha egy összevont tartományban vannak, fogja átirányított tooyour helyszíni Secure Token Service (STS) kiszolgáló – például az Active Directory összevonási szolgáltatások (AD FS).
7. Ha egy felhasználó egy nem összevont tartományban, adja meg a hitelesítő adatok közvetlenül az Azure AD által szolgáltatott lap hello. Vállalati arculat megjelenítése be lett állítva, ha akkor tekintse meg a vállalati embléma és az is támogatja a szöveget.
8. A multi-factor authentication kihívást kéri. Ez a probléma a rendszergazdák által konfigurálható.
9. Az Azure AD ellenőrzi, hogy a felhasználó/eszköz igényli-e a mobileszköz-kezelés regisztrációs.
10. A Windows hello eszköz regisztrálja hello munkahely címtárában az Azure AD, és regisztrálja a mobileszköz-kezelés, ha szükséges.
11. Ha egy felügyelt felhasználók Windows viszi toohello asztali hello automatikus bejelentkezési folyamat során.
12. Ha egy összevont felhasználó, akkor a rendszer átirányítja a Windows-bejelentkezés toohello képernyőn tooenter a hitelesítő adatait.

> [!NOTE]
> A hello Windows a helyi Windows Server Active Directory-tartományhoz való csatlakozás out-of-box élmény nem támogatott. Ezért, ha azt tervezi, hogy a számítógép tooa tartomány toojoin, ki kell jelölni hello hivatkozás **állítsa be a Windowst helyi fiókkal** helyette. Majd csatlakozhat hello tartomány hello-beállítások a számítógépen, mielőtt ezt.
> 
> 

## <a name="additional-information"></a>További információ
* [Windows 10 Enterprise hello: módon toouse eszközök munkára](active-directory-azureadjoin-windows10-devices-overview.md)
* [Felhőalapú képességek tooWindows 10 eszközöknek az Azure Active Directory csatlakozási kiterjesztése](active-directory-azureadjoin-user-upgrade.md)
* [Microsoft Passporton keresztül jelszó nélkül identitások hitelesítése](active-directory-azureadjoin-passport.md)
* [További információk az Azure AD Join használati forgatókönyveiről](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Csatlakozás a tartományhoz csatlakozó eszközök tooAzure AD-hez Windows 10-es környezetben](active-directory-azureadjoin-devices-group-policy.md)
* [Az Azure AD Join beállítása](active-directory-azureadjoin-setup.md)

