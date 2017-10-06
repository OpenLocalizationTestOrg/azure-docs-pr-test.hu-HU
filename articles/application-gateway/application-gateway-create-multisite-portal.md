---
title: "aaaHost Azure Application Gateway több hely |} Microsoft Docs"
description: "Ezen a lapon nyújt útmutatást tooconfigure egy meglévő Azure Alkalmazásátjáró üzemeltetéséhez a hello több webalkalmazás az Azure-portálon hello ugyanahhoz az átjáróhoz."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 95f892f6-fa27-47ee-b980-7abf4f2c66a9
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: 2172aa2c80720f6f1ab7dd91745b44654bcaee00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a>Konfiguráljon egy meglévő alkalmazás átjárót több webalkalmazás üzemeltetéséhez

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-multisite-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-multisite-azureresourcemanager-powershell.md)
> 
> 

Több helyet üzemeltető lehetővé teszi egy webalkalmazás több toodeploy a hello ugyanazt az Alkalmazásátjáró. Az állomásfejlécnek hello bejövő HTTP-kérelmek, mely figyelő kapja forgalom toodetermine jelenlétére támaszkodik. hello figyelő majd arra utasítja a forgalom tooappropriate háttérkészlet be hello átjáró hello szabályok meghatározását. Az SSL engedélyezve van a webalkalmazások Alkalmazásátjáró hello kiszolgálónév jelzése (SNI) bővítmény toochoose hello megfelelő figyelő hello webes forgalom támaszkodik. Több hely üzemeltetéséhez használatos tooload-érkező kérések elosztása a különböző webes tartományok toodifferent háttér-kiszolgálófiók készletek. Hasonlóképpen a legfelső szintű tartománynak is futhat a hello több altartományt hello ugyanazt az Alkalmazásátjáró.

## <a name="scenario"></a>Forgatókönyv

A következő példa hello, Alkalmazásátjáró van kiszolgáló a contoso.com és fabrikam.com forgalom a két háttér-kiszolgálófiók rendelkezik: contoso kiszolgálókészlet és a fabrikam kiszolgálókészlethez. Hasonló lehet, például app.contoso.com és blog.contoso.com használt toohost altartományokat.

![többhelyes forgatókönyv][multisite]

## <a name="before-you-begin"></a>Előkészületek

Ebben a forgatókönyvben a többhelyes támogatás tooan meglévő Alkalmazásátjáró ad hozzá. toocomplete ebben a forgatókönyvben egy meglévő Alkalmazásátjáró toobe elérhető tooconfigure kell. Látogasson el [Alkalmazásátjáró létrehozása hello portál használatával](application-gateway-create-gateway-portal.md) toolearn hogyan toocreate egy alapszintű application gateway hello portálon.

hello az alábbiakban hello lépéseket tooupdate hello Alkalmazásátjáró szükséges:

1. Hozzon létre a háttér-készletek toouse minden egyes hely esetében.
2. Hozzon létre egy figyelőt a helyekhez Alkalmazásátjáró támogatja.
3. Hozzon létre szabályokat toomap minden egyes figyelő hello megfelelő háttér.

## <a name="requirements"></a>Követelmények

* **Háttér-kiszolgálófiók készlet:** hello hello háttér-kiszolgálók IP-címek listáját. hello IP-címek felsorolt toohello virtuális hálózati alhálózat vagy kell tartoznia, vagy egy nyilvános IP-cím/VIP kell lennie. Teljes Tartománynevét is használható.
* **Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás. Ezek a beállítások esetén tooa kapcsolt verem és a hello készlet alkalmazott tooall-kiszolgálók.
* **Előtér-port:** Ez a port nem hello nyilvános portot, amelyet a hello Alkalmazásátjáró meg van nyitva. Forgalom találatok ezt a portot, és lekérdezi átirányítja tooone hello háttér-kiszolgálók.
* **Figyelő:** hello figyelő rendelkezik egy előtér-portot, a protokollt (Http vagy Https, ezek az értékek kis-és nagybetűket), és hello SSL tanúsítvány neve (ha az SSL beállításának-kiszervezés). A többhelyes engedélyezett alkalmazásátjárót, állomásnév és SNI mutatók is bekerülnek.
* **Szabály:** hello szabály van kötve hello figyelő, hello háttér-kiszolgálófiók vermet, és azt határozza meg, mely háttér-kiszolgálófiók készlet hello forgalom irányított toowhen találatok száma a egy adott figyelő. Szabályok feldolgozása hello sorrendben, és a forgalom hello első egyező szabály függetlenül sajátlagossága figyelembe vétele keresztül jutnak. Például ha a szabályt egy alapszintű figyelő és az azonos port, hello szabály hello többhelyes figyelő mindkét használó szabály hello többhelyes figyelő szerepelnie kell a hello szabály előtt hello alapvető figyelőjével ahhoz, hogy hello többhelyes szabály toofunction várt. 
* **Tanúsítványok:** minden egyes figyelő egy egyedi tanúsítványt igényel, ebben a példában 2 figyelői többhelyes jön létre. Két .pfx-tanúsítványok és a számukra hello jelszavak kell létrehozni toobe.

## <a name="create-back-end-pools-for-each-site"></a>Minden egyes hely esetében a háttér-címkészletek létrehozása

A háttér-készlet minden egyes hely esetében, hogy szükség van az alkalmazás átjáró támogatja, ebben az esetben 2 vannak hozható létre, egy a contoso11.com és egy fabrikam11.com.

### <a name="step-1"></a>1. lépés

Keresse meg a meglévő Alkalmazásátjáró tooan a hello Azure portal (https://portal.azure.com). Válassza ki **háttérkészletek** kattintson **hozzáadása**

![háttér-készletek hozzáadása][7]

### <a name="step-2"></a>2. lépés

Töltse ki hello információkat hello háttér-készlet **pool1**, hello IP-cím vagy teljes tartománynevek hozzáadása hello háttér-kiszolgálókhoz, és kattintson a **OK**

![háttér Készletbeállítások pool1][8]

### <a name="step-3"></a>3. lépés

Hello háttérkészletek panelen kattintson a **Hozzáadás** tooadd egy további háttér címkészletet **pool2**, hello IP-cím vagy teljes TARTOMÁNYNEVEK hozzáadása hello háttér-kiszolgálókhoz, és kattintson a **OK**

![háttér alkalmazáskészlet pool2 beállításai][9]

## <a name="create-listeners-for-each-back-end"></a>Az egyes háttér-figyelők létrehozása

Alkalmazásátjáró támaszkodik a HTTP 1.1 egy webhely több állomás fejlécek toohost hello azonos nyilvános IP-cím és port. hello alapvető figyelő hello portálon létrehozott nem tartalmazza ezt a tulajdonságot.

### <a name="step-1"></a>1. lépés

Kattintson a **figyelői** a meglévő Alkalmazásátjáró hello, és kattintson a **többhelyes** tooadd hello első figyelő.

![figyelők áttekintése panel][1]

### <a name="step-2"></a>2. lépés

Töltse ki a hello hello figyelő adatait. Ebben a példában SSL lezárást van konfigurálva, hozzon létre egy új elülső rétegbeli portot. Hello .pfx tanúsítvány toobe használt SSL-lezárást feltöltése. hello csak a standard alapvető figyelő panel képest panel toohello különbség hello állomásnevet.

![figyelő tulajdonságok panelen][2]

### <a name="step-3"></a>3. lépés

Kattintson a **többhelyes** , és hozzon létre egy másik figyelő hello második hely hello előző lépésben leírt módon. Győződjön meg arról, hogy toouse hello második figyelő különböző tanúsítványt. hello csak a standard alapvető figyelő panel képest panel toohello különbség hello állomásnevet. Hello adatok hello figyelő, és kattintson a **OK**.

![figyelő tulajdonságok panelen][3]

> [!NOTE]
> Egy hosszú ideig futó feladat létrehozása az Azure-portálon az Alkalmazásátjáró hello figyelők, eltarthat néhány alkalommal toocreate hello két figyelői ebben a forgatókönyvben. Ha teljes hello figyelők megjelenítése hello portálon hello kép a következő látható:

![figyelő áttekintése][4]

## <a name="create-rules-toomap-listeners-toobackend-pools"></a>Szabályok toomap figyelői toobackend-címkészletek létrehozása

### <a name="step-1"></a>1. lépés

Keresse meg a meglévő Alkalmazásátjáró tooan a hello Azure portal (https://portal.azure.com). Válassza ki **szabályok** és hello meglévő alapértelmezett szabály választása **Szabály1** kattintson **szerkesztése**.

### <a name="step-2"></a>2. lépés

Töltse ki a hello szabályok panelen a következő kép hello látható módon. Hello első figyelő és első készlet kiválasztása, és kattintson a **mentése** teljes.

![meglévő szabály szerkesztése][6]

### <a name="step-3"></a>3. lépés

Kattintson a **alapvető szabály** toocreate hello második szabály. Töltse ki a hello második figyelő, a második háttérkészlet hello űrlapot, és kattintson a **OK** toosave.

![hozzáadása alapszintű szabály panel][10]

Ez a forgatókönyv befejezése meglévő Alkalmazásátjáró konfigurálása a többhelyes támogatás hello Azure-portálon keresztül.

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogyan tooprotect a webhelyek [Application Gateway - webalkalmazási tűzfal](application-gateway-webapplicationfirewall-overview.md)

<!--Image references-->
[1]: ./media/application-gateway-create-multisite-portal/figure1.png
[2]: ./media/application-gateway-create-multisite-portal/figure2.png
[3]: ./media/application-gateway-create-multisite-portal/figure3.png
[4]: ./media/application-gateway-create-multisite-portal/figure4.png
[5]: ./media/application-gateway-create-multisite-portal/figure5.png
[6]: ./media/application-gateway-create-multisite-portal/figure6.png
[7]: ./media/application-gateway-create-multisite-portal/figure7.png
[8]: ./media/application-gateway-create-multisite-portal/figure8.png
[9]: ./media/application-gateway-create-multisite-portal/figure9.png
[10]: ./media/application-gateway-create-multisite-portal/figure10.png
[multisite]: ./media/application-gateway-create-multisite-portal/multisite.png
