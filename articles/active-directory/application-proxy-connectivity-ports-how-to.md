---
title: "aaaHow tooopen hello tűzfalportok az alkalmazásproxy-alkalmazáshoz szükséges |} Microsoft Docs"
description: "Megtudhatja, milyen portokat tooopen a hello Azure AD alkalmazásproxy toowork megfelelően"
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
ms.openlocfilehash: cdc7badb7c15591689a3bfd6bb26da182b00fb3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooopen-hello-firewall-ports-required-for-an-application-proxy-application"></a>Hogyan tooopen hello az alkalmazásproxy-alkalmazáshoz szükséges tűzfal portok

hello szükséges portok és hello funkció az egyes portok teljes listáját toosee hello Előfeltételek című hello [alkalmazásproxy dokumentáció](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable). Vegye figyelembe, hogy a alkalmazásproxy csak használja-e a kimenő portokat.

Azt is ellenőrizheti, hogy rendelkezik-e minden hello szükséges portok nyitva megnyitása hello által [összekötő portok vizsgálati eszköz](https://aadap-portcheck.connectorporttest.msappproxy.net/) a helyszíni hálózatról. További zöld jelölők azt jelenti, hogy a nagyobb rugalmasság. 

## <a name="app-proxy-regions"></a>Alkalmazás Proxy régiók

Úgy tudja, melyik e régiók toolet kell toobe zöld, jelenleg is dolgozunk. Most győződjön meg arról, hogy az összes. USA középső RÉGIÓJA is függetlenül melyik régióban jogkörrel rendelkezik arra szükség.

toomake meg arról, hogy hello eszközt ad meg hello jobb eredményeket, ügyeljen arra, hogy:

-   Nyissa meg a böngésző hello eszköz hello kiszolgálóra, amelyre telepítve van a hello összekötő.

-   Győződjön meg arról, hogy bármely proxyk és tűzfalak megfelelő tooyour összekötő is vannak alkalmazva toothis lap. Ezt megteheti az Internet Explorer túl címen**beállítások**  - &gt; **Internetbeállítások**  - &gt; **kapcsolatok**  - &gt; **Lan-beállítások**. Ezen a lapon megjelenik a "Hálózati használata az egy Proxy kiszolgáló esetében a" hello mező. Jelölje be a, és hogy hello proxycím hello "Address" mezőbe.

## <a name="next-steps"></a>Következő lépések
[Az Azure AD-alkalmazásproxy összekötők ismertetése](application-proxy-understand-connectors.md)
