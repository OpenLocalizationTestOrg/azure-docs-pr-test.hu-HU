---
title: "egy egyéni tartománynevet, az Azure Web Apps aaaBuy"
description: "Ismerje meg, hogyan toobuy egyéni tartományt adjon egy webalkalmazást az Azure App Service-ben."
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 70fb0e6e-8727-4cca-ba82-98a4d21586ff
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: cephalin
ms.openlocfilehash: 2ff61a3f27020516c917fe105ece99eb2a5754b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="buy-a-custom-domain-name-for-azure-web-apps"></a>Egy egyéni tartománynevet, az Azure Web Apps megvásárlása

App Service (előzetes verzió) tartományai közvetlenül az Azure-ban kezelt a legfelső szintű tartományoknak. Teszik könnyen toomanage egyéni tartományok [Azure Web Apps](app-service-web-overview.md). Az oktatóanyag bemutatja, hogyan toobuy egy App Service-tartományhoz, és rendelje hozzá a DNS neve tooAzure webalkalmazások.

Ez a cikk az Azure App Service-(Web Apps, az API Apps, Mobile Apps, a Logic Apps) van. Azure virtuális gép és az Azure Storage: [rendelje hozzá az App Service tartomány tooAzure VM vagy az Azure Storage](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/31/assign-app-service-domain-to-azure-vm-or-azure-storage/). Cloud Services, lásd: [egy egyéni tartománynevet, az Azure-felhőszolgáltatás konfigurálása](../cloud-services/cloud-services-custom-domain-name-portal.md).

## <a name="prerequisites"></a>Előfeltételek

toocomplete Ez az oktatóanyag:

* [App Service-alkalmazás létrehozása](/azure/app-service/), vagy egy másik az oktatóanyaghoz létrehozott alkalmazást használni.

## <a name="prepare-hello-app"></a>Hello alkalmazás előkészítése

egyéni tartományok toouse az Azure Web Apps, a webes alkalmazás által [App Service-csomag](https://azure.microsoft.com/pricing/details/app-service/) fizetős rétegben kell lennie (**megosztott**, **alapvető**, **szabványos**, vagy **Prémium**). Ebben a lépésben biztosíthatja, hogy hello webalkalmazás a hello támogatott IP-címek.

### <a name="sign-in-tooazure"></a>Jelentkezzen be tooAzure

Nyissa meg hello [Azure-portálon](https://portal.azure.com) és jelentkezzen be az Azure-fiókjával.

### <a name="navigate-toohello-app-in-hello-azure-portal"></a>Keresse meg az alkalmazás toohello hello Azure-portálon

Hello bal oldali menüben válassza ki a **alkalmazásszolgáltatások**, majd válassza ki a hello alkalmazás hello nevét.

![Portálnavigációjával tooAzure alkalmazás](./media/app-service-web-tutorial-custom-domain/select-app.png)

Az App Service alkalmazás hello hello felügyeleti oldal akkor jelenik meg.  

### <a name="check-hello-pricing-tier"></a>IP-címek hello ellenőrzése

A bal oldali navigációs hello app lap hello, görgessen a toohello **beállítások** válassza ki azt **vertikális felskálázás (App Service-csomag)**.

![Méretezett menü](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

hello alkalmazás jelenlegi rétegtől kék szegély ki van jelölve. Ellenőrizze, hogy hello alkalmazáshoz nincs hello toomake **szabad** réteg. Hello nem támogatja az egyéni DNS **szabad** réteg. 

![Ellenőrizze a tarifacsomag](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

Ha hello App Service-csomag nincs **szabad**zárja be, hello **válasszon tarifacsomagot** lapon, és hagyja ki túl[vásárlás hello tartomány](#buy-the-domain).

### <a name="scale-up-hello-app-service-plan"></a>Vertikális felskálázás hello App Service-csomag

Válassza ki valamelyik hello nem szabad rétegek (**megosztott**, **alapvető**, **szabványos**, vagy **prémium**). 

Kattintson a **Kiválasztás** gombra.

![Ellenőrizze a tarifacsomag](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

Amikor megjelenik a következő értesítési hello, hello skálázási művelet be nem fejeződött.

![Skálázási művelet megerősítése](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

## <a name="buy-hello-domain"></a>Hello tartomány vásárlása

### <a name="sign-in-tooazure"></a>Jelentkezzen be tooAzure
Nyissa meg hello [Azure-portálon](https://portal.azure.com/) és jelentkezzen be az Azure-fiókjával.

### <a name="launch-buy-domains"></a>Indítsa el a vásárlás tartományok
A hello **Web Apps** fülre, kattintson a webalkalmazás, jelölje be a hello neve **beállítások**, majd válassza ki **egyéni tartományok**
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

A hello **egyéni tartományok** kattintson **tartományok megvásárlása**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

### <a name="configure-hello-domain-purchase"></a>Hello tartomány beszerzési konfigurálása

A hello **App Service tartomány** lap hello **keresési tartomány** mezőbe, a típus hello tartománynevet toobuy, és írja be `Enter`. hello javasolt elérhető tartományok látható hello szövegmező alatt. Válassza ki a kívánt toobuy egy vagy több tartományt. 
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

Kattintson a hello **elérhetőségi adatai** hello tartomány kapcsolattartási adatokat űrlap kitöltése és. Ha befejezte, kattintson a **OK** tooreturn toohello App Service tartományban lap.
   
Fontos, hogy a lehető legtöbb pontossággal minden kötelező mezőt kitöltötte. Helytelen adatok elérhetőségét hiba toopurchase tartományok eredményezhet. 

Ezután válassza ki a tartomány hello szükséges beállításokat. Tekintse meg a következő táblázat ismereteket szeretnének elsajátítani hello:

| Beállítás | Ajánlott érték | Leírás |
|-|-|-|
|Automatikus megújításához | **Bekapcsolás** | Évente automatikusan megújítja a szolgáltatás alkalmazástartományban. A hitelkártya díjfizetéssel ugyanazon beszerzés ár hello a megújítási hello időpontjában. |
|Adatvédelem | Bekapcsolás | Részt vevő túl "Adatvédelem", amely hello vételár szerepel _szabad_ (kivéve a beállításjegyzékhez nem támogatják a adatvédelmet, mint például a legfelső szintű tartományoknak _. co.in_, _. Co.uk_, és így tovább). |
| Rendelje hozzá az alapértelmezett állomásnevek | **www** és**@** | SELECT hello állomásnévkötéseket, szükséges, ha szükséges. Hello beszerzési műveletek befejeződése után a webalkalmazás a kijelölt hello állomásnevek érhető el. Ha hello webalkalmazás mögött található [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/), hello beállítás tooassign hello gyökértartomány nem jelenik meg (@), mivel a Traffic Manager nem támogatja A rekordok. Módosításokat végezheti el toohello állomásnév hozzárendelések hello tartomány vásárlás befejezése után. |

### <a name="accept-terms-and-purchase"></a>Fogadja el a feltételeket és a vásárlások

Kattintson a **jogi feltételeket** tooreview hello feltételeit és hello díjakat, majd kattintson **megvásárlása**.

> [!NOTE]
> App Service-tartományok Azure DNS toohost hello tartományt használ. Továbbá tartományi Regisztr toohello, hálózathasználati költségeket az Azure DNS alkalmazni. További információ: [Azure DNS árképzési](https://azure.microsoft.com/pricing/details/dns/).
>
>

Vissza a hello **App Service tartomány** kattintson **OK**. Amíg hello művelet van folyamatban, a következő értesítések hello jelenik meg:

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-validate.png)

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-purchase-success.png)

### <a name="test-hello-hostnames"></a>Hello állomásnevek tesztelése

Alapértelmezett állomásnevek tooyour webalkalmazás hozzárendelt, is láthatja az összes kijelölt állomásnév sikeres értesítést. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

Is láthatja a hello hello kijelölt állomásnevek **egyéni tartományok** lap hello **Hostnames** szakasz. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added.png)

tootest hello állomásnevek, keresse meg a felsorolt toohello állomásnevek hello böngészőben. A hello hello példát megelőző képernyőképe, próbálja ki a navigáció too_kontoso.net_ és _www.kontoso.net_.

## <a name="assign-hostnames-tooweb-app"></a>Rendelje hozzá a gazdagépneveknek tooweb alkalmazás

Ha úgy dönt, hogy nem tooassign egy vagy több alapértelmezett állomásnevek tooyour webalkalmazás során hello folyamat vásárolhat, vagy ha az állomásnév tooassign nem szerepel a listában, bármikor rendelhet egy állomásnevet.

A hello szolgáltatás alkalmazástartományban tooany állomásnevek más webes alkalmazás is hozzárendelhetők. hello függ, hogy hello szolgáltatás alkalmazástartományban és hello webalkalmazás tartozik toohello ugyanahhoz az előfizetéshez.

- Másik előfizetést: egyéni DNS-rekordok webalkalmazásból hello szolgáltatás alkalmazástartományban toohello például egy beszerzett kívülről tartomány hozzárendelését. Információ hozzáadása egyéni DNS-nevek tooan App Service-tartomány, a következő témakörben: [egyéni DNS-rekordok kezelése](#custom). egy külső megvásárolt tartomány tooa webalkalmazás toomap lásd [leképezése egy meglévő egyéni DNS nevét tooAzure webalkalmazások](app-service-web-tutorial-custom-domain.md). 
- Ugyanahhoz az előfizetéshez: használata hello a következő lépéseket.

### <a name="launch-add-hostname"></a>Indítsa el az állomásnév hozzáadása
A hello **alkalmazásszolgáltatások** lapra, válassza hello nevét, amelyet tooassign állomásnevek, válassza ki a webalkalmazás **beállítások**, majd válassza ki **egyéni tartományok**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

Győződjön meg arról, hogy a megvásárolt tartomány szerepel hello **App Service tartományok** szakaszában, de nem adja meg azt. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-select-domain.png)

> [!NOTE]
> App Service tartományai ugyanahhoz az előfizetéshez hello web app alkalmazásban látható hello **egyéni tartományok** lap. Ha a tartomány hello web app előfizetésben, de nem látható a hello web app **egyéni tartományok** lapján próbálja megnyitni hello **egyéni tartományok** lapon vagy hello weblap frissítése. Ellenőrizze a hello értesítési Csengő hello Azure-portálon hello tetején folyamat és a létrehozási hibák is.
>
>

Válassza ki **állomásnév hozzáadása**.

### <a name="configure-hostname"></a>Állomásnév konfigurálása
A hello **állomásnév hozzáadása** párbeszédpanelen hello teljesen minősített típusnév az App Service tartomány vagy bármely altartomány. Példa:

- kontoso.NET
- www.kontoso.NET
- ABC.kontoso.NET

Ha elkészült, válassza ki a **ellenőrzése**. hello állomásnév rekordtípus automatikusan ki van jelölve, az Ön.

Válassza ki **állomásnév hozzáadása**.

Hello művelet befejeződése után a hozzárendelt állomásnév hello sikeres értesítést láthatja.  

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

### <a name="close-add-hostname"></a>Zárja be az állomásnév hozzáadása
A hello **állomásnév hozzáadása** lapon adjon meg semmilyen más állomásnév tooyour webalkalmazás, tetszés szerint,. Ha elkészült, zárja be a hello **állomásnév hozzáadása** lap.

Ekkor megjelenik az alkalmazás újonnan hozzárendelt hello hostname(s) **egyéni tartományok** lap.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added2.png)

### <a name="test-hello-hostnames"></a>Hello állomásnevek tesztelése

Keresse meg a felsorolt toohello állomásnevek hello böngészőben. Hello megelőző képernyőfelvétel a hello példában too_abc.kontoso.net_ Navigálás próbálja.

<a name="custom" />

## <a name="manage-custom-dns-records"></a>Egyéni DNS-rekordok kezelése

Az Azure DNS-rekordok egy App Service-tartomány segítségével felügyelhetők [Azure DNS](https://azure.microsoft.com/services/dns/). Hozzáadhat, távolítsa el, és DNS-rekordok frissítése, akárcsak a beszerzett kívülről tartományban.

### <a name="open-app-service-domain"></a>Nyissa meg az App Service-tartomány

Hello Azure-portálon, hello bal oldali menüben válassza ki **több szolgáltatások** > **App Service tartományok**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

Válassza ki a hello tartomány toomanage. 

### <a name="access-dns-zone"></a>Hozzáférés DNS-zóna

Hello tartomány bal oldali menüben válasszon ki **DNS-zóna**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-dns-zone.png)

Ez a művelet megnyitja hello [DNS-zóna](../dns/dns-zones-records.md) az alkalmazástartomány szolgáltatás az Azure DNS oldalán. Információ tooedit DNS-rekordokat, lásd: [hogyan toomanage DNS-zónák az hello Azure-portálon](../dns/dns-operations-dnszones-portal.md).

## <a name="cancel-purchase-delete-domain"></a>Szakítsa meg a beszerzési (delete tartomány)

App Service tartomány hello vásárol, után öt nap toocancel teljes visszatérítést vásárlását. Öt nap után törölheti a hello szolgáltatás alkalmazástartományban, de nem visszatérítést.

### <a name="open-app-service-domain"></a>Nyissa meg az App Service-tartomány

Hello Azure-portálon, hello bal oldali menüben válassza ki **több szolgáltatások** > **App Service tartományok**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

Válassza ki a hello tartomány tooyou kívánt toocancel vagy törlése. 

### <a name="delete-hostname-bindings"></a>Törölje az állomásnévkötéseket

Hello tartomány bal oldali menüben válasszon ki **állomásnévkötéseket**. az itt felsorolt hello állomásnévkötéseket az összes Azure-szolgáltatásokhoz.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostname-bindings.png)

Csak az összes állomásnévkötéseket törlése hello szolgáltatás alkalmazástartományban nem törölhető.

Minden egyes állomásnév kötés törlése kiválasztásával **...**   >  **Törlése**. Miután minden hello kötés törölte, válassza ki **mentése**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-delete-hostname-bindings.png)

### <a name="cancel-or-delete"></a>Mégse vagy törlése

Hello tartomány bal oldali menüben válasszon ki **áttekintése**. 

Ha hello megszakítási idő vásárolt hello tartományban nem telt meg, válassza ki a **szakítsa meg a beszerzési**. Ellenkező esetben látni egy **törlése** gombra kattint, helyette. visszatérítés, jelölje be nélkül toodelete hello tartomány **törlése**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-cancel.png)

Válassza ki **OK** tooconfirm hello műveletet. Ha nem szeretné tooproceed, kattintson bárhová kívül hello megerősítő párbeszédpanele.

Hello művelet befejezése után hello tartomány-e az előfizetésből kiadott és bárki toopurchase újra. 
