---
title: "Active Directory naplójelentési eseményei aaaAzure |} Microsoft Docs"
description: "Elérhető megtekintésére és letöltésére az Azure Active Directory események naplózása"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: 307eedf7-05bc-448d-a84d-bead5a4c5770
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/16/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: 4a84cce2be56bde006164abf10ad1e72ca6e99bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-audit-report-events"></a>Azure Active Directory naplózási jelentésének eseményei
*Ebben a dokumentációban hello része [Azure Active Directory-jelentéskészítés – útmutató](active-directory-reporting-guide.md).*

hello Azure Active Directory naplózási jelentés segítségével az ügyfelek azonosítása az Azure Active Directoryban történt privilegizált műveleteket. A privilegizált műveletekhez változásokat jogosultságszint-emelés (például a szerepkör létrehozása vagy a jelszó alaphelyzetbe állítása), házirend-konfigurációt (például jelszóházirendek) vagy módosításokat toodirectory konfigurációját (például toodomain összevonási beállításainak módosítása) módosítása. hello jelentések hello naplórekordot hello eseménynév, hello szereplő hello művelet, hello változás, és hello dátuma és időpontja (UTC) által érintett hello célerőforrása elvégző adja meg. Azokat a felhasználókat, a naplózott események az Azure Active Directory hello keresztül képes tooretrieve hello listáját [Azure Portal](https://portal.azure.com/)leírtak szerint [megtekintése a vizsgálati naplókban](active-directory-reporting-azure-portal.md).

## <a name="list-of-audit-report-events"></a>Naplózási jelentés események listája
<!--- audit event descriptions should be in hello past tense --->

| Események | Esemény leírása |
| --- | --- |
| **Felhasználói események** | |
| Felhasználó hozzáadása |Egy felhasználó toohello directory hozzá. |
| Felhasználó törlése |A felhasználó törölte a hello könyvtárból. |
| Licenc tulajdonságainak beállítása |Egy felhasználó hello címtárban hello licenc tulajdonságainak beállítása. |
| Felhasználói jelszó alaphelyzetbe állítása |Egy felhasználó hello címtárban hello jelszó visszaállítása. |
| Felhasználói jelszó módosítása |Hello hello könyvtárban felhasználó jelszavának módosítása. |
| Változás felhasználói licenc |Hello licenccel tooa felhasználó hello címtárban megváltozott. milyen licencek lettek frissítve, keresse meg hello toosee [frissítés felhasználói](#update-user-attributes) az alábbi tulajdonságok |
| Felhasználó módosítása |Frissíti egy felhasználó hello címtárban. [Lásd az alábbi](#update-user-attributes) attribútumok frissíthető. |
| Set kényszerített felhasználói jelszó módosítása |Hello tulajdonság, amely arra kényszeríti a felhasználó toochange állítható be a jelszavát a bejelentkezés. |
| Felhasználói hitelesítő adatok frissítése |A jelszó megváltozott hello |
| **Események csoportosítása** | |
| Csoport hozzáadása |Olyan csoportot hozott létre hello könyvtárban. |
| Frissítési csoporthoz |Hello könyvtárban csoport frissítése. toosee milyen csoport tulajdonságok lettek frissítve, tekintse meg a túl[csoport tulajdonságainak naplózása](#update-group-attributes) hello az alábbi részben |
| Csoport törlése |A csoport törölve hello könyvtárból. |
| CreateGroupSettings |Létrehozott csoport beállításai |
| UpdateGroupSettings |Csoport beállításainak frissítése. toosee, milyen beállítások lettek frissítve, tekintse meg a túl[csoport tulajdonságainak naplózása](#update-group-attributes) hello az alábbi részben |
| DeleteGroupSettings |Törölt beállításai |
| SetGroupLicense |Állítsa be a csoport licenc. |
| SetGroupManagedBy |Állítsa be a felhasználó által kezelt csoport toobe |
| AddGroupMember |A hozzáadott tag toogroup |
| RemoveGroupMember |Tag eltávolítása a csoportból |
| AddGroupOwner |A hozzáadott tulajdonos toogroup |
| RemoveGroupOwner |A csoportból eltávolított tulajdonosa |
| **Alkalmazás-események** | |
| Egyszerű szolgáltatásnév hozzáadása |A szolgáltatás egyszerű toohello directory hozzá. |
| Távolítsa el az egyszerű szolgáltatásnév |Hello directory eltávolítani egy egyszerű szolgáltatást. |
| Adja hozzá a szolgáltatás egyszerű hitelesítő adatait |A hozzáadott hitelesítő adatok tooa szolgáltatás egyszerű. |
| Távolítsa el a szolgáltatás egyszerű hitelesítő adatait |Egy egyszerű szolgáltatásnév eltávolított hitelesítő adatokat. |
| Delegálás bejegyzés hozzáadása |Létrehozott egy [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) hello könyvtárban. |
| Készlet delegálása bejegyzés |Frissítve egy [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) hello könyvtárban. |
| Delegálás bejegyzés eltávolítása |Törölt egy [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) hello könyvtárban. |
| **Szerepkör-események** | |
| Szerepkör tag tooRole hozzáadása |Olyan tooa directory szerepkörhöz hozzá. |
| Szerepkör szerepkör tagjának eltávolítása |A felhasználó eltávolítja a címtár szerepkörrel. |
| AddRoleDefinition |A hozzáadott szerepkör-definíció. |
| UpdateRoleDefinition |Szerepkör-definíció frissítése. toosee, milyen beállítások lettek frissítve, tekintse meg a túl[szerepkör definíció tulajdonságainak naplózása](#update-role-definition-attributes) hello az alábbi részben |
| DeleteRoleDefinition |Szerepkör-definíció törlése. |
| AddRoleAssignmentToRoleDefinition |Szerepkör-hozzárendelés toorole definíció hozzá. |
| RemoveRoleAssignmentFromRoleDefinition |A szerepkör-definíció eltávolított szerepkör-hozzárendelés. |
| AddRoleFromTemplate |A hozzáadott szerepkör sablonból. |
| UpdateRole |Frissített szerepkört. |
| AddRoleScopeMemberToRole |Hatókört használó tag toorole hozzá. |
| RemoveRoleScopedMemberFromRole |Szerepkör hatókörrel rendelkező tagot eltávolítani. |
| **Események (az összes új esemény)** | |
| AddDevice |A hozzáadott eszköz. |
| UpdateDevice |Frissített eszköz. toosee milyen eszköztulajdonságok frissítve, tekintse meg a túl[eszköztulajdonságok Audited](#update-device-attributes) hello az alábbi részben |
| DeleteDevice |Törölt eszköz. |
| AddDeviceConfiguration |A hozzáadott eszközök konfigurációját. |
| UpdateDeviceConfiguration |Frissített eszközök konfigurációját. toosee milyen eszköz konfigurációs tulajdonságok lettek frissítve, tekintse meg a túl[eszköz konfigurációs tulajdonságok Audited](#update-device-configuration-attributes) hello az alábbi részben |
| DeleteDeviceConfiguration |Törölt eszközök konfigurációját. |
| AddRegisteredOwner |Regisztrált felhasználó toodevice hozzá. |
| AddRegisteredUsers |A regisztrált felhasználók toodevice hozzá. |
| RemoveRegisteredOwner |Regisztrált felhasználó távolíthatja el. |
| RemoveRegisteredUsers |A regisztrált felhasználók távolíthatja el. |
| RemoveDeviceCredentials |Távolítsa el a hitelesítő adatai. |
| **B2B események** | |
| Kötegelt meghívja feltöltve. |A rendszergazda feltöltötte toopartner felhasználók küldött meghívót toobe tartalmazó fájlt. |
| Kötegelt küldték el feldolgozásra. |Meghívókat toopartner felhasználók tartalmazó fájl feldolgozása megtörtént. |
| Külső felhasználói meghívásához. |A külső felhasználó lett meghívott toohello könyvtár. |
| Külső felhasználói a meghívás beváltani. |A külső felhasználó rendelkezik a meghívó toohello címtár beváltott. |
| Vegye fel a külső felhasználó toogroup. |A külső felhasználó rendelték tooa tagságcsoportot hello könyvtárban. |
| Rendelje hozzá a külső felhasználó tooapplication. |A külső felhasználó közvetlen hozzáférést tooan alkalmazás rendelték. |
| Ugrásszerű bérlő létrehozásakor. |Egy új bérlőt hello meghívó érvényesítési hozta az Azure ad-ben. |
| A felhasználó ugrásszerű létrehozása. |A felhasználó hozta található meglévő bérlő az Azure AD hello meghívó érvényesítési. |
| **Adminisztratív egységei (az összes új esemény)** | |
| AddAdministrativeUnit |Adja hozzá a felügyeleti egység. |
| UpdateAdministrativeUnit |Frissítés felügyeleti egység. toosee milyen felügyeleti egység tulajdonságai frissítve, tekintse meg a túl[naplózva felügyeleti egység tulajdonságai](#update-administrative-unit-attributes) hello az alábbi részben |
| DeleteAdministrativeUnit |Törölje a felügyeleti egységet. |
| AddMemberToAdministrativeUnit |Adja hozzá a tag tooadministrative elemet. |
| RemoveMemberFromAdministrativeUnit |Tag eltávolítása a felügyeleti egység. |
| **Directory eseményei** | |
| Partner toocompany hozzáadása |Egy partner toohello directory hozzá. |
| Távolítsa el a Partner a vállalati |Eltávolítja a partner hello könyvtárból. |
| DemotePartner |Partner fokozni. |
| Tartomány toocompany hozzáadása |A tartomány toohello directory hozzá. |
| Távolítsa el a tartomány a vállalati |Hello directory eltávolítani egy tartományhoz. |
| Tartomány frissítése |A tartomány hello címtár frissítése. toosee milyen tartomány tulajdonságok lettek frissítve, tekintse meg a túl[ellenőrzött tartomány tulajdonságai](#update-domain-attributes) hello az alábbi részben |
| Set tartományi hitelesítéshez |Hello alapértelmezett beállítása a hello vállalat megváltozott. |
| Állítsa be a vállalat kapcsolattartási adatai |Vállalati szintű kapcsolattartási beállítások megadása. Ez magában foglalja a Microsoft Online Services marketing, műszaki értesítések tartozó e-mail-címeket. |
| A tartomány összevonási beállításai |A tartomány hello összevonási beállításainak frissítése. |
| Ellenőrizze a tartomány |Hello címtár tartomány ellenőrzése. |
| E-mailek ellenőrzött tartomány ellenőrzése |E-mail ellenőrzése hello könyvtárra vonatkozó tartomány ellenőrzése. |
| A vállalat DirSyncEnabled jelző beállítása |Lehetővé teszi, hogy a címtár az Azure AD Sync hello tulajdonság beállítása. |
| Jelszószabályzat beállítása |Felhasználói jelszavak hosszát vagy a karakter megkötések beállítása. |
| Cégadatok beállítása |Frissített hello vállalati szintű információkat. Lásd: hello [Get-MsolCompanyInformation](https://msdn.microsoft.com/library/azure/dn194126.aspx) további részleteket a PowerShell-parancsmagot. |
| SetCompanyAllowedDataLocation |Set vállalati adatok helye engedélyezett. |
| SetCompanyDirSyncEnabled |Állítsa a DirSyncEnabled jelzőjét. |
| SetCompanyDirSyncFeature |Állítsa be a DirSync szolgáltatást. |
| SetCompanyInformation |Set vállalati adatokat. |
| SetCompanyMultiNationalEnabled |Állítsa be a vállalati nemzetközi szolgáltatás engedélyezve van. |
| SetDirectoryFeatureOnTenant |Állítsa be a címtár szolgáltatást tenant. |
| SetTenantLicenseProperties |Bérlői licenc tulajdonságainak beállítása. |
| CreateCompanySettings |Hozzon létre a vállalati beállítások |
| UpdateCompanySettings |Vállalati beállítások frissítése. toosee milyen vállalati tulajdonságok lettek frissítve, tekintse meg a túl[naplózva vállalati tulajdonságok](#update-company-attributes) hello az alábbi részben |
| DeleteCompanySettings |Törölje a vállalati beállítások |
| SetAccidentalDeletionThreshold |Megadhatja a véletlen törlés küszöbértéket. |
| SetRightsManagementProperties |Rights management beállításainak megadása. |
| PurgeRightsManagementProperties |Rights management tulajdonságok kiürítése. |
| UpdateExternalSecrets |Külső kulcsok frissítése |
| **Házirend-események (az összes új esemény)** | |
| AddPolicy |Szabályzat hozzáadása. |
| UpdatePolicy |Házirend frissítése. |
| Szabályzat törlése |Házirend törlése. |
| AddDefaultPolicyApplication |Adja hozzá a házirend tooapplication. |
| AddDefaultPolicyServicePrincipal |Adja hozzá a házirend tooservice rendszerbiztonsági tag. |
| RemoveDefaultPolicyApplication |Távolítsa el a házirend-alkalmazás. |
| RemoveDefaultPolicyServicePrincipal |Távolítsa el a házirendet a szolgáltatásnevet. |
| RemovePolicyCredentials |Távolítsa el a házirend hitelesítő adatokat. |

## <a name="audit-report-retention"></a>Naplózási jelentés adatmegőrzési

Hello megőrzési kapcsolatos legfrissebb információkat, lásd: [Azure Active Directory-jelentés adatmegőrzési](active-directory-reporting-retention.md).


Az ügyfelek a naplóesemények tárolásához hosszabb megőrzési ideig iránt érdeklődik hello Reporting API lehet használt tooregularly lekéréses naplózási események külön adattárba. Lásd: [Ismerkedés a Reporting API hello](active-directory-reporting-api-getting-started.md) részleteiről.

## <a name="properties-included-with-each-audit-event"></a>Összes naplózási eseményt részét képező tulajdonságok
| Tulajdonság | Leírás |
| --- | --- |
| Dátum és idő |hello dátum és idő, amely hello naplózási esemény történt |
| Aktor |hello felhasználó vagy szolgáltatás egyszerű által végrehajtott hello művelet |
| Műveletek |hello műveletet végeznie |
| cél |hello felhasználó vagy szolgáltatás egyszerű, amely hello műveletet hajtott végre |

## <a name="update-user-attributes"></a>"A felhasználó frissítése" attribútumok
"frissítés user" Hello naplózási eseményt milyen felhasználói attribútumok frissültek kapcsolatban további tudnivalókat tartalmaz. Minden attribútum is hello korábbi értéket, és hello új érték szerepel.

| Attribútum | Leírás |
| --- | --- |
| AccountEnabled |hello felhasználói bejelentkezés. |
| AssignedLicense |Az összes hozzárendelt toohello felhasználói licenc. |
| AssignedPlan |Hello licencek eredő Service-csomagokról hozzárendelt toohello felhasználó. |
| LicenseAssignmentDetail |A licencek hozzárendelt toohello felhasználói adatokat. Például ha Csoportalapú licencelési volt szó, ebbe beletartozik nyújtott hello licenc hello csoportja. |
| Mobiltelefon |hello felhasználói mobiltelefon. |
| OtherMail |hello felhasználó másodlagos e-mail címét. |
| OtherMobile |felhasználó alternatív mobiltelefon hello. |
| StrongAuthenticationMethod |Hitelesítési módszerek hello felhasználó által beállított többtényezős hitelesítést, például a hang hívja, SMS vagy ellenőrző kódot a mobilalkalmazások listáját. |
| StrongAuthenticationRequirement |Többtényezős hitelesítés van érvényben, ha engedélyezve van, vagy a felhasználó le van tiltva. |
| StrongAuthenticationUserDetails |hello felhasználói phone number, alternatív telefonszámát és e-mail-címet használja, a többtényezős hitelesítés és a jelszó-változtatási hitelesítési. |
| StrongAuthenticationPhoneAppDetail |Részletes forof phone-alkalmazásokat regisztrálni tooperform 2FA bejelentkezés |
| TelephoneNumber |hello felhasználó telefonszámát. |
| AlternativeSecurityId |Hello objektum alternatív biztonsági azonosítója. |
| CreationType |Létrehozási módszer hello felhasználó (vagy meghívó ugrásszerű). |
| InviteTicket |Meghívót jegyek hello felhasználó listája. |
| InviteReplyUrl |URL-címek tooreply meghívó elfogadták listája. |
| InviteResources |Erőforrások toowhich hello felhasználó kérték. |
| LastDirSyncTime |Legutóbbi hello objektum frissült a mérvadó hello (ügyfél helyi) szinkronizálása miatt könyvtár. |
| MSExchRemoteRecipientType |A Maps tooMSO címzett típusok. Tekintse meg a túl [MSO címzett típusok] https://msdn.microsoft.com/library/microsoft.office.interop.outlook.recipient.type.aspx címzett típusok |
| PreferredDataLocation |hello hello felhasználó, csoport, ismerős, nyilvános mappa elsődleges hely vagy eszköz adatokat. |
| ProxyAddresses |hello címét, amellyel egy Exchange Server címzett objektum egy idegen levelezőrendszer elismert. |
| StsRefreshTokensValidFrom |Végzi a token visszavonási információk frissítése, mert egyetlen most minősülnek kiadott STS frissítési jogkivonatokat lejárt. |
| UserPrincipalName |hello egyszerű felhasználónév, amely egy felhasználó egy Internet-stílusú bejelentkezési nevét. |
| Userstate objektumhoz |Felhasználói jóváhagyásra vár/PendingAcceptance/elfogadva/PendingVerification állapota. |
| UserStateChangedOn |Utolsó módosítás tooUserState időbélyegzője. Tootrigger életciklus munkafolyamatok használt. |
| UserType |Felhasználó típusa. A tag (0), (1), Vendég Viral(2). |

## <a name="update-group-attributes"></a>"A frissítési csoport" attribútumok
| Attribútum | Leírás |
| --- | --- |
| Besorolás |hello besorolást egy egyesített csoportot (HBI, MBI stb.). |
| Leírás |Emberek számára olvasható leíró kifejezések hello objektumról. |
| displayName |az objektum hello megjelenített neve |
| DirSyncEnabled |Meghatározza, hogy szinkronizálás az egy mérvadó (ügyfél helyi) directory. |
| GroupLicenseAssignment |Licenc-hozzárendelést egy csoport |
| GroupType |Csoportja, egységes (0) |
| IsMembershipRuleLocked |Azt jelzi, hogy hello MembershipRule tulajdonság hello önkiszolgáló csoportkezelési szolgáltatás van beállítva, és a felhasználók által nem módosítható. Ha GroupType tartalmaz GroupType.DynamicMembership alkalmazható csak toogroups |
| IsPublic |Ez a jelző tooindicate, ha hello csoport nyilvános/titkos. |
| LastDirSyncTime |Legutóbbi hello objektum frissült a mérvadó hello (az ügyfél, a helyszínen) szinkronizálása miatt könyvtár. |
| Levelezés |Elsődleges e-mail címét |
| MailEnabled |Azt jelzi, hogy ez a csoport e-mail funkció. |
| mailNickname |Cím könyv objektum kézjegy, általában az e-mail névben hello megelőző hello része "@" szimbólummal. |
| MembershipRule |Alfeladat hello feltételek hello önkiszolgáló csoportkezelési felügyeleti szolgáltatás toodetermine tagok kell tartoznia, mint toothis csoport által használt karakterlánc. További információ a IsMembershipRuleLocked. Ha GroupType tartalmaz GroupType.DynamicMembership csak toogroups alkalmazható. |
| MembershipRuleProcessingState |Ez a csoport számára történő feldolgozásakor. a tagsági hello állapotának meghatározása hello önkiszolgáló csoportkezelési felügyeleti szolgáltatása által definiált enum érték. Ha GroupType tartalmaz GroupType.DynamicMembership csak toogroups alkalmazható. |
| ProxyAddresses |hello címét, amellyel egy Exchange Server címzett objektum egy idegen levelezőrendszer elismert. |
| RenewedDateTime |Ha egy csoport legutóbb megújított Timestamp típusú rekord. |
| SecurityEnabled |Azt jelzi, hogy hello csoporttagság befolyásolhatják felhasználását engedélyezési döntésekhez. |
| WellKnownObject |A Deleted object, előre meghatározott egyik a kijelölő létrehozott. |

## <a name="update-device-attributes"></a>"Az eszköz frissítése" attribútumok
| Attribútum | Leírás |
| --- | --- |
| AccountEnabled |Azt jelzi, hogy a rendszerbiztonsági tag hitelesítheti. |
| CloudAccountEnabled |Azt jelzi, hogy a rendszerbiztonsági tag hitelesítheti. Eszközállapotokat az InTune, amikor a helyszínen hello eszköz értékűre. |
| CloudDeviceOSType |Eszköztípusba hello hello például Windows RT, iOS operációs rendszer alapján. Ha a beállítás egy felhőalapú szolgáltatás (például az Intune), ez az attribútum a lesz a mérvadó hello könyvtárban DeviceOSType. |
| CloudDeviceOSVersion |Hello az operációs rendszer verzióját. Ha a beállítás egy felhőalapú szolgáltatás (például az Intune), ez az attribútum a lesz a mérvadó hello könyvtárban DeviceOSVersion. |
| CloudDisplayName |Hello displayName LDAP attribútum értéke. Ha a beállítás egy felhőalapú szolgáltatás (például az Intune), ez az attribútum lesz mérvadó displayName hello könyvtárában. |
| CloudCreated |Azt jelzi, hogy hello objektum felhőszolgáltatások hozta-e. |
| CompliantUntil |Milyen időpontig eszköz megfelelőnek minősül. |
| DeviceMetadata |Egyéni metaadat hello eszköz |
| DeviceObjectVersion |Ez az attribútum használt tooidentify hello séma hello eszköz verziója telepítve. |
| DeviceOSType |Eszköztípusba hello hello például Windows RT, iOS operációs rendszer alapján. Által írt hello Eszközregisztrációs szolgáltatás és a tervezett toobe frissítette hello MDM felügyeleti szolgáltatás, vagy STS világos felügyeleti szolgáltatást. |
| DeviceOSVersion |Hello az operációs rendszer verzióját. Által írt hello Eszközregisztrációs szolgáltatás és a tervezett toobe frissítette hello MDM felügyeleti szolgáltatás, vagy STS világos felügyeleti szolgáltatást. |
| DevicePhysicalIds |Többértékű attribútum szánt hello fizikai eszköz toostore azonosítói. Ebbe beletartozik a BIOS-azonosítókat, TPM-ujjlenyomatok, a hardver egyedi azonosítók stb. |
| DirSyncEnabled |Meghatározza, hogy szinkronizálás egy mérvadó (ügyfél, a helyszínen) directory. |
| displayName |az objektum hello megjelenített neve |
| IsCompliant |Ez az attribútum használt toomanage hello mobileszközök kezelési állapota hello eszköz. |
| IsManaged |Ez az attribútum tooindicate hello felügyeli azt egy felhőalapú mobileszköz-kezelést. |
| LastDirSyncTime |Legutóbbi hello objektum frissült a mérvadó hello (az ügyfél, a helyszínen) szinkronizálása miatt könyvtár. |

## <a name="update-device-configuration-attributes"></a>"Az eszköz konfigurációs frissítése" attribútumok
| Attribútum | Leírás |
| --- | --- |
| MaximumRegistrationInactivityPeriod |hello legfeljebb hány nap lehet inaktív megállapították eltávolítása előtt. |
| RegistrationQuota |Szabályzat egy-egy felhasználóhoz engedélyezett eszköz-regisztrációinak száma toolimit hello használt. |

## <a name="update-service-principal-configuration-attributes"></a>"A szolgáltatás egyszerű konfigurációjának frissítése" attribútumok
| Attribútum | Leírás |
| --- | --- |
| AccountEnabled |Azt jelzi, hogy a rendszerbiztonsági tag hitelesítheti. |
| AppPrincipalId |Külső, az alkalmazás által meghatározott identitás rendszerbiztonsági tag. |
| displayName |az objektum hello megjelenített neve |
| ServicePrincipalName |Egy egyszerű szolgáltatásnevét, amely tartalmazza a "név/authority" ahol nevét megadja egy alkalmazás osztály értékét, és hatóság tartalmaz legalább állomásnév [: port] vagy a "name", amely megadja, hogy hello szolgáltatás egyszerű azonosítója. |

## <a name="update-app-attributes"></a>"Az alkalmazás frissítése" attribútumok
| Attribútum | Leírás |
| --- | --- |
| AppAddress |kijelölt tooa egyszerű szolgáltatásnév (átirányítási URL-címek) cím hello készlete. |
| Alkalmazásazonosító |Alkalmazás Azonosítóját a hello alkalmazás |
| AppIdentifierUri |Alkalmazás URI, amely hello alkalmazás azonosítja.  Akkor általában hello alkalmazás URL-CÍMEN. |
| AppLogoUrl |hello kép URL-címe hello alkalmazás embléma CDN tárolja. |
| AvailableToOtherTenants |Igaz hello alkalmazás több-bérlős alkalmazás (azaz használhatja más bérlők). |
| displayName |hello megjelenítési nevet az alkalmazás nevét |
| Jogosultság |Alkalmazás jogosultságok listáját. |
| ExternalUserAccountDelegationsAllowed |Jelzi, hogy erőforrás alkalmazás egy megbízható, és delegálás bejegyzéseket külső felhasználói fiókokat hozhat létre. |
| GroupMembershipClaims |hello csoportházirend tagsági jogcímeket. |
| PublicClient |Értéke TRUE, ha hello ügyfél nem képes lépést tartani titkos (azaz nem bizalmas ügyfél OAuth2.0) |
| RecordConsentConditions |Hozzájárulás feltételek hello által meghatározott típusú szerződési feltételek: None (0), SilentConsentForPartnerManagedApp(1). Ez az érték hello Graph API sémában elérhetővé tehető, és lehet, hogy csak a bérlői rendszergazda által beállított vagy módosult. |
| RequiredResourceAccess |XML-tartalom hello RequiredResourceAccess tulajdonság értéke. |
| Webalkalmazás |Amennyiben az értéke igaz, azt jelzi, hogy az alkalmazás egy webalkalmazást. |
| WwwHomepage |hello elsődleges weblapot. |

## <a name="update-role-attributes"></a>"Szerepkör módosítása" attribútumok
| Attribútum | Leírás |
| --- | --- |
| AppAddress |kijelölt tooa egyszerű szolgáltatásnév (átirányítási URL-címek) cím hello készlete. |
| BelongsToFirstLoginObjectSet |Igaz értéke esetén azt jelzi, hogy ez az objektum tartozik objektumok szükséges tooenable bejelentkezési hello első rendszergazda egy új bérlő toohello készletét. |
| A beépített |Azt jelzi, hogy e hello élettartamát az objektum tulajdonosa hello rendszer. |
| Leírás |Emberek számára olvasható leíró kifejezések hello objektumról. |
| displayName |az objektum hello megjelenített neve |
| mailNickname |Cím könyv objektum kézjegy, általában az e-mail névben hello megelőző hello része "@" szimbólummal. |
| RoleDisabled |Azt jelzi, hogy hello szerepkör figyelmen kívül hagyja a hozzáférés-ellenőrzés céljából. |
| RoleTemplateId |Hello szerepkör sablonok identitása. |
| ServiceInfo |Szolgáltatásspecifikus kiépítési információkat, amelyeket esetleg használni MOAC és/vagy más szolgáltatáspéldány (az azonos vagy azoktól eltérő hello szolgáltatás típusok). |
| TaskSetScopeReference |Azonosítja a TaskSet és egy szerepkör vagy RoleTemplate társított hatókörök csoportja. |
| ValidationError |Egy összevont szolgáltatás egy nem átmeneti, szolgáltatással kapcsolatos hiba hello tulajdonságok vagy a hivatkozás leválasztása egy objektum rendszergazda művelet tooresolve leíró közzétett adatokat. |
| WellKnownObject |A Deleted object, előre meghatározott egyik a kijelölő létrehozott. |

## <a name="update-role-definition-attributes"></a>"Szerepkör-definíció frissítése" attribútumok
| Attribútum | Leírás |
| --- | --- |
| AssignableScopes |A RoleDefinition tooa rendszerbiztonsági hozzárendelésekor hivatkozható engedélyezési hatókörök gyűjteménye. |
| displayName |az objektum hello megjelenített neve |
| GrantedPermissions |A RoleDefinition engedélyekre. |

## <a name="update-administrative-unit-attributes"></a>"Az adminisztratív egység frissítése" attribútumok
| Attribútum | Leírás |
| --- | --- |
| Leírás |Ez a tulajdonság akkor frissül, amikor módosítja egy felügyeleti egység hello leírását. |
| displayName |Ez a tulajdonság akkor frissül, amikor egy felügyeleti egység hello nevének módosítása. |

## <a name="update-company-attributes"></a>"Frissítés vállalati" attribútumok
| Attribútum | Leírás |
| --- | --- |
| AllowedDataLocation |Egy hely, mely hello vállalati felhasználók engedélyezettek toobe kiépítve. |
| AuthorizedServiceInstance |A csomag esetleg telepített szolgáltatás példányok toowhich nevei. |
| DirSyncEnabled |Meghatározza, hogy szinkronizálás egy mérvadó (ügyfél, a helyszínen) directory. |
| DirSyncStatus |Meghatározza, hogy szinkronizálás cím könyv objektumok a bérlő környezetben egy mérvadó (ügyfél, a helyszínen) directory; hello DirSyncEnabled tulajdonság vállalati objektumok bővítése. |
| DirSyncFeatures |Bit jelző tookeep nyomon az engedélyezett és letiltott dirsync szolgáltatásokat hello bérlő számára. |
| DirectoryFeatures |Engedélyezett vagy letiltott címtárfunkciókat. |
| DirSyncConfiguration |Tartalmazza az összes DirSync konfigurációs adott toohello aktuális bérlőhöz. |
| displayName |az objektum hello megjelenített neve |
| IsMnc |Egy logikai jelző készlet túl "true" iff hello vállalati hello nemzetközi vállalati szolgáltatás engedélyezve van. |
| ObjectSettings |Hello objektum alkalmazható toohello hatókörén beállítások gyűjteménye. |
| PartnerCommerceUrl |Kereskedelmi webhely URL-cím toohello Partner. |
| PartnerHelpUrl |Súgó webhely URL-cím toohello Partner. |
| PartnerSupportEmail |URL-cím toohello Partner támogatási e-mail cím. |
| PartnerSupportTelephone |URL-cím toohello Partner támogatás telefonon. |
| PartnerSupportUrl |Támogatási webhely URL-cím toohello Partner. |
| StrongAuthenticationDetails |Részletek kapcsolódó tooStrongAuthentication. |
| StrongAuthenticationPolicy |Erős hitelesítés házirend hello vállalat számára. |
| TechnicalNotificationMail |E-Mail cím toonotify műszaki kapcsolatos ügyek tooa vállalati. |
| TelephoneNumber |Az telefonszámokat, amelyek megfelelnek a hello ITU ajánlás E.123. |
| TenantType |a bérlő hello típusa. Ha ez az érték nincs megadva, a hello bérlői egy olyan cég. Ellenkező esetben a lehetséges értékek:: (0) MicrosoftSupport, SyndicatePartner (1), (2) BreadthPartner BreadthPartnerDelegatedAdmin (3) ResellerPartnerDelegatedAdmin (4) ValueAddedResellerPartnerDelegatedAdmin (5). |
| VerifiedDomain |DNS-nevek készlete tooa vállalati kötött. |

## <a name="update-domain-attributes"></a>"A tartomány frissítése" attribútumok
| Attribútum | Leírás |
| --- | --- |
| Funkciók |Bit jelzők hello tartomány leíró hello képességeit, ha van ilyen. |
| Alapértelmezett |Azt jelzi, hogy hello tartomány hello alapértelmezett érték; például hello alapértelmezett UserPrincipalName utótag amikor a rendszergazda létrehoz egy új felhasználót a MOAC. |
| Kezdeti |Azt jelzi, hogy hello tartomány hello kezdeti tartomány hello vállalat által OCP. hello kezdeti tartomány a Microsoft Online tartomány; egyedi altartomány e.g.contoso3.microsoftonline.com. |
| LiveType |Hello megfelelő Windows Live névtér, ha van ilyen típusú. |
| Név |Hello végpont azonosítója. |
| PasswordNotificationWindowDays |hello hány nap elteltével jelszó lejár hello felhasználó értesítést kap. |
| PasswordValidityPeriodDays |hello hány napig jelszó ideális előtt meg kell változtatni. |

Auditálási rekordok a szükséges vezérlőt a sok megfelelőségi szabályzat. A felhasználóknak hello Azure Active Directory naplózási jelentés toomeet a megfelelőségi szabályzat ajánlott tartozó hello ügyfél nyújt hello ügyfél hello példányát a Súgó téma másolatát exportált toohelp ismertetik hello jelentés címtárnaplózási jelentés részletes adatokat. Hello auditor szeretné toounderstand hello megfelelőségi szabályzat Azure jelenleg megfelel-e, ha közvetlen hello auditor toohello [megfelelőségi oldalának](https://azure.microsoft.com/support/trust-center/compliance/) a Microsoft Azure Trust Center hello.

