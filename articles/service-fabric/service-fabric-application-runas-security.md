---
title: "az Azure mikroszolgáltatások biztonsági házirendekkel kapcsolatos aaaLearn |} Microsoft Docs"
description: "Hogyan toorun a Service Fabric-alkalmazás, a rendszer és a helyi biztonsági fiókok, beleértve a hello SetupEntry pont, amikor a kérelmet kell tooperform néhány privilegizált művelet megkezdése előtt áttekintése"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: mfussell
ms.openlocfilehash: f5afba69e09aa4f3c9efa4d3efc6995c813a1f71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-security-policies-for-your-application"></a>Biztonsági házirendek beállítása az alkalmazáshoz
Azure Service Fabric használatával biztonságossá teheti a különböző felhasználói fiókok hello fürtben futó alkalmazás számára. A Service Fabric is lehetővé teszi az alkalmazások által használt hello időpontban a központi telepítés hello felhasználói fiókokon – például biztonságos hello erőforrásokat, fájlok, könyvtárak és tanúsítványokat. Így futó alkalmazást, még akkor is, a megosztott üzemeltetési környezetben, nagyobb biztonságot nyújt, egymástól.

Alapértelmezés szerint a Service Fabric-alkalmazások hello fiókkal, amely alatt futó hello Fabric.exe folyamatban fut. A Service Fabric is lehetővé teszi hello toorun alkalmazások helyi felhasználói fiók vagy helyi rendszerfiók, belül hello alkalmazás jegyzékében meg. Támogatott helyi rendszer fiók típusok **LocalUser**, **NetworkService**, **LocalService**, és **LocalSystem**.

 Ha a számítógépén a Service Fabric Windows Server az adatközpontban található hello önálló telepítő használatával, az Active Directory tartományi fiókok, beleértve a csoportosan felügyelt szolgáltatásfiókok is használhatja.

Adja meg, és hozzon létre felhasználói csoportokat, hogy egy vagy több felhasználó tooeach csoport toobe felügyelete együtt adható hozzá. Ez akkor hasznos, ha több felhasználó különböző belépési pontot, és bizonyos közös jogosultságok által biztosított hello csoportok szintjén kell toohave.

## <a name="configure-hello-policy-for-a-service-setup-entry-point"></a>A telepítő belépési pont hello szabályzat konfigurálása
Hello leírtak [alkalmazásmodell](service-fabric-application-model.md), hello beállítása belépési pont, **SetupEntryPoint**, egy kiemelt belépési pontja, amelynek ugyanaz, mint a Service Fabric hitelesítő adatok hello (általában hello *NetworkService* fiók) más belépési pont előtt. hello végrehajtható fájl megadott **EntryPoint** általában hello hosszan futó szolgáltatás gazdagép legyen. Ezért ha egy külön telepítő belépési pont elkerülhető toorun hello szolgáltatásgazda végrehajtható magas jogosultsággal rendelkező huzamosabb ideig. hello végrehajtható, amely **EntryPoint** megadja futtatása **SetupEntryPoint** sikeresen kilép. hello eredményül kapott folyamat figyeli, és újra kell indítani, és újra kezdődik **SetupEntryPoint** Ha valaha is leáll, vagy összeomlik.

hello következő látható egy egyszerű service manifest példa, hogy hello SetupEntryPoint mutat be, és hello hello szolgáltatás fő belépési pont.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="Config" Version="1.0.0" />
</ServiceManifest>
```

### <a name="configure-hello-policy-by-using-a-local-account"></a>Helyi fiók használatával hello házirend konfigurálása
Hello szolgáltatás toohave egy telepítő belépési pont konfigurálása után módosíthatja, amely akkor fut a hello alkalmazásjegyzékben hello biztonsági engedélyeket. hello a következő példa bemutatja, hogyan tooconfigure hello szolgáltatás toorun a felhasználó rendszergazdai jogosultságot.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupAdminUser">
            <MemberOf>
               <SystemGroup Name="Administrators" />
            </MemberOf>
         </User>
      </Users>
   </Principals>
</ApplicationManifest>
```

Először hozzon létre egy **rendszerbiztonsági tagok** egy felhasználónevet, pl. SetupAdminUser szakasz ismerteti. Ez azt jelzi, hogy a felhasználó hello hello rendszergazdák system csoport tagja.

Ezután bontsa hello **ServiceManifestImport** területen konfigurálja a házirend tooapply rendszerbiztonsági túl**SetupEntryPoint**. Ez alapján a Service Fabric, amikor hello **MySetup.bat** futtatása, kell lennie `RunAs` rendszergazdai jogosultságokkal. Fényében, hogy már rendelkezik *nem* alkalmazza a házirend toohello fő belépési pont, a kód hello **MyServiceHost.exe** hello rendszer alatt futó **NetworkService** fiók. Ez az hello alapértelmezett fiókot, amely az összes szolgáltatás belépési pont alatt futnak.

Adjuk hozzá most hello fájl MySetup.bat toohello Visual Studio projekt tootest hello rendszergazdai jogosultságokkal. A Visual Studióban kattintson a jobb gombbal a projekt hello, és adja hozzá a MySetup.bat nevű új fájlt.

A következő győződjön meg arról, hogy hello MySetup.bat fájl hello szolgáltatás csomagban található. Alapértelmezés szerint nincs. Válassza ki hello fájlt, kattintson a jobb gombbal a tooget hello helyi menüt, és válassza a **tulajdonságok**. A hello tulajdonságai párbeszédpanelen győződjön meg arról, hogy **tooOutput Directory másolása** értéke túl**másolhatja, ha újabb**. Tekintse meg a következő képernyőkép hello.

![A Visual Studio CopyToOutput SetupEntryPoint kötegelt fájl][image1]

Most nyissa meg hello MySetup.bat fájlt, és adja hozzá a következő parancsok hello:

```
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable "MyValue"
echo System TestVariable set too> out.txt
echo %TestVariable% >> out.txt

REM toodelete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
```

Ezt követően hozza létre, és hello megoldás tooa helyi fejlesztési fürtök telepítése. Hello szolgáltatás elindult, a Service Fabric Explorerben látható, miután láthatja, hogy hello MySetup.bat fájl két módon sikeres volt. Nyisson meg egy PowerShell-parancssort, és írja be:

```
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
```

Jegyezze fel hello csomópont ahol hello szolgáltatást telepítették, és elindítani a Service Fabric Explorerben – például csomópont 2 hello neve. Ezután keresse meg a toohello példány munkahelyi mappa toofind hello out.txt alkalmazásfájl hello értékének megjelenítő **TestVariable**. Például, ha a szolgáltatás telepített tooNode 2 volt, majd elvégezheti a toothis elérési útját hello **MyApplicationType**:

```
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
```

### <a name="configure-hello-policy-by-using-local-system-accounts"></a>Hello házirend konfigurálása a helyi rendszer fiók használatával
Gyakran érdemes előnyösebb toorun hello indítási parancsfájl rendszergazdai fiók helyett a helyi rendszer fiók használatával. A Rendszergazdák csoport általában nem működik jól, mert a felhasználói hozzáférés felügyelete (UAC) alapértelmezés szerint engedélyezve vannak hello tagjaként hello RunAs házirend fut. Ilyen esetben **helyi felhasználói hozzáadott tooAdministrators csoportként hello ajánljuk, az helyett toorun hello LocalSystem, SetupEntryPoint**. hello alábbi példa bemutatja a beállítás hello SetupEntryPoint toorun LocalSystem néven:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupLocalSystem" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupLocalSystem" AccountType="LocalSystem" />
      </Users>
   </Principals>
</ApplicationManifest>
```

## <a name="start-powershell-commands-from-a-setup-entry-point"></a>A telepítő belépési pontról indítsa el a PowerShell-parancsok
a hello PowerShell toorun **SetupEntryPoint** pont, futtathatja **PowerShell.exe** mutat, tooa PowerShell parancsfájlban fájlt. Első lépésként adja meg egy PowerShell fájl toohello projekt – például **MySetup.ps1**. Ne feledje tooset hello *másolhatja, ha újabb* tulajdonság úgy, hogy hello fájl is hello szolgáltatás csomagban található. hello alábbi példa azt mutatja, amely elindítja a MySetup.ps1, amelyek a nevezett rendszer környezeti változó beállítása nevű PowerShell fájl kötegelt mintafájl **TestVariable**.

MySetup.bat toostart egy PowerShell-fájlt:

```
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
```

Hello PowerShell fájlban adja hozzá a következő környezeti változót tooset hello:

```
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
```

> [!NOTE]
> Alapértelmezés szerint hello kötegfájl futtatásakor jelek nevű hello alkalmazásmappa **működik** fájlok. Ebben az esetben, amikor MySetup.bat fut, azt szeretnénk, ha a toofind hello MySetup.ps1 fájlt hello azonos mappába, amely hello alkalmazás **kódcsomag** mappa. toochange ezt a mappát, a set hello munkamappa:
> 
> 

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    </ExeHost>
</SetupEntryPoint>
```

## <a name="use-console-redirection-for-local-debugging"></a>A hibakereséshez a helyi konzol átirányítást használ
Alkalmanként is hasznos toosee hello konzol kimeneti hibakeresési célra parancsfájl futtatását. toodo, beállíthatja a konzol átirányítási házirend, amely hello kimeneti tooa fájlba írja. hello fájl kimeneti alkalmazásmappa írásbeli toohello nevezik **napló** hello csomóponton, amelyen hello alkalmazás üzemel, és futtassa. (Lásd: a where toofind ezt az előző példa hello.)

> [!WARNING]
> Az alkalmazásban, mert ez hatással lehet a hello alkalmazás feladatátvételi éles környezetben telepített soha ne használja a hello konzol átirányítási házirend. *Csak* használja ezt a helyi fejlesztési és hibakeresési célra.  
> 
> 

hello alábbi példa bemutatja a beállítás hello segítségével FileRetentionCount értékkel:

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
```

Ha most módosítja hello MySetup.ps1 fájl toowrite egy **Echo** parancsot fog kiírni, a hibakeresési célra toohello kimeneti fájl:

```
Echo "Test console redirection which writes toohello application log folder on hello node that hello application is deployed to"
```

**A parancsfájl hibakeresése azonnal távolítsa el a konzol átirányítási házirend**.

## <a name="configure-a-policy-for-service-code-packages"></a>A szolgáltatás kód csomagok házirend konfigurálása
Az előző lépésekben hello, megtudhatta, hogyan tooapply egy csoportházirend RunAs tooSetupEntryPoint. Nézzük kissé mélyebb be, hogyan toocreate különböző rendszerbiztonsági tagok közül melyek alkalmazhatók, mint szolgáltatás házirendek.

### <a name="create-local-user-groups"></a>Helyi felhasználói csoportok létrehozása
Határozza meg, és hozzon létre felhasználói csoportokat, amelyek lehetővé teszik egy vagy több felhasználók toobe hozzáadta tooa csoportot. Ez akkor hasznos, ha több felhasználó különböző belépési pontot, és bizonyos közös jogosultságok által biztosított hello csoportok szintjén kell toohave. hello következő példa bemutatja egy nevű helyi csoporthoz **LocalAdminGroup** , amely rendszergazdai jogosultságokkal rendelkezik. Két felhasználók Customer1 és Customer2, a helyi csoport tagjai válnak.

```xml
<Principals>
 <Groups>
   <Group Name="LocalAdminGroup">
     <Membership>
       <SystemGroup Name="Administrators"/>
     </Membership>
   </Group>
 </Groups>
  <Users>
     <User Name="Customer1">
        <MemberOf>
           <Group NameRef="LocalAdminGroup" />
        </MemberOf>
     </User>
    <User Name="Customer2">
      <MemberOf>
        <Group NameRef="LocalAdminGroup" />
      </MemberOf>
    </User>
  </Users>
</Principals>
```

### <a name="create-local-users"></a>Helyi felhasználók létrehozása
Létrehozhat egy helyi felhasználót, amely biztonságos használt toohelp hello alkalmazásban egy szolgáltatás. Ha egy **LocalUser** fióktípus szakaszban van megadva hello rendszerbiztonsági tagok hello az alkalmazás jegyzékének, a Service Fabric helyi felhasználói fiókok létrehozása a gépek hello alkalmazás telepítési helyét. Alapértelmezés szerint ezek a fiókok nem rendelkeznek hello (például a következő minta hello Customer3) manifest hello alkalmazás megadottakkal azonos néven. Ehelyett dinamikusan jön létre, és véletlenszerű jelszót.

```xml
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
```

Ha egy alkalmazás megköveteli, hogy hello felhasználói fiókkal és jelszóval azonos minden számítógépen (például tooenable NTLM-hitelesítés), hello fürtjegyzékben NTLMAuthenticationEnabled tootrue be kell állítani. hello fürtjegyzékben is meg kell adnia egy NTLMAuthenticationPasswordSecret használt toogenerate ugyanazt a jelszót hello összes számítógépével.

```xml
<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
```

### <a name="assign-policies-toohello-service-code-packages"></a>Házirendek toohello szolgáltatáscsomagok kód hozzárendelése
Hello **RunAsPolicy** szakasz egy **ServiceManifestImport** hello rendszerbiztonsági tagok szakaszában, amelyeket a kódcsomag használt toorun hello fiókot adja meg. A felhasználói fiókok hello rendszerbiztonsági tagok szakaszban hello service-csomagok jegyzékfájljában kódot is társítja. Megadhatja a hello beállítása vagy a fő belépési pontok, vagy megadhat `All` tooapply azt tooboth. a következő példa azt mutatja be másik-szabályzatok hello:

```xml
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
```

Ha **EntryPointType** nincs megadva, tooEntryPointType hello az alapértelmezett érték = "Main". Adja meg **SetupEntryPoint** is különösen hasznos, ha azt szeretné, toorun bizonyos magas szintű jogosultságokat igénylő telepítési műveletekhez a rendszerfiók alatt. hello tényleges szolgáltatás kód egy alacsonyabb szintű szolgáltatásfiók alatt futhatnak.

### <a name="apply-a-default-policy-tooall-service-code-packages"></a>Egy alapértelmezett kód házirend tooall szolgáltatáscsomagok alkalmazása
Hello használata **DefaultRunAsPolicy** szakasz toospecify egy alapértelmezett felhasználói fiókot az összes kódot csomagokat, amelyek nem rendelkeznek egy adott **RunAsPolicy** definiálva. Ha egy alkalmazás által használt hello szolgáltatás jegyzékben megadott hello kód csomagok többsége toorun alatt kell hello ugyanahhoz a felhasználóhoz, hello alkalmazás csak határozhatja meg, hogy felhasználói fiókkal alapértelmezett RunAs házirend. hello alábbi példa azt jelenti, hogy ha a kódcsomag nem rendelkezik egy **RunAsPolicy** megadott, hello kódcsomag kell futni hello **MyDefaultAccount** hello rendszerbiztonsági tagok szakaszában megadott.

```xml
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
```
### <a name="use-an-active-directory-domain-group-or-user"></a>Az Active Directory tartományi csoport vagy felhasználó használata
A Service Fabric hello önálló telepítő segítségével a Windows Server telepített példánya hello szolgáltatás hello credentials az Active Directory-felhasználó vagy csoport fiókkal is futtathatja. Ez az Active Directory helyszíni saját tartományában, és nincs az Azure Active Directoryval (Azure AD). Tartományi felhasználó vagy csoport segítségével érheti el más erőforrásokat hello tartomány (például fájlmegosztások), amely engedéllyel rendelkezik.

hello alábbi példa bemutatja az Active Directory-felhasználó nevű *tesztfelhasználó néven* a tartomány-tanúsítvány használatával titkosítja jelszó az úgynevezett *MyCert*. Használhatja a hello `Invoke-ServiceFabricEncryptText` PowerShell toocreate hello titkos titkosítási parancsszöveget. Lásd: [Service Fabric-alkalmazások a titkos kulcsok kezelése](service-fabric-application-secret-management.md) részleteiről.

Titkos kulcs hello hello tanúsítvány toodecrypt hello jelszó toohello helyi számítógép telepítenie kell egy sávon kívüli módszer használatával (Azure-ban ez az Azure Resource Manager használatával). Ezután Service Fabric hello szolgáltatás csomag toohello számítógépe telepíti, ha képes toodecrypt hello titkos kulcs, és ezeket a hitelesítő adatokat az Active Directory toorun végzett hitelesítéshez (együtt hello felhasználónév).

```xml
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
```
### <a name="use-a-group-managed-service-account"></a>Adjon meg egy csoportot felügyelt szolgáltatásfiók.
A Service Fabric hello önálló telepítő segítségével a Windows Server telepített példánya hello szolgáltatást is futhat, a csoportosan felügyelt szolgáltatásfiókok (gMSA). Vegye figyelembe, hogy ez az Active Directory a helyszíni tartományán belüli és nem az Azure Active Directoryval (Azure AD). A csoportosan felügyelt szolgáltatásfiók nincs jelszó vagy hello tárolt titkosított jelszót `Application Manifest`.

hello következő példa bemutatja, hogyan toocreate csoportosan felügyelt szolgáltatásfiókok nevű *svc-teszt$*; hogyan toodeploy, amely felügyeli szolgáltatás fiók toohello fürtcsomópontok; és hogyan tooconfigure hello egyszerű.

##### <a name="prerequisites"></a>Előfeltételek.
- hello tartomány kell KDS-gyökérkulcsot.
- hello tartomány toobe egy Windows Server 2012 vagy újabb működési szint szükséges.

##### <a name="example"></a>Példa
1. Kérjen meg egy active directory tartományi rendszergazdát hello segítségével csoportosan felügyelt szolgáltatásfiók létrehozása `New-ADServiceAccount` parancsmagot, és győződjön meg arról, hogy hello `PrincipalsAllowedToRetrieveManagedPassword` tartalmazza az összes hello service fabric-csomópont. Vegye figyelembe, hogy `AccountName`, `DnsHostName`, és `ServicePrincipalName` egyedinek kell lennie.
```
New-ADServiceAccount -name svc-Test$ -DnsHostName svc-test.contoso.com  -ServicePrincipalNames http/svc-test.contoso.com -PrincipalsAllowedToRetrieveManagedPassword SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$
```
2. Minden egyes hello service fabric-csomópont (például `SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$`), telepítése és tesztelése hello csoportosan felügyelt szolgáltatásfiókot.
```
Add-WindowsFeature RSAT-AD-PowerShell
Install-AdServiceAccount svc-Test$
Test-AdServiceAccount svc-Test$
```
3. Hello egyszerű, valamint hello RunAsPolicy tooreference hello felhasználói konfigurálni.
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="DomaingMSA"/>
      </Policies>
   </ServiceManifestImport>
  <Principals>
    <Users>
      <User Name="DomaingMSA" AccountType="ManagedServiceAccount" AccountName="domain\svc-Test$"/>
    </Users>
  </Principals>
</ApplicationManifest>
```

## <a name="assign-a-security-access-policy-for-http-and-https-endpoints"></a>Rendelje hozzá a biztonsági házirend HTTP és HTTPS-végpontok
Ha csoportházirend RunAs tooa szolgáltatás telepítését, és hello szolgáltatás jegyzékfájl deklarál végpont erőforrások hello HTTP protokollal, meg kell adnia egy **SecurityAccessPolicy** , hogy a portok toothese végpontok lefoglalt tooensure megfelelően vannak hozzáférés-vezérlési felsorolt hello futtató felhasználói fiók, amely hello szolgáltatás fut a. Ellenkező esetben **http.sys** toohello szolgáltatás nem rendelkezik, és sikertelen hívások lehívása hello ügyfél. hello alábbi példa érvényes hello Customer1 fiók tooan végpont nevű **EndpointName**, amely azt teszi teljes körű hozzáférési jogosultságokat.

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and hello Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
```

A HTTPS-végpont hello is hello tanúsítvány tooreturn toohello ügyfél tooindicate hello neve. Ehhez a **EndpointBindingPolicy**, egy tanúsítvány szakaszban hello alkalmazásjegyzékben hello tanúsítvánnyal.

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and hello Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if hello EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
```
## <a name="upgrading-multiple-applications-with-https-endpoints"></a>A https-végpontnak több alkalmazás frissítése
Gondosan kell toobe nem toouse hello **ugyanazt a portot** hello különböző példányai ugyanahhoz az alkalmazáshoz http használata esetén**s**. hello oka, hogy a Service Fabric nem lesz képes tooupgrade hello cert egy hello alkalmazáspéldányok. Ha például 1 vagy alkalmazás 2 mindkét alkalmazás szeretné tooupgrade az 1. tanúsítvány toocert 2. Hello frissítés akkor fordul elő, amikor a Service Fabric előfordulhat, hogy rendelkezik tisztítani hello 1. tanúsítvány regisztrálása a következőben: http.sys annak ellenére, hogy hello többi alkalmazás továbbra is használja. tooprevent, a Service Fabric észleli, hogy nincs-e már egy másik alkalmazáspéldány hello port hello tanúsítvánnyal regisztrált (esedékes toohttp.sys) és a sikertelen hello műveletet.

Ezért a Service Fabric nem támogatja a frissítést két különböző szolgáltatásokat **ugyanazt a portot hello** másik alkalmazási esetekben. Ez azt jelenti, nem használható ugyanaz a tanúsítvány hello hello különböző szolgáltatások ugyanazt a portot. Ha toohave van szüksége egy megosztott tanúsítványt futó hello azonos porton, tooensure adott szolgáltatások kerülnek, a különböző gépeken elhelyezési-korlátozásokkal rendelkező hello van szüksége. Vagy fontolja meg a Service Fabric dinamikus portok lehetőség szerint minden egyes szolgáltatás minden egyes alkalmazás-példányban. 

Ha egy frissítés sikertelen, a https, megjelenik egy hibaüzenet figyelmeztetést, amely szerint a "hello Windows HTTP-kiszolgáló API nem támogatja több tanúsítvány egy portot használó alkalmazásokhoz."

## <a name="a-complete-application-manifest-example"></a>A teljes alkalmazás jegyzékének példa
a következő alkalmazás jegyzékfájlja hello látható hello számos különböző beállításokat:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application3Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="Stateless1_InstanceCount" DefaultValue="-1" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
         <RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup" />
        <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and hello Endpoint is http -->
         <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
        <!--EndpointBindingPolicy is needed hello EndpointName is secured with https -->
        <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
     </Policies>
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="Stateless1">
         <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
   <Principals>
      <Groups>
         <Group Name="LocalAdminGroup">
            <Membership>
               <SystemGroup Name="Administrators" />
            </Membership>
         </Group>
      </Groups>
      <Users>
         <User Name="LocalAdmin">
            <MemberOf>
               <Group NameRef="LocalAdminGroup" />
            </MemberOf>
         </User>
         <!--Customer1 below create a local account that this service runs under -->
         <User Name="Customer1" />
         <User Name="MyDefaultAccount" AccountType="NetworkService" />
      </Users>
   </Principals>
   <Policies>
      <DefaultRunAsPolicy UserRef="LocalAdmin" />
   </Policies>
   <Certificates>
     <EndpointCertificate Name="Cert1" X509FindValue="FF EE E0 TT JJ DD JJ EE EE XX 23 4T 66 "/>
  </Certificates>
</ApplicationManifest>
```


<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Következő lépések
* [Hello alkalmazásmodell ismertetése](service-fabric-application-model.md)
* [Adja meg a szolgáltatás jegyzék erőforrások](service-fabric-service-manifest-resources.md)
* [Alkalmazás üzembe helyezése](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
