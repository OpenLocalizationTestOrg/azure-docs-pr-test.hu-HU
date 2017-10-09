---
title: "elérési út alapú aaaCreate szabály - Azure Application Gateway - Azure portálon |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate használatával Alkalmazásátjáró elérési alapú szabály hello portál"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 87bd93bc-e1a6-45db-a226-555948f1feb7
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/03/2017
ms.author: gwallace
ms.openlocfilehash: 21cb52c426ca5f7dfedf07a96e87fbc85d243647
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-hello-portal"></a>Alkalmazásátjáró elérési alapú szabály létrehozásához hello portál használatával

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

URL-cím elérési út-alapú útválasztási lehetővé teszi, hogy Ön tooassociate útvonalakat a Http-kérelem hello URL-címe alapján. Ellenőrzi, hogy az útvonal tooa háttér-készlet hello URL-címe a hello Alkalmazásátjáró konfigurálva van, és elküldi a hello hálózati forgalom toohello definiált háttér-készlet. URL-alapú útválasztási gyakori felhasználási tooload-érkező kérések elosztása a különböző típusú tartalmakat toodifferent háttér-kiszolgálófiók készletek.

Új szabály típusa tooapplication átjáró URL-alapú útválasztási vezet be. Alkalmazásátjáró két szabály tartozik: alapszintű és az elérési út-alapú szabályok. Alapszintű szabálytípus hello, biztosít a ciklikus multiplexelési hello háttér-szolgáltatás a készletbe közben elérési-alapú szabályok továbbá tooround multiplexelés terjesztési, is hello kérelem URL-címének elérési út mintája figyelembe veszi a hello megfelelő háttérkészlet kiválasztásakor.

## <a name="scenario"></a>Forgatókönyv

hello következő forgatókönyv végighalad az elérési út alapú szabályt hoz létre, egy meglévő Alkalmazásátjáró.
hello a forgatókönyv azt feltételezi, hogy már követte hello lépéseket túl[Alkalmazásátjáró létrehozása](application-gateway-create-gateway-portal.md).

![URL-cím útvonal][scenario]

## <a name="createrule"></a>Hello elérési alapú szabály létrehozása

Elérési út alapuló szabály saját figyelő szükséges lehet, hogy rendelkezik egy elérhető figyelő toouse tooverify hello szabály létrehozása előtt.

### <a name="step-1"></a>1. lépés

Keresse meg a toohello [Azure-portálon](http://portal.azure.com) válassza ki a meglévő Alkalmazásátjáró. Kattintson a **szabályok**

![Az Application Gateway áttekintése][1]

### <a name="step-2"></a>2. lépés

Kattintson a **elérési alapú** gomb tooadd új elérési alapuló szabály.

### <a name="step-3"></a>3. lépés

Hello **Hozzáadás elérési alapú szabály** panel két részből áll. első szakasz hello, ahol definiálva hello figyelő, szabály hello és hello alapértelmezett elérési útjának megadása hello neve. hello alapértelmezett elérési utak beállításait, amelyek nem tartoznak az egyéni elérési út-alapú útvonal hello útvonalak vannak. második része hello hello **Hozzáadás elérési alapú szabály** panel, ahol megadhatja a hello elérési-alapú szabályok magukat.

**Alapbeállítások**

* **Név** -ezt az értéket, amely elérhető a portál hello rövid név toohello szabály.
* **Figyelő** -hello szabály használt hello figyelő értéke.
* **Alapértelmezett háttérkészlet** – Ez a beállítás akkor hello beállítás, amely meghatározza a hello háttér-toobe használt hello alapértelmezett szabály
* **Alapértelmezett HTTP beállítások** – Ez a beállítás akkor hello beállítás, amely meghatározza a hello HTTP beállítások toobe hello alapértelmezett szabály használatos.

**Elérési út-alapú szabályok**

* **Név** -e értéke egy rövid név toopath alapuló szabály.
* **Elérési utak** – Ez a beállítás határozza meg hello elérési hello szabály keresni kezdi-forgalom
* **Háttérkészlet** – Ez a beállítás akkor hello háttér-toobe hello szabályhoz használt definiáló hello beállítása
* **HTTP-beállítása** -ezt a beállítást, amely meghatározza a hello HTTP beállítások toobe hello szabályhoz használt hello beállítás esetén.

> [!IMPORTANT]
> Útvonalak megadása: minták toomatch elérési hello listája. Minden egyes kell kezdődnie / és hello egyetlen hely, ahol a "\*" engedélyezett hello végén van. Érvényes többek között az /xyz, /xyz* vagy/xyz / *.  

![Hozzáadása az elérési út alapú szabály panel adataival kitöltött][2]

Egy elérési utat alapú szabály tooan hozzáadása meglévő Alkalmazásátjáró az egy könnyen folyamat hello portálon keresztül. Elérési út alapú szabály létrehozása után célszerű szerkesztett tooadd további szabályok. 

![További elérési-alapú szabályok hozzáadása][3]

Ez a lépés konfigurál egy útvonal-alapú útvonal. Fontos, hogy a rendszer nem újraírja kérelmek toounderstand, mivel kérelmek Alkalmazásátjáró térjen megvizsgálja hello kérés és az alapszintű hello URL-cím mintát küld hello kérelem toohello megfelelő back-end.

## <a name="next-steps"></a>Következő lépések

Hogyan SSL kiszervezésével az Azure Application Gateway, tooconfigure: toolearn [SSL-kiszervezés konfigurálása](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png
