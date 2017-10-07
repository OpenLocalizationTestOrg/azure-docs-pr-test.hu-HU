---
title: "aaaNo előfizetések hibát talált, amikor próbálja toosign tooAzure portálon vagy az Azure-fiók center |} Microsoft Docs"
description: "Hello megoldást kínál a probléma, amelyben nem találhatók előfizetések hiba akkor fordul elő, amikor tooAzure portálon vagy az Azure-fiók center bejelentkezés."
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.assetid: d1545298-99db-4941-8e97-f24a06bb7cb6
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: genli
ms.openlocfilehash: def4d4a1f883dd948fe8132f2d85abc4c23ae624
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="no-subscriptions-found-error-in-azure-portal-or-azure-account-center"></a>Nem található a hiba az Azure portálon vagy az Azure-fiók center előfizetés.
A "Nem található előfizetés." hibaüzenet kaphat, ha a toohello toosign meg [Azure-portálon](https://portal.azure.com/) vagy hello [Azure-fiók center](https://account.windowsazure.com/Subscriptions). Ez a cikk megoldást kínál a probléma.

## <a name="symptom"></a>Jelenség

Amikor megpróbál toosign toohello a [Azure-portálon](https://portal.azure.com/) vagy hello [Azure-fiók center](https://account.windowsazure.com/Subscriptions), hello a következő hibaüzenetet kapja: "Nem található előfizetés".

## <a name="cause"></a>Ok

Ez a probléma akkor fordul elő, ha a fiók nem rendelkezik megfelelő engedélyekkel. 

## <a name="solution"></a>Megoldás

Győződjön meg arról, hogy jelentkezzen be rendszergazdaként hello helyes-e. A fiók rendszergazda csak a hello Account Center férhetnek hozzá. Szolgáltatás-rendszergazdák (SA) és a Társadminisztrátorok (CA) rendelkezik hozzáférési engedély csak toohello Azure-portálon, vagy a klasszikus Azure portálon hello.

### <a name="scenario-1-error-message-is-received-in-hello-azure-portalhttpsportalazurecom"></a>1. forgatókönyv: Hibaüzenet érkezik hello [Azure-portálon](https://portal.azure.com)

toofix probléma:

* Győződjön meg arról, hogy helyes-e az Azure directory van kiválasztva a fiókját a jobb felső hello kattintva hello.

  ![Jelölje be hello könyvtár: hello felső jobb oldalán hello Azure-portálon](./media/billing-no-subscriptions-found/directory-switch.png)

* Ha hello közvetlenül az Azure directory van kiválasztva, de továbbra is kaphat hello hibaüzenet, [fiókját tulajdonos rendelkezik](billing-add-change-azure-subscription-administrator.md).

### <a name="scenario-2-error-message-is-received-in-hello-azure-account-centerhttpsaccountwindowsazurecomsubscriptions"></a>2. forgatókönyv: Hibaüzenet érkezik hello [Azure-fiók center](https://account.windowsazure.com/Subscriptions)

Ellenőrizze, hogy használt fiók hello hello fiók rendszergazdájához. tooverify, akik hello fiók rendszergazdájához, kövesse az alábbi lépéseket:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello központ menüben válassza ki a **előfizetés**.
3. Válassza ki, hogy szeretné, hogy toocheck, és válassza hello előfizetést **beállítások**.
4. Válassza ki **tulajdonságok**. hello fiókadminisztrátort hello előfizetés hello megjelenő **Fiókadminisztrátor** mezőbe.

## <a name="need-help-contact-support"></a>Segítség Forduljon a támogatási szolgálathoz.
Ha további segítségre van, [forduljon a támogatási szolgálathoz](http://go.microsoft.com/fwlink/?linkid=544831&clcid=0x409) a probléma megoldódik gyorsan tooget. 
