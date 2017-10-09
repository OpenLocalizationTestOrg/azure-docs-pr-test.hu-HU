---
title: "a Service Fabric-fürt aaaSecure |} Microsoft Docs"
description: "Ismerteti a Service Fabric fürt és hello használt különböző technológiák tooimplement hello biztonsági forgatókönyvei azokra."
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
ms.openlocfilehash: 249a9e85b8fbe174e2accee85a94d95b2872a3af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-cluster-security-scenarios"></a>A Service Fabric-fürt biztonsági forgatókönyvek
A Service Fabric-fürt saját erőforrás. Fürtök biztonságos tooprevent nem engedélyezett felhasználók tooyour fürthöz csatlakozzon, különösen akkor, ha a termelési számítási feladatokhoz fut rajta van kell lennie. Bár lehetséges toocreate egy nem biztonságos fürthöz, így lehetővé teszi a névtelen felhasználók tooconnect tooit, ha felügyeleti végpontok toohello teszi elérhetővé nyilvános internethez. 

Ez a cikk biztonsági forgatókönyvek hello áttekintést nyújt az Azure vagy az önálló rendszert futtató fürtöket, és különböző technológiák tooimplement hello azokra. hello fürt biztonsági forgatókönyvek a következők:

* Csomópontok biztonsági
* Ügyfél-csomópont biztonsági
* Szerepköralapú hozzáférés-vezérlést (RBAC)

## <a name="node-to-node-security"></a>Csomópontok biztonsági
Hello virtuális gép vagy gépek hello fürt közötti kommunikáció biztonságossá tételére. Ez biztosítja, hogy csak engedélyezett toojoin hello fürt számítógépek részt vehetnek-alkalmazások és szolgáltatások hello fürt üzemeltetéséhez.

![Csomópontok kommunikáció ábrája][Node-to-Node]

Az Azure vagy az önálló fürtökön fut a Windows rendszert futtató fürtöket választhat [tanúsítvány biztonsági](https://msdn.microsoft.com/library/ff649801.aspx) vagy [Windows biztonsági](https://msdn.microsoft.com/library/ff649396.aspx) a Windows Server-gépek.

### <a name="node-to-node-certificate-security"></a>Csomópontok tanúsítvány biztonság
Service Fabric X.509 kiszolgálói tanúsítványok, melyet a hello csomóponttípus konfigurációk részeként egy fürt használja. Ez a cikk végén hello Mik azok a tanúsítványok, és hogyan szerezni, vagy hozza létre a címzetteket gyors áttekintést valósul meg.

Tanúsítvány biztonsági hello Azure-portálon keresztül, Azure Resource Manager-sablonok vagy egy önálló JSON-sablon létrehozásakor hello fürt úgy van beállítva. Megadhat egy elsődleges tanúsítványnak és egy nem kötelező másodlagos tanúsítvány előgörgetések használt tanúsítványt. hello megadhat elsődleges és másodlagos tanúsítványok nem lehet ugyanaz, mint hello rendszergazdai ügyfél és a csak olvasható ügyféltanúsítványokat állít be [ügyfél-csomópont biztonsági](#client-to-node-security).

Az Azure olvasási [fürt beállítása az Azure Resource Manager-sablon használatával](service-fabric-cluster-creation-via-arm.md) toolearn hogyan tooconfigure tanúsítvány biztonsági fürtben.

Windows Server olvassa el önálló [önálló fürtben, X.509-tanúsítványokat használ a Windows biztonságos](service-fabric-windows-cluster-x509-security.md)

### <a name="node-to-node-windows-security"></a>Csomópont-csomópont windows biztonsági
Windows Server olvassa el önálló [önálló fürtben, használja a Windows biztonsági Windows biztonságos](service-fabric-windows-cluster-windows-security.md)

## <a name="client-to-node-security"></a>Ügyfél-csomópont biztonsági
Hitelesíti az ügyfeleket, és egy ügyfél és az egyes csomópontok hello fürt közötti kommunikáció biztonságossá tételére. Az ilyen típusú biztonsági hitelesíti, és ügyfél-kommunikáció, amely biztosítja, hogy csak hitelesített felhasználók férhetnek hozzá hello fürt és a telepített hello fürtön hello alkalmazások biztonságossá tételére. Ügyfelek egyedileg azonosítja a Windows hitelesítő adatokat vagy a tanúsítvány hitelesítő adatokat.

![Ügyfél-csomópont kommunikációs ábrája][Client-to-Node]

Az Azure vagy az önálló fürtökön fut a Windows rendszert futtató fürtöket választhat [tanúsítvány biztonsági](https://msdn.microsoft.com/library/ff649801.aspx) vagy [Windows biztonsági](https://msdn.microsoft.com/library/ff649396.aspx).

### <a name="client-to-node-certificate-security"></a>Ügyfél-csomópont tanúsítvány biztonság
 Ügyfél-csomópont tanúsítvány biztonsági létrehozásakor hello fürt hello Azure-portálon keresztül, Resource Manager-sablonok vagy egy önálló JSON-sablon egy rendszergazdai ügyféltanúsítvány és/vagy felhasználói ügyféltanúsítványt megadásával van konfigurálva.  hello felügyeleti ügyfél és a felhasználói ügyféltanúsítványok adja meg, hogy nem lehet ugyanaz, mint hello elsődleges és másodlagos tanúsítványokat állít be [-csomópontok biztonsági](#node-to-node-security) az ajánlott eljárás. Alapértelmezés szerint hello fürt a csomópontok biztonság hozzáadja a tanúsítványokat toohello engedélyezte az ügyfél felügyeleti tanúsítványok listája.

Toohello fürt hello felügyeleti tanúsítvány használatával csatlakozó ügyfelek teljes körű hozzáférési toomanagement képességek állnak a rendelkezésére.  Toohello fürt hello írásvédett felhasználó ügyféltanúsítvány használatával csatlakozó ügyfelek csak olvasási hozzáféréssel toomanagement képességekkel rendelkezik. Ezek a tanúsítványok más szóval hello szerepkör körrel hozzáférés-vezérlés (RBAC) a cikk későbbi részében leírt használják.

Az Azure olvasási [fürt beállítása az Azure Resource Manager-sablon használatával](service-fabric-cluster-creation-via-arm.md) toolearn hogyan tooconfigure tanúsítvány biztonsági fürtben.

Windows Server olvassa el önálló [önálló fürtben, X.509-tanúsítványokat használ a Windows biztonságos](service-fabric-windows-cluster-x509-security.md)

### <a name="client-to-node-azure-active-directory-aad-security-on-azure"></a>Ügyfél-csomópont Azure Active Directory (AAD) biztonsági az Azure-on
Azure-on futó fürtök is biztosíthassa hozzáférés toohello felügyeleti végpontok Azure Active Directory (AAD) használatával. Lásd: [fürt beállítása az Azure Resource Manager-sablon használatával](service-fabric-cluster-creation-via-arm.md) hogyan toocreate hello szükséges AAD összetevők olvashat, hogyan toopopulate őket során fürt létrehozása, és hogyan tooconnect toothose ezután fürtök.

## <a name="security-recommendations"></a>Biztonsági javaslatok
Az Azure-fürtök esetén ajánlott használni, amikor az AAD biztonsági tooauthenticate ügyfelek és tanúsítványokat-csomópontok biztonsági.

Az önálló Windows Server javasoljuk használja a Windows biztonsági csoporttal fürtök fiókok (GMA) felügyelt, ha Windows Server 2012 R2 és az Active Directory. Ellenkező esetben továbbra is használni Windows biztonsági Windows-fiókkal.

## <a name="role-based-access-control-rbac"></a>A szerepköralapú hozzáférés-vezérlést (RBAC)
Hozzáférés-vezérlés lehetővé teszi, hogy hello rendszergazda toolimit hozzáférés toocertain fürt fürtműveletekben hello fürt biztonságosabbá tétele a felhasználók különböző csoportok számára. Két különböző hozzáférési ellenőrzési típusok támogatottak tooa fürt csatlakozó ügyfeleket: rendszergazdai szerepkör és a felhasználói szerepkör.

A rendszergazdák teljes hozzáféréssel toomanagement képességek (beleértve az olvasási/írási képességek) rendelkeznek. Felhasználók, alapértelmezés szerint rendelkezik csak olvasási hozzáféréssel toomanagement képességek (például lekérdezési lehetőségek), és hello képességét tooresolve alkalmazások és szolgáltatások.

Megadhatja hello rendszergazdai és felhasználói ügyfél szerepkörök a fürt létrehozása hello időpontjában, adja meg a külön identitások (tanúsítványok, AAD stb.) az egyes. Hello alapértelmezett hozzáférés-vezérlési beállításokkal, és hogyan toochange hello alapértelmezett beállítások további információkért lásd: [szerepköralapú hozzáférés-vezérlés a Service Fabric-ügyfelek](service-fabric-cluster-security-roles.md).

## <a name="x509-certificates-and-service-fabric"></a>X.509-tanúsítványokat és a Service Fabric
X.509 digitális tanúsítványok a gyakran használt tooauthenticate ügyfelek és kiszolgálók és tooencrypt és üzenetek digitális aláírását. Ezekről a tanúsítványokról a további részletekért lépjen túl[tanúsítványok használata](http://msdn.microsoft.com/library/ms731899.aspx).

Néhány fontos dolgot tooconsider:

* Termelési számítási feladatokhoz rendszert futtató fürtöket a használt tanúsítványok legyen a megfelelően konfigurált Windows kiszolgálói tanúsítvány szolgáltatások segítségével létrehozott vagy beszerzett egy jóváhagyott [tanúsítvány hitelesítésszolgáltatói (CA)](https://en.wikipedia.org/wiki/Certificate_authority).
* Soha ne használja az összes ideiglenes vagy tanúsítványok tesztelése éles üzemben eszközöket, például a MakeCert.exe használatával létrehozott.
* Használhat önaláírt tanúsítványt, de csak akkor tegye ezt tesztfürtökön és nem éles környezetben.

### <a name="server-x509-certificates"></a>Kiszolgáló X.509-tanúsítványokat
Kiszolgáló tanúsítványának rendelkeznie kell elsődleges feladat hello hitelesítése a kiszolgálón (csomópont) tooclients, vagy a kiszolgáló (csomópont) tooa kiszolgáló (csomópont) hitelesítése. Hello kezdeti ellenőrzéseket, amikor egy ügyfél vagy a csomópont hitelesíti a csomópont egyik toocheck hello értékének hello köznapi név hello tulajdonos mezőben. A közös név vagy hello tanúsítványok tulajdonosának alternatív neveinek egyike hello engedélyezett köznapi nevek listája a jelen kell lennie.

hello alábbi cikkből megtudhatja, hogyan toogenerate tanúsítványok a tulajdonos alternatív neve (SAN): [hogyan tooadd a tulajdonos alternatív neve tooa biztonságos LDAP tanúsítvány](http://support.microsoft.com/kb/931351).

hello tulajdonos mezőben több érték, a minden egyes előtagként az inicializálási tooindicate hello értéktípus. A leggyakrabban hello inicializálásának "CN" köznapi neve; például "CN = www.contoso.com". Akkor is a hello tulajdonos mező toobe üres. Ha hello választható tulajdonos alternatív neve mező nem üres, hello hello tanúsítvány köznapi nevének és a tulajdonos alternatív neve egy tételt kell tartalmaznia. A DNS-név értékként lett megadva.

hello kívánt célok mező hello tanúsítvány hello értékének tartalmaznia kell egy megfelelő értéket, például a "Server-hitelesítés" vagy "Ügyfél-hitelesítés".

### <a name="client-x509-certificates"></a>Ügyfél X.509-tanúsítványokat
Ügyféltanúsítványok nem általában egy külső hitelesítésszolgáltató által kiállított. Ehelyett hello személyes a hello aktuális felhasználó helyének általában a tárolóban ügyféltanúsítványokat, melyeket egy legfelső szintű hitelesítésszolgáltatóval, az "Ügyfél-hitelesítés" tervezett céljával. hello ügyfél használhatja ez a tanúsítvány, ha kölcsönös hitelesítésre szükség.

> [!NOTE]
> A Service Fabric-fürtök összes felügyeleti műveleteihez kiszolgálói tanúsítványokat igényel. Ügyféltanúsítványok felügyeleti nem használható.
> 
> 

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->


## <a name="next-steps"></a>Következő lépések
Ez a cikk a fürt biztonsági tájékoztatást. A következő,


1.  [fürt létrehozása az Azure Resource Manager-sablon használatával](service-fabric-cluster-creation-via-arm.md) 
2.  [Azure-portálon](service-fabric-cluster-creation-via-portal.md).

<!--Image references-->
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png
