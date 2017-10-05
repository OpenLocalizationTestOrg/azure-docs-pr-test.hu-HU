---
title: "A Service Fabric-fürt biztonságos |} Microsoft Docs"
description: "A Service Fabric-fürt és a különböző technológiák azokra végrehajtásához használt biztonsági kapcsolatos forgatókönyveit ismerteti."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 26b58724-6a43-4f20-b965-2da3f086cf8a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: chackdan
ms.openlocfilehash: 5afbe575a8affc37b8f902c0988585a83921e3d2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="service-fabric-cluster-security-scenarios"></a>A Service Fabric-fürt biztonsági forgatókönyvek
A Service Fabric-fürt saját erőforrás. Fürtök védetté kell tennie, hogy jogosulatlan felhasználók csatlakozzon a fürthöz, különösen akkor, ha rendelkezik termelési számítási feladatokhoz fut rajta. Bár lehetséges egy nem biztonságos fürtök létrehozásához, ezzel lehetővé teszik a névtelen felhasználók csatlakoznak, ha a nyilvános internethez felügyeleti végpontok teszi elérhetővé. 

Ez a cikk a biztonsági forgatókönyvek áttekintést nyújt az Azure vagy önálló és a különböző technológiák azokra végrehajtásához használt futtató fürtök számára. A fürt biztonsági forgatókönyvek a következők:

* Csomópontok biztonsági
* Ügyfél-csomópont biztonsági
* Szerepköralapú hozzáférés-vezérlést (RBAC)

## <a name="node-to-node-security"></a>Csomópontok biztonsági
A virtuális gépek vagy a fürt gépek közötti kommunikáció biztonságossá tételére. Ez biztosítja, hogy csak azok a számítógépek, amelyek jogosultak a fürthöz való csatlakoztatáshoz részt vehetnek-alkalmazások és szolgáltatások a fürt üzemeltetéséhez.

![Csomópontok kommunikáció ábrája][Node-to-Node]

Az Azure vagy az önálló fürtökön fut a Windows rendszert futtató fürtöket választhat [tanúsítvány biztonsági](https://msdn.microsoft.com/library/ff649801.aspx) vagy [Windows biztonsági](https://msdn.microsoft.com/library/ff649396.aspx) a Windows Server-gépek.

### <a name="node-to-node-certificate-security"></a>Csomópontok tanúsítvány biztonság
A Service Fabric X.509 kiszolgálói tanúsítványok, melyet a csomóponttípus konfigurációk részeként egy fürt használja. Ez a cikk végén Mik azok a tanúsítványok, és hogyan szerezni, vagy hozza létre a címzetteket gyors áttekintést valósul meg.

Tanúsítvány a biztonsági beállítások konfigurálása a fürt legyen az Azure portál, Azure Resource Manager-sablonok vagy egy önálló JSON-sablon létrehozása során. Megadhat egy elsődleges tanúsítványnak és egy nem kötelező másodlagos tanúsítvány előgörgetések használt tanúsítványt. A megadott elsődleges és másodlagos tanúsítványokat nem lehet ugyanaz, mint a felügyeleti ügyfél és a csak olvasható ügyféltanúsítványokat állít be [ügyfél-csomópont biztonsági](#client-to-node-security).

Az Azure olvasási [fürt beállítása az Azure Resource Manager-sablon használatával](service-fabric-cluster-creation-via-arm.md) megtudhatja, hogyan konfigurálhatja a tanúsítványt biztonsági fürtben.

Windows Server olvassa el önálló [önálló fürtben, X.509-tanúsítványokat használ a Windows biztonságos](service-fabric-windows-cluster-x509-security.md)

### <a name="node-to-node-windows-security"></a>Csomópont-csomópont windows biztonsági
Windows Server olvassa el önálló [önálló fürtben, használja a Windows biztonsági Windows biztonságos](service-fabric-windows-cluster-windows-security.md)

## <a name="client-to-node-security"></a>Ügyfél-csomópont biztonsági
Hitelesíti az ügyfeleket, és egy ügyfél és a fürt egyes csomópontok közötti kommunikáció biztonságossá tételére. Az ilyen típusú biztonsági hitelesíti, és bérlőkulcshoz kapcsolódó ügyfelekkel folytatott kommunikáció, amely biztosítja, hogy csak hitelesített felhasználók férhetnek hozzá a fürt és a fürt a telepített alkalmazások. Ügyfelek egyedileg azonosítja a Windows hitelesítő adatokat vagy a tanúsítvány hitelesítő adatokat.

![Ügyfél-csomópont kommunikációs ábrája][Client-to-Node]

Az Azure vagy az önálló fürtökön fut a Windows rendszert futtató fürtöket választhat [tanúsítvány biztonsági](https://msdn.microsoft.com/library/ff649801.aspx) vagy [Windows biztonsági](https://msdn.microsoft.com/library/ff649396.aspx).

### <a name="client-to-node-certificate-security"></a>Ügyfél-csomópont tanúsítvány biztonság
 Ügyfél-csomópont tanúsítvány a biztonsági beállítások konfigurálása az Azure portálon, a Resource Manager-sablonok vagy egy önálló JSON-sablon egy rendszergazdai ügyféltanúsítvány és/vagy felhasználói ügyféltanúsítványt megadásával keresztül vagy a fürt létrehozása során.  Felügyeleti ügyfél és a felhasználói ügyféltanúsítványok megadott nem lehet ugyanaz, mint az elsődleges és másodlagos tanúsítványokat állít [-csomópontok biztonsági](#node-to-node-security) az ajánlott eljárás. Alapértelmezés szerint a fürt a csomópontok biztonság hozzáadja a tanúsítványokat engedélyezett ügyfél felügyeleti tanúsítványok listájához.

A fürthöz, a felügyeleti tanúsítványt használ az ügyfelek felügyeleti képességek teljes hozzáféréssel rendelkeznek.  Csatlakozhat a fürthöz, a csak olvasható felhasználói ügyféltanúsítvány használó ügyfelek felügyeleti képességek csak olvasási hozzáféréssel rendelkeznek. Más szóval a rendszer tanúsítványokat használ a szerepkör körrel hozzáférés-vezérlést (RBAC) a cikk későbbi részében leírt.

Az Azure olvasási [fürt beállítása az Azure Resource Manager-sablon használatával](service-fabric-cluster-creation-via-arm.md) megtudhatja, hogyan konfigurálhatja a tanúsítványt biztonsági fürtben.

Windows Server olvassa el önálló [önálló fürtben, X.509-tanúsítványokat használ a Windows biztonságos](service-fabric-windows-cluster-x509-security.md)

### <a name="client-to-node-azure-active-directory-aad-security-on-azure"></a>Ügyfél-csomópont Azure Active Directory (AAD) biztonsági az Azure-on
Azure-on futó fürtök is biztonságossá teheti az Azure Active Directory (AAD) használó felügyeleti végpontok elérését. Lásd: [fürt beállítása az Azure Resource Manager-sablon használatával](service-fabric-cluster-creation-via-arm.md) szükséges aad-ben az összetevők létrehozása, hogyan tölthetők fel a fürt létrehozása során, és ezt követően ezeket a fürtök csatlakoztatása olvashat.

## <a name="security-recommendations"></a>Biztonsági javaslatok
Az Azure-fürtök esetén ajánlott AAD biztonsági használhat az ügyfelek és -csomópontok biztonsági tanúsítványok hitelesítéséhez.

Az önálló Windows Server javasoljuk használja a Windows biztonsági csoporttal fürtök fiókok (GMA) felügyelt, ha Windows Server 2012 R2 és az Active Directory. Ellenkező esetben továbbra is használni Windows biztonsági Windows-fiókkal.

## <a name="role-based-access-control-rbac"></a>A szerepköralapú hozzáférés-vezérlést (RBAC)
Hozzáférés-vezérlés lehetővé teszi, hogy a fürt rendszergazdája a különböző csoportok számára, így a fürt biztonságosabb bizonyos fürtműveletekben való hozzáférés korlátozásához. Két különböző hozzáférési ellenőrzési típusok támogatottak a fürt csatlakozó ügyfeleket: rendszergazdai szerepkör és a felhasználói szerepkör.

Rendszergazdák (beleértve az olvasási/írási képességek) felügyeleti képességek teljes hozzáféréssel rendelkeznek. Felhasználók, alapértelmezés szerint rendelkezik csak olvasási hozzáférés kezelési képességek (például lekérdezési lehetőségek), és oldja meg az alkalmazások és szolgáltatások képességét.

Megadhatja a rendszergazdai és felhasználói szerepkörök ügyfél a fürt létrehozása során, adja meg a külön identitások (tanúsítványok, AAD stb.) az egyes. Az alapértelmezett hozzáférés-vezérlési beállításokkal és az alapértelmezett beállítások módosításáról további információkért lásd: [szerepköralapú hozzáférés-vezérlés a Service Fabric-ügyfelek](service-fabric-cluster-security-roles.md).

## <a name="x509-certificates-and-service-fabric"></a>X.509-tanúsítványokat és a Service Fabric
Digitális X.509-tanúsítványokat általában használhatók ügyfelek és kiszolgálók hitelesítéséhez és titkosításához, és az üzenetek digitális aláírását. Ezekről a tanúsítványokról a további részletekért látogasson el [tanúsítványok használata](http://msdn.microsoft.com/library/ms731899.aspx).

Fontos szempontokat kell figyelembe venni:

* Termelési számítási feladatokhoz rendszert futtató fürtöket a használt tanúsítványok legyen a megfelelően konfigurált Windows kiszolgálói tanúsítvány szolgáltatások segítségével létrehozott vagy beszerzett egy jóváhagyott [tanúsítvány hitelesítésszolgáltatói (CA)](https://en.wikipedia.org/wiki/Certificate_authority).
* Soha ne használja az összes ideiglenes vagy tanúsítványok tesztelése éles üzemben eszközöket, például a MakeCert.exe használatával létrehozott.
* Használhat önaláírt tanúsítványt, de csak akkor tegye ezt tesztfürtökön és nem éles környezetben.

### <a name="server-x509-certificates"></a>Kiszolgáló X.509-tanúsítványokat
Kiszolgáló tanúsítványának rendelkeznie kell elsődleges feladata, a kiszolgáló (csomópont) az ügyfelek hitelesítése, vagy a kiszolgáló (csomópont) kiszolgálóhoz (csomópont) hitelesítése. A kezdeti ellenőrzéseket, amikor egy ügyfél vagy a csomópont hitelesíti a csomópont egyik ellenőrizze az adatot, a tulajdonos mezőben köznapi nevet. A közös név vagy a tanúsítványok tulajdonosának alternatív neveinek egyike az engedélyezett köznapi nevek listája a jelen kell lennie.

A következő cikk ismerteti az alternatív tulajdonosnevekkel (SAN) tanúsítványainak létrehozásához: [egy biztonságos LDAP-tanúsítványhoz a tulajdonos alternatív neve hozzáadása](http://support.microsoft.com/kb/931351).

A tulajdonos mezőben több érték, a minden egyes előtagként az inicializálás érték típusát jelzi. Leggyakrabban az inicializálásának "CN" köznapi neve; például "CN = www.contoso.com". Akkor is a tulajdonos mező üresen. Ha a választható tulajdonos alternatív neve mező nem üres, a tanúsítvány köznapi nevének és a tulajdonos alternatív neve egy tételt kell tartalmaznia. A DNS-név értékként lett megadva.

A tanúsítvány a kívánt célok mező értékének tartalmaznia kell egy megfelelő értéket, például a "Server-hitelesítés" vagy "Ügyfél-hitelesítés".

### <a name="client-x509-certificates"></a>Ügyfél X.509-tanúsítványokat
Ügyféltanúsítványok nem általában egy külső hitelesítésszolgáltató által kiállított. Ehelyett a személyes tárolóba, az aktuális felhasználó helye általában egy legfelső szintű hitelesítésszolgáltatóval, az "Ügyfél-hitelesítés" rendeltetési célját, melyeket ügyféltanúsítványok tartalmazza. Az ügyfél által használt Ez a tanúsítvány, ha kölcsönös hitelesítésre szükség.

> [!NOTE]
> A Service Fabric-fürtök összes felügyeleti műveleteihez kiszolgálói tanúsítványokat igényel. Ügyféltanúsítványok felügyeleti nem használható.
> 
> 

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->


## <a name="next-steps"></a>Következő lépések
Ez a cikk a fürt biztonsági tájékoztatást. A következő,


1.  [fürt létrehozása az Azure Resource Manager-sablon használatával](service-fabric-cluster-creation-via-arm.md) 
2.  [Azure-portálon](service-fabric-cluster-creation-via-portal.md).

<!--Image references-->
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png
