---
title: "Azure AD Connect szinkronizálása: működtetési feladatok és szempontok |} Microsoft Docs"
description: "Ez a témakör ismerteti az Azure AD Connect szinkronizálási szolgáltatás működési feladatokat, és hogyan tooprepare működtetéséhez ezt az összetevőt."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: b29c1790-37a3-470f-ab69-3cee824d220d
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: e6b21262e0924785808888d66f08a04a56581edc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-operational-tasks-and-consideration"></a>Azure AD Connect szinkronizálása: működtetési feladatok és szempont
hello Ez a témakör célja toodescribe operatív feladatok az Azure AD Connect szinkronizálási szolgáltatás.

## <a name="staging-mode"></a>Átmeneti mód
Az átmeneti környezetű üzemmód használható több forgatókönyvek, köztük:

* Magas rendelkezésre állású.
* Tesztelése és telepítése az új konfigurációs módosításokat.
* Új kiszolgáló esetében, és régi hello leszereléséhez.

Átmeneti módban a kiszolgálóval, módosíthatja módosítások toohello konfigurációs és preview hello előtt hello kiszolgáló aktív. Azt is lehetővé teszi toorun teljes importálást és teljes szinkronizálást tooverify változik, hogy minden változást várható előtt ezeket az éles környezetben.

A telepítés során kiválaszthatja a hello server toobe **átmeneti módban**. Ez a művelet kiszolgálóvá hello importálása és szinkronizálás aktív, de nem futtatható bármely exportálja. Átmeneti módban nem fut a jelszó-szinkronizálás és jelszóvisszaíró, még akkor is, ha ezek a szolgáltatások telepítésekor kiválasztott. Az átmeneti környezetű üzemmód letiltása esetén hello server exportálása elindul, lehetővé teszi, hogy a jelszó-szinkronizálást, és lehetővé teszi, hogy a jelszóvisszaíró.

Az export hello synchronization service Managert használatával továbbra is kényszeríthető.

Átmeneti módban továbbra is fennáll, az Active Directoryból és az Azure AD tooreceive módosításokat. Keresztül hello kötelezettségeit, egy másik kiszolgáló egy másolattal hello legutóbbi módosítások és a részleg nagyon gyorsan elvégezhető mindig rendelkezik. Ha konfigurációs módosítások tooyour elsődleges kiszolgáló, a felelősség toomake hello azonos módosítások toohello kiszolgáló átmeneti módban.

A mellékelt régebbi szinkronizálási technológiák ismerete átmeneti módban hello különbözik mivel hello server csak a saját SQL-adatbázis. Ez az architektúra lehetővé teszi, hogy a hello átmeneti mód server toobe ugyanabban az adatközpontban található.

### <a name="verify-hello-configuration-of-a-server"></a>A kiszolgáló hello konfigurációjának ellenőrzése
tooapply ebben az esetben kövesse az alábbi lépéseket:

1. [Előkészítése](#prepare)
2. [Konfigurálás](#configuration)
3. [Importálja és szinkronizálja](#import-and-synchronize)
4. [Ellenőrzése](#verify)
5. [Az active server kapcsoló](#switch-active-server)

#### <a name="prepare"></a>Előkészítés
1. Azure AD Connectet telepíti, válassza ki **átmeneti módban**, és kikapcsolni **a szinkronizálás megkezdéséhez** hello hello telepítési varázsló utolsó lapján. Ez a mód lehetővé teszi toorun hello szinkronizálási motor manuálisan.
   ![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)
2. Jelentkezzen ki/bejelentkezés és hello a start menüből válassza **szinkronizálási szolgáltatás**.

#### <a name="configuration"></a>Konfiguráció
Ha a kiszolgáló átmeneti hello végrehajtott az egyéni módosítások toohello elsődleges kiszolgáló és a kívánt toocompare hello konfigurációs használja [az Azure AD Connect konfigurációs dokumentáló](https://github.com/Microsoft/AADConnectConfigDocumenter).

#### <a name="import-and-synchronize"></a>Importálja és szinkronizálja
1. Válassza ki **összekötők**, és válassza ki a hello típusú első összekötő hello **Active Directory tartományi szolgáltatások**. Kattintson a **futtatása**, jelölje be **teljes importálás**, és **OK**. Hajtsa végre ezeket a lépéseket az összes ilyen típusú-összekötőhöz.
2. Válassza ki a Connector típusú hello **Azure Active Directoryban (Microsoft)**. Kattintson a **futtatása**, jelölje be **teljes importálás**, és **OK**.
3. Győződjön meg arról, hogy hello lapon összekötők aktív marad. Minden típusú összekötő **Active Directory tartományi szolgáltatások**, kattintson a **futtatása**, jelölje be **különbözeti szinkronizálás**, és **OK**.
4. Válassza ki a Connector típusú hello **Azure Active Directoryban (Microsoft)**. Kattintson a **futtatása**, jelölje be **különbözeti szinkronizálás**, és **OK**.

Lehetősége van most előkészített exportálás tooAzure AD módosításai és a helyszíni AD (ha az Exchange hibrid telepítést használ). hello lépések lehetővé teszik a tooinspect Újdonságok kapcsolatos toochange toohello könyvtárak hello exportálása ténylegesen megkezdése előtt.

#### <a name="verify"></a>Ellenőrzés
1. Indítsa el egy parancssort, és nyissa meg túl`%ProgramFiles%\Microsoft Azure AD Sync\bin`
2. Futtatás: `csexport "Name of Connector" %temp%\export.xml /f:x` hello összekötő hello neve található szinkronizálási szolgáltatás. Rendelkezik egy neve hasonló too"contoso.com – AAD" az Azure AD.
3. Hello PowerShell parancsfájl másolása hello szakasz [CSAnalyzer](#appendix-csanalyzer) tooa fájlt `csanalyzer.ps1`.
4. Nyisson meg egy PowerShell-ablakot, és keresse meg a toohello mappa, amelyben létrehozta hello PowerShell-parancsfájlt.
5. Futtatás: `.\csanalyzer.ps1 -xmltoimport %temp%\export.xml`.
6. Most már rendelkezik egy fájlt **processedusers1.csv** , amely a Microsoft Excel kell vizsgálni. Minden előkészített módosítások exportált toobe tooAzure AD ebben a fájlban találhatók.
7. A szükséges módosításokat toohello adatok vagy konfiguráció, és ezeket a lépéseket újra (importálás és szinkronizálás és ellenőrzése) futását a változtatásokat, hogy exportált toobe kapcsolatos várható hello.

#### <a name="switch-active-server"></a>Az active server kapcsoló
1. A jelenleg aktív kiszolgálói hello vagy kapcsolja ki a hello server (DirSync vagy FIM vagy az Azure AD Sync), így azt nem exportálnia kell tooAzure AD, vagy állítsa az átmeneti módban (az Azure AD Connect).
2. Hello telepítővarázslójának futtatása a hello kiszolgálón **átmeneti módban** és tiltsa le a **átmeneti módban**.
   ![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)

## <a name="disaster-recovery"></a>Vészhelyreállítás
Hello megvalósítási terv része a milyen toodo tooplan abban az esetben, ha van egy olyan vészhelyzet esetén hol elvesznek a hello szinkronizálási kiszolgálót. Nincsenek különböző modell toouse és mely egy toouse többek között számos tényezőtől függ:

* Mi az a tűrés, azzal nem tud ellenőrizze tooobjects hello leállások során az Azure AD-ben?
* A jelszó-szinkronizálás használatakor tegye hello felhasználók fogadja el, hogy rendelkeznek toouse hello régi jelszó az Azure AD abban az esetben, ha azok megváltoztatnia a helyszínen?
* Rendelkezik a valós idejű műveletek, például a jelszóvisszaírás függőség?

Attól függően, hogy hello válaszok toothese kérdések és a szervezet házirendje hello következő stratégiák egyikét is kell végrehajtani:

* Építse újra, amikor szükséges.
* Tartalék készenléti kiszolgáló, úgynevezett **átmeneti módban**.
* Használja a virtuális gépek.

Ha nem használja a hello beépített SQL Express adatbázist, majd tekintse meg hello [SQL magas rendelkezésre állású](#sql-high-availability) szakasz.

### <a name="rebuild-when-needed"></a>Szükség esetén újraépítése
Életképes stratégia tooplan egy kiszolgáló újjáépítése szükség esetén a rendszer. Általában telepítése szinkronizálási motor hello és hello kezdeti importálása és szinkronizálás néhány órán belül végrehajtható. Tartalék kiszolgáló nem érhető el, ha egy tartomány tartományvezérlői toohost hello szinkronizálási motor lehetséges tootemporarily használatát is.

hello a Szinkronizáló vezérlő kiszolgálója nem tárol hello objektumokról olyan állapotokat, hello adatbázis is építhető újra, az Active Directory és az Azure AD hello adataiból. Hello **sourceAnchor** attribútum használt toojoin hello objektumokat a helyszíni és felhőalapú hello. Újraépítése hello kiszolgáló meglévő objektumokat a helyszíni és hello felhő, majd hello tényleges szinkronizálási motor megfelel azokat az objektumokat újra újratelepítés együtt. hello toodocument kell, és mentse a dolog hello konfigurációs módosítások toohello kiszolgálót, például szűrési és szinkronizálási szabályok. Ezen egyéni konfigurációk kell kell újra alkalmazza a szinkronizálás megkezdése előtt.

### <a name="have-a-spare-standby-server---staging-mode"></a>Tartalék készenléti kiszolgáló - átmeneti módban
Ha összetettebb környezetben, akkor ajánlott, ha egy vagy több készenléti kiszolgálót. A telepítés során engedélyezheti a kiszolgáló toobe a **átmeneti módban**.

További információkért lásd: [átmeneti módban](#staging-mode).

### <a name="use-virtual-machines"></a>Virtuális gépek használata
A közös és támogatott metódus toorun hello szinkronizálási motor egy virtuális gépen. Abban az esetben hello állomás egy hibát tartalmaz, hello a Szinkronizáló vezérlő kiszolgálója hello rendszerképének áttelepített tooanother kiszolgáló lehet.

### <a name="sql-high-availability"></a>Az SQL magas rendelkezésre állás
Ha nem használ SQL Server Express, az Azure AD Connect előre hello, majd az SQL Server magas rendelkezésre állás is meg kell fontolni. támogatott hello magas rendelkezésre állás megoldásai a következők: SQL-fürtözés és AOA (az Always On rendelkezésre állási csoportok). Nem támogatott megoldásai a következők tükrözés.

Az SQL AOA támogatást kaptak tooAzure AD Connect 1.1.524.0 verzióban. Az Azure AD Connect telepítése előtt engedélyeznie kell az SQL AOA. A telepítés során az Azure AD Connect észleli a megadott hello SQL-példány engedélyezve van az SQL AOA-e nem. Ha SQL AOA engedélyezve van, az Azure AD Connect további adatok Ha SQL AOA-e a konfigurált toouse szinkron replikáció vagy aszinkron replikáció. Hello rendelkezésre állási csoport figyelőjének beállításakor javasoljuk, hogy állítsa a hello RegisterAllProvidersIP tulajdonság too0. Ennek az az oka az Azure AD Connect jelenleg használ SQL Native Client tooconnect tooSQL és az SQL natív ügyfél nem támogatja a MultiSubNetFailover tulajdonság hello használatával.

## <a name="appendix-csanalyzer"></a>A függelék CSAnalyzer
Című rész hello [ellenőrizze](#verify) hogyan toouse ezt a parancsfájlt.

```
Param(
    [Parameter(Mandatory=$true, HelpMessage="Must be a file generated using csexport 'Name of Connector' export.xml /f:x)")]
    [string]$xmltoimport="%temp%\exportedStage1a.xml",
    [Parameter(Mandatory=$false, HelpMessage="Maximum number of users per output file")][int]$batchsize=1000,
    [Parameter(Mandatory=$false, HelpMessage="Show console output")][bool]$showOutput=$false
)

#LINQ isn't loaded automatically, so force it
[Reflection.Assembly]::Load("System.Xml.Linq, Version=3.5.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089") | Out-Null

[int]$count=1
[int]$outputfilecount=1
[array]$objOutputUsers=@()

#XML must be generated using "csexport "Name of Connector" export.xml /f:x"
write-host "Importing XML" -ForegroundColor Yellow

#XmlReader.Create won't properly resolve hello file location,
#so expand and then resolve it
$resolvedXMLtoimport=Resolve-Path -Path ([Environment]::ExpandEnvironmentVariables($xmltoimport))

#use an XmlReader toodeal with even large files
$result=$reader = [System.Xml.XmlReader]::Create($resolvedXMLtoimport) 
$result=$reader.ReadToDescendant('cs-object')
do 
{
    #create hello object placeholder
    #adding them up here means we can enforce consistency
    $objOutputUser=New-Object psobject
    Add-Member -InputObject $objOutputUser -MemberType NoteProperty -Name ID -Value ""
    Add-Member -InputObject $objOutputUser -MemberType NoteProperty -Name Type -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name DN -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name operation -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name UPN -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name displayName -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name sourceAnchor -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name alias -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name primarySMTP -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name onPremisesSamAccountName -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name mail -Value ""

    $user = [System.Xml.Linq.XElement]::ReadFrom($reader)
    if ($showOutput) {Write-Host Found an exported object... -ForegroundColor Green}

    #object id
    $outID=$user.Attribute('id').Value
    if ($showOutput) {Write-Host ID: $outID}
    $objOutputUser.ID=$outID

    #object type
    $outType=$user.Attribute('object-type').Value
    if ($showOutput) {Write-Host Type: $outType}
    $objOutputUser.Type=$outType

    #dn
    $outDN= $user.Element('unapplied-export').Element('delta').Attribute('dn').Value
    if ($showOutput) {Write-Host DN: $outDN}
    $objOutputUser.DN=$outDN

    #operation
    $outOperation= $user.Element('unapplied-export').Element('delta').Attribute('operation').Value
    if ($showOutput) {Write-Host Operation: $outOperation}
    $objOutputUser.operation=$outOperation

    #now that we have hello basics, go get hello details

    foreach ($attr in $user.Element('unapplied-export-hologram').Element('entry').Elements("attr"))
    {
        $attrvalue=$attr.Attribute('name').Value
        $internalvalue= $attr.Element('value').Value

        switch ($attrvalue)
        {
            "userPrincipalName"
            {
                if ($showOutput) {Write-Host UPN: $internalvalue}
                $objOutputUser.UPN=$internalvalue
            }
            "displayName"
            {
                if ($showOutput) {Write-Host displayName: $internalvalue}
                $objOutputUser.displayName=$internalvalue
            }
            "sourceAnchor"
            {
                if ($showOutput) {Write-Host sourceAnchor: $internalvalue}
                $objOutputUser.sourceAnchor=$internalvalue
            }
            "alias"
            {
                if ($showOutput) {Write-Host alias: $internalvalue}
                $objOutputUser.alias=$internalvalue
            }
            "proxyAddresses"
            {
                if ($showOutput) {Write-Host primarySMTP: ($internalvalue -replace "SMTP:","")}
                $objOutputUser.primarySMTP=$internalvalue -replace "SMTP:",""
            }
        }
    }

    $objOutputUsers += $objOutputUser

    Write-Progress -activity "Processing ${xmltoimport} in batches of ${batchsize}" -status "Batch ${outputfilecount}: " -percentComplete (($objOutputUsers.Count / $batchsize) * 100)

    #every so often, dump hello processed users in case we blow up somewhere
    if ($count % $batchsize -eq 0)
    {
        Write-Host Hit hello maximum users processed without completion... -ForegroundColor Yellow

        #export hello collection of users as as CSV
        Write-Host Writing processedusers${outputfilecount}.csv -ForegroundColor Yellow
        $objOutputUsers | Export-Csv -path processedusers${outputfilecount}.csv -NoTypeInformation

        #increment hello output file counter
        $outputfilecount+=1

        #reset hello collection and hello user counter
        $objOutputUsers = $null
        $count=0
    }

    $count+=1

    #need toobail out of hello loop if no more users tooprocess
    if ($reader.NodeType -eq [System.Xml.XmlNodeType]::EndElement)
    {
        break
    }

} while ($reader.Read)

#need toowrite out any users that didn't get picked up in a batch of 1000
#export hello collection of users as as CSV
Write-Host Writing processedusers${outputfilecount}.csv -ForegroundColor Yellow
$objOutputUsers | Export-Csv -path processedusers${outputfilecount}.csv -NoTypeInformation
```

## <a name="next-steps"></a>Következő lépések
**Áttekintő témakör**  

* [Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása](active-directory-aadconnectsync-whatis.md)  
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)  
