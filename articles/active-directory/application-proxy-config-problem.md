---
title: "Az alkalmazásproxy-alkalmazás létrehozása aaaProblem |} Microsoft Docs"
description: "Hogyan tootroubleshoot problémák létrehozása alkalmazásproxy alkalmazások hello Azure AD felügyeleti portál"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 24fab83c38a49a9e5754854acf2f9711e374e559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="problem-creating-an-application-proxy-application"></a>A probléma az alkalmazásproxy-alkalmazás létrehozása 

Az alábbiakban néhány gyakori problémákat hello személyek arcfelismerési egy új application proxy alkalmazás létrehozásakor.

## <a name="recommended-documents"></a>Ajánlott dokumentumok 

További információ az alkalmazásproxy alkalmazások hello felügyeleti portálon keresztül létrehozására toolearn lásd: [alkalmazások közzététele az Azure AD-alkalmazásproxy használatával](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).

Ha hello dokumentum lépéseit követi, és kihozhatják hello alkalmazás létrehozásakor hiba lépett fel, tekintse meg a hello hiba részletes információkat, és hogyan toofix hello alkalmazás javaslatokat. A legtöbb hiba üzenetekben javasolt javítás. 

## <a name="specific-things-toocheck"></a>Konkrét dolgot toocheck

tooavoid gyakori hibákat, ellenőrizze, hogy:

-   Az engedély toocreate proxyval alkalmazás rendszergazda

-   hello belső URL-címe egyedi:

-   hello külső URL-címe egyedi:

-   hello http vagy https URL-címek kezdődik, és a szöveg végén a "/"

-   hello URL-cím a tartomány neve és IP-cím nem kell lennie.

hello hibaüzenet jelenik meg hello jobb felső sarokban található hello alkalmazás létrehozásakor. Ehelyett választhatja hello értesítési ikon toosee hello hibaüzenetek.

   ![Értesítési üzenet](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a>Következő lépések
[Alkalmazásproxy engedélyezése az Azure-portálon hello](active-directory-application-proxy-enable.md)
