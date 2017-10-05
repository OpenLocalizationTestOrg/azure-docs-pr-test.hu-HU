---
title: "A Service Fabric-alkalmazások titkos kulcsok kezelése |} Microsoft Docs"
description: "A cikkből megtudhatja, mennyire biztonságos a Service Fabric-alkalmazás titkos értékeit."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 94a67e45-7094-4fbd-9c88-51f4fc3c523a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: d71924cda8bb3bffbe221946d80dba150359e38e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="managing-secrets-in-service-fabric-applications"></a><span data-ttu-id="f0385-103">A Service Fabric-alkalmazások titkos kulcsok kezelése</span><span class="sxs-lookup"><span data-stu-id="f0385-103">Managing secrets in Service Fabric applications</span></span>
<span data-ttu-id="f0385-104">Ez az útmutató végigvezeti a Service Fabric-alkalmazás a titkos kulcsok kezelése.</span><span class="sxs-lookup"><span data-stu-id="f0385-104">This guide walks you through the steps of managing secrets in a Service Fabric application.</span></span> <span data-ttu-id="f0385-105">Titkos kulcsok lehet bármely bizalmas adatokat, például a tárolási kapcsolati karakterláncok, jelszavak és egyéb értékek, amelyek nem egyszerű szöveges kezelje.</span><span class="sxs-lookup"><span data-stu-id="f0385-105">Secrets can be any sensitive information, such as storage connection strings, passwords, or other values that should not be handled in plain text.</span></span>

<span data-ttu-id="f0385-106">Ez az útmutató az Azure Key Vault használatával kulcsokat és titkos kódokat.</span><span class="sxs-lookup"><span data-stu-id="f0385-106">This guide uses Azure Key Vault to manage keys and secrets.</span></span> <span data-ttu-id="f0385-107">Azonban *használatával* egy alkalmazás titkos kulcsainak van cloud platform-független bárhol üzemeltetett fürt központilag telepített alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="f0385-107">However, *using* secrets in an application is cloud platform-agnostic to allow applications to be deployed to a cluster hosted anywhere.</span></span> 

## <a name="overview"></a><span data-ttu-id="f0385-108">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="f0385-108">Overview</span></span>
<span data-ttu-id="f0385-109">Az ajánlott módszer a szolgáltatás konfigurációs beállítások kezeléséhez van keresztül [szolgáltatás konfigurációs csomagok][config-package].</span><span class="sxs-lookup"><span data-stu-id="f0385-109">The recommended way to manage service configuration settings is through [service configuration packages][config-package].</span></span> <span data-ttu-id="f0385-110">Konfigurációs csomagok a következők: rendszerverzióval ellátott és a rendszerállapot-érvényesítési és automatikus visszaállítási felügyelt közbeni keresztül frissíthető.</span><span class="sxs-lookup"><span data-stu-id="f0385-110">Configuration packages are versioned and updatable through managed rolling upgrades with health-validation and auto rollback.</span></span> <span data-ttu-id="f0385-111">Ez az előnyben részesített a globális konfigurációs, mivel csökkenti a veszélyét annak, hogy a globális szolgáltatáskimaradás.</span><span class="sxs-lookup"><span data-stu-id="f0385-111">This is preferred to global configuration as it reduces the chances of a global service outage.</span></span> <span data-ttu-id="f0385-112">Titkosított titkokat sem kivétel.</span><span class="sxs-lookup"><span data-stu-id="f0385-112">Encrypted secrets are no exception.</span></span> <span data-ttu-id="f0385-113">A Service Fabric titkosítására és visszafejtésére értékek a tanúsítvány titkosítással konfigurációs csomag Settings.xml fájlban beépített lehetőséggel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f0385-113">Service Fabric has built-in features for encrypting and decrypting values in a configuration package Settings.xml file using certificate encryption.</span></span>

<span data-ttu-id="f0385-114">A következő ábra szemlélteti a titkos kezelése a Service Fabric-alkalmazás általános folyamata:</span><span class="sxs-lookup"><span data-stu-id="f0385-114">The following diagram illustrates the basic flow for secret management in a Service Fabric application:</span></span>

![titkos – áttekintés][overview]

<span data-ttu-id="f0385-116">Ez a folyamat négy fő lépésből áll:</span><span class="sxs-lookup"><span data-stu-id="f0385-116">There are four main steps in this flow:</span></span>

1. <span data-ttu-id="f0385-117">Adatok rejtjelezése tanúsítványának beszerzése.</span><span class="sxs-lookup"><span data-stu-id="f0385-117">Obtain a data encipherment certificate.</span></span>
2. <span data-ttu-id="f0385-118">Telepítse a tanúsítványt a fürtön.</span><span class="sxs-lookup"><span data-stu-id="f0385-118">Install the certificate in your cluster.</span></span>
3. <span data-ttu-id="f0385-119">Titkosítani a titkos értékek, a tanúsítvány az alkalmazás telepítésekor, és azokat behelyezése egy szolgáltatás Settings.xml konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="f0385-119">Encrypt secret values when deploying an application with the certificate and inject them into a service's Settings.xml configuration file.</span></span>
4. <span data-ttu-id="f0385-120">Olvassa el a titkosított értékeket abból Settings.xml visszafejti rejtjelezése ugyanazzal a tanúsítvánnyal.</span><span class="sxs-lookup"><span data-stu-id="f0385-120">Read encrypted values out of Settings.xml by decrypting with the same encipherment certificate.</span></span> 

<span data-ttu-id="f0385-121">[Az Azure Key Vault] [ key-vault-get-started] itt történik, a tanúsítványok biztonságos tárolási helyeként, valamint az Azure Service Fabric-fürtök telepített tanúsítványok lekérése is.</span><span class="sxs-lookup"><span data-stu-id="f0385-121">[Azure Key Vault][key-vault-get-started] is used here as a safe storage location for certificates and as a way to get certificates installed on Service Fabric clusters in Azure.</span></span> <span data-ttu-id="f0385-122">Ha nem telepíti az Azure-ba, nem kell Key Vault segítségével kezelheti a Service Fabric-alkalmazások a titkos kulcsok.</span><span class="sxs-lookup"><span data-stu-id="f0385-122">If you are not deploying to Azure, you do not need to use Key Vault to manage secrets in Service Fabric applications.</span></span>

## <a name="data-encipherment-certificate"></a><span data-ttu-id="f0385-123">Adatok rejtjelezése tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="f0385-123">Data encipherment certificate</span></span>
<span data-ttu-id="f0385-124">Adatok rejtjelezése tanúsítvány szigorúan titkosítást használja és konfigurációs visszafejtése egy szolgáltatás Settings.xml értékei, és nem a hitelesítéshez használt vagy aláírási titkosított szöveg.</span><span class="sxs-lookup"><span data-stu-id="f0385-124">A data encipherment certificate is used strictly for encryption and decryption of configuration values in a service's Settings.xml and is not used for authentication or signing of cipher text.</span></span> <span data-ttu-id="f0385-125">A tanúsítvány a következő követelményeknek kell megfelelniük:</span><span class="sxs-lookup"><span data-stu-id="f0385-125">The certificate must meet the following requirements:</span></span>

* <span data-ttu-id="f0385-126">A tanúsítványnak tartalmaznia kell egy titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="f0385-126">The certificate must contain a private key.</span></span>
* <span data-ttu-id="f0385-127">A kulcscseréhez használt, a személyes információcsere (.pfx) fájl exportálható léteznie kell a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="f0385-127">The certificate must be created for key exchange, exportable to a Personal Information Exchange (.pfx) file.</span></span>
* <span data-ttu-id="f0385-128">A tanúsítvány kulcshasználati adattitkosítás (10) kell tartalmaznia, és nem tartalmazhat kiszolgálóhitelesítés vagy ügyfél-hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="f0385-128">The certificate key usage must include Data Encipherment (10), and should not include Server Authentication or Client Authentication.</span></span> 
  
  <span data-ttu-id="f0385-129">Például, amikor a powershellel, önaláírt tanúsítvány létrehozása a `KeyUsage` jelzőt kell beállítani `DataEncipherment`:</span><span class="sxs-lookup"><span data-stu-id="f0385-129">For example, when creating a self-signed certificate using PowerShell, the `KeyUsage` flag must be set to `DataEncipherment`:</span></span>
  
  ```powershell
  New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject mydataenciphermentcert -Provider 'Microsoft Enhanced Cryptographic Provider v1.0'
  ```

## <a name="install-the-certificate-in-your-cluster"></a><span data-ttu-id="f0385-130">A tanúsítvány telepítése a fürt</span><span class="sxs-lookup"><span data-stu-id="f0385-130">Install the certificate in your cluster</span></span>
<span data-ttu-id="f0385-131">Ez a tanúsítvány a fürt minden csomópontján telepítenie kell.</span><span class="sxs-lookup"><span data-stu-id="f0385-131">This certificate must be installed on each node in the cluster.</span></span> <span data-ttu-id="f0385-132">Ez használható futásidőben visszafejteni a szolgáltatás Settings.xml tárolt értékek.</span><span class="sxs-lookup"><span data-stu-id="f0385-132">It will be used at runtime to decrypt values stored in a service's Settings.xml.</span></span> <span data-ttu-id="f0385-133">Lásd: [hogyan hozhat létre egy fürtöt, Azure Resource Manager használatával] [ service-fabric-cluster-creation-via-arm] a telepítési utasításokat.</span><span class="sxs-lookup"><span data-stu-id="f0385-133">See [how to create a cluster using Azure Resource Manager][service-fabric-cluster-creation-via-arm] for setup instructions.</span></span> 

## <a name="encrypt-application-secrets"></a><span data-ttu-id="f0385-134">Alkalmazás titkos kulcsok titkosítása</span><span class="sxs-lookup"><span data-stu-id="f0385-134">Encrypt application secrets</span></span>
<span data-ttu-id="f0385-135">A Service Fabric SDK beépített titkos titkosítási és visszafejtési funkciókkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f0385-135">The Service Fabric SDK has built-in secret encryption and decryption functions.</span></span> <span data-ttu-id="f0385-136">Titkos értékek is kell beépített időpontban titkosított visszafejti és programozott módon olvassa el a szolgáltatáskód hibáit.</span><span class="sxs-lookup"><span data-stu-id="f0385-136">Secret values can be encrypted at built-time and then decrypted and read programmatically in service code.</span></span> 

<span data-ttu-id="f0385-137">A következő PowerShell-parancsot a titkos kulcs titkosítására szolgál.</span><span class="sxs-lookup"><span data-stu-id="f0385-137">The following PowerShell command is used to encrypt a secret.</span></span> <span data-ttu-id="f0385-138">Ez a parancs csak titkosítja a érték; létezik **nem** a titkosított szöveg aláírásához.</span><span class="sxs-lookup"><span data-stu-id="f0385-138">This command only encrypts the value; it does **not** sign the cipher text.</span></span> <span data-ttu-id="f0385-139">Titkosított szöveg titkos értékek létrehozásához a fürtben telepített azonos rejtjelezése tanúsítványt kell használnia:</span><span class="sxs-lookup"><span data-stu-id="f0385-139">You must use the same encipherment certificate that is installed in your cluster to produce ciphertext for secret values:</span></span>

```powershell
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint "<thumbprint>" -Text "mysecret" -StoreLocation CurrentUser -StoreName My
```

<span data-ttu-id="f0385-140">Az eredményül kapott base-64 karakterláncot tartalmaz, mind a titkos titkosított szöveg, valamint a titkosításhoz használt tanúsítvány adatait.</span><span class="sxs-lookup"><span data-stu-id="f0385-140">The resulting base-64 string contains both the secret ciphertext as well as information about the certificate that was used to encrypt it.</span></span>  <span data-ttu-id="f0385-141">A base-64 kódolású karakterlánc szúrhatók be, a szolgáltatás Settings.xml tartalmazó konfigurációs fájl egyik paraméterének a `IsEncrypted` attribútum értékének beállítása `true`:</span><span class="sxs-lookup"><span data-stu-id="f0385-141">The base-64 encoded string can be inserted into a parameter in your service's Settings.xml configuration file with the `IsEncrypted` attribute set to `true`:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=" />
  </Section>
</Settings>
```

### <a name="inject-application-secrets-into-application-instances"></a><span data-ttu-id="f0385-142">Az alkalmazáspéldányok alkalmazás titkos kulcsok behelyezése</span><span class="sxs-lookup"><span data-stu-id="f0385-142">Inject application secrets into application instances</span></span>
<span data-ttu-id="f0385-143">Ideális esetben kell lennie az automatizált lehető különböző környezetekben történő telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="f0385-143">Ideally, deployment to different environments should be as automated as possible.</span></span> <span data-ttu-id="f0385-144">A titkos végrehajtása build környezetben, és a titkosított titkos kulcsok megadásával paraméterként, alkalmazáspéldányok létrehozásakor kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="f0385-144">This can be accomplished by performing secret encryption in a build environment and providing the encrypted secrets as parameters when creating application instances.</span></span>

#### <a name="use-overridable-parameters-in-settingsxml"></a><span data-ttu-id="f0385-145">Felülbírálható paraméterekkel Settings.xml használata</span><span class="sxs-lookup"><span data-stu-id="f0385-145">Use overridable parameters in Settings.xml</span></span>
<span data-ttu-id="f0385-146">A Settings.xml konfigurációs fájl lehetővé teszi, hogy az alkalmazás létrehozáskor megadható felülbírálható paramétereket.</span><span class="sxs-lookup"><span data-stu-id="f0385-146">The Settings.xml configuration file allows overridable parameters that can be provided at application creation time.</span></span> <span data-ttu-id="f0385-147">Használja a `MustOverride` attribútum helyett egy érték megadása az egyik paraméter:</span><span class="sxs-lookup"><span data-stu-id="f0385-147">Use the `MustOverride` attribute instead of providing a value for a parameter:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="" MustOverride="true" />
  </Section>
</Settings>
```

<span data-ttu-id="f0385-148">A Settings.xml érték felülírására deklarál egy felülbíráló paraméter ApplicationManifest.xml szolgáltatása esetében:</span><span class="sxs-lookup"><span data-stu-id="f0385-148">To override values in Settings.xml, declare an override parameter for the service in ApplicationManifest.xml:</span></span>

```xml
<ApplicationManifest ... >
  <Parameters>
    <Parameter Name="MySecret" DefaultValue="" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides>
      <ConfigOverride Name="Config">
        <Settings>
          <Section Name="MySettings">
            <Parameter Name="MySecret" Value="[MySecret]" IsEncrypted="true" />
          </Section>
        </Settings>
      </ConfigOverride>
    </ConfigOverrides>
  </ServiceManifestImport>
 ```

<span data-ttu-id="f0385-149">Most a értéket adhat meg egy *alkalmazás paraméter* az alkalmazás példányának létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="f0385-149">Now the value can be specified as an *application parameter* when creating an instance of the application.</span></span> <span data-ttu-id="f0385-150">Az alkalmazáspéldány létrehozása parancsfájlalapú lehet powershellel, vagy az egyszerű integráció az összeállítási folyamat a C#, verziójához.</span><span class="sxs-lookup"><span data-stu-id="f0385-150">Creating an application instance can be scripted using PowerShell, or written in C#, for easy integration in a build process.</span></span>

<span data-ttu-id="f0385-151">PowerShell használatával, a paraméter van megadva a a `New-ServiceFabricApplication` parancs, mint egy [kivonattábla](https://technet.microsoft.com/library/ee692803.aspx):</span><span class="sxs-lookup"><span data-stu-id="f0385-151">Using PowerShell, the parameter is supplied to the `New-ServiceFabricApplication` command as a [hash table](https://technet.microsoft.com/library/ee692803.aspx):</span></span>

```powershell
PS C:\Users\vturecek> New-ServiceFabricApplication -ApplicationName fabric:/MyApp -ApplicationTypeName MyAppType -ApplicationTypeVersion 1.0.0 -ApplicationParameter @{"MySecret" = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM="}
```

<span data-ttu-id="f0385-152">C# használatával alkalmazás paraméter meg van adva a egy `ApplicationDescription` , egy `NameValueCollection`:</span><span class="sxs-lookup"><span data-stu-id="f0385-152">Using C#, application parameters are specified in an `ApplicationDescription` as a `NameValueCollection`:</span></span>

```csharp
FabricClient fabricClient = new FabricClient();

NameValueCollection applicationParameters = new NameValueCollection();
applicationParameters["MySecret"] = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=";

ApplicationDescription applicationDescription = new ApplicationDescription(
    applicationName: new Uri("fabric:/MyApp"),
    applicationTypeName: "MyAppType",
    applicationTypeVersion: "1.0.0",
    applicationParameters: applicationParameters)
);

await fabricClient.ApplicationManager.CreateApplicationAsync(applicationDescription);
```

## <a name="decrypt-secrets-from-service-code"></a><span data-ttu-id="f0385-153">Szolgáltatás kódból titkos kulcsok visszafejtéséhez</span><span class="sxs-lookup"><span data-stu-id="f0385-153">Decrypt secrets from service code</span></span>
<span data-ttu-id="f0385-154">A Service Fabric szolgáltatások alapértelmezés szerint a Windows hálózati szolgáltatás alatt futnak, és nem férnek hozzá a csomópont egy külön beállítások elvégzése nélkül telepített tanúsítványok.</span><span class="sxs-lookup"><span data-stu-id="f0385-154">Services in Service Fabric run under NETWORK SERVICE by default on Windows and don't have access to certificates installed on the node without some extra setup.</span></span>

<span data-ttu-id="f0385-155">Ha adatok rejtjelezése tanúsítványt használ, meg kell győződnie, hogy a hálózati szolgáltatás, vagy tetszőleges felhasználói fiókot a szolgáltatás fut. a tanúsítvány titkos kulcsához hozzáfér.</span><span class="sxs-lookup"><span data-stu-id="f0385-155">When using a data encipherment certificate, you need to make sure NETWORK SERVICE or whatever user account the service is running under has access to the certificate's private key.</span></span> <span data-ttu-id="f0385-156">A Service Fabric fogja kezelni, a szolgáltatás-hozzáférés automatikusan engedélyezése, ha úgy állítja be ehhez.</span><span class="sxs-lookup"><span data-stu-id="f0385-156">Service Fabric will handle granting access for your service automatically if you configure it to do so.</span></span> <span data-ttu-id="f0385-157">Ez a konfiguráció ApplicationManifest.xml felhasználók és biztonsági házirendeket a tanúsítványok megadásával teheti.</span><span class="sxs-lookup"><span data-stu-id="f0385-157">This configuration can be done in ApplicationManifest.xml by defining users and security policies for certificates.</span></span> <span data-ttu-id="f0385-158">A következő példában a hálózati szolgáltatás fiók olvasás való hozzáférése annak ujjlenyomata által definiált tanúsítványt:</span><span class="sxs-lookup"><span data-stu-id="f0385-158">In the following example, the NETWORK SERVICE account is given read access to a certificate defined by its thumbprint:</span></span>

```xml
<ApplicationManifest … >
    <Principals>
        <Users>
            <User Name="Service1" AccountType="NetworkService" />
        </Users>
    </Principals>
  <Policies>
    <SecurityAccessPolicies>
      <SecurityAccessPolicy GrantRights=”Read” PrincipalRef="Service1" ResourceRef="MyCert" ResourceType="Certificate"/>
    </SecurityAccessPolicies>
  </Policies>
  <Certificates>
    <SecretsCertificate Name="MyCert" X509FindType="FindByThumbprint" X509FindValue="[YourCertThumbrint]"/>
  </Certificates>
</ApplicationManifest>
```

> [!NOTE]
> <span data-ttu-id="f0385-159">Amikor másol egy tanúsítvány-ujjlenyomata a tanúsítvány tároló beépülő modul a Windows, egy láthatatlan karakter el van helyezve az ujjlenyomat karakterlánc elején.</span><span class="sxs-lookup"><span data-stu-id="f0385-159">When copying a certificate thumbprint from the certificate store snap-in on Windows, an invisible character is placed at the beginning of the thumbprint string.</span></span> <span data-ttu-id="f0385-160">Ez láthatatlan karakter hibát okozhat, keresse meg a tanúsítvány ujjlenyomata által, ezért ügyeljen arra, hogy törli ezt a felesleges karaktert közben.</span><span class="sxs-lookup"><span data-stu-id="f0385-160">This invisible character can cause an error when trying to locate a certificate by thumbprint, so be sure to delete this extra character.</span></span>
> 
> 

### <a name="use-application-secrets-in-service-code"></a><span data-ttu-id="f0385-161">Alkalmazás titkos kulcsok használata szolgáltatáskódot</span><span class="sxs-lookup"><span data-stu-id="f0385-161">Use application secrets in service code</span></span>
<span data-ttu-id="f0385-162">A konfigurációs értékek Settings.xml eléréséhez a konfigurációs csomag API lehetővé teszi, hogy az értékek, amelyek könnyen visszafejtése a `IsEncrypted` attribútum értékének beállítása `true`.</span><span class="sxs-lookup"><span data-stu-id="f0385-162">The API for accessing configuration values from Settings.xml in a configuration package allows for easy decrypting of values that have the `IsEncrypted` attribute set to `true`.</span></span> <span data-ttu-id="f0385-163">Mivel a titkosított szöveg tartalmazza-e a titkosításhoz használt tanúsítvány adatait, nem kell manuálisan található a tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="f0385-163">Since the encrypted text contains information about the certificate used for encryption, you do not need to manually find the certificate.</span></span> <span data-ttu-id="f0385-164">A tanúsítvány csak a szolgáltatás fut a csomóponton telepítenie kell.</span><span class="sxs-lookup"><span data-stu-id="f0385-164">The certificate just needs to be installed on the node that the service is running on.</span></span> <span data-ttu-id="f0385-165">Egyszerűen hívja a `DecryptValue()` metódusának segítéségével lekérheti az eredeti titkos érték:</span><span class="sxs-lookup"><span data-stu-id="f0385-165">Simply call the `DecryptValue()` method to retrieve the original secret value:</span></span>

```csharp
ConfigurationPackage configPackage = this.Context.CodePackageActivationContext.GetConfigurationPackageObject("Config");
SecureString mySecretValue = configPackage.Settings.Sections["MySettings"].Parameters["MySecret"].DecryptValue()
```

## <a name="next-steps"></a><span data-ttu-id="f0385-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f0385-166">Next Steps</span></span>
<span data-ttu-id="f0385-167">További információ [futó alkalmazások különböző biztonsági engedélyekkel](service-fabric-application-runas-security.md)</span><span class="sxs-lookup"><span data-stu-id="f0385-167">Learn more about [running applications with different security permissions](service-fabric-application-runas-security.md)</span></span>

<!-- Links -->
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[config-package]: service-fabric-application-model.md
[service-fabric-cluster-creation-via-arm]: service-fabric-cluster-creation-via-arm.md

<!-- Images -->
[overview]:./media/service-fabric-application-secret-management/overview.png
