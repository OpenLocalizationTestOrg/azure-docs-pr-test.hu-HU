---
title: "Szolgáltatások és a felügyeleti tanúsítványok aaaCloud |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate és -felhasználási tanúsítványokat a Microsoft Azure"
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: fc70d00d-899b-4771-855f-44574dc4bfc6
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: 69cb5467ece58a91dae06b4120954aeb2826bde1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="certificates-overview-for-azure-cloud-services"></a>Azure Cloud Services tanúsítványok áttekintése
Rendszer tanúsítványokat használ az Azure felhőszolgáltatások ([tanúsítványok szolgáltatás](#what-are-service-certificates)) és hello felügyeleti API hitelesítéséhez ([felügyeleti tanúsítványok](#what-are-management-certificates) amikor hello klasszikus Azure portál használatával, és nem hello nem klasszikus Azure portálon). Ez a témakör mindkét tanúsítványtípusok általános áttekintést nyújt hogyan túl[létrehozása](#create) és [telepítése](#deploy) őket tooAzure.

Az Azure-ban használt tanúsítványok x.509 v3 alapján létrehozott tanúsítványok, és egy másik megbízható tanúsítvány aláírását, vagy önaláírt el. Önaláírt tanúsítvány aláírásával rendelkezik saját létrehozó, ezért az nem megbízható alapértelmezés szerint. A legtöbb böngésző figyelmen kívül hagyhatja ezt a problémát. Csak akkor ajánlott önaláírt tanúsítványokat, amikor a fejlesztés és tesztelés felhőszolgáltatásban. 

Azure által használt tanúsítványok is tartalmazhat, egy saját vagy nyilvános kulcsot. Tanúsítványának rendelkeznie kell egy olyan azt jelenti, hogy tooidentify biztosító ujjlenyomatot egyértelmű módon őket. Ezzel az ujjlenyomattal használatos hello Azure [konfigurációs fájl](cloud-services-configure-ssl-certificate.md) tooidentify, amely egy felhőalapú szolgáltatás tanúsítvány kell használnia. 

## <a name="what-are-service-certificates"></a>Mik azok a szolgáltatási tanúsítványok?
Szolgáltatási tanúsítványok csatolt toocloud szolgáltatást, és engedélyezze a biztonságos kommunikáció érdekében tooand hello szolgáltatás. Például ha a webes szerepkör, célszerű toosupply olyan tanúsítvány, amely képes hitelesíteni az elérhetőségi HTTPS-végpontnak. Szolgáltatási tanúsítványok, a szolgáltatás definíciós definiált automatikusan telepített toohello virtuális gép, amelyen a szerepkör példánya fut.. 

Feltöltheti a szolgáltatási tanúsítványok tooAzure klasszikus portál segítségével hello Azure klasszikus portál vagy a hello klasszikus telepítési modell használatával. Szolgáltatási tanúsítványokhoz társított egyes adott felhőalapú szolgáltatás. Hozzárendeli tooa telepítési hello szolgáltatásdefiníciós fájlban.

Szolgáltatási tanúsítványok kezelheti külön-külön a szolgáltatások, és előfordulhat, hogy különböző osztályai kell kezelnie. Egy fejlesztő például előfordulhat, hogy feltöltése a szolgáltatáscsomagot, hogy az informatikai manager korábban feltöltött tooAzure tooa tanúsítvány hivatkozik. Az informatikai Managerrel kezelhetők és anélkül, hogy egy új service-csomag tooupload (hello hello szolgáltatás konfigurációjának módosítása) tanúsítvány megújításához. Nélkül egy új service-csomag frissítése már lehetséges, mivel hello logikai nevét, a tároló nevét és a hello tanúsítvány helyét hello szolgáltatásdefiníciós fájlban, és amíg hello szolgáltatás konfigurációs fájlban megadott hello tanúsítvány ujjlenyomata. tooupdate hello tanúsítvány, akkor csak szükséges tooupload egy új tanúsítványt és a módosítás hello ujjlenyomat értékének hello szolgáltatás konfigurációs fájljában.

>[!Note]
>Hello [Cloud Services – gyakori kérdések](cloud-services-faq.md) cikk rendelkezik néhány hasznos információ a tanúsítványok.

## <a name="what-are-management-certificates"></a>Mik azok a felügyeleti tanúsítványok?
Felügyeleti tanúsítványok hello klasszikus üzembe helyezési modellel tooauthenticate engedélyezése. Számos programok telepítése és eszközök (például a Visual Studio vagy hello Azure SDK-t) használni ezeket a tanúsítványokat tooautomate konfigurációs és különböző Azure-szolgáltatásokhoz központi telepítését. Ezek a szolgáltatások valóban kapcsolódó toocloud nem. 

> [!WARNING]
> légy óvatos! Ezek a típusok, a tanúsítványok bárki hitelesíti magát őket toomanage hello előfizetéshez vannak hozzárendelve. 
> 
> 

### <a name="limitations"></a>Korlátozások
A 100 felügyeleti tanúsítványok előfizetésenként korlátozva van. Szerepel továbbá egy legfeljebb 100 felügyeleti tanúsítványok előfizetéseket alapján egy adott szolgáltatás-rendszergazda felhasználói azonosítóját. Ha hello fiókadminisztrátort hello azonosítója már be lett használt tooadd 100 felügyeleti tanúsítványok, de a további tanúsítványok szükség van, egy társadminisztrátoraként tooadd hello további tanúsítványokat is hozzáadhat. 

100-nál több tanúsítványok hozzáadása, előtt tekintse meg, ha újra felhasználhatja egy meglévő tanúsítvány. Esetleg felesleges összetettsége tooyour tanúsítvány felügyeleti folyamat társrendszergazdák használatával ad hozzá.

<a name="create"></a>
## <a name="create-a-new-self-signed-certificate"></a>Hozzon létre egy új önaláírt tanúsítványt
Minden eszköz elérhető toocreate egy önaláírt tanúsítványt is használhatja, mindaddig, amíg azok igazodik toothese beállítások:

* Egy X.509 tanúsítvány.
* A titkos kulcsot tartalmaz.
* Létre kulcscsere (.pfx fájlt).
* Meg kell egyeznie hello használt tartomány tooaccess hello felhőalapú szolgáltatás.

    > Nem olvasható be az SSL-tanúsítványa hello cloudapp.net (vagy bármely Azure-hoz kapcsolódó) tartomány; hello tanúsítvány tulajdonosának nevét meg kell egyeznie hello egyéni tartomány használt név tooaccess az alkalmazást. Például **contoso.net**, nem **contoso.cloudapp.net**.

* Legalább 2048 bites titkosítást.
* **Csak szolgáltatási tanúsítvány**: hello ügyféloldali tanúsítványt kell lennie, *személyes* tanúsítványtárolójába.

Nincsenek két módon toocreate egy tanúsítványt a Windows, a hello `makecert.exe` segédprogram, vagy az IIS.

### <a name="makecertexe"></a>MakeCert.exe
A segédprogram elavult, és már nem itt dokumentált. További információkért lásd: [MSDN-cikkben](https://msdn.microsoft.com/library/windows/desktop/aa386968).

### <a name="powershell"></a>PowerShell
```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```

> [!NOTE]
> Ha azt szeretné, hogy toouse hello tanúsítványt a tartomány helyett IP-címet, használjon hello IP-címet hello - DnsName paraméterben.


Ha azt szeretné, toouse ez [tanúsítvány hello felügyeleti portállal](../azure-api-management-certs.md), exportálni kell tooa **.cer** fájlt:

```powershell
Export-Certificate -Type CERT -Cert $cert -FilePath .\my-cert-file.cer
```

### <a name="internet-information-services-iis"></a>Az Internet Information Services (IIS)
Vannak-e lapok hello választékából internet hogyan toodo Ez az IIS-kiszolgálón. [Itt](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) találtam ismerteti azt is gondolja kiváló egy. 

### <a name="java"></a>Java
Java használhatja túl[hozzon létre egy tanúsítványt](../app-service-web/java-create-azure-website-using-java-sdk.md#create-a-certificate).

### <a name="linux"></a>Linux
[Ez](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) a cikk ismerteti, hogyan toocreate SSH rendelkező tanúsítványokat.

## <a name="next-steps"></a>Következő lépések
[Töltse fel a szolgáltatás tanúsítvány toohello a klasszikus Azure portálon](cloud-services-configure-ssl-certificate.md) (vagy hello [Azure-portálon](cloud-services-configure-ssl-certificate-portal.md)).

Töltse fel a [felügyeleti API tanúsítvány](../azure-api-management-certs.md) toohello a klasszikus Azure portálon. hello Azure-portál nem felügyeleti tanúsítványokat használnak a hitelesítéshez.

