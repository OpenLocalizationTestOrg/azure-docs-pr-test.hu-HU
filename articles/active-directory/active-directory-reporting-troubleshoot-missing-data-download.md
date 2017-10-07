---
title: "Hibáinak elhárítása: Hiányzó adatok hello letöltött Azure Active Directory tevékenységi naplóit |} Microsoft Docs"
description: "A letöltött Azure Active Directory tevékenységi naplóit feloldási toomissing adatokat biztosít."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 027b70e6efc570f81d3c836f50ee52aaa89be71a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="i-cant-find-any-data-in-hello-azure-active-directory-activity-logs-i-have-downloaded"></a>Nem található adatok hello Azure Active Directory tevékenységi naplóit le van töltve


## <a name="symptoms"></a>Probléma

Le hello tevékenységi naplóit (naplózási vagy bejelentkezéseket) töltve, és nem szerepel az összes hello rekord elfogadása hello ideje. Hogy miért? 

 ![Jelentéskészítés](./media/active-directory-reporting-troubleshoot-missing-data-download/01.png)
 

## <a name="cause"></a>Ok

Ha le az Azure-portálon hello tevékenységi naplóit, azt hello méretezési too120K rekordok, legtöbb rendezve korlátozására legutóbbi. 

## <a name="resolution"></a>Megoldás:

Kihasználhatja [az Azure AD Reporting API-k](active-directory-reporting-api-getting-started.md) toofetch tooa millió rekordok álljon. Az ajánlott megoldás, egy meghatározott időtartamra vonatkozóan egy egyedi módon rögzíti, amely behívja hello jelentéskészítési API-k toofetch ütemezés szerint parancsfájlt toorun, (például naponta vagy hetente).

## <a name="next-steps"></a>Következő lépések
Lásd: hello [Azure Active Directory – gyakori kérdések reporting](active-directory-reporting-faq.md).

