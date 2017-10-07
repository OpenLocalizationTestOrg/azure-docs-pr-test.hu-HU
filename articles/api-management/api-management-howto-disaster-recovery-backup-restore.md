---
title: "aaaImplement vész-helyreállítási használatával biztonsági mentése és visszaállítása az Azure API Management |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse biztonsági mentése és visszaállítása az Azure API Management tooperform katasztrófa utáni helyreállítás."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 6f10be3c-f796-4a6c-bacd-7931b6aa82af
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 058bfb579e3a3f51fb1dac8ea37eb4fdbc83a4ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooimplement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>Hogyan tooimplement vész-helyreállítási segítségével biztonsági mentés és a szolgáltatás visszaállítása az Azure API Management
Az toopublish kiválasztása és kezelése az API-k ki sok hiba tűréshatár és infrastruktúra képességeket, amelyeket egyébként külön toodesign, megvalósítása és kezelése Azure API Management szolgáltatáson keresztül. hello Azure platformon csökkenti a lehetséges hibák hello költség töredéke alatt, nagy része.

rendelkezésre állási problémákhoz hello terület, ahol az API Management szolgáltatást érintő toorecover üzemeltetett meg kell tooreconstitute készen áll a szolgáltatás egy másik régióban bármikor. Attól függően, hogy a rendelkezésre állási célokat és a helyreállítási idő célkitűzése előfordulhat, hogy szeretné, hogy egy biztonsági mentési szolgáltatás egy vagy több régióban tooreserve, és próbálja meg toomaintain, a konfiguráció és a tartalom szinkronban vannak hello aktív szolgáltatás. hello szolgáltatás biztonsági mentési és visszaállítási funkciót biztosít a hello szükséges építőelem végrehajtási vész-helyreállítási stratégiát.

Ez az útmutató bemutatja, hogyan tooauthenticate Azure Resource Manager kér, és hogyan toobackup és az API Management szolgáltatáspéldány visszaállítása.

> [!NOTE]
> hello biztonsági mentése és visszaállítása egy API-kezelés szolgáltatáspéldány vész-helyreállítási folyamata a API-kezelés szolgáltatás példányainak ahhoz hasonló forgatókönyvek esetén átmeneti replikálására is használható.
>
> Vegye figyelembe, hogy minden egyes biztonsági másolat 30 nap múlva lejár. Ha a biztonsági mentés toorestore hello 30 napos lejárati időszak lejárta után, hello visszaállítása sikertelen lesz, és egy `Cannot restore: backup expired` üzenet.
>
>

## <a name="authenticating-azure-resource-manager-requests"></a>Hitelesítő Azure Resource Manager-kérelmek
> [!IMPORTANT]
> hello REST API-t a biztonsági mentési és visszaállítási Azure Resource Managert használja, és különböző hitelesítési mechanizmussal rendelkezik, mint a REST API-k kezelése az API Management entitások hello. Ebben a szakaszban hello lépések bemutatják, hogyan tooauthenticate Azure Resource Manager kéri. További információkért lásd: [kérelmek hitelesítéséhez az Azure Resource Manager](http://msdn.microsoft.com/library/azure/dn790557.aspx).
>
>

Arról, hogy a hello Azure Resource Manager eszközzel hello feladatokat az Azure Active Directoryban a lépéseket követve hello hitelesíteni kell.

* Adja hozzá az alkalmazás toohello Azure Active Directory-bérlő.
* Hello alkalmazáshoz hozzáadott engedélyeket.
* Hello jogkivonat lekérése kérelmek tooAzure erőforrás-kezelő hitelesítéséhez.

első lépés hello toocreate egy Azure Active Directory-alkalmazás. Jelentkezzen be a hello [klasszikus Azure portál](http://manage.windowsazure.com/) hello az előfizetést, amely tartalmazza az API Management service-példány, és keresse meg a toohello **alkalmazások** fülre az alapértelmezett Azure Active Directoryban.

> [!NOTE]
> Ha nem látható tooyour fiók hello Azure Active Directory alapértelmezett címtár, hello Azure-előfizetés toogrant hello kapcsolattartási hello rendszergazdája szükséges engedélyek tooyour fiók.

![Azure Active Directory-alkalmazás létrehozása][api-management-add-aad-application]

Kattintson a **Hozzáadás**, **a szerveztem által fejlesztett alkalmazás hozzáadása**, és válassza a **natív ügyfélalkalmazás**. Adjon meg egy leíró nevet, majd kattintson a Tovább nyílra hello. Írjon be egy helyőrző URL-címet, például `http://resources` a hello **átirányítási URI-**, mert a mezőt kötelező kitölteni, de hello értéket később nem használja. Kattintson a hello jelölőnégyzetet toosave hello alkalmazásra.

Miután hello alkalmazás mentettük, kattintson a **konfigurálása**, toohello görgetve **engedélyek tooother alkalmazások** szakaszt, és kattintson a **alkalmazás hozzáadása**.

![Engedélyek hozzáadása][api-management-aad-permissions-add]

Válassza ki **Windows** **Azure szolgáltatásfelügyeleti API** kattintson hello jelölőnégyzet tooadd hello alkalmazásra.

![Engedélyek hozzáadása][api-management-aad-permissions]

Kattintson a **delegált engedélyek** mellett az újonnan hozzáadott hello **Windows** **Azure szolgáltatásfelügyeleti API** alkalmazás, hello jelölőnégyzetét **Access Azure Service Management (preview)**, és kattintson a **mentése**.

![Engedélyek hozzáadása][api-management-aad-delegated-permissions]

Előzetes tooinvoking hello API-k, amelyek létrehozzák hello biztonsági mentési, majd állítsa vissza azt, a szükséges tooget jogkivonatot. hello alábbi példában hello [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) nuget csomag tooretrieve hello jogkivonat.

```c#
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace GetTokenResourceManagerRequests
{
    class Program
    {
        static void Main(string[] args)
        {
            var authenticationContext = new AuthenticationContext("https://login.microsoftonline.com/{tenant id}");
            var result = authenticationContext.AcquireToken("https://management.azure.com/", {application id}, new Uri({redirect uri});

            if (result == null) {
                throw new InvalidOperationException("Failed tooobtain hello JWT token");
            }

            Console.WriteLine(result.AccessToken);

            Console.ReadLine();
        }
    }
}
```

Cserélje le `{tentand id}`, `{application id}`, és `{redirect uri}` utasításai hello segítségével.

Cserélje le `{tenant id}` hello bérlői azonosítójú hello Azure Active Directory-alkalmazás imént létrehozott. Hello azonosító eléréséhez kattintson **végpontok megtekintése**.

![Végpontok][api-management-aad-default-directory]

![Végpontok][api-management-endpoint]

Cserélje le `{application id}` és `{redirect uri}` hello segítségével **ügyfél-azonosító** és URL-címet a hello hello **átirányítási URI-azonosítók** szakaszban az Azure Active Directory-alkalmazás **konfigurálása**  fülre.

![Erőforrások][api-management-aad-resources]

Miután hello értékek vannak megadva, hello példakód a következő példa egy token hasonló toohello kell visszaadnia.

![Token][api-management-arm-token]

Mielőtt hívása hello biztonsági mentési és visszaállítási hello a következő részekben leírt műveleteket, állítsa be a hello engedélyezési kérelem fejléce a REST-hívást.

```c#
request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);
```

## <a name="step1"></a>Készítsen biztonsági másolatot egy API-kezelés szolgáltatás
tooback fel az API Management szolgáltatás probléma hello HTTP-kérelem a következő:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

Ahol:

* `subscriptionId`-toobackup próbált hello API Management szolgáltatást tartalmazó hello előfizetés azonosítója
* `resourceGroupName`-hello képernyőn "API - alapértelmezett-{szolgáltatás-régió}" karakterlánc ahol `service-region` hello Azure-régió, ahol hello API-kezelés szolgáltatás, amelyhez toobackup tárolása, pl. azonosítja`North-Central-US`
* `serviceName`-hello hello végez biztonsági másolatot, ahol létrehozták hello időpontban megadott API-kezelés szolgáltatás neve
* `api-version`-cseréje`2014-02-14`

A hello hello kérelem törzse adja meg a hello cél az Azure storage-fiók neve, a hozzáférési kulcsot, a blob-tároló neve és a biztonsági másolat neve:

```
'{  
    storageAccount : {storage account name for hello backup},  
    accessKey : {access key for hello account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

Állítsa be hello hello `Content-Type` kérelemfejléc túl`application/json`.

Biztonsági mentés egy hosszú ideig futó művelet, amely több percig toocomplete is igénybe vehet.  Ha hello kérés sikeres volt, és biztonsági mentési folyamat hello kezdeményezett kapni fog egy `202 Accepted` a válasz állapotkódja egy `Location` fejléc.  Ellenőrizze a "GET" kérelmek hello toohello URL-címet `Location` fejléc toofind kimenő hello művelet hello állapotát. Hello biztonsági mentés közben továbbra tooreceive 202 elfogadott állapotkódot. Válaszkód `200 OK` fogja jelezni hello biztonsági mentési művelet sikeres befejezését.

Vegye figyelembe a következő korlátozások, amikor egy biztonsági mentési kérést hello.

* **Tároló** hello kérés törzsében megadott **léteznie kell**.
* Biztonsági mentés folyamatban, amíg **nem szabadna megpróbálniuk a szolgáltatás felügyeleti műveleteket sem** SKU frissítése vagy alacsonyabb szintre való visszalépést, a tartomány kiszolgálónév-változás, például stb.
* A visszaállítás egy **biztonsági mentés csak 30 napig garantáltan** hello pillanattól a létrehozása óta.
* **Használati adatok** elemzési jelentések létrehozásához használt **nincs megadva** hello biztonsági mentése. Használjon [Azure API Management REST API] [ Azure API Management REST API] tooperiodically beolvasni elemzés jelentések meg van őrizve.
* hello gyakoriság, amellyel elvégezhető a szolgáltatás biztonsági mentések hatással lesz a helyreállításipont-célkitűzést. azt javasoljuk van, rendszeres biztonsági mentések végrehajtására, valamint a fontos elvégzése után igény szerinti biztonsági mentést végez toominimize tooyour API-kezelés szolgáltatás változik.
* **Módosítások** készült toohello szolgáltatáskonfiguráció (pl. API-k, házirendek, fejlesztői portál megjelenését) biztonsági mentési művelet során van folyamatban **hello biztonsági mentés nem tartalmazhatja, és ezért elvész**.

## <a name="step2"></a>Egy API-kezelés szolgáltatás visszaállítása
az API Management szolgáltatásnak, egy korábban létrehozott biztonsági másolatból toorestore győződjön hello HTTP-kérelem a következő:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

Ahol:

* `subscriptionId`-hello API-kezelés szolgáltatás visszaállítását végzi, a biztonsági másolatot tartalmazó hello előfizetés azonosítója
* `resourceGroupName`-hello képernyőn "API - alapértelmezett-{szolgáltatás-régió}" karakterlánc ahol `service-region` hello Azure-régió, ahol hello API-kezelés szolgáltatás visszaállítását végzi, a biztonsági másolat tárolása, pl. azonosítja`North-Central-US`
* `serviceName`-hello API-kezelés szolgáltatás a visszaállítandó megadott hello időpontban, ahol létrehozták hello neve
* `api-version`-cseréje`2014-02-14`

Hello hello kérelem törzse adja meg a hello biztonságimásolat-fájl helyét, azaz az Azure storage-fiók neve, a hozzáférési kulcsot, a blob-tároló neve és biztonsági másolat neve:

```
'{  
    storageAccount : {storage account name for hello backup},  
    accessKey : {access key for hello account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

Állítsa be hello hello `Content-Type` kérelemfejléc túl`application/json`.

Visszaállítás egy hosszú ideig futó művelet, amely too30 vagy több percig toocomplete is tarthat.  Ha hello kérés sikeres volt, és hello visszaállítási folyamat kezdeményezett kapni fog egy `202 Accepted` a válasz állapotkódja egy `Location` fejléc.  Ellenőrizze a "GET" kérelmek hello toohello URL-címet `Location` fejléc toofind kimenő hello művelet hello állapotát. Hello visszaállítása közben továbbra tooreceive '202 elfogadott' állapotkód. Válaszkód `200 OK` fogja jelezni hello visszaállítási művelet sikeres befejezését.

> [!IMPORTANT]
> **hello SKU** hello szolgáltatást a visszaállítandó **meg kell egyeznie** hello hello Termékváltozata biztonsági másolat szolgáltatás visszaállítása folyamatban.
>
> **Módosítások** készült toohello szolgáltatáskonfiguráció (pl. API-k, házirendek, fejlesztői portál megjelenését) visszaállítási művelet során folyamatban van a **felülíródnak**.
>
>

## <a name="next-steps"></a>Következő lépések
Tekintse meg a következő Microsoft blogok hello biztonsági mentési/visszaállítási folyamat két különböző forgatókönyvek a hello.

* [Az Azure API Management fiókok replikálása](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/)
  * Köszönjük, hogy tooGisela saját hozzájárulás toothis cikk.
* [Az Azure API Management: Biztonsági mentése, és konfiguráció visszaállítása](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
  * Stuart által részletes hello módszer nem egyezik meg a hello hivatalos útmutatást de nagyon fontos.

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2


[Azure API Management REST API]: http://msdn.microsoft.com/library/azure/dn781421.aspx

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
