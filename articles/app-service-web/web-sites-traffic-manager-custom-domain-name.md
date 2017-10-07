---
title: "aaaConfigure egy webalkalmazást az Azure App Service szolgáltatásban a Traffic Manager a(z) terheléselosztást használó egyéni tartomány nevét."
description: "Adjon nevet az egyéni tartomány egy egy webalkalmazást az Azure App Service szolgáltatásban, amely tartalmazza a Traffic Manager a(z) terheléselosztást."
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 0f96c0e7-0901-489b-a95a-e3b66ca0a1c2
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: cephalin
ms.openlocfilehash: dfde5fc6b445b30b10e03dcb03e8d072130d9377
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a>A webalkalmazáshoz tartozó egyéni tartománynév konfigurálása az Azure App Service szolgáltatásban a Traffic Managerrel
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

Ez a cikk általános útmutatást nyújt az Azure App Service egy egyéni tartománynév használatával, amely a Traffic Manager használata terheléselosztáshoz.

[!INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a>DNS-rekordok ismertetése
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>

## <a name="configure-your-web-apps-for-standard-mode"></a>A szabványos mód a webalkalmazások konfigurálása
[!INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a>Az egyéni tartomány DNS-rekord hozzáadása
> [!NOTE]
> Ha tartományi Azure App Service Web Apps keresztül vásárolt, majd ugorjon a következő lépéseket és az utolsó lépésében toohello hivatkozik [tartomány vásárlása Web Apps](custom-dns-web-site-buydomains-web-app.md) cikk.
> 
> 

tooassociate egy webalkalmazást az Azure App Service szolgáltatásban az egyéni tartományt, hozzá kell adnia egy új bejegyzést hello DNS-tábla az egyéni tartomány hello tartományregisztráló a tartománynév, a megvásárolt által biztosított eszközök segítségével. A következő lépéseket toolocate hello használja, és hello DNS-eszközök használata.

1. Jelentkezzen be a tartományregisztrálónál tooyour fiókot, és keresse meg a DNS-rekordok kezelése lapon. Keressen hivatkozásokat vagy hello hely területek címkézve **tartománynév**, **DNS**, vagy **neve kezelő**. Gyakran toothis lapján található hivatkozás lehet a fiók adatainak megtekintésekor, majd egy hivatkozást, mint **a tartományok**.
2. A tartománynév talált hello kezelés lapon, keresse meg a hivatkozást, amellyel lehetővé teszi a tooedit hello DNS-rekordokat. Ez lehet, hogy legyen felsorolva egy **zónafájl**, **DNS-rekordok**, vagy mint egy **speciális** konfigurációs hivatkozására.
   
   * hello lap valószínűleg fog rendelkezni a már létrehozott, például egy bejegyzés társítása néhány rekord "**@**"vagy"\*" "tartomány várakozást" lapot. Például közös altartományok rekordjait is tartalmazhat **www**.
   * hello lapon fog említse meg **CNAME rekordok**, vagy adjon meg egy legördülő tooselect az típusát. Azt is tüntethető fel más bejegyzések például **A rekordok** és **MX-rekordok**. Bizonyos esetekben a CNAME-rekordok fogja meghívni az egyéb, mint egy **aliasrekordot**.
   * hello lap is, amelyek lehetővé teszik túl mezők**térkép** a egy **állomásnév** vagy **tartománynév** tooanother tartomány nevét.
3. Minden tartományregisztráló hello mintaadatokról eltérők lehetnek, amíg általában leképez *a* az egyéni tartománynevet (például **contoso.com**,) *való* hello Traffic Manager szolgáltatásbeli tartománynevére (**contoso.trafficmanager.net**) használt a webalkalmazás.
   
   > [!NOTE]
   > Másik lehetőségként Ha a bejegyzést már használatban van, és toopreemptively kell kötni az alkalmazások tooit, létrehozhat egy további CNAME-rekordot. Például toopreemptively bind **www.contoso.com** tooyour webes alkalmazás, hozzon létre egy CNAME rekordot a **awverify.www** túl**contoso.trafficmanager.net**. Felveheti a "www.contoso.com" tooyour webalkalmazás hello "www" CNAME rekordot a módosítása nélkül. További információkért lásd: [hozzon létre DNS-rekordok az egyéni tartomány webalkalmazáshoz][CREATEDNS].
   > 
   > 
4. Miután befejezte a hozzáadása vagy módosítása a regisztráló DNS-rekordokat, hello módosítások mentéséhez.

<a name="enabledomain"></a>

## <a name="enable-traffic-manager"></a>A Traffic Manager engedélyezése
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a>Következő lépések
További információkért lásd: hello [Node.js fejlesztői központ](/develop/nodejs/).

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
