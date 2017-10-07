---
title: "aaaHow tooperform egy áttekintése |} Microsoft Docs"
description: "Ismerje meg, hogyan tooperform a áttekintés a hello Azure Privileged Identity Management alkalmazás."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 49ee2feb-7d2e-4acf-82c1-40ff23062862
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 301a5e9f97b68fedfbf4954e0bd7dadb7f0fc510
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooperform-an-access-review-in-azure-ad-privileged-identity-management"></a>Hogyan tooperform hozzáférés tekintse át az Azure AD Privileged Identity Management
Az Azure Active Directory (AD) Privileged Identity Management egyszerűbbé teszi a hogyan kezelhetik a vállalatok számára az Azure AD privileged access tooresources és más Microsoft online szolgáltatások, például az Office 365-öt vagy a Microsoft Intune.  

Ha tooan rendszergazdai szerepkör van hozzárendelve, a szervezet kiemelt szerepkörű rendszergazda megkérheti, tooregularly győződjön meg arról, hogy továbbra is szerepkörre van szüksége, hogy a feladat. Előfordulhat, hogy kap egy e-mailt, amely tartalmaz egy hivatkozást, vagy elvégezheti a egyenes toohello [Azure-portálon](https://portal.azure.com). Kövesse a cikk tooperform egy önálló tekintse át a hozzárendelt szerepkörök hello lépéseit.

Ha egy kiemelt szerepkörű rendszergazda hozzáférést értékelést iránt érdeklődik, ezzel további adatokat, [hogyan toostart hozzáférés tekintse át](active-directory-privileged-identity-management-how-to-start-security-review.md).

## <a name="add-hello-privileged-identity-management-application"></a>Hello Privileged Identity Management alkalmazás felvétele
Hello hello Azure AD Privileged Identity Management (PIM) alkalmazás használható [Azure-portálon](https://portal.azure.com/) tooperform felülvizsgálandó.  Hello Azure AD Privileged Identity Management alkalmazás nem rendelkezik a portálon, hajtsa végre az ezen lépések tooget elindult.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Válassza ki a felhasználónév hello jobb felső sarokban a hello Azure-portálon, és ahol lesz, jelölje be hello directory működik.
3. Válassza ki **további szolgáltatások** és hello szűrő szövegmező toosearch a **Azure AD Privileged Identity Management**.
4. Ellenőrizze **PIN-kód toodashboard** majd **létrehozása**. Ekkor megnyílik a Privileged Identity Management alkalmazás hello.

## <a name="approve-or-deny-access"></a>Hagyja jóvá vagy nem engedélyezik a hozzáférést
Hagyja jóvá vagy nem engedélyezik a hozzáférést, akkor még csak szólítja fel hello felülvizsgáló e továbbra is használhatja ezt a szerepkört vagy nem. Válasszon **jóváhagyás** Ha azt szeretné, hogy toostay hello szerepkörben vagy **Megtagadás** Ha azt nem kell hello hozzáférés többé. Az állapota nem azonnal, megváltoztatni, amíg meg nem hello felülvizsgáló hello eredmények vonatkozik.
Kövesse az alábbi lépéseket toofind, és végezze el a hello áttekintése:

1. Hello PIM alkalmazást, válassza ki **felülvizsgálati emelt szintű hozzáférés**. Ha minden függőben lévő hozzáférés felülvizsgálatra, azok megjelennek hello Azure AD hozzáférési panel ellenőrzi.
2. Válassza ki a kívánt toocomplete hello áttekintése.
3. Hello felülvizsgálati hozott létre, kivéve, hello csak felhasználói hello felülvizsgálat alatt jelennek meg. Válassza ki a hello pipa következő tooyour nevét.
4. Válasszon **jóváhagyása** vagy **megtagadása**. Szükség lehet a hello döntés okát tooinclude **okot** szövegmezőben.  
5. Bezárás hello **tekintse át az Azure AD-szerepkörök** panelen.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Következő lépések
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
