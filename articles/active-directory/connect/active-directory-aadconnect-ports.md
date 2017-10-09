---
title: "Hibrid identitás szükséges portok és protokollok - Azure |} Microsoft Docs"
description: "Ezen a lapon a szükséges toobe nyissa meg az Azure AD Connect-portokon technikai referencia jellegű oldal"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: de97b225-ae06-4afc-b2ef-a72a3643255b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: billmath
ms.openlocfilehash: 9c62b74b45e7f266e3a55baa2db07a9ff1c9c6aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="hybrid-identity-required-ports-and-protocols"></a>Hibrid identitás – szükséges portok és protokollok
a következő dokumentum hello hello szükséges portok és protokollok hibrid identitáskezelési megoldás megvalósításának műszaki hivatkozás. A következő ábra hello használja, majd tekintse át a toohello megfelelő tábla.

![Mi az az Azure AD Connect?](./media/active-directory-aadconnect-ports/required3.png)

## <a name="table-1---azure-ad-connect-and-on-premises-ad"></a>1. táblázat – Azure AD Connect és a helyszíni AD
Ez a táblázat ismerteti hello portok és hello Azure AD Connect-kiszolgáló közötti kommunikációhoz szükséges protokollok és a helyszíni AD.

| Protokoll | Portok | Leírás |
| --- | --- | --- |
| DNS |53 (TCP/UDP) |DNS-keresések hello cél erdőre. |
| Kerberos |88 (TCP/UDP) |Kerberos hitelesítési toohello AD-erdőben. |
| MS-RPC |135-ÖS (TCP/UDP) |Ha is kötődik toohello AD-erdő hello Azure AD Connect varázsló hello a kezdeti konfiguráció során és a jelszó-szinkronizálás során használt. |
| LDAP |389 (TCP/UDP) |Adatok importálása az Active Directoryból használatos. A Kerberos-aláírás és a titkosítással titkosítja az adatokat. |
| RPC | 445 (TCP/UDP) |Zökkenőmentes SSO toocreate által használt számítógépfiók hello AD-erdőben. |
| LDAP-/ SSL |636 (TCP/UDP) |Adatok importálása az Active Directoryból használatos. hello adatátvitel van aláírását és titkosítását. Csak akkor használható, ha SSL-t használ. |
| RPC |49152 és 65535 (véletlenszerű magas RPC Port)(TCP/UDP) |Az Azure AD Connect, amikor is kötődik toohello AD erdők hello a kezdeti konfiguráció során, és a jelszó-szinkronizálás során használt. Lásd: [KB929851](https://support.microsoft.com/kb/929851), [KB832017](https://support.microsoft.com/kb/832017), és [KB224196](https://support.microsoft.com/kb/224196) további információt. |

## <a name="table-2---azure-ad-connect-and-azure-ad"></a>2. táblázat – Azure AD Connect és az Azure AD
Ez a táblázat hello portok és protokollok hello Azure AD Connect-kiszolgáló és az Azure AD közötti kommunikációhoz szükséges.

| Protokoll | Portok | Leírás |
| --- | --- | --- |
| HTTP |80 (TCP/UDP) |Használt toodownload CRL-ek (tanúsítvány-visszavonási listákat) tooverify SSL-tanúsítványokat. |
| HTTPS |443(TCP/UDP) |Az Azure ad-val használt toosynchronize. |

Listáját URL-címek és IP-címek tooopen van szüksége a tűzfal, lásd: [Office 365 URL-címei és IP-címtartományok](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="table-3---azure-ad-connect-and-ad-fs-federation-serverswap"></a>3. táblázat – Azure AD Connect és az AD FS összevonási kiszolgálók/WAP
Ez a táblázat hello portok és protokollok hello Azure AD Connect-kiszolgáló és az AD FS összevonási/WAP-kiszolgálókkal közötti kommunikációhoz szükséges.  

| Protokoll | Portok | Leírás |
| --- | --- | --- |
| HTTP |80 (TCP/UDP) |Használt toodownload CRL-ek (tanúsítvány-visszavonási listákat) tooverify SSL-tanúsítványokat. |
| HTTPS |443(TCP/UDP) |Az Azure ad-val használt toosynchronize. |
| A Rendszerfelügyeleti webszolgáltatások |5985 |A WinRM figyelő |

## <a name="table-4---wap-and-federation-servers"></a>4. táblázat – WAP és az összevonási kiszolgálók
Ez a táblázat hello portok és protokollok hello összevonási kiszolgálók és a WAP-kiszolgálókkal közötti kommunikációhoz szükséges.

| Protokoll | Portok | Leírás |
| --- | --- | --- |
| HTTPS |443(TCP/UDP) |A hitelesítéshez használt. |

## <a name="table-5---wap-and-users"></a>5. táblázat – WAP és felhasználók
Ez a táblázat hello portok és protokollok, a felhasználók és a WAP-kiszolgálókkal hello közötti kommunikációhoz szükséges.

| Protokoll | Portok | Leírás |
| --- | --- | --- |
| HTTPS |443(TCP/UDP) |Eszköz-hitelesítéshez használt. |
| TCP |A 49443-AS (TCP) |Tanúsítvány-hitelesítéshez használt. |

## <a name="table-6a--6b---pass-through-authentication-with-single-sign-on-sso-and-password-hash-sync-with-single-sign-on-sso"></a>Tábla 6a & 6b - áteresztő hitelesítés az egyszeri bejelentkezés (SSO) és a Jelszókivonat-szinkronizálás az egyszeri bejelentkezés (SSO)
a következő táblák hello hello portok és protokollok, az Azure AD Connect hello és az Azure AD közötti kommunikációhoz szükséges ismerteti.

### <a name="table-6a---pass-through-authentication-with-sso"></a>Tábla 6a - áteresztő hitelesítéses egyszeri Bejelentkezéssel
|Protokoll|Portszám|Leírás
| --- | --- | ---
|HTTP|80|Engedélyezze a kimenő HTTP-forgalom biztonsági ellenőrzés céljából, például az SSL Használatát. Is szükséges hello összekötő automatikus frissítési funkció toofunction megfelelően.
|HTTPS|443| Kimenő HTTPS-forgalom engedélyezése és letiltása hello szolgáltatás, összekötők regisztrálása, összekötő frissítések letöltése és kezelési minden felhasználói bejelentkezési kérelem hasonló műveletek engedélyezése.

Ezenkívül az Azure AD Connect kell toobe képes toomake közvetlen IP kapcsolatok toohello [Azure adatközpont IP-címtartományok](https://www.microsoft.com/en-us/download/details.aspx?id=41653).

### <a name="table-6b---password-hash-sync-with-sso"></a>Tábla 6b - alapú egyszeri Jelszókivonat-szinkronizálás

|Protokoll|Portszám|Leírás
| --- | --- | ---
|HTTPS|443| Engedélyezze az egyszeri bejelentkezés regisztrációt (csak hello SSO regisztrációs folyamat esetén szükséges).

Ezenkívül az Azure AD Connect kell toobe képes toomake közvetlen IP kapcsolatok toohello [Azure adatközpont IP-címtartományok](https://www.microsoft.com/en-us/download/details.aspx?id=41653). Ebben az esetben ez a tulajdonság csak hello SSO-regisztrációs folyamathoz szükséges.

## <a name="table-7a--7b---azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tábla 7a & 7b - a (AD FS/szinkronizálási) Azure AD Connect Health ügynök és az Azure AD
hello alábbi táblázatok bemutatják az hello végpontok, portok és protokollok, amelyek szükségesek az Azure AD Connect Health-ügynökök és az Azure AD közti kommunikációhoz.

### <a name="table-7a---ports-and-protocols-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tábla 7a - portok és protokollok a Azure AD Connect Health agent számára (AD FS/szinkronizálása) és az Azure AD
Ez a táblázat ismerteti a következő kimenő portok és protokollok hello Azure AD Connect Health-ügynökök és az Azure AD közötti kommunikációhoz szükséges hello.  

| Protokoll | Portok | Leírás |
| --- | --- | --- |
| HTTPS |443(TCP/UDP) |Kimenő |
| Azure Service Bus |5671 (TCP/UDP) |Kimenő |

### <a name="7b---endpoints-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>7b - végpontok Azure AD Connect Health Agent számára (AD FS/szinkronizálása) és az Azure AD
A végpontok listájáért lásd: [hello Azure AD Connect Health-ügynök követelmények szakasz hello](../connect-health/active-directory-aadconnect-health-agent-install.md#requirements).

