---
title: "aaaGetting Azure CDN használatbavételének |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan tooenable hello Azure Content Delivery Network (CDN). hello az oktatóanyag végigvezeti egy új CDN-profil és -végpont hello létrehozása."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 4ca51224-5423-419b-98cf-89860ef516d2
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 0ce9802bfd7b60e70a9a62330f5593fc17ea07d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-cdn"></a>Az Azure CDN használatának első lépései
Ez a témakör egy új CDN-profil és -végpont létrehozásán keresztül vezeti Önt végig az Azure CDN aktiválásán.

> [!IMPORTANT]
> Egy bevezető toohow CDN működik, valamint a szolgáltatások listáját, lásd: hello [CDN áttekintésével](cdn-overview.md).
> 
> 

## <a name="create-a-new-cdn-profile"></a>Új CDN-profil létrehozása
A CDN-profil CDN-végpontok gyűjteménye.  Minden profil egy vagy több CDN-végpontot tartalmaz.  Kezdésként érdemes lehet toouse több profilok tooorganize a CDN-végpontokat internetes tartomány, webalkalmazás vagy más feltétel alapján.

> [!NOTE]
> Alapértelmezés szerint egy Azure-előfizetéssel korlátozott tooeight CDN-profil. Minden egyes CDN-profil CDN-végpontok korlátozott tooten.
> 
> A CDN-díjszabást hello CDN profil szinten alkalmazzák. Ha a toouse Azure CDN vegyesen tarifacsomagok, szüksége lesz a több CDN-profilt.
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Új CDN-végpont létrehozása
**új CDN-végpont toocreate**

1. A hello [Azure Portal](https://portal.azure.com), keresse meg a tooyour CDN-profilt.  Előfordulhat, hogy rendelkezik rögzítette azt toohello irányítópult hello előző lépésben.  Ha nem, akkor megtalálja kattintva **Tallózás**, majd **CDN-profilra**, és kattintson a hello-profil tervezi tooadd a végponthoz.
   
    CDN-profil panelje hello jelenik meg.
   
    ![CDN-profil][cdn-profile-settings]
2. Kattintson a hello **végpont hozzáadása** gombra.
   
    ![Végpont hozzáadása gomb][cdn-new-endpoint-button]
   
    Hello **végpont hozzáadása** panel jelenik meg.
   
    ![Végpont hozzáadása panel][cdn-add-endpoint]
3. Adja meg a CDN-végpont kívánt nevét a **Név** mezőben.  Ez a név a gyorsítótárazott erőforrások hello tartomány lesz használt tooaccess `<endpointname>.azureedge.net`.
4. A hello **forrása típusa** legördülő menüben válassza ki a forrástípust.  Azure Storage-fiók esetén válassza a **Storage**, Azure Cloud Service szolgáltatás esetén a **Cloud Service**, Azure Web Apps esetén a **Webalkalmazás**, az összes többi nyilvánosan elérhető (az Azure rendszerben vagy máshol tárolt) webkiszolgáló-forrás esetén pedig az **Egyéni forrás** lehetőséget.
   
    ![A CDN-forrása típusa](./media/cdn-create-new-endpoint/cdn-origin-type.png)
5. A hello **forrásállomásnév** legördülő menüben válasszon ki vagy írja be a forrástartományt.  hello legördülő lista felsorolja az összes rendelkezésre álló források a 4. lépésben megadott hello típusú.  Ha a kiválasztott *egyéni forrás* , a **forrása típusa**, akkor be kell írnia az egyéni forrás hello tartományban.
6. A hello **forrás elérési útvonalának** szöveg mezőbe írja be a kívánt toocache, vagy hagyja üresen tooallow gyorsítótár 5. lépésben megadott hello tartomány bármely erőforrásának hello elérési toohello erőforrásokat.
7. A hello **forrás állomásfejlécét**adja meg a kívánt minden egyes kérelemmel CDN toosend hello hello állomásfejléc, vagy hagyja hello alapértelmezett.
   
   > [!WARNING]
   > Bizonyos típusú források – például az Azure Storage és a Web Apps hello host fejléc toomatch hello hello eredeti tartomány szükséges. Ha nincs forrás a tartománytól eltérő állomásfejléc használatát nem igényli, hagyja hello alapértelmezett értéket.
   > 
   > 
8. A **protokoll** és **forrásport**adja meg a hello protokollok és portok használt tooaccess az erőforrások hello származási helyen.  Legalább egy protokollt (a HTTP vagy a HTTPS protokollt) ki kell választani.
   
   > [!NOTE]
   > Hello **forrásport** csak érinti a milyen port hello végpont hello forrásból tooretrieve információkat használja.  hello maga végpont csak akkor elérhető tooend ügyfelek hello alapértelmezett HTTP és HTTPS-portok (80-as és 443-as), függetlenül attól, hello **forrásport**.  
   > 
   > **Akamai Azure CDN** végpontok nem teszik lehetővé az hello teljes TCP-porttartomány esetén a források számára.  A nem engedélyezett forrásportok listáját lást: [Azure CDN from Akamai Allowed Origin Ports](https://msdn.microsoft.com/library/mt757337.aspx) (Az Akamai Azure CDN engedélyezett forrásportjai).  
   > 
   > CDN elérése HTTPS-kapcsolaton keresztül tartalmat rendelkezik a következő korlátozások hello:
   > 
   > * Hello hello CDN által biztosított SSL-tanúsítványt kell használnia. A rendszer nem támogatja a harmadik féltől származó tanúsítványokat.
   > * Hello CDN által biztosított tartományt kell használnia (`<endpointname>.azureedge.net`) tooaccess HTTPS-tartalom. HTTPS-támogatás nem érhető el egyéni tartománynevek (CNAME), mert hello CDN nem támogatja az egyéni tanúsítványokat jelenleg.
   > 
   > 
9. Kattintson a hello **Hozzáadás** gomb toocreate hello új végpont.
10. Hello végpont létrehozását követően hello-profil végpontjainak listájában jelenik meg. hello listanézetben látható, hello URL-cím toouse tooaccess gyorsítótárazott tartalmat, valamint hello forrástartományt.
    
    ![CDN-végpont][cdn-endpoint-success]
    
    > [!IMPORTANT]
    > hello végpont nem azonnal elérhetővé válik, használatra hello regisztrációs toopropagate keresztül hello CDN időt vesz igénybe.  Az <b>Akamai Azure CDN</b> típusú profilok propagálása általában egy percen belül befejeződik.  A <b>Verizon Azure CDN</b> típusú profilok propagálása általában 90 percen belül befejeződik, ám egyes esetekben több időt is igénybe vehet.
    > 
    > Próbálja meg toouse hello CDN-tartománynevet, mielőtt hello végpont-konfiguráció propagálása toohello POP felhasználók HTTP 404-es válaszkódot kapnak.  Ha a végpont létrehozása óta már több óra is eltelt, mégis 404-es válaszkódot kap, olvassa el a [404-es állapotot visszaadó CDN-végpontok hibaelhárítása](cdn-troubleshoot-endpoint.md) című cikket.
    > 
    > 

## <a name="see-also"></a>Lásd még:
* [Lekérdezési karakterláncot tartalmazó kérelmek gyorsítótárazási viselkedésének vezérlése](cdn-query-string.md)
* [Hogyan tooMap CDN tartalom tooa egyéni tartományhoz](cdn-map-content-to-custom-domain.md)
* [Eszközök előzetes betöltése Azure CDN-végponton](cdn-preload-endpoint.md)
* [CDN-végpont végleges törlése](cdn-purge-endpoint.md)
* [404-es állapotot visszaadó CDN-végpontok hibaelhárítása](cdn-troubleshoot-endpoint.md)

[cdn-profile-settings]: ./media/cdn-create-new-endpoint/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-new-endpoint/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-new-endpoint/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-new-endpoint/cdn-endpoint-success.png
