---
title: "Használati jelentés aaaUnlicensed |} Microsoft Docs"
description: "hello licenc nélküli használati jelentés licenc nélküli felhasználók által használt azonosíthatja, hogy a fizetős Azure AD-funkciókat."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 92138f43-9528-4c8a-b834-66a47da476e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.openlocfilehash: c44d1756b4641d7ca88434017eedb6c5e2567cb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="unlicensed-usage-report"></a>Licenc nélküli használati jelentés
hello licenc nélküli használati jelentés licenc nélküli felhasználók által használt azonosíthatja, hogy a fizetős Azure AD-funkciókat. Ez lehetővé teszi toomake jobb megvásárolt licencek és tudja, amikor szükség lehet további licencek tooidentify használatát. 

a jelentésben hello hello szolgáltatások fizetős utolsó 30 nap hello aktív használatát. 

## <a name="report-structure"></a>A jelentés struktúra
| Oszlop neve | Leírás |
|:--- |:--- |
| Licenc nélküli felhasználók |Hello felhasználó neve |
| Szolgáltatás |hello szolgáltatás neve. Például: a feltételes hozzáférés |
| Alkalmazás érhető el |hello hello funkcióval is hozzáférnek hello alkalmazás neve. Például: Office 365 SharePoint online-hoz |

> [!NOTE]
> Ha egy felhasználói fiókot törölték hello "Licenc nélküli felhasználó" oszlop tölti fel az ID, például a 1003000090D8B285
> 
> 

## <a name="conditional-access-feature"></a>Feltételes hozzáférési funkciónak
Licenc nélküli felhasználók lesz megjelölve, amikor hozzáférni a feltételes hozzáférési szabályzatot, akkor használható, ha nem rendelkeznek egy Azure AD Premium-licenc egy szolgáltatást. 

Ez vonatkozik tooMFA, / hely házirendeket, valamint az eszköz házirendek, amelyek az Intune-nal.

## <a name="see-also"></a>Lásd még:
* [Feltételes hozzáférés az Office 365 és az egyéb Azure Active Directory használatával csatlakozó alkalmazások](active-directory-conditional-access.md)
* [Feltételes hozzáférés tooAzure AD első lépések](active-directory-conditional-access-azuread-connected-apps.md) 

