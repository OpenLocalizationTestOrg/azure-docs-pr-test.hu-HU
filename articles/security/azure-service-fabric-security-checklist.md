---
title: "aaaAzure service fabric biztonsági ellenőrzőlista |} Microsoft Docs"
description: "Ez a cikk ellenőrzőlista biztosít az Azure-hálót biztonsági biztonsági."
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: tomsh
ms.openlocfilehash: 10ffaea9e7e4de6d758b0a57a79e269c87bfd14d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-security-checklist"></a>Az Azure Service Fabric biztonsági ellenőrzőlista
Ez a cikk egy könnyen használható ellenőrzőlistát, amely segít az Azure Service Fabric-környezet biztonságának tartalmazza.

## <a name="introduction"></a>Bevezetés
Az Azure Service Fabric egy elosztottrendszer platform, így könnyen toopackage, telepítéséhez és felügyeletéhez a méretezhető és megbízható mikroszolgáltatások létrehozására. A Service Fabric is megoldást hello jelentős kihívásaira fejlesztésének és kezelésének a felhőalapú alkalmazásokhoz. A fejlesztők és a rendszergazdák elkerülhetik az infrastruktúrával kapcsolatos összetett problémákat, és a kritikus fontosságú, nagy erőforrás-igényű, skálázható, megbízható és felügyelhető számítási feladatok megvalósítására koncentrálhatnak.

## <a name="checklist"></a>Feladatlista
A következő ellenőrzőlista toohelp, akkor győződjön meg arról, hogy minden fontos problémák a kezelését és konfigurálását a biztonságos Azure Service Fabric-megoldás még nem-e kihagyott hello használata.


|Feladatlista kategória| Leírás |
| ------------ | -------- |
|[A szerepköralapú hozzáférés-vezérlést (RBAC)](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-security-roles) | <ul><li>Hozzáférés-vezérlés lehetővé teszi, hogy hello rendszergazda toolimit hozzáférés toocertain fürt fürtműveletekben hello fürt biztonságosabbá tétele a felhasználók különböző csoportok számára.</li><li>A rendszergazdák teljes hozzáféréssel toomanagement képességek (beleértve az olvasási/írási képességek) rendelkeznek. </li><li>   Felhasználók, alapértelmezés szerint rendelkezik csak olvasási hozzáféréssel toomanagement képességek (például lekérdezési lehetőségek), és hello képességét tooresolve alkalmazások és szolgáltatások.</li></ul>|
|[X.509-tanúsítványokat és a Service Fabric](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-security) | <ul><li>[Tanúsítványok](https://docs.microsoft.com/en-us/dotnet/framework/wcf/feature-details/working-with-certificates) fürtön futó termelési számítási feladatokhoz legyen a megfelelően konfigurált Windows kiszolgálói tanúsítvány szolgáltatások segítségével létrehozott vagy beszerzett egy jóváhagyott használt [tanúsítvány hitelesítésszolgáltatói (CA)](https://en.wikipedia.org/wiki/Certificate_authority).</li><li>Soha ne használja a [ideiglenes vagy tanúsítványok tesztelése](https://docs.microsoft.com/en-us/dotnet/framework/wcf/feature-details/how-to-create-temporary-certificates-for-use-during-development) használatával létrehozott eszközök például éles környezetben [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968.aspx). </li><li>Használhatja a [önaláírt tanúsítvány](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-windows-cluster-x509-security) azonban csak akkor tegye ezt tesztfürtökön és nem éles környezetben.</li></ul>|
|[Fürtbiztonság](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-security) | <ul><li>hello fürt biztonsági forgatókönyvek például a csomópontok biztonsági, ügyfél-csomópont biztonsági [szerepköralapú hozzáférés-vezérlést (RBAC)](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-security-roles).</li></ul>|
|[Fürt hitelesítés](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-arm) | <ul><li>Hitelesíti [csomópontok kommunikáció](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/service-fabric/service-fabric-cluster-security.md) fürt összevonási. </li></ul>|
|[Kiszolgálóhitelesítés](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-arm) | <ul><li>Hello hitelesíti [fürt felügyeleti végpontok](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-portal) tooa felügyeleti ügyfél.</li></ul>|
|[Az alkalmazásbiztonság](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-arm)| <ul><li>Titkosítás és visszafejtés az alkalmazáskonfigurációs értékeket.</li><li> Replikáció során az adatok csomópontok közötti titkosítása.</li></ul>|
|[Fürt tanúsítvány](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-windows-cluster-x509-security) | <ul><li>Ez a tanúsítvány szükséges toosecure hello kommunikációs fürt hello csomópontok között.</li><li>  Állítsa be hello ujjlenyomat hello elsődleges tanúsítvány ujjlenyomata szakasz hello és az hello másodlagos hello ThumbprintSecondary változók.</li></ul>|
|[ServerCertificate](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-windows-cluster-x509-security)| <ul><li>Ez a tanúsítvány toohello ügyfél áll rendelkezésre, amikor tooconnect toothis fürt. Frissítés két különböző kiszolgálói tanúsítványok, egy elsődleges és másodlagos használható.</li></ul>|
|ClientCertificateThumbprints| <ul><li>Ez olyan tanúsítványok, amelyet az tooinstall hitelesített hello ügyfeleken. </li></ul>|
|ClientCertificateCommonNames| <ul><li>Hello köznapi neve hello első ügyféltanúsítvány hello CertificateCommonName beállítása. hello CertificateIssuerThumbprint hello ujjlenyomata a tanúsítvány kiállítója hello. </li></ul>|
|ReverseProxyCertificate| <ul><li>Ez az egy nem kötelező tanúsítvány, amely meg, ha azt szeretné, toosecure a [fordított Proxy](https://docs.microsoft.com/en-in/azure/service-fabric/service-fabric-reverseproxy). </li></ul>|
|Key Vault| <ul><li>Az Azure Service Fabric-fürtök toomanage tanúsítványok használatos.  </li></ul>|


## <a name="next-steps"></a>Következő lépések
- [Service Fabric-fürt verziófrissítés folyamatáról és az Ön elvárásainak](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-upgrade)
- [A Service Fabric-alkalmazások, a Visual Studio kezelése](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-manage-application-in-visual-studio).
- [Service Fabric állapotfigyelő modell bemutatása](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-health-introduction).
