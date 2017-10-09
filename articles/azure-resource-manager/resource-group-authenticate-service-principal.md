---
title: "az Azure PowerShell-alkalmazás identitását aaaCreate |} Microsoft Docs"
description: "Ismerteti, hogyan szabályozza a toouse Azure PowerShell toocreate egy Azure Active Directory-alkalmazás és szolgáltatás egyszerű, és azt keresztül férnek hozzá tooresources szerepkörön alapuló hozzáférés biztosítása. Azt illusztrálja, hogyan tooauthenticate alkalmazás jelszóval vagy tanúsítvánnyal."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: d2caf121-9fbe-4f00-bf9d-8f3d1f00a6ff
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: c534360799b590054a051e4426e5e27dccb559b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-powershell-toocreate-a-service-principal-tooaccess-resources"></a>A szolgáltatás egyszerű tooaccess erőforrásainak használatához az Azure PowerShell toocreate

Ha egy alkalmazás vagy parancsfájlt, amelyet a tooaccess erőforrások, hello alkalmazás identitás beállítása, és hitelesíteni hello alkalmazást a saját hitelesítő adatokkal. Ezzel az identitással egyszerű szolgáltatás néven ismert. Ez a megközelítés lehetővé teszi:

* Engedélyek hozzárendelése saját engedélyek eltérő toohello identitását. Ezek az engedélyek általában korlátozott tooexactly milyen hello alkalmazásnak kell toodo.
* A tanúsítvány használata a hitelesítéshez, egy felügyelet nélküli parancsfájl végrehajtása közben.

Ez a témakör bemutatja, hogyan toouse [Azure PowerShell](/powershell/azure/overview) tooset másolatot minden olyan alkalmazás toorun alatt a saját hitelesítő adatokat és -identitás van szüksége.

## <a name="required-permissions"></a>Szükséges engedélyek
toocomplete Ez a témakör megfelelő engedélyekkel kell rendelkeznie az Azure Active Directory és az Azure-előfizetésében is. Pontosabban tudja toocreate hello Azure Active Directory az alkalmazásban, és hello szolgáltatás egyszerű tooa szerepkör hozzárendelése. 

hello legegyszerűbb módja toocheck hello portálon keresztül van a fiók rendelkezik-e megfelelő engedélyekkel. Lásd: [szükséges engedély ellenőrzése](resource-group-create-service-principal-portal.md#required-permissions).

A következő lépésben tooa szakasz a hitelesítéséhez:

* [jelszó](#create-service-principal-with-password)
* [önaláírt tanúsítvány](#create-service-principal-with-self-signed-certificate)
* [a hitelesítésszolgáltató tanúsítványa](#create-service-principal-with-certificate-from-certificate-authority)

## <a name="powershell-commands"></a>PowerShell-parancsok

egy egyszerű szolgáltatás tooset, használhatja:

| Parancs | Leírás |
| ------- | ----------- | 
| [Új AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) | Létrehoz egy Azure Active Directory szolgáltatás egyszerű |
| [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) | Rendel hello RBAC szerepkör toohello megadott egyszerű megadva, a hello: a megadott hatókörben. |


## <a name="create-service-principal-with-password"></a>Egyszerű szolgáltatásnév létrehozása jelszóval

toocreate hello közreműködő szerepkört az előfizetéséhez, a szolgáltatásnevet használja: 

```powershell
Login-AzureRmAccount
$sp = New-AzureRmADServicePrincipal -DisplayName exampleapp -Password "{provide-password}"
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

hello példa alvó állapotba kerül 20 másodperc tooallow némi időbe, hello új szolgáltatás egyszerű toopropagate egész Azure Active Directoryban. Ha a parancsfájl nem elegendő ideig kell várni, megjelenik egy üzenet szerint: "PrincipalNotFound: {azonosítójú} egyszerű hello könyvtárban nem létezik."

hello következő parancsfájl lehetővé teszi toospecify eltérő hello alapértelmezett előfizetési hatókört, és az újrapróbálkozások hello szerepkör-hozzárendelés, ha a hiba akkor fordul elő:

```powershell
Param (

 # Use tooset scope tooresource group. If no value is provided, scope is set toosubscription.
 [Parameter(Mandatory=$false)]
 [String] $ResourceGroup,

 # Use tooset subscription. If no value is provided, default subscription is used. 
 [Parameter(Mandatory=$false)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $Password
)

 Login-AzureRmAccount
 Import-Module AzureRM.Resources

 if ($SubscriptionId -eq "") 
 {
    $SubscriptionId = (Get-AzureRmContext).Subscription.Id
 }
 else
 {
    Set-AzureRmContext -SubscriptionId $SubscriptionId
 }

 if ($ResourceGroup -eq "")
 {
    $Scope = "/subscriptions/" + $SubscriptionId
 }
 else
 {
    $Scope = (Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop).ResourceId
 }

 
 # Create Service Principal for hello AD app
 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -Password $Password
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds tooallow hello service principal application toobecome active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
 }
```

Néhány elemet toonote hello parancsfájl kapcsolatban:

* toogrant hello identitás hozzáférés toohello alapértelmezett előfizetést, akkor nem kell tooprovide ResourceGroup vagy a SubscriptionId paraméterek.
* Adja meg a hello ResourceGroup paraméter csak akkor, ha azt szeretné, hogy toolimit hello hatókör hello szerepkör hozzárendelése tooa erőforráscsoport.
*  Ebben a példában hello szolgáltatás egyszerű toohello közreműködői szerepkör hozzáadása. Más szerepköreivel kapcsolatban, tekintse meg a [RBAC: beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md).
* hello parancsfájl alvó állapotban marad, a 15 másodperces tooallow némi időbe, hello új szolgáltatás egyszerű toopropagate egész Azure Active Directory. Ha a parancsfájl nem elegendő ideig kell várni, megjelenik egy üzenet szerint: "PrincipalNotFound: {azonosítójú} egyszerű hello könyvtárban nem létezik."
* Ha toogrant hello szolgáltatás egyszerű hozzáférés toomore előfizetések vagy erőforráscsoportok van szüksége, futtassa a hello `New-AzureRMRoleAssignment` parancsmagot újra különböző hatóköröket.


### <a name="provide-credentials-through-powershell"></a>Adjon meg hitelesítő adatokat a PowerShell segítségével
Most van szüksége a toolog hello alkalmazás tooperform műveletként. Hello felhasználónévvel, használja a hello `ApplicationId` hello alkalmazáshoz létrehozott. Hello jelszót egy hello fiók létrehozásakor megadott hello használata. 

```powershell   
$creds = Get-Credential
Login-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId {tenant-id}
```

hello Bérlőazonosító nincs különbözőnek számítanak, így közvetlenül a parancsfájlban beágyazása. Ha tooretrieve hello Bérlőazonosító van szüksége, használja:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

## <a name="create-service-principal-with-self-signed-certificate"></a>Egyszerű szolgáltatásnév létrehozása önaláírt tanúsítvánnyal

toocreate egy önaláírt tanúsítványt és hello közreműködő szerepkört az előfizetés szolgáltatásnevet használja: 

```powershell
Login-AzureRmAccount
$cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleappScriptCert" -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

$sp = New-AzureRMADServicePrincipal -DisplayName exampleapp -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

hello példa alvó állapotba kerül 20 másodperc tooallow némi időbe, hello új szolgáltatás egyszerű toopropagate egész Azure Active Directoryban. Ha a parancsfájl nem elegendő ideig kell várni, megjelenik egy üzenet szerint: "PrincipalNotFound: {azonosítójú} egyszerű hello könyvtárban nem létezik."

hello következő parancsfájl lehetővé teszi toospecify eltérő hello alapértelmezett előfizetési hatókört, és az újrapróbálkozások hello szerepkör-hozzárendelés, ha a hiba akkor fordul elő. Azure PowerShell 2.0 a Windows 10 vagy Windows Server 2016 kell rendelkeznie.

```powershell
Param (

 # Use tooset scope tooresource group. If no value is provided, scope is set toosubscription.
 [Parameter(Mandatory=$false)]
 [String] $ResourceGroup,

 # Use tooset subscription. If no value is provided, default subscription is used. 
 [Parameter(Mandatory=$false)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName
 )

 Login-AzureRmAccount
 Import-Module AzureRM.Resources

 if ($SubscriptionId -eq "") 
 {
    $SubscriptionId = (Get-AzureRmContext).Subscription.Id
 }
 else
 {
    Set-AzureRmContext -SubscriptionId $SubscriptionId
 }

 if ($ResourceGroup -eq "")
 {
    $Scope = "/subscriptions/" + $SubscriptionId
 }
 else
 {
    $Scope = (Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop).ResourceId
 }

 $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleappScriptCert" -KeySpec KeyExchange
 $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds tooallow hello service principal application toobecome active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
 }
```

Néhány elemet toonote hello parancsfájl kapcsolatban:

* toogrant hello identitás hozzáférés toohello alapértelmezett előfizetést, akkor nem kell tooprovide ResourceGroup vagy a SubscriptionId paraméterek.
* Adja meg a hello ResourceGroup paraméter csak akkor, ha azt szeretné, hogy toolimit hello hatókör hello szerepkör hozzárendelése tooa erőforráscsoport.
* Ebben a példában hello szolgáltatás egyszerű toohello közreműködői szerepkör hozzáadása. Más szerepköreivel kapcsolatban, tekintse meg a [RBAC: beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md).
* hello parancsfájl alvó állapotban marad, a 15 másodperces tooallow némi időbe, hello új szolgáltatás egyszerű toopropagate egész Azure Active Directory. Ha a parancsfájl nem elegendő ideig kell várni, megjelenik egy üzenet szerint: "PrincipalNotFound: {azonosítójú} egyszerű hello könyvtárban nem létezik."
* Ha toogrant hello szolgáltatás egyszerű hozzáférés toomore előfizetések vagy erőforráscsoportok van szüksége, futtassa a hello `New-AzureRMRoleAssignment` parancsmagot újra különböző hatóköröket.

Ha Ön **nem rendelkeznek Windows 10 vagy Windows Server 2016 Technical Preview**, toodownload hello kell [önaláírt tanúsítvány generátor](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) Microsoft Script Center. Bontsa ki a tartalmát, és hello parancsmag kell importálni.

```powershell  
# Only run if you could not use New-SelfSignedCertificate
Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
```
  
Hello parancsfájlban helyettesítse be a következő két sorok toogenerate hello tanúsítvány hello.
  
```powershell
New-SelfSignedCertificateEx  -StoreLocation CurrentUser -StoreName My -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"
$cert = Get-ChildItem -path Cert:\CurrentUser\my | where {$PSitem.Subject -eq 'CN=exampleapp' }
```

### <a name="provide-certificate-through-automated-powershell-script"></a>Adja meg a tanúsítvány automatikus PowerShell-parancsfájl segítségével
Amikor jelentkezik be, és egy egyszerű szolgáltatást, az AD-alkalmazás kell tooprovide hello bérlőazonosító hello könyvtár. A bérlő az Azure Active Directory példánya. Ha több előfizetéssel rendelkezik, akkor használhatja:

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertSubject,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $Thumbprint = (Get-ChildItem cert:\CurrentUser\My\ | Where-Object {$_.Subject -match $CertSubject }).Thumbprint
 Login-AzureRmAccount -ServicePrincipal -CertificateThumbprint $Thumbprint -ApplicationId $ApplicationId -TenantId $TenantId
```

hello alkalmazás és bérlő-azonosító nem különbség, így közvetlenül a parancsfájlban beágyazhatók. Ha tooretrieve hello Bérlőazonosító van szüksége, használja:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

Ha tooretrieve hello Alkalmazásazonosítót kell használni:

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="create-service-principal-with-certificate-from-certificate-authority"></a>Egyszerű szolgáltatásnév létrehozása hitelesítésszolgáltatótól származó tanúsítvánnyal
egy tanúsítványt egy hitelesítésszolgáltatótól toocreate szolgáltatás egyszerű, a következő parancsfájl használata hello toouse:

```powershell
Param (
 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword
 )

 Login-AzureRmAccount
 Import-Module AzureRM.Resources
 Set-AzureRmContext -SubscriptionId $SubscriptionId

 $KeyId = (New-Guid).Guid
 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force

 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $KeyValue = [System.Convert]::ToBase64String($PFXCert.GetRawCertData())

 $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
 $KeyCredential.StartDate = $PFXCert.NotBefore
 $KeyCredential.EndDate= $PFXCert.NotAfter
 $KeyCredential.KeyId = $KeyId
 $KeyCredential.CertValue = $KeyValue

 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -KeyCredentials $keyCredential
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds tooallow hello service principal application toobecome active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
 }
 
 $NewRole
```

Néhány elemet toonote hello parancsfájl kapcsolatban:

* Hozzáférési hatókörrel rendelkező toohello előfizetés.
* Ebben a példában hello szolgáltatás egyszerű toohello közreműködői szerepkör hozzáadása. Más szerepköreivel kapcsolatban, tekintse meg a [RBAC: beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md).
* hello parancsfájl alvó állapotban marad, a 15 másodperces tooallow némi időbe, hello új szolgáltatás egyszerű toopropagate egész Azure Active Directory. Ha a parancsfájl nem elegendő ideig kell várni, megjelenik egy üzenet szerint: "PrincipalNotFound: {azonosítójú} egyszerű hello könyvtárban nem létezik."
* Ha toogrant hello szolgáltatás egyszerű hozzáférés toomore előfizetések vagy erőforráscsoportok van szüksége, futtassa a hello `New-AzureRMRoleAssignment` parancsmagot újra különböző hatóköröket.

### <a name="provide-certificate-through-automated-powershell-script"></a>Adja meg a tanúsítvány automatikus PowerShell-parancsfájl segítségével
Amikor jelentkezik be, és egy egyszerű szolgáltatást, az AD-alkalmazás kell tooprovide hello bérlőazonosító hello könyvtár. A bérlő az Azure Active Directory példánya.

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $Thumbprint = $PFXCert.Thumbprint

 Login-AzureRmAccount -ServicePrincipal -CertificateThumbprint $Thumbprint -ApplicationId $ApplicationId -TenantId $TenantId
```

hello alkalmazás és bérlő-azonosító nem különbség, így közvetlenül a parancsfájlban beágyazhatók. Ha tooretrieve hello Bérlőazonosító van szüksége, használja:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

Ha tooretrieve hello Alkalmazásazonosítót kell használni:

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="change-credentials"></a>Hitelesítő adatok módosítása

AD-alkalmazás, vagy a biztonsági sérülés vagy a hitelesítő adatok érvényessége miatt toochange hello hitelesítő adatait használja hello [Remove-AzureRmADAppCredential](/powershell/resourcemanager/azurerm.resources/v3.3.0/remove-azurermadappcredential) és [New-AzureRmADAppCredential](/powershell/module/azurerm.resources/new-azurermadappcredential) parancsmagok.

tooremove minden hello hitelesítő alkalmazás esetén használja:

```powershell
Remove-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -All
```

a jelszó tooadd használja:

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -Password p@ssword!
```

tooadd tanúsítvány értéket, hozzon létre egy önaláírt tanúsítványt, ebben a témakörben ismertetett módon. Ezután használja:

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
```

## <a name="save-access-token-toosimplify-log-in"></a>Mentse a hozzáférési token toosimplify jelentkezzen be
tooavoid hello szolgáltatás egyszerű hitelesítő adatok minden alkalommal, amikor a toolog szükséges, mentheti hello hozzáférési jogkivonat.

toouse hello aktuális hozzáférési jogkivonat egy újabb munkamenetben hello profilt menthet.
   
```powershell
Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```
   
Nyissa meg a hello-profil, és vizsgálja meg annak tartalmát. Figyelje meg, hogy az tartalmazza-e olyan hozzáférési jogkivonatot. Manuálisan a bejelentkezés újra helyett egyszerűen hello-profil betöltése.
   
```powershell
Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```

> [!NOTE]
> hello hozzáférési jogkivonat lejár, így csak a működik a mentett profil használatával mindaddig, amíg hello lexikális elem érvénytelen.
>  

Azt is megteheti hívhat meg a PowerShell toolog REST műveletek. Hello hitelesítési választ, a hozzáférési jogkivonat hello használható egyéb műveletek kérheti le. Például egy hello hozzáférési jogkivonat beolvasása REST műveleteinek figyelőn, lásd: [generálása egy hozzáférési jogkivonat](resource-manager-rest-api.md#generating-an-access-token).

## <a name="debug"></a>Hibakeresés

Egy egyszerű szolgáltatás létrehozásakor a következő hibák hello merülhetnek fel:

* **"Authentication_Unauthorized"** vagy **"előfizetést hello a környezetben található."** – Ezt a hibaüzenetet látja, ha a fiók nem rendelkezik hello [szükséges engedélyek](#required-permissions) a hello Azure Active Directory tooregister egy alkalmazást. Általában ezt a hibát látva az Azure Active Directoryban csak rendszergazda felhasználók regisztrálhatják az alkalmazásokat, és a fiók nincs a rendszergazda segítségét. Kérje a rendszergazda tooeither rendelje hozzá, akkor tooan rendszergazdai szerepkör, illetve tooenable felhasználók tooregister alkalmazások.

* A fiók **"Nincs engedély tooperform művelet"Microsoft.Authorization/roleAssignments/write"hatókörben"/Subscriptions/ {guid}"."**  -Ezt a hibaüzenetet látja, ha a fiók nem rendelkezik elegendő engedélyekkel tooassign egy szerepkör tooan identitást. Kérje meg az előfizetés rendszergazdája tooadd meg tooUser hozzáférési rendszergazdai szerepkört.

## <a name="sample-applications"></a>Mintaalkalmazások
Különböző platformokon keresztül hello alkalmazásként naplózás kapcsolatos információkért lásd:

* [.NET](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [Java](/java/azure/java-sdk-azure-authenticate)
* [Node.js](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [Python](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Következő lépések
* Az alkalmazás integrálása az Azure erőforrások kezeléséhez részletes lépéseiért lásd: [– fejlesztői útmutató tooauthorization hello Azure Resource Manager API-val rendelkező](resource-manager-api-authentication.md).
* Egy részletes ismertetése az alkalmazások és szolgáltatásnevekről [alkalmazás és szolgáltatás egyszerű objektumok](../active-directory/active-directory-application-objects.md). 
* Azure Active Directory-hitelesítéssel kapcsolatos további információkért lásd: [hitelesítési forgatókönyvek az Azure AD](../active-directory/active-directory-authentication-scenarios.md).
* Az elérhető műveleteket, lehet megadott vagy toousers megtagadva listájáért lásd: [Azure Resource Manager erőforrás-szolgáltató műveletek](../active-directory/role-based-access-control-resource-provider-operations.md).

