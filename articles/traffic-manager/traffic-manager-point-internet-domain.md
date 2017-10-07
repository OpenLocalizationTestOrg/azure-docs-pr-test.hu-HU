---
title: "a vállalati internetes tartomány tooa Traffic Manager szolgáltatásbeli tartománynevére aaaPoint |} Microsoft Docs"
description: "Ez a cikk segítséget nyújt a vállalati tartomány neve tooa Traffic Manager szolgáltatásbeli tartománynevére mutasson."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 29822946-2d45-4434-ba47-fc180a445cc3
ms.service: traffic-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: kumud
ms.openlocfilehash: 84c428f60a1dc70452bf957d98a68c95e0b51715
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="point-a-company-internet-domain-tooan-azure-traffic-manager-domain"></a>A vállalati internetes tartomány tooan Azure Traffic Manager-tartományra mutasson.

Amikor Traffic Manager-profilt hoz létre, az Azure automatikusan hozzárendel egy DNS-nevet a profilhoz. toouse egy nevet a DNS-zónából, hozzon létre egy CNAME DNS-rekordot, amely leképezi a Traffic Manager-profil tartománynevére toohello. Hello Traffic Manager szolgáltatásbeli tartománynevére hello található **általános** hello Traffic Manager-profil szakasza hello konfigurálása lapon.

Például toopoint neve www.contoso.com toohello Traffic Manager DNS-név contoso.trafficmanager.net, akkor létrehoznia a következő DNS-erőforrásrekordot hello:

    www.contoso.com IN CNAME contoso.trafficmanager.net

Az összes forgalom kérelmek túl*www.contoso.com* beolvasása irányítja a rendszer túl*contoso.trafficmanager.net*.

> [!IMPORTANT]
> A második szintű tartomány például nem mutathat *contoso.com*, toohello Traffic Manager-tartományra. A DNS-protokollszabványok nem engedélyezik a CNAME-rekordokat a másodlagos szintű tartománynevek esetében.

## <a name="next-steps"></a>Következő lépések

* [A Traffic Manager útválasztási módszerei](traffic-manager-routing-methods.md)
* [Traffic Manager – Profil letiltása, engedélyezése vagy törlése](disable-enable-or-delete-a-profile.md)
* [Traffic Manager – Végpont letiltása vagy engedélyezése](disable-or-enable-an-endpoint.md)
