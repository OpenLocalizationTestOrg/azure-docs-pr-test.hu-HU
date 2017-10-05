---
title: "Az Azure Active Directory tartományi szolgáltatások: Az Azure Active Directory-Tartományvezérlők rendszergazdák csoport létrehozása |} Microsoft Docs"
description: "Az Azure Active Directory Domain Services engedélyezése a klasszikus Azure portál használatával"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: maheshu
ms.openlocfilehash: 5ed0125e05928cf0f6d9941e099b433ecb46e6e2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-classic-portal"></a>Az Azure Active Directory Domain Services engedélyezése a klasszikus Azure portál használatával
Ez a cikk ismerteti, és végigvezeti a konfigurációs feladatok, amelyek szükségesek ahhoz, hogy engedélyezze az Azure Active Directory tartományi szolgáltatások (az Azure Active Directory tartományi szolgáltatások) az Azure Active Directory (Azure AD) bérlő.

> [!NOTE]
> [**Próbálja meg helyette az új (előzetes verzió) Azure portál felületet**](active-directory-ds-getting-started.md). 
>

## <a name="task-1-create-the-azure-ad-dc-administrators-group"></a>1. feladat: az Azure Active Directory-Tartományvezérlők rendszergazdák csoport létrehozása
Az első tevékenységet, ha egy felügyeleti csoport az Azure AD-bérlőben. Ez a különleges felügyeleti csoport neve *AAD DC rendszergazdák*. A csoport tagjai a gépeken, amelyek a tartományhoz az Azure Active Directory tartományi szolgáltatások által kezelt tartomány rendszergazdai jogosultsággal rendelkező. A tartományhoz csatlakoztatott számítógépeken ehhez a csoporthoz hozzáadni a Rendszergazdák csoport. A csoport tagjai emellett használhatja a távoli asztal távolról csatlakozni a tartományhoz csatlakoztatott számítógépeken.  

> [!NOTE]
> Nincs tartományi rendszergazda vagy a vállalati rendszergazda engedélyekkel kell rendelkeznie az Azure Active Directory tartományi szolgáltatások által létrehozott felügyelt tartományon. A felügyelt tartományok ezeket az engedélyeket a szolgáltatás által fenntartott, és nem érhetik el a bérlő belüli felhasználók. A speciális felügyeleti csoport létrehozása a konfigurációs feladat használatával azonban néhány kiemelt műveletek végrehajtására. Ezek a műveletek közé tartoznak a számítógépek csatlakoztatása a tartományhoz, tartományhoz csatlakozó számítógépeken a felügyeleti csoportba tartozó és a csoportházirend konfigurálásához.
>

A konfigurációs feladat a felügyeleti csoport létrehozásához, és egy vagy több felhasználó hozzáadása a csoporthoz a könyvtárban. A felügyeleti csoport létrehozása az Azure Active Directory tartományi szolgáltatások, tegye a következőket:

1. Nyissa meg a [klasszikus Azure portált](https://manage.windowsazure.com).
2. A bal oldali panelen válassza az **Active Directory** gombot.
3. Válassza ki, amelynek az Azure Active Directory tartományi szolgáltatások engedélyezni szeretné az Azure AD bérlőt (címtárat). Csak egy tartomány minden Azure AD-címtár is létrehozhat.

    ![Válassza ki az Azure AD-címtár](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. Az a **preview directory** lapján kattintson a **csoportok** fülre.
5. Csoport hozzáadása az Azure AD-bérlő, a az ablak alján feladat panelen kattintson a **csoport hozzáadása**.

    ![A csoport hozzáadása gomb](./media/active-directory-domain-services-getting-started/add-group-button.png)
6. A a **csoport hozzáadása** párbeszédpanelen hozzon létre egy nevű csoportot **AAD DC rendszergazdák**, és utána állítsa be **csoporttípus** való **biztonsági**.

   > [!WARNING]
   > Engedélyezi a hozzáférést az Azure Active Directory tartományi szolgáltatások által felügyelt tartományon belül, a pontos nevű csoport létrehozása.
   >
   >

    ![A csoport hozzáadása párbeszédpanel](./media/active-directory-domain-services-getting-started/create-admin-group.png)
7. Az a **leírás** mezőbe írjon be egy leírást, amely lehetővé teszi, hogy mások számára, hogy ez a csoport engedélyt ad a rendszergazda belül Azure Active Directory tartományi szolgáltatások megismerése.
8. A csoport létrehozása után kattintson a csoport nevét a tulajdonságai megtekintéséhez.
9. Felhasználók hozzáadása a csoporthoz, az ablak alján kattintson a **tagok hozzáadása** gombra.

    ![Adja hozzá a csoport tagjai gomb](./media/active-directory-domain-services-getting-started/add-group-members-button.png)
10. Az a **tagok hozzáadása** párbeszédpanelen jelölje ki a felhasználókat, akik a csoport tagjai legyen, és kattintson a pipa ikonra a képernyő jobb alsó sarkában.

    ![Felhasználók hozzáadása a rendszergazdák csoporthoz](./media/active-directory-domain-services-getting-started/add-group-members.png)


## <a name="next-step"></a>Következő lépés
[2. feladat: hozzon létre, vagy válasszon egy Azure virtuális hálózat](active-directory-ds-getting-started-vnet.md)
