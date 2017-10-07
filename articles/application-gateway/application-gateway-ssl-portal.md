---
title: "aaaConfigure SSL-kiszervezés - Azure Application Gateway - Azure portálon |} Microsoft Docs"
description: "Ezen a lapon nyújt útmutatást toocreate Alkalmazásátjáró SSL-kiszervezés hello portál használatával"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 8373379a-a26a-45d2-aa62-dd282298eff3
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: e87ac0bbe10ac45e307c18802741c7bc31764a20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-hello-portal"></a>Konfigurálja az SSL kiszervezési Alkalmazásátjáró hello portál használatával

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Klasszikus Azure PowerShell](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Az Azure Application Gateway konfigurált tooterminate hello Secure Sockets Layer (SSL) munkamenet: hello átjáró tooavoid költséges SSL visszafejtési feladatok toohappen: hello webfarm lehet. SSL kiszervezési is egyszerűbbé teszi a hello előtér-kiszolgáló beállítása és felügyelete hello webalkalmazás.

## <a name="scenario"></a>Forgatókönyv

a következő forgatókönyv hello végighalad konfigurálása SSL kiszervezésére meglévő Alkalmazásátjáró. hello a forgatókönyv azt feltételezi, hogy már követte hello lépéseket túl[Alkalmazásátjáró létrehozása](application-gateway-create-gateway-portal.md).

## <a name="before-you-begin"></a>Előkészületek

tooconfigure SSL kiszervezési az Alkalmazásátjáró, egy tanúsítványra szükség. Ez a tanúsítvány be töltve a hello Alkalmazásátjáró és tooencrypt használja, és SSL-en keresztüli küldött visszafejtése hello. hello tanúsítványt kell toobe személyes információcsere (pfx) formátumú. Ez lehetővé teszi a hello titkos kulcs toobe exportált hello alkalmazás átjáró tooperform hello titkosítási és visszafejtési forgalom által előírt.

## <a name="add-an-https-listener"></a>Adja hozzá a HTTPS-figyelő

hello HTTPS-figyelő konfigurációja alapján keres, és segít útvonal hello forgalom toohello háttérkészletek menüpontot.

### <a name="step-1"></a>1. lépés

Nyissa meg a toohello Azure-portálon, és válassza ki a meglévő Alkalmazásátjáró

### <a name="step-2"></a>2. lépés

Kattintson a figyelők, és kattintson a hello Hozzáadás gomb tooadd egy figyelő.

![átjáró – áttekintés panelen][1]

### <a name="step-3"></a>3. lépés

Töltse ki a szükséges adatokat hello figyelő hello és feltöltés hello .pfx tanúsítvány, amikor befejeződött az OK gombra.

**Név** -értéke hello figyelő rövid nevét.

**Előtérbeli IP-konfiguráció** -hello előtérbeli IP-konfigurációja hello figyelő használt értéke.

**Elülső rétegbeli portot (név/Port)** -kezelőfelületre hello hello Alkalmazásátjáró és hello tényleges használt port használt hello port rövid nevét.

**Protokoll** -egy kapcsoló toodetermine, ha a https vagy HTTP protokollt használja a hello előtér.

**Tanúsítvány (és jelszó)** – Ha SSL kiszervezési szolgál, egy .pfx-tanúsítványra szükség a ezt a beállítást, és felhasználóbarát név és jelszó szükség.

![figyelő panel hozzáadása][2]

## <a name="create-a-rule-and-associate-it-toohello-listener"></a>Hozzon létre egy szabályt, és társítsa azt toohello figyelő

hello figyelő létrehozása megtörtént. Idő toocreate egy hello figyelő szabály toohandle hello forgalmát is. Szabályok határozzák meg, hogyan akkor irányított toohello háttérkészletek több konfigurációs beállításokat, beleértve, hogy a munkamenet cookie-alapú kapcsolat szolgál, protokoll, port és állapotteljesítmény alapján.

### <a name="step-1"></a>1. lépés

Kattintson a hello **szabályok** hello Alkalmazásátjáró, majd kattintson a Hozzáadás gombra.

![átjáró szabályok panelen][3]

### <a name="step-2"></a>2. lépés

A hello **hozzáadása alapszintű szabály** panelen írja be a hello szabály hello rövid nevét, és válasszon hello figyelő hello előző lépésben létrehozott. Válassza ki a megfelelő háttérkészlet hello és a beállítás http és kattintson **OK**

![HTTPS-beállítások ablak][4]

hello-beállítások mostantól mentése toohello Alkalmazásátjáró. hello menteni ezeket a beállításokat a folyamat eltarthat egy ideig még nem érhető el tooview hello portálon keresztül vagy a Powershellen keresztül. Egyszer mentett hello Alkalmazásátjáró hello titkosítási és visszafejtési forgalmat kezeli. Hello Alkalmazásátjáró és hello háttér-webkiszolgálók közötti összes forgalom kezelése HTTP protokollon keresztül. Bármely kommunikációs hátsó toohello ügyfélszoftverhez, ha a HTTPS-kapcsolaton keresztül kezdeményezett visszaadott toohello ügyfél titkosítva.

## <a name="next-steps"></a>Következő lépések

toolearn hogyan tooconfigure egyéni állapotfigyelő mintavételi rendelkező Azure Application Gateway, lásd: [hozzon létre egy egyéni állapotmintáihoz](application-gateway-create-gateway-portal.md).

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png
