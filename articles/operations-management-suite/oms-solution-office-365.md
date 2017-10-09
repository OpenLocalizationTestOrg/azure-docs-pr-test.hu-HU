---
title: "az Operations Management Suite (OMS) aaaOffice 365 megoldás |} Microsoft Docs"
description: "Ez a cikk részletesen konfigurációs és a hello Office 365-megoldás az OMS Szolgáltatáshoz.  A Naplóelemzési létrehozott hello Office 365-rekordok részletes leírását tartalmazza."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: bwren
ms.openlocfilehash: a1507745251ff015abb785bae8352fea7cea0734
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="office-365-solution-in-operations-management-suite-oms"></a>Az Office 365-megoldás az Operations Management Suite (OMS)

![Az Office 365-embléma](media/oms-solution-office-365/icon.png)

Office 365-megoldás az Operations Management Suite (OMS) hello toomonitor lehetővé teszi az Office 365-környezethez az Naplóelemzési.  

- Az Office 365 fiókok tooanalyze használati minták felhasználói tevékenység figyelésének, valamint a viselkedési trendek azonosításához. Például is kibonthat egy konkrét használati forgatókönyvek, például a szervezet vagy hello legnépszerűbb SharePoint-webhelyek kívül megosztott fájlokat.
- A figyelő rendszergazdai tevékenységek tootrack konfigurációs módosítások vagy magas jogosultsági műveleteket.
- Észleli, és vizsgálja meg a nem kívánt felhasználói viselkedés, amely a szervezet igényeinek testre is szabható.
- Naplózási és megfelelőségi bemutatása. Például hozzáférési fájlműveletek bizalmas fájlokat, amely segíthet a hello naplózási és megfelelőségi eljárásait a figyelheti.
- Hajtsa végre a működési hibaelhárítása fölött Office 365-tevékenységek adatait a szervezet OMS keresési használatával.

## <a name="prerequisites"></a>Előfeltételek
hello következő szükséges előzetes toothis megoldás folyamatban telepítve és konfigurálva.

- Szervezeti Office 365-előfizetéssel.
- Egy felhasználói fiókot, amely globális rendszergazda hitelesítő adatait.
- tooreceive naplózási adatok, kell [naplózásának konfigurálása](https://support.office.com/en-us/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=en-US&rs=en-US&ad=US#PickTab=Before_you_begin) az Office 365-előfizetésben.  Vegye figyelembe, hogy [postaláda-naplózás](https://technet.microsoft.com/library/dn879651.aspx) külön kell beállítani.  Továbbra is hello megoldás telepítése és egyéb adatok be, ha a naplózás nincs konfigurálva.
 


## <a name="management-packs"></a>Felügyeleti csomagok
Ez a megoldás a csatlakoztatott felügyeleti csoportok nem telepíti a felügyeleti csomagok.
  

## <a name="configuration"></a>Konfiguráció
Egyszer, [hello Office 365 megoldás tooyour előfizetés hozzáadása](../log-analytics/log-analytics-add-solutions.md), tooconnect van az Office 365-előfizetéssel tooyour.

1. Adja hozzá a hello Riasztáskezelési megoldás tooyour ismertetett eljárással hello OMS-munkaterület [megoldások hozzáadása](../log-analytics/log-analytics-add-solutions.md).
2. Nyissa meg túl**beállítások** hello OMS-portálon.
3. A **csatlakoztatott források**, jelölje be **Office 365**.
4. Kattintson a **csatlakozni az Office 365**.<br>![Kapcsolat az Office 365](media/oms-solution-office-365/configure.png)
5. Jelentkezzen be egy olyan fiókkal, amely globális rendszergazda az előfizetés 365 tooOffice. 
6. hello előfizetés hello munkaterhelések hello megoldás figyeli a jelennek meg.<br>![Kapcsolat az Office 365](media/oms-solution-office-365/connected.png) 


## <a name="data-collection"></a>Adatgyűjtés
### <a name="supported-agents"></a>Támogatott ügynökök
Office 365-megoldás hello nem adatainak lekérése hello bármelyikét [OMS-ügynököt](../log-analytics/log-analytics-data-sources.md).  Office 365-ről, lekéri az adatokat.

### <a name="collection-frequency"></a>A gyűjtés gyakorisága
Az Office 365 küld egy [webhook értesítési](https://msdn.microsoft.com/office-365/office-365-management-activity-api-reference#receiving-notifications) a részletes adatok tooLog Analytics minden alkalommal létrejön egy bejegyzés.

## <a name="using-hello-solution"></a>Hello megoldással
Office 365 hello megoldás tooyour OMS-munkaterület felvételekor hello **Office 365** csempe megkapja tooyour OMS irányítópult. Ez a csempe a környezetben, és a frissítési megfelelőség szempontjából egy száma és a grafikus ábrázolása hello számítógépek számát jeleníti meg.<br><br>
![Az Office 365 összefoglalás Csempéje](media/oms-solution-office-365/tile.png)  

Kattintson a hello **Office 365** csempe tooopen hello **Office 365** irányítópult.

![Az Office 365 irányítópult](media/oms-solution-office-365/dashboard.png)  

hello irányítópult szerepel a következő táblázat hello hello oszlopa. Egyes oszlopok hello felső tíz riasztások száma egyeztetésével, hogy az oszlop feltételek hello megadva hatókör és a kívánt időtartományt sorolja fel. A napló-keresés, amely hello teljes listáját lásd: minden a hello oszlop hello aljára vagy hello oszlop fejlécére kattintva futtathatja.

| Oszlop | Leírás |
|:--|:--|
| Műveletek | Hello információkat biztosít az összes figyelt Office 365-előfizetés az aktív felhasználók. Tevékenységek időbeli fordulhat elő tud toosee hello számát is.
| Exchange | Exchange Server tevékenységek, például az Add-postaláda engedély, vagy a Set-postaláda hello bontását jeleníti meg. |
| SharePoint | Hello felső tevékenységek jeleníti meg, hogy a felhasználók a SharePoint-dokumentumok végrehajtani. Ha ez a csempe a Lehatolás hello keresés lapon látható ezeket a tevékenységeket, például a hello céldokumentumban és ezzel a tevékenységgel hello helyét hello részleteit. Például a fájl elérhető esemény, fogja tudni toosee hello dokumentumot, amely használatban van, hozzárendelt fiók nevét és IP-címet. |
| Azure Active Directory | Felső felhasználói tevékenységek, például a felhasználói jelszó alaphelyzetbe állítása és a bejelentkezési kísérletek tartalmazza. Részletező, amikor ezek a tevékenységek, például hello eredmény állapot részleteit képes toosee hello fogja. Ez főként akkor hasznos, ha a toomonitor gyanús tevékenységek a kívánt az Azure Active Directoryban. |




## <a name="log-analytics-records"></a>Log Analytics-rekordok

Hello Naplóelemzési munkaterület hello Office 365-megoldás által létrehozott összes rekord rendelkezik egy **típus** a **OfficeActivity**.  Hello **OfficeWorkload** tulajdonság határozza meg, melyik Office 365 szolgáltatás hello rekord túl Exchange, az AzureActiveDirectory, a SharePoint vagy a onedrive vállalati verzió hivatkozik.  Hello **RecordType** tulajdonság határozza meg a hello típusú műveletet.  hello tulajdonságok az egyes művelet függ, és megjelennek-e az alábbi táblázatok hello.

### <a name="common-properties"></a>Általános tulajdonságai
a következő tulajdonságok hello közös tooall Office 365-rekordok.

| Tulajdonság | Leírás |
|:--- |:--- |
| Típus | *OfficeActivity* |
| Ügyfélip | hello hello eszköz hello tevékenység naplózásakor használt IP-címe. hello IP-cím IPv4 vagy IPv6 cím formátumban jelenik meg. |
| OfficeWorkload | Az Office 365 szolgáltatás, amely hello rekord hivatkozik.<br><br>AzureActiveDirectory<br>Exchange<br>SharePoint|
| Művelet | hello hello felhasználói vagy rendszergazdai tevékenység nevét.  |
| A szervezeti | hello GUID a szervezet Office 365-bérlő. Ezt az értéket fogja mindig kell hello ugyanaz a szervezet számára, függetlenül attól, amelyben az informatikai hello Office 365 szolgáltatás következik be. |
| recordType | A végrehajtott művelet típusa. |
| ResultStatus | Azt jelzi, hogy (a megadott hello művelet tulajdonság) hello művelet sikeres volt-e vagy sem. Lehetséges értékek: sikeres, PartiallySucceded vagy sikertelen. Az Exchange felügyeleti tevékenység, hello értéke pedig IGAZ vagy hamis. |
| Felhasználói azonosítóját | hello UPN (egyszerű felhasználónév), amelyek a naplózott; hello rekord hello műveletet végre hello felhasználó például my_name@my_domain_name. Vegye figyelembe, hogy rendszer fiókok (például SHAREPOINT\system vagy NTAUTHORITY\SYSTEM) által végzett tevékenység rekordok is szerepelnek. | 
| UserKey | A UserId tulajdonság hello hello felhasználó alternatív azonosítója.  Például ezt a tulajdonságot a telepítéskor hello passport egyedi azonosító (PUID) és az Exchange a SharePoint, a onedrive vállalati verzió a felhasználók által végrehajtott események. Ez a tulajdonság is adhatnak meg rendszerfiókok által végzett egyéb szolgáltatásaira és a bekövetkező események a UserID tulajdonság hello megegyező értékűnek hello|
| UserType | felhasználó által végrehajtott művelet hello hello típusa.<br><br>Rendszergazda<br>Alkalmazás<br>DcAdmin<br>Rendszeres<br>Foglalt<br>Szolgáltatásnév<br>Rendszer |


### <a name="azure-active-directory-base"></a>Az Azure Active Directory-alap
a következő tulajdonságok hello bejegyzései közös tooall Azure Active Directoryban.

| Tulajdonság | Leírás |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| recordType     | AzureActiveDirectory |
| AzureActiveDirectory_EventType | az Azure AD-esemény hello típusa. |
| Az ExtendedProperties | hello további tulajdonságok hello Azure AD-esemény. |


### <a name="azure-active-directory-account-logon"></a>Az Azure Active Directory-fiók bejelentkezési
Az Active Directory-felhasználók kísérletek toolog ezeket a rekordokat hoz létre.

| Tulajdonság | Leírás |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| recordType     | AzureActiveDirectoryAccountLogon |
| Alkalmazás | hello alkalmazás, amely elindítja a hello fiók bejelentkezési esemény, például az Office 15. |
| Ügyfél | Hello ügyfél adatait eszközt, az eszköz operációs rendszere és hello fiók bejelentkezési esemény hello használt eszköz böngészőjében. |
| LoginStatus | Ez a tulajdonság közvetlenül OrgIdLogon.LoginStatus származik. hello leképezési különböző érdekes bejelentkezési hibák által algoritmusok riasztási megteheti. |
| UserDomain | hello bérlői azonosító adatokat (TII). | 


### <a name="azure-active-directory"></a>Azure Active Directory
Ezeket a rekordokat jönnek létre, amikor változás vagy kiegészítés érkezett tooAzure Active Directory-objektumokat.

| Tulajdonság | Leírás |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| recordType     | AzureActiveDirectory |
| AADTarget | hello felhasználó, aki (hello művelet tulajdonság által azonosított) hello műveletet hajtott végre. |
| Aktor | hello felhasználó vagy szolgáltatás egyszerű által végrehajtott hello művelet. |
| ActorContextId | hello szereplő hello hello szervezet GUID azonosítója, amelyhez tartozik. |
| ActorIpAddress | hello szereplő tartozó IP-cím IPV4 vagy IPv6-cím formátuma. |
| InterSystemsId | hello GUID hello műveletek követő hello Office 365 szolgáltatáson belül összetevői között. |
| IntraSystemId |   hello Azure Active Directory tootrack hello művelet által létrehozott GUID. |
| SupportTicketId | hello ügyfélszolgálatot Jegyazonosító "művelet" helyzetekben hello a művelethez. |
| TargetContextId | hello hello olyan szervezet, amely a célzott felhasználói hello GUID azonosítója, amelyhez tartozik. |


### <a name="data-center-security"></a>Adatközpont-biztonsági
Ezeket a rekordokat Data Center biztonsági naplózási adatokból jönnek létre.  

| Tulajdonság | Leírás |
|:--- |:--- |
| EffectiveOrganization | hello nevét, amely a jogosultságszint-emelés/parancsmag hello hello bérlő irányuló volt. |
| ElevationApprovedTime | Ha jóváhagyva a hello jogosultságszint-emelés hello időbélyegzőjét. |
| ElevationApprover | a Microsoft-kezelő hello neve. |
| ElevationDuration | hello időtartam, mely hello jogosultságszint-emelés aktív volt. |
| ElevationRequestId |  Hello jogosultságszint-emelési kérelem egyedi azonosítója. |
| ElevationRole | hello szerepkör hello jogosultságszint-emelés szükséges. |
| ElevationTime | hello hello jogosultságszint-emelés kezdete. |
| Start_Time | hello kezdési időpontja hello parancsmagok végrehajtása. |


### <a name="exchange-admin"></a>Exchange-rendszergazda
TooExchange konfiguráció módosításai ezeket a rekordokat hoz létre.

| Tulajdonság | Leírás |
|:--- |:--- |
| OfficeWorkload | Exchange |
| recordType     | ExchangeAdmin |
| ExternalAccess |  Meghatározza, hogy hello parancsmag futtatása egy felhasználó a szervezete a Microsoft datacenter személyzet vagy datacenter szolgáltatásfiók, vagy a meghatalmazott rendszergazda. hello False érték hello parancsmagot a szervezetben lévő volt futtatva. hello IGAZ érték hello parancsmagot volt futtatva, datacenter csoporthoz, datacenter szolgáltatásfiók vagy a meghatalmazott rendszergazda. |
| ModifiedObjectResolvedName |  Ez az hello rövid hello objektum felhasználónevét, amelyet hello parancsmaggal. Ez a rendszer naplózza csak akkor, ha hello parancsmag módosítja hello objektum. |
| Szervezetnév | hello bérlői hello neve. |
| OriginatingServer | hello neve hello server mely hello a parancsmag végre lett hajtva. |
| Paraméterek | hello nevét és minden hello parancsmaggal hello műveletek tulajdonság azonosított során használt paraméterek értékét. |


### <a name="exchange-mailbox"></a>Exchange postaláda
Ezeket a rekordokat jönnek létre, amikor módosításait és kiegészítéseit tooExchange postaládák.

| Tulajdonság | Leírás |
|:--- |:--- |
| OfficeWorkload | Exchange |
| recordType     | ExchangeItem |
| ClientInfoString | Hello e-mail ügyfélprogramokban, de a használt tooperform hello művelet, például egy böngésző verziója, az Outlook verzióját és a mobileszköz-információk kapcsolatos információkat. |
| Client_IPAddress | hello hello eszköz hello művelet naplózásakor használt IP-címe. hello IP-cím IPv4 vagy IPv6 cím formátumban jelenik meg. |
| ClientMachineName | hello gép neve, amelyen hello Outlook ügyfélprogramot. |
| ClientProcessName | hello e-mail ügyfélprogramokban, de a használt tooaccess hello postaláda. |
| ClientVersion | hello e-mail ügyfél hello verzióját. |
| InternalLogonType | Belső használatra fenntartva. |
| Logon_Type | Hello postaláda érhető el, és naplózott hello műveletet végre felhasználó hello típusát jelzi. |
| LogonUserDisplayName |    hello hello műveletet végre hello felhasználó felhasználóbarát neve. |
| LogonUserSid | hello hello felhasználó biztonsági azonosítója, akik hello műveletet. |
| MailboxGuid | hello Exchange GUID azonosítója, amelynek hello postaláda. |
| MailboxOwnerMasterAccountSid | Postaláda tulajdonos fiók fő fiók SID azonosítója. |
| MailboxOwnerSid | hello hello tulajdonos biztonsági azonosítója. |
| MailboxOwnerUPN | hello hello postaláda, amelynek birtokló hello személy e-mail címe. |


### <a name="exchange-mailbox-audit"></a>Exchange postaláda naplózása
Ezeket a rekordokat hoz létre egy postaláda naplóbejegyzés jön létre.

| Tulajdonság | Leírás |
|:--- |:--- |
| OfficeWorkload | Exchange |
| recordType     | ExchangeItem |
| Elem | Művelet végrehajtása során melyik hello hello elem jelenti. | 
| SendAsUserMailboxGuid | hello Exchange GUID azonosítója, de a hello postaláda elért toosend e-mailekhez. |
| SendAsUserSmtp | SMTP-cím megszemélyesített hello felhasználó. |
| SendonBehalfOfUserMailboxGuid | hello Exchange GUID azonosítója, de a hello postaláda elért toosend mail nevében. |
| SendOnBehalfOfUserSmtp | SMTP-cím, amelynek a nevében hello e-mail hello felhasználó zajlik. |


### <a name="exchange-mailbox-audit-group"></a>Exchange postaláda naplózási csoport
Ezeket a rekordokat jönnek létre, amikor módosításait és kiegészítéseit tooExchange csoportok.

| Tulajdonság | Leírás |
|:--- |:--- |
| OfficeWorkload | Exchange |
| OfficeWorkload | ExchangeItemGroup |
| AffectedItems | Hello csoport összes elemére vonatkozó adatokat. |
| CrossMailboxOperations | Azt jelzi, ha a hello művelet csak egy postaláda szerepet játszanak. |
| DestMailboxId | Állítsa be, csak ha hello CrossMailboxOperations paraméter értéke True. Megadja a hello cél postaláda GUID. |
| DestMailboxOwnerMasterAccountSid | Állítsa be, csak ha hello CrossMailboxOperations paraméter értéke True. Megadja a hello SID hello fő fiók SID-je hello cél tulajdonos. |
| DestMailboxOwnerSid | Állítsa be, csak ha hello CrossMailboxOperations paraméter értéke True. Megadja a hello hello cél postaláda biztonsági azonosítója. |
| DestMailboxOwnerUPN | Állítsa be, csak ha hello CrossMailboxOperations paraméter értéke True. Hello hello cél postaláda hello tulajdonosának egyszerű Felhasználónevét határozza meg. |
| DestFolder | hello célmappát, műveletek, például a Move. |
| Mappa | hello mappáját cikkek csoportja. |
| Mappák |     Hello forrás mappák; egy műveletben érintett kapcsolatos információk Ha például mappák van kiválasztva, és a törölt. |


### <a name="sharepoint-base"></a>SharePoint-alap
Ezek a Tulajdonságok bejegyzései közös tooall SharePoint.

| Tulajdonság | Leírás |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePoint |
| Eseményforrás | Meghatározza, hogy az esemény történt-e a SharePoint. Lehetséges értékek a következők: a SharePoint vagy ObjectModel. |
| ItemType | hello a típusú objektum, amely nem érhető el, vagy módosították. Lásd: hello ItemType tábla hello típusú objektumok leírását. |
| MachineDomainInfo | Eszköz szinkronizálási műveleteire vonatkozó adatokat. Ezt az információt készüljön jelentés, csak akkor, ha a hello kérés található. |
| MachineId |   Eszköz szinkronizálási műveleteire vonatkozó adatokat. Ezt az információt készüljön jelentés, csak akkor, ha a hello kérés található. |
| Site_ | hello GUID hello hely, ahol hello fájl vagy mappa hello felhasználók férjenek hozzá. |
| Source_Name | hello entitás hello kiváltó műveletet naplózza. Lehetséges értékek a következők: a SharePoint vagy ObjectModel. |
| Felhasználói ügynök | Információ hello felhasználói ügyfél vagy a böngészőben. Ez az információ hello ügyfél vagy a böngésző által biztosított. |


### <a name="sharepoint-schema"></a>SharePoint-séma
Ezeket a rekordokat jönnek létre, amikor a beállítások módosításai az tooSharePoint.

| Tulajdonság | Leírás |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePoint |
| CustomEvent | Egyéni események választható karakterláncot. |
| Event_Data |  Egyéni események választható hasznos. |
| ModifiedProperties | hello tulajdonság megtalálható felügyeleti események, mint például a felhasználó hozzáadása egy helyhez vagy egy hely gyűjtési felügyeleti csoport tagjaként. hello tulajdonság hello tulajdonság módosításának (például hello webhely-felügyeleti csoport), új hello értékét módosítva tulajdonság (ilyen hello felhasználó hozzá lett adva, a webhely rendszergazdájától), és hello hello előző érték módosított objektum hello hello nevét tartalmazza. |


### <a name="sharepoint-file-operations"></a>SharePoint fájlműveletek
Ezeket a rekordokat a válasz toofile műveletek a SharePoint jönnek létre.

| Tulajdonság | Leírás |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePointFileOperation |
| DestinationFileExtension | hello fájl kiterjesztése egy fájlt másol vagy áthelyezték. Ez a tulajdonság csak FileCopied és FileMoved esemény jelenik meg. |
| DestinationFileName | hello neve hello fájlt másol vagy áthelyezték. Ez a tulajdonság csak FileCopied és FileMoved esemény jelenik meg. |
| DestinationRelativeUrl | hello URL-címe hello célmappát, ahol egy fájlt másolja vagy áthelyezték. hello értékkombináció-készletét hello SiteURL DestinationRelativeURL és DestinationFileName paraméter van hello ugyanaz, mint a hello érték hello ObjectID tulajdonság, amely hello átmásolt hello fájl teljes elérési útját. Ez a tulajdonság csak FileCopied és FileMoved esemény jelenik meg. |
| SharingType | megosztási engedélyek, amelyek lettek társítva a megosztott erőforrás hello toohello felhasználói hello típusú. Ez a felhasználó azonosítására hello UserSharedWith paraméter. |
| Site_Url | hello webhely, ahol hello fájl vagy mappa hello felhasználók férjenek hozzá hello URL-címe |
| SourceFileExtension | hello fájlkiterjesztés hello fájl, amely hello felhasználó hozzáfért. Ezt a tulajdonságot, akkor ha hello objektum, amelynek az a mappa üres. |
| SourceFileName |  hello neve hello fájl vagy mappa hello felhasználók férjenek hozzá. |
| SourceRelativeUrl | hello URL-címe hello felhasználók férjenek hozzá hello fájlt tartalmazó hello mappát. hello értékkombináció-készletét hello hello SiteURL SourceRelativeURL és SourceFileName paraméter van hello ugyanaz, mint a hello érték hello ObjectID tulajdonság, amely hello hello felhasználók férjenek hozzá hello fájl teljes elérési útját. |
| UserSharedWith |  a megosztott erőforrás hello felhasználó. |




## <a name="sample-log-searches"></a>Naplókeresési minták
a következő táblázat hello minta napló keres, ez a megoldás által gyűjtött frissítési rekordok biztosít.

| Lekérdezés | Leírás |
| --- | --- |
|Az Office 365-előfizetés összes hello műveletek száma |"Típusú = OfficeActivity | mérheti művelet count() " |
|SharePoint-webhelyek használata|"Típus = OfficeActivity OfficeWorkload = sharepoint | mérték count() által SiteUrl darabszámként | rendezési száma asc "|
|Hozzáférés fájlműveletek felhasználói típus szerint|"Típus = OfficeActivity OfficeWorkload sharepoint művelet = = FileAccessed | mértékcsoport által UserType count() "|
|Egy adott kulcsszóval keresése|`Type=OfficeActivity OfficeWorkload=azureactivedirectory "MyTest"`|
|A figyelő külső műveletek Exchange|`Type=OfficeActivity OfficeWorkload=exchange ExternalAccess = true`|



## <a name="troubleshooting"></a>Hibaelhárítás

Ha az Office 365-megoldás nem gyűjt adatokat várt módon, ellenőrizze az állapotát hello OMS-portálon: **beállítások** -> **csatlakoztatott források** -> **Office 365** . hello a következő táblázat ismerteti az egyes állapotokhoz.

| status | Leírás |
|:--|:--|
| Aktív | Office 365-előfizetéssel hello aktív pedig hello munkaterhelés sikeresen csatlakoztatott tooyour OMS-munkaterület. |
| Függőben | Office 365-előfizetéssel hello aktív, de hello munkaterhelés még nem tooyour OMS-munkaterület sikeresen csatlakoztatva. hello hello Office 365-előfizetéssel, első csatlakozáskor összes hello munkaterhelések lesz ez az állapot addig, amíg sikeresen csatlakoznak. Várjon 24 órát összes hello munkaterhelések tooswitch tooActive. |
| Inaktív | Office 365-előfizetéssel hello inaktív állapotban van. Ellenőrizze az Office 365 felügyeleti a lapot. Az Office 365-előfizetés aktiválása után szüntesse meg a kapcsolatát az OMS-munkaterület és hivatkozás létrehozása újra toostart adatok fogadása. |



## <a name="next-steps"></a>Következő lépések
* A napló kereséssel [Naplóelemzési](../log-analytics/log-analytics-log-searches.md) tooview részletes adatok frissítése.
* [Hozzon létre egy saját irányítópultok](../log-analytics/log-analytics-dashboards.md) toodisplay kedvenc Office 365 keresési lekérdezések.
* [Hozzon létre a riasztások](../log-analytics/log-analytics-alerts.md) toobe proaktív fontos Office 365-tevékenységek értesítést kap.  
