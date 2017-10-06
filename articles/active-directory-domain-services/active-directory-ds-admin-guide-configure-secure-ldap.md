---
title: "Biztonságos LDAP (LDAPS) az Azure AD tartományi szolgáltatásokban aaaConfigure |} Microsoft Docs"
description: "Biztonságos LDAP (LDAPS) konfigurálása az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: maheshu
ms.openlocfilehash: 99e44d917b115d7f7a67ff0a5e22c5c9165287e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Biztonságos LDAP (LDAPS) használatos az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz tartozó konfigurálása
Ez a cikk bemutatja, hogyan engedélyezheti biztonságos Lightweight Directory Access Protocol (LDAPS) vonatkozóan az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz. Biztonságos LDAP más néven az "Lightweight Directory Access Protocol (LDAP) Secure Sockets Layer (SSL) rétegen keresztül / Transport Layer Security (TLS)".

## <a name="before-you-begin"></a>Előkészületek
a cikkben szereplő tooperform hello feladatok lesz szüksége:

1. Egy érvényes **Azure-előfizetés**.
2. Egy **Azure AD-címtár** -vagy egy helyszíni címtár vagy egy csak felhőalapú directory szinkronizálva.
3. **Azure AD tartományi szolgáltatások** hello Azure Active directory engedélyezni kell. Ha még nem tette meg, kövesse a hello ismertetett feladatok hello [első lépések útmutató](active-directory-ds-getting-started.md).
4. A **tanúsítvány használt toobe tooenable biztonságos LDAP**.

   * **Ajánlott** -megbízható, nyilvános hitelesítésszolgáltatótól származó tanúsítvány beszerzése. Ez a beállítás értéke nagyobb biztonságot nyújt.
   * Alternatív megoldásként is választhatja, hogy túl[hozzon létre egy önaláírt tanúsítványt](#task-1---obtain-a-certificate-for-secure-ldap) a cikk későbbi részében látható módon.

<br>

### <a name="requirements-for-hello-secure-ldap-certificate"></a>Hello biztonságos LDAP tanúsítványra vonatkozó követelményekről
Biztonságos LDAP engedélyezése előtt a következő irányelveket, hello egy érvényes tanúsítványt beszerezni. Ha tooenable hibákat észlel LDAP biztonságos a felügyelt tartományok az érvénytelen vagy nem megfelelő tanúsítványt.

1. **A megbízható kiállítók** -toohello biztonságos LDAP segítségével felügyelt tartományhoz csatlakozó számítógépek megbízható hatóság hello tanúsítványt kell kiállítani. A szolgáltató egy nyilvános hitelesítésszolgáltatót megbízhatónak ezeket a számítógépeket is lehet.
2. **Élettartam** -hello tanúsítvány illeszkedniük kell legalább a következő 3-6 hónapban hello. Biztonságos LDAP hozzáférés tooyour által kezelt tartomány megszakad, amikor hello tanúsítványa lejár.
3. **Tulajdonos neve** -hello tanúsítvány tulajdonosának neve hello kell lennie a felügyelt tartományok helyettesítő karakter. Például, ha a tartomány neve "contoso100.com", hello tanúsítvány tulajdonosának neve lehet "*. contoso100.com". Állítsa be a hello DNS-nevét (tulajdonos alternatív neve) toothis helyettesítő nevét.
4. **Kulcshasználat** - hello tanúsítványt kell beállítani a következő hello használ - digitális aláírásokra és kulcstitkosítás.
5. **Tanúsítvány célja** -hello tanúsítványnak kell lennie az SSL-kiszolgáló hitelesítésre.

> [!NOTE]
> **Vállalati hitelesítésszolgáltatók:** Azure AD tartományi szolgáltatások nem támogatja a szervezete vállalati hitelesítésszolgáltató által kiállított biztonságos LDAP-tanúsítványok használatával. Ez a korlátozás az oka, hogy hello szolgáltatást nem bízik meg a vállalati hitelesítésszolgáltató egy legfelső szintű hitelesítésszolgáltatóként. 
>
>

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a>1. feladat – biztonságos LDAP tanúsítvány beszerzése
hello első feladat magában foglalja a biztonságos LDAP hozzáférés toohello által kezelt tartomány a tanúsítványok beszerzéséről. Erre két lehetősége van:

* Szerezzen be egy tanúsítványt egy hitelesítésszolgáltatótól. hello hatóság lehet egy nyilvános hitelesítésszolgáltatót.
* Hozzon létre egy önaláírt tanúsítványt.

### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a>Lehetőség (ajánlott) - biztonságos LDAP tanúsítvány beszerzése hitelesítésszolgáltatótól
Ha a szervezet beszerzi a tanúsítványokat nyilvános hitelesítésszolgáltatótól származó, tanúsítványra van szükség tooobtain hello biztonságos LDAP, hogy nyilvános hitelesítésszolgáltatótól.

A tanúsítvány igénylésekor győződjön meg arról, hogy megfelelnek-e leírt követelményeinek hello [hello biztonságos LDAP tanúsítványra vonatkozó követelményekről](#requirements-for-the-secure-ldap-certificate).

> [!NOTE]
> Ügyfélszámítógépek tooconnect toohello felügyelt tartományra biztonságos LDAP igénylő hello biztonságos LDAP tanúsítvány kiállítója hello megbízhatónak kell lennie.
>
>

### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a>B lehetőség – biztonságos LDAP önaláírt tanúsítvány létrehozása
Ha nem tervezi toouse tanúsítványt nyilvános hitelesítésszolgáltatótól származó, választhatja, hogy toocreate önaláírt tanúsítvány biztonságos LDAP.

**Hozzon létre egy önaláírt tanúsítványt PowerShell használatával**

A Windows számítógépen nyisson meg egy új PowerShell-ablakot, **rendszergazda** és hello típus a következő parancsokat, toocreate egy új önaláírt tanúsítványt.

    $lifetime=Get-Date

    New-SelfSignedCertificate -Subject *.contoso100.com -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment -Type SSLServerAuthentication -DnsName *.contoso100.com

Cserélje le a hello minta megelőző, "*. contoso100.com" hello DNS-tartománynévvel a felügyelt tartomány. For example, ha létrehozott egy "contoso100.onmicrosoft.com" nevű felügyelt tartomány, cserélje le a(z)*. contoso100.com "hello parancsfájl a fent a" *. contoso100.onmicrosoft.com ").

![Azure AD címtár kiválasztása](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

az újonnan létrehozott hello önaláírt tanúsítvány kerül hello helyi számítógép tanúsítványtárolójába.


## <a name="next-step"></a>Következő lépés
[2. feladat – hello biztonságos LDAP tanúsítvány tooa exportálása. PFX-fájlból](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md)
