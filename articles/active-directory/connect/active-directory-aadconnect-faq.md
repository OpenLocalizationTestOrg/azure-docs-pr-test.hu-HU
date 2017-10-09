---
title: "Az Azure Active Directory Connect: Gyakori kérdések – |} Microsoft Docs"
description: "Ezen a lapon az Azure AD Connect vonatkozó gyakran ismételt kérdések."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
ms.assetid: 4e47a087-ebcd-4b63-9574-0c31907a39a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 39c0b127d3dcf6f45607ad8b4647a9ad79dfc732
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-azure-active-directory-connect"></a>Gyakori kérdések az Azure Active Directory Connect

## <a name="general-installation"></a>Általános telepítési
**K: telepítési működnek, ha hello Azure AD globális rendszergazda 2FA engedélyezve van?**  
A hello buildekben a 2016. februári Ez támogatott.

**K: van egy módon tooinstall felügyelet az Azure AD Connect?**  
Már csak a támogatott tooinstall az Azure AD Connect telepítővarázsló hello használata. Felügyelet nélküli és a csendes telepítés nem támogatott.

**K: erdővel rendelkezem ahol tartománya nem érhető el. Hogyan kell telepíteni az Azure AD Connect?**  
A hello buildekben a 2016. februári Ez támogatott.

**K: hello Active Directory tartományi szolgáltatások health ügynök munka a server core?**  
Igen. Hello ügynök telepítése után hello regisztrációs folyamat a következő PowerShell-parancsmag hello segítségével hajthatja végre: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

## <a name="network"></a>Network (Hálózat)
**K: van egy tűzfal, a hálózati eszköz, vagy valami mást, amely korlátozza a hello maximális idő kapcsolatok maradhat, nyissa meg a hálózaton. Mennyi ideig kell a ügyfél ügyféloldali időkorlát küszöbértéke lehet, ha az Azure AD Connect használatával?**  
Minden hálózati szoftvert, fizikai eszközök vagy bármely más, a kapcsolatok nyitva maradhat hello maximális idő közötti kapcsolatot kell használnia a küszöbérték legalább 5 perc (300 másodperc) korlátok hello hello Azure AD Connect-ügyfelet futtató kiszolgáló és az Azure Active Directory. Ez a korábban kiadott tooall Microsoft Identity szinkronizálási eszközöket is vonatkozik.

**K: vannak (egy címke tartományok) által támogatott?**  
Nem, az Azure AD Connect nem támogatja a helyi erdők/tartományok által használatával.

**K: vannak "pontozott" nevű NetBios támogatott?**  
Nem, az Azure AD Connect nem támogatja a helyi erdők/tartományok ahol hello NetBIOS-név pontot tartalmaz "." hello nevében.

## <a name="federation"></a>Összevonási
**K: Mi a teendő ha jelenik meg egy e-mailt, amely toorenew jóváhagyás kérése az Office 365 tanúsítvány**  
Használja a hello hello útmutatást [megújítani a tanúsítványokat](active-directory-aadconnect-o365-certs.md) című hogyan toorenew hello tanúsítványt.

**K: van "automatikus frissítése függő" állítsa be az Office 365 függő entitáshoz. Van tootake bármely művelet, amikor a jogkivonat-aláíró tanúsítványa automatikusan visszaállítja a?**  
Használja a hello cikk hello útmutatást [megújítani a tanúsítványokat](active-directory-aadconnect-o365-certs.md).

## <a name="environment"></a>Környezet
**K: az informatikai támogatott toorename hello kiszolgáló, az Azure AD Connect telepítése után?**  
Nem. Hello kiszolgáló nevének módosítása OK hello szinkronizálási motor toonot képes tooconnect toohello SQL-adatbázis és hello szolgáltatás nem lesz képes toostart.

## <a name="identity-data"></a>Azonosító adatok
**K: az Azure AD-hello UPN (userPrincipalName) attribútuma nem egyezik a helyszínen UPN - miért hello?**  
Ezek a cikkek lásd:

* [A felhasználónevek nem egyeznek az Office 365, Azure vagy Intune hello helyszíni egyszerű Felhasználónévvel vagy másodlagos bejelentkezési Azonosítóval](https://support.microsoft.com/en-us/kb/2523192)
* [A felhasználói fiók toouse másik összevont tartományt egyszerű hello módosítása után a módosítások nem hello Azure Active Directory szinkronizáló eszköz által szinkronizálása](https://support.microsoft.com/en-us/kb/2669550)

Beállíthatja úgy is az Azure AD tooallow hello szinkronizálási motor tooupdate hello userPrincipalName leírtak [az Azure AD Connect szinkronizálási szolgáltatás szolgáltatások](active-directory-aadconnectsyncservice-features.md).

**K: az informatikai támogatott toosoft egyezés a helyszíni AD-csoport vagy partner objektumok meglévő Azure AD-csoport vagy partner objektummal?**  
Nem, ez jelenleg nem támogatott.

**K: az informatikai támogatott toomanually ImmutableId attribútum beállítása a meglévő Azure AD csoport vagy partner objektumok toohard egyezni tooon helyszíni AD-csoport vagy partner objektumok?**  
Nem, ez jelenleg nem támogatott.



## <a name="custom-configuration"></a>Egyéni konfiguráció
**K: hol vannak hello PowerShell-parancsmagok az Azure AD Connect dokumentált?**  
Ezen a helyen hello parancsmagokat hello kivételével más található az Azure AD Connect PowerShell-parancsmagok nem támogatottak a ügyfél használja.

**K: használhatok "Kiszolgáló exportálási/kiszolgáló importálás" található *Synchronization Service Managert* toomove konfigurációs kiszolgáló között?**  
Nem. Ez a beállítás az összes konfigurációs beállítások beolvasása sikertelen lesz, és nem használható. Inkább helyette hello varázsló toocreate hello alapkonfiguráció hello második kiszolgálón használja és használatát hello szinkronizálási szabály szerkesztő toogenerate PowerShell parancsfájlok toomove egyéni szabályok kiszolgálók között. Lásd: [áttelepítési éppen](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).

**K: kell gyorsítótárazzák a jelszavakat az Azure-bejelentkezés hello lap, és ez megelőzhető ugyanis tartalmaz. a jelszó bemeneti elemnek hello autocomplete = "false" attribútum?**</br>
Jelenleg nem támogatjuk hello jelszó beviteli mezőt, beleértve a hello autocomplete címke módosítása hello HTML attribútumait. Jelenleg dolgozunk egy szolgáltatás, amely lehetővé teszi a egyéni Javascript, amely lehetővé teszi a tooadd bármely attribútum toohello jelszó mező. 2017 elérhető későbbi része legyen.

**K: az Azure bejelentkezési oldal hello a felhasználók számára, akik korábban már sikeresen bejelentkezett felhasználónevek jelennek meg.  Ez a viselkedés kikapcsolható?**</br>
Jelenleg nem támogatott módosítása hello hello bejelentkezési lap HTML-attribútumok. Jelenleg dolgozunk egy szolgáltatás, amely lehetővé teszi a egyéni Javascript, amely lehetővé teszi a tooadd bármely attribútum toohello jelszó mező. 2017 elérhető későbbi része legyen.

**K: van egy módon tooprevent egyidejű munkamenetek?**</br>
Nem.



## <a name="troubleshooting"></a>Hibaelhárítás
**K: hogyan kaphat segítséget az Azure AD Connect?**

[Keresési hello Microsoft Tudásbázis (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* Keresse meg a Microsoft Tudásbázis (KB) hello műszaki megoldások toocommon javítás / csere problémák az Azure AD Connect-támogatással kapcsolatos.

[A Microsoft Azure Active Directory-fórumok](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* Keressen, és keresse meg a technikai kérdések és hello Közösségtől felveszi vagy saját kérdés feltevése kattintva [Itt](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Az Azure AD Connect ügyfélszolgálata](https://manage.windowsazure.com/?getsupport=true)

* Használja a kapcsolat tooget támogatás hello Azure-portálon keresztül.

