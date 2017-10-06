---
title: "Hibrid identitás: a címtár-integrációs eszközök összehasonlítása | Microsoft Docs"
description: "A lap biztosítanak egy átfogó táblázatot, amely összehasonlítja a hello különböző címtár-integrációs eszközök címtár-integráció használható."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 1e62a4bd-4d55-4609-895e-70131dedbf52
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 18ac0a0f58726eceb85510df516e8db71429b313
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="hybrid-identity-directory-integration-tools-comparison"></a>Hibrid identitás: a címtár-integrációs eszközök összehasonlítása
Hello évek hello címtár-integrációs eszközök megnövelte és továbbfejlődtek.  Ez a dokumentum toohelp biztosít ezen eszközök egyesített nézetét, és minden rendelkezésre álló hello szolgáltatásainak összehasonlítása.

<!-- hello hardcoded link is a workaround for campaign ids not working in acom links-->

> [!NOTE]
> Az Azure AD Connect magában foglalja a hello összetevők és a korábban Dirsync és AAD Sync néven kibocsátott funkciót. Ezek az eszközök vannak már nem érhetők el különállóan, és minden jövőbeli fejlesztések szerepelni fog a frissítések tooAzure AD Connect, úgy, hogy mindig tudja, hol tooget hello a legújabb funkciókat.
> 
> A DirSync és az Azure AD Sync elavultnak számít. További információt [itt](active-directory-aadconnect-dirsync-deprecated.md) találhat.
> 
> 

Az egyes hello táblák kulcsot következő hello használata.

● = Már elérhető  
JK = Jövőbeli kiadás  
NyE = Nyilvános előzetes verzió  

## <a name="on-premises-toocloud-synchronization"></a>Szinkronizálás a helyszíni tooCloud
| Szolgáltatás | Azure Active Directory Connect | Azure Active Directory Synchronization Services (AAD Sync) | Azure Active Directory Synchronization Tool (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Toosingle csatlakozás helyszíni AD-erdőhöz |● |● |● |● |● |
| Csatlakozás toomultiple a helyszíni AD-erdőhöz |● |● | |● |● |
| Csatlakozás toomultiple a helyszíni Exchange-szervezethez |● | | | | |
| Csatlakozás toosingle helyszíni LDAP-címtár |JK | | |● |● |
| Csatlakozás toomultiple helyszíni LDAP-címtárakhoz |JK | | |● |● |
| Csatlakozás tooon helyszíni AD és a helyszíni LDAP-címtárakhoz |JK | | |● |● |
| Toocustom rendszerek (pl. SQL, Oracle, MySQL stb.) |JK | | |● |● |
| Ügyfél által meghatározott attribútumok szinkronizálása (címtárbővítmények) |● | | | | |
| Csatlakozás tooon helyszíni HR (SAP, Oracle eBusiness, PeopleSoft) |JK | | |● |● |
| Támogatja a FIM szinkronizálási szabályokat és összekötőket tooon helyszíni rendszerekre történő üzembe helyezéséhez. | | | |● |● |

## <a name="cloud-tooon-premises-synchronization"></a>Felhő tooOn helyszíni szinkronizálás
| Szolgáltatás | Azure Active Directory Connect | Azure Active Directory Synchronization Services | Azure Active Directory Synchronization Tool (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Eszközök visszaírása |● | |● | | |
| Attribútum visszaírása (hibrid Exchange-környezethez) |● |● |● |● |● |
| Csoportobjektumok visszaírása |● | | | | |
| Jelszavak visszaírása (önkiszolgáló jelszó-visszaállításból (SSPR) és jelszómódosításból) |● |● | | | |

## <a name="authentication-feature-support"></a>Hitelesítési funkciók támogatása
| Funkció | Azure Active Directory Connect | Azure Active Directory Synchronization Services | Azure Active Directory Synchronization Tool (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Jelszó-szinkronizálás egyetlen helyszíni AD-erdőhöz |● |● |● | | |
| Jelszó-szinkronizálás több helyszíni AD-erdőhöz |● |● | | | |
| Egyszeri bejelentkezés összevonással |● |● |● |● |● |
| Jelszavak visszaírása (SSPR-ből és jelszómódosításból) |● |● | | | |

## <a name="set-up-and-installation"></a>Beállítás és telepítés
| Szolgáltatás | Azure Active Directory Connect | Azure Active Directory Synchronization Services | Azure Active Directory Synchronization Tool (DirSync) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|
| Támogatja a tartományvezérlőre történő telepítést |● |● |● | |
| Támogatja az SQL Express használatát |● |● |● | |
| Egyszerű frissítés DirSync-ről |● | | | |
| Rendszergazdai felület tooWindows kiszolgálónyelveket honosítása |● |● |● | |
| Végfelhasználó UX tooWindows kiszolgálónyelveket honosítása | | | |● |
| A Windows Server 2008 és a Windows Server 2008 R2 támogatása |● Szinkronizáláshoz, nem összevonáshoz |● |● |● |
| A Windows Server 2012 és a Windows Server 2012 R2 támogatása |● |● |● |● |

## <a name="filtering-and-configuration"></a>Szűrés és konfigurálás
| Szolgáltatás | Azure Active Directory Connect | Azure Active Directory Synchronization Services | Azure Active Directory Synchronization Tool (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Szűrés a tartományokon és szervezeti egységeken |● |● |● |● |● |
| Szűrés az objektumok attribútumértékein |● |● |● |● |● |
| Minimálisan szükséges szinkronizált attribútumok toobe (MinSync) engedélyezése |● |● | | | |
| Különböző sablonok toobe attribútumfolyamokhoz alkalmazott engedélyezése |● |● | | | |
| Az AD tooAzure AD a folyamból az attribútumok eltávolításának engedélyezése |● |● | | | |
| Az attribútumfolyamok speciális testreszabásának engedélyezése |● |● | |● |● |

## <a name="next-steps"></a>Következő lépések
További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).

