---
title: "aaaFAQs - Azure Active Directory tartományi szolgáltatások |} Microsoft Docs"
description: "Azure Active Directory tartományi szolgáltatások kapcsolatos gyakori kérdések"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 48731820-9e8c-4ec2-95e8-83dba1e58775
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: maheshu
ms.openlocfilehash: 42a6d659f6408bf694cb2aa6ec24bff7a76b0565
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-domain-services-frequently-asked-questions-faqs"></a>Az Azure Active Directory tartományi szolgáltatások: Gyakran ismételt kérdések (GYIK)
Ezen a lapon hello Azure Active Directory tartományi szolgáltatások kapcsolatos gyakori kérdésekre. Tartsa biztonsági frissítések keresése.

### <a name="troubleshooting-guide"></a>Hibaelhárítási útmutató
Tekintse meg a tooour [hibaelhárítási útmutatója](active-directory-ds-troubleshooting.md) megoldások toocommon problémákat észlelt, amikor konfigurálása és felügyelete az Azure AD tartományi szolgáltatásokat.

### <a name="configuration"></a>Konfiguráció
#### <a name="can-i-create-multiple-domains-for-a-single-azure-ad-directory"></a>Hozható létre több tartomány egyetlen Azure AD-címtár?
Nem. Csak egyetlen Azure AD tartományi szolgáltatások által kiszolgált egyetlen tartományt hozhat létre Azure AD-címtárban.  

#### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-resource-manager-virtual-network"></a>Egy Azure Resource Manager virtuális hálózatot az Azure AD tartományi szolgáltatások engedélyezése
Nem. Azure AD tartományi szolgáltatások csak akkor engedélyezhető, a klasszikus Azure virtuális hálózatban. Hello klasszikus virtuális hálózatot tooa erőforrás-kezelő virtuális hálózat virtuális hálózati társviszony-létesítési toouse a felügyelt tartományok egy erőforrás-kezelő virtuális hálózat használatával is elérheti.

#### <a name="can-i-enable-azure-ad-domain-services-in-a-federated-azure-ad-directory-i-use-adfs-tooauthenticate-users-for-access-toooffice-365-can-i-enable-azure-ad-domain-services-for-this-directory"></a>Engedélyezhető az Azure AD tartományi szolgáltatások egy összevont Azure AD-címtár? Az AD FS tooauthenticate felhasználók hozzáférési tooOffice 365 használatával. Ez a könyvtár az Azure AD tartományi szolgáltatások engedélyezése
Nem. Azure AD tartományi szolgáltatások toohello jelszókivonatait a felhasználói fiókok, tooauthenticate felhasználók NTLM vagy Kerberos használatával kell elérni. Egy összevont könyvtárban a jelszó-kivonatok nem tárolódnak hello Azure AD-címtár. Azure AD tartományi szolgáltatásokat, ezért az ilyen Azure AD-címtártól nem működik.

#### <a name="can-i-make-azure-ad-domain-services-available-in-multiple-virtual-networks-within-my-subscription"></a>Tehetek Azure AD tartományi szolgáltatások több virtuális hálózat érhető el az előfizetésen belül?
hello szolgáltatás nem támogatja közvetlenül az ebben a forgatókönyvben. Azure AD tartományi szolgáltatások egyszerre csak egy virtuális hálózaton rendelkezésre álló lehet. Azonban előfordulhat, hogy konfigurálnia több virtuális hálózatok tooexpose Azure AD tartományi szolgáltatások tooother virtuális hálózatok közötti kapcsolatot. Ez a cikk ismerteti, hogyan zajlik [csatlakozzon az Azure virtuális hálózatairól](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

#### <a name="can-i-enable-azure-ad-domain-services-using-powershell"></a>PowerShell-lel Azure AD tartományi szolgáltatások engedélyezése
PowerShell/automatikus telepítése az Azure AD tartományi szolgáltatások jelenleg nem érhető el.

#### <a name="is-azure-ad-domain-services-available-in-hello-new-azure-portal"></a>Az Azure AD tartományi szolgáltatások rendelkezésre hello új Azure-portálon?
Nem. Azure AD tartományi szolgáltatások csak hello konfigurálható [a klasszikus Azure portálon](https://manage.windowsazure.com). Hello tooextend támogatása várhatóan [Azure-portálon](https://portal.azure.com) a jövőbeli hello.

#### <a name="can-i-add-domain-controllers-tooan-azure-ad-domain-services-managed-domain"></a>Tartományi vezérlők tooan Azure AD tartományi szolgáltatások által kezelt tartomány adhat hozzá?
Nem. Azure AD tartományi szolgáltatások által biztosított hello tartomány egy felügyelt tartomány. Nem kell tooprovision, konfigurálásához, vagy ellenkező esetben a tartományvezérlők a tartomány - e felügyeleti tevékenységek Microsoft által biztosított szolgáltatás kezelése. További tartományvezérlők (olvasási és írási vagy olvasási) hello felügyelt tartomány, ezért nem adható hozzá.

### <a name="administration-and-operations"></a>Felügyelet és műveletek
#### <a name="can-i-connect-toohello-domain-controller-for-my-managed-domain-using-remote-desktop"></a>Lehet-e csatlakozni toohello tartományvezérlő a távoli asztal használata felügyelt tartományomat?
Nem. Nincs engedélyek tooconnect toodomain tartományvezérlőjén hello felügyelt távoli asztalon keresztül. Hello "AAD DC rendszergazdák" csoport tagjai felügyelhetik hello által kezelt tartomány AD felügyeleti eszközök, például az Active Directory felügyeleti központban (ADAC) hello vagy AD PowerShell segítségével. Ezek az eszközök hello "Távoli kiszolgálófelügyelet eszközeivel" telepített szolgáltatás a Windows server toohello felügyelt tartományhoz csatlakozott.

#### <a name="ive-enabled-azure-ad-domain-services-what-user-account-do-i-use-toodomain-join-machines-toothis-domain"></a>Azure AD tartományi szolgáltatások engedélyezése I. Milyen felhasználói fiók használata toodomain csatlakozás gépek toothis tartományhoz?
Hello felügyeleti csoport "AAD DC rendszergazdák" Csatlakozás tartományhoz gép is. Ezen felül a csoport tagjai kapnak távoli asztal eléréséhez toomachines illesztett toohello tartományt is.

#### <a name="do-i-have-domain-administrator-privileges-for-hello-managed-domain-provided-by-azure-ad-domain-services"></a>Azure AD tartományi szolgáltatások által biztosított hello felügyelt tartományhoz tartozó tartományi rendszergazdai jogosultságokkal kell?
Nem. Hello által kezelt tartomány rendszergazdai jogokkal nem kapnak. "Tartományi rendszergazda" és a "vállalati rendszergazda" jogosultságokkal válnak elérhetővé a toouse hello tartományon belül. Meglévő tartomány rendszergazdája vagy vállalati rendszergazdai csoportok belül az Azure AD-címtár is nem kapnak hello tartomány tartományi vagy vállalati rendszergazdai jogosultságokkal.

#### <a name="can-i-modify-group-memberships-using-ldap-or-other-ad-administrative-tools-on-managed-domains"></a>Módosíthatja a csoporttagságot a felügyelt tartományok LDAP és egyéb AD felügyeleti eszközök használatával?
Nem. Csoporttagságok Azure AD tartományi szolgáltatások által kiszolgált tartományok nem módosítható. Ugyanez vonatkozik a felhasználói attribútumok hello. Csoporttagságok vagy a felhasználói attribútumokat azonban változtassa meg az Azure ad-ben vagy a helyszíni tartományban. Az ilyen változtatások olyan automatikusan szinkronizált tooAzure AD tartományi szolgáltatásokban.

#### <a name="how-long-does-it-take-for-changes-i-make-toomy-azure-ad-directory-toobe-visible-in-my-managed-domain"></a>Mennyi időt vesz igénybe a változásokat I toomy az Azure Active directory toobe a felügyelt tartomány láthatóvá?
Az Azure AD-címtár hello Azure AD felhasználói felületén vagy a PowerShell használatával a végrehajtott változtatásokat szinkronizált tooyour által kezelt tartomány. Ez a szinkronizálási folyamat hello hátterében fut. : A címtár kezdeti szinkronizálása egyszeri hello befejezése után, általában az az Azure AD toobe végzett módosítások körülbelül 20 percet vesz igénybe megjelennek a felügyelt tartományok.

#### <a name="can-i-extend-hello-schema-of-hello-managed-domain-provided-by-azure-ad-domain-services"></a>Ki lehet terjeszteni a hello séma hello Azure AD tartományi szolgáltatások által biztosított felügyelt tartomány?
Nem. hello séma felügyelete a Microsoft hello felügyelt tartomány. Sémakiterjesztések nem támogatottak az Azure AD tartományi szolgáltatásokat.

#### <a name="can-i-modify-or-add-dns-records-in-my-managed-domain"></a>Módosítsa vagy a felügyelt tartomány DNS-rekordok hozzáadásához?
Igen. Hello "AAD DC rendszergazdák" csoportba "DNS-rendszergazda" jogosultságokat kapnak toomodify DNS-rekordokat hello kezelt tartományban. A gépen futó Windows Server illesztett toohello felügyelt tartományhoz toomanage DNS használhatnak hello DNS-kezelő konzolt. DNS-kezelő konzolt, toouse hello "DNS-kiszolgálói eszközök", amely része hello "Távoli kiszolgáló felügyeleti eszközei" választható szolgáltatás hello kiszolgálón telepítse. További információ a [segédprogramok felügyelete, figyelés és hibaelhárítás DNS](https://technet.microsoft.com/library/cc753579.aspx) a TechNet webhelyen érhető el.

### <a name="billing-and-availability"></a>Számlázási és rendelkezésre állás
#### <a name="is-azure-ad-domain-services-a-paid-service"></a>Az Azure AD tartományi szolgáltatások egy fizetős szolgáltatás?
Igen. További információkért lásd: hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/active-directory-ds/).

#### <a name="is-there-a-free-trial-for-hello-service"></a>Van egy ingyenes próbaverzióra hello szolgáltatás?
Ez a szolgáltatás megtalálható hello Azure ingyenes próbaidőszakot. Regisztrálhatnak az egy [ingyenes egy hónapos próbaverzió Azure](https://azure.microsoft.com/pricing/free-trial/).

#### <a name="can-i-get-azure-ad-domain-services-as-part-of-enterprise-mobility-suite-ems-do-i-need-azure-ad-premium-toouse-azure-ad-domain-services"></a>Kaphatok Azure AD tartományi szolgáltatások részét képező nagyvállalati mobilitási csomag (EMS)? Prémium szintű Azure AD toouse az Azure AD tartományi szolgáltatások kell?
Nem. Azure AD tartományi szolgáltatások egy Azure-szolgáltatás – használatalapú fizetés, és nincs EMS-hez. Azure AD tartományi szolgáltatások összes kiadása esetén az Azure AD is használható (szabad, Basic, és, Premium). Óránként, attól függően, hogy a használati kell fizetni.

#### <a name="what-azure-regions-is-hello-service-available-in"></a>Milyen Azure-régiók érhető el hello szolgáltatást?
Tekintse meg a toohello [Azure-szolgáltatások régiónként](https://azure.microsoft.com/regions/#services/) lap toosee álló listát hello Azure-régiók amennyiben rendelkezésre áll-e az Azure AD tartományi szolgáltatásokban.
