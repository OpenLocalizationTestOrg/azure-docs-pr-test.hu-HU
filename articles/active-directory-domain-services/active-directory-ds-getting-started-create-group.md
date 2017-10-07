---
title: "Az Azure Active Directory tartományi szolgáltatások: Az Azure AD hello DC rendszergazdák csoport létrehozása |} Microsoft Docs"
description: "Engedélyezze az Azure Active Directory tartományi szolgáltatások hello klasszikus Azure portál használatával"
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
ms.openlocfilehash: d629ab9632ef6a45b549630999ff9a122d60c040
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-classic-portal"></a>Engedélyezze az Azure Active Directory tartományi szolgáltatások hello klasszikus Azure portál használatával
Ez a cikk ismerteti, és bemutatja, hogyan kell elvégeznie tooenable Azure Active Directory tartományi szolgáltatások (az Azure Active Directory tartományi szolgáltatások) az Azure Active Directory (Azure AD) bérlő hello konfigurációs feladatok.

> [!NOTE]
> [**Próbálja meg helyette hello új (előzetes verzió) Azure portál élmény**](active-directory-ds-getting-started.md). 
>

## <a name="task-1-create-hello-azure-ad-dc-administrators-group"></a>1. feladat: hello Azure AD-tartományvezérlő rendszergazdák csoport létrehozása
első hello feladata a toocreate egy felügyeleti csoport az Azure AD-bérlőben. Ez a különleges felügyeleti csoport neve *AAD DC rendszergazdák*. A csoport tagjai a gépeken, amelyek a tartományhoz csatlakoztatott toohello Azure Active Directory tartományi szolgáltatások által kezelt tartomány rendszergazdai jogosultsággal rendelkező. A tartományhoz csatlakoztatott számítógépeken ez a csoport toohello rendszergazdák csoport jelenik meg. Emellett a csoport tagjai is használ a távoli asztal tooconnect távolról toodomain csatlakoztatott számítógépeken.  

> [!NOTE]
> A felügyelt tartományra hello Azure Active Directory tartományi szolgáltatások által létrehozott nincs tartományi rendszergazda vagy a vállalati rendszergazda engedélyekkel kell rendelkeznie. A felügyelt tartományok ezek az engedélyek hello szolgáltatás által fenntartott és az nem elérhető toousers hello bérlői belül. Azonban használhat hello speciális felügyeleti csoport létrehozása a konfigurációs feladat tooperform a néhány privilegizált műveleteket. Ezek a műveletek közé tartoznak a számítógépek toohello tartományhoz való csatlakozás, tartományhoz csatlakoztatott számítógépeken toohello felügyeleti csoporthoz tartozó és a csoportházirend konfigurálásához.
>

A konfigurációs feladat hello felügyeleti csoport létrehozása, és a directory toohello csoportban lévő egy vagy több felhasználó hozzáadása. toocreate hello felügyeleti csoport az Azure Active Directory tartományi szolgáltatásokhoz, hello a következő:

1. Nyissa meg toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Hello bal oldali ablaktáblában jelöljön ki hello **Active Directory** gombra.
3. Válassza ki a hello Azure AD bérlőt (címtárat), amelynek tooenable Azure Active Directory tartományi szolgáltatások keresi. Csak egy tartomány minden Azure AD-címtár is létrehozhat.

    ![Válassza ki az Azure AD-címtár](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. A hello **preview directory** hello kattintson **csoportok** fülre.
5. tooadd egy csoport tooyour az Azure AD bérlő hello munkaablak hello ablakban hello alján kattintson **csoport hozzáadása**.

    ![hello csoport hozzáadása gomb](./media/active-directory-domain-services-getting-started/add-group-button.png)
6. A hello **csoport hozzáadása** párbeszédpanelen hozzon létre egy nevű csoportot **AAD DC rendszergazdák**, és utána állítsa be **csoporttípus** túl**biztonsági**.

   > [!WARNING]
   > tooenable hozzáférés az Azure Active Directory tartományi szolgáltatások által felügyelt tartományon belüli ilyen pontos nevű hozzon létre egy csoportot.
   >
   >

    ![hello csoport hozzáadása párbeszédpanel](./media/active-directory-domain-services-getting-started/create-admin-group.png)
7. A hello **leírás** mezőbe írjon be egy leírást, amely lehetővé teszi, hogy mások számára, hogy ez a csoport engedélyt ad a rendszergazda belül Azure Active Directory tartományi szolgáltatások toounderstand.
8. Hello csoport létrehozása után kattintson a Tulajdonságok hello csoport neve tooview.
9. tooadd felhasználók hello csoportba hello ablakban hello alján kattintson hello **tagok hozzáadása** gombra.

    ![Adja hozzá a csoport tagjai gomb](./media/active-directory-domain-services-getting-started/add-group-members-button.png)
10. A hello **tagok hozzáadása** párbeszédpanelen jelölje be hello felhasználók, akik a csoport tagjai legyen, és kattintson a pipa ikonra hello hello alacsonyabb jobbra.

    ![Felhasználók tooadministrators csoport hozzáadása](./media/active-directory-domain-services-getting-started/add-group-members.png)


## <a name="next-step"></a>Következő lépés
[2. feladat: hozzon létre, vagy válasszon egy Azure virtuális hálózat](active-directory-ds-getting-started-vnet.md)
