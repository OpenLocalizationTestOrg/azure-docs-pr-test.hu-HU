---
title: "az Azure Key Vault-webalkalmazások aaaUse |} Microsoft Docs"
description: "Az oktatóanyag toohelp megtudhatja, hogyan toouse Azure kulcsot használó webalkalmazások használja."
services: key-vault
documentationcenter: 
author: adhurwit
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 9b7d065e-1979-4397-8298-eeba3aec4792
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: adhurwit
ms.openlocfilehash: d5e2299e60b379c4e234d5cd6be03411c5a5c958
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-key-vault-from-a-web-application"></a><span data-ttu-id="12d05-103">Használja az Azure Key Vault-webalkalmazások</span><span class="sxs-lookup"><span data-stu-id="12d05-103">Use Azure Key Vault from a Web Application</span></span>
## <a name="introduction"></a><span data-ttu-id="12d05-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="12d05-104">Introduction</span></span>
<span data-ttu-id="12d05-105">Az oktatóanyag toohelp megtudhatja, hogyan toouse Azure kulcsot tároló az Azure-webalkalmazások használja.</span><span class="sxs-lookup"><span data-stu-id="12d05-105">Use this tutorial toohelp you learn how toouse Azure Key Vault from a web application in Azure.</span></span> <span data-ttu-id="12d05-106">Végigvezeti a titkos kulcs eléréséhez, hogy a webes alkalmazás használható az Azure Key Vault hello folyamatán.</span><span class="sxs-lookup"><span data-stu-id="12d05-106">It walks you through hello process of accessing a secret from an Azure Key Vault so that it can be used in your web application.</span></span>

<span data-ttu-id="12d05-107">**Becsült idő toocomplete:** 15 perc</span><span class="sxs-lookup"><span data-stu-id="12d05-107">**Estimated time toocomplete:** 15 minutes</span></span>

<span data-ttu-id="12d05-108">Áttekintést az Azure Key Vaultról a [What is Azure Key Vault?](key-vault-whatis.md) (Mi az Azure Key Vault?) című cikkben talál.</span><span class="sxs-lookup"><span data-stu-id="12d05-108">For overview information about Azure Key Vault, see [What is Azure Key Vault?](key-vault-whatis.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="12d05-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="12d05-109">Prerequisites</span></span>
<span data-ttu-id="12d05-110">toocomplete ebben az oktatóanyagban rendelkeznie kell a következő hello:</span><span class="sxs-lookup"><span data-stu-id="12d05-110">toocomplete this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="12d05-111">Egy Azure Key Vault az URI tooa titkos kulcs</span><span class="sxs-lookup"><span data-stu-id="12d05-111">A URI tooa secret in an Azure Key Vault</span></span>
* <span data-ttu-id="12d05-112">Egy ügyfél-Azonosítót és egy Ügyfélkulcsot webalkalmazás regisztrálva az Azure Active Directoryban, amely rendelkezik Key Vault hozzáférés tooyour</span><span class="sxs-lookup"><span data-stu-id="12d05-112">A Client ID and a Client Secret for a web application registered with Azure Active Directory that has access tooyour Key Vault</span></span>
* <span data-ttu-id="12d05-113">A webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="12d05-113">A web application.</span></span> <span data-ttu-id="12d05-114">Fogja azt megjelenítő hello lépéseket egy ASP.NET MVC alkalmazás üzembe helyezett Azure-bA egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="12d05-114">We will be showing hello steps for an ASP.NET MVC application deployed in Azure as a Web App.</span></span>

> [!NOTE]
> <span data-ttu-id="12d05-115">Fontos, hogy végrehajtotta hello felsorolt [Ismerkedés az Azure Key Vault](key-vault-get-started.md) ehhez az oktatóanyaghoz, hogy hello URI tooa titkos kulcs és a hello ügyfél-azonosító és a titkos ügyfélkódot webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="12d05-115">It is essential that you have completed hello steps listed in [Get Started with Azure Key Vault](key-vault-get-started.md) for this tutorial so that you have hello URI tooa secret and hello Client ID and Client Secret for a web application.</span></span>
> 
> 

<span data-ttu-id="12d05-116">hello webalkalmazás, amelyek hozzáférhetnek a Key Vault hello hello egy regisztrált az Azure Active Directoryban, és megkapta-e hozzáférési tooyour Key Vault.</span><span class="sxs-lookup"><span data-stu-id="12d05-116">hello web application that will be accessing hello Key Vault is hello one that is registered in Azure Active Directory and has been given access tooyour Key Vault.</span></span> <span data-ttu-id="12d05-117">Ha ez nem hello helyzet, lépjen vissza az alkalmazás hello első lépéseket bemutató oktatóanyaghoz tooRegister, és ismételje meg a felsorolt hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="12d05-117">If this is not hello case, go back tooRegister an Application in hello Get Started tutorial and repeat hello steps listed.</span></span>

<span data-ttu-id="12d05-118">Ez az oktatóanyag célja webfejlesztőknek, amely az Azure-webalkalmazások létrehozása hello alapok elsajátítása.</span><span class="sxs-lookup"><span data-stu-id="12d05-118">This tutorial is designed for web developers that understand hello basics of creating web applications on Azure.</span></span> <span data-ttu-id="12d05-119">Azure Web Apps kapcsolatos további információkért lásd: [webalkalmazások áttekintése](../app-service-web/app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="12d05-119">For more information about Azure Web Apps, see [Web Apps overview](../app-service-web/app-service-web-overview.md).</span></span>

## <span data-ttu-id="12d05-120"><a id="packages"></a>Adja hozzá a Nuget-csomagok</span><span class="sxs-lookup"><span data-stu-id="12d05-120"><a id="packages"></a>Add Nuget Packages</span></span>
<span data-ttu-id="12d05-121">Két csomagot, amelyet a webes alkalmazás a toohave telepítve van.</span><span class="sxs-lookup"><span data-stu-id="12d05-121">There are two packages that your web application needs toohave installed.</span></span>

* <span data-ttu-id="12d05-122">Active Directory Authentication Library - Azure Active Directoryban való interakció és az identitásfelügyelet metódust tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="12d05-122">Active Directory Authentication Library - contains methods for interacting with Azure Active Directory and managing user identity</span></span>
* <span data-ttu-id="12d05-123">Az Azure Key Vault Library - kommunikáció az Azure Key Vault metódust tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="12d05-123">Azure Key Vault Library - contains methods for interacting with Azure Key Vault</span></span>

<span data-ttu-id="12d05-124">Mindkét ezeket a csomagokat telepítheti hello Csomagkezelő konzol hello Install-Package parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="12d05-124">Both of these packages can be installed using hello Package Manager Console using hello Install-Package command.</span></span>

    // this is currently hello latest stable version of ADAL
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault


## <span data-ttu-id="12d05-125"><a id="webconfig"></a>Web.Config módosítása</span><span class="sxs-lookup"><span data-stu-id="12d05-125"><a id="webconfig"></a>Modify Web.Config</span></span>
<span data-ttu-id="12d05-126">Nincsenek három-alkalmazásbeállításokat, amelyek a következőképpen toobe hozzáadott toohello web.config fájl szükséges.</span><span class="sxs-lookup"><span data-stu-id="12d05-126">There are three application settings that need toobe added toohello web.config file as follows.</span></span>

    <!-- ClientId and ClientSecret refer toohello web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is hello URI for hello secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />


<span data-ttu-id="12d05-127">Ha toohost az Azure Web Apps, az alkalmazás nem fog, majd adja hozzá hello tényleges ClientId, a titkos Ügyfélkulcs és a titkos kulcs URI értékek toohello web.config. Ellenkező esetben hagyja ezeket az üres értékeket, mert bővítjük hello tényleges értékek hello Azure portál egy további szintű biztonság a.</span><span class="sxs-lookup"><span data-stu-id="12d05-127">If you are not going toohost your application as an Azure Web App, then you should add hello actual ClientId, Client Secret, and Secret URI values toohello web.config. Otherwise leave these dummy values because we will be adding hello actual values in hello Azure Portal for an additional level of security.</span></span>

## <span data-ttu-id="12d05-128"><a id="gettoken"></a>Az Access Token módszer tooGet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="12d05-128"><a id="gettoken"></a>Add Method tooGet an Access Token</span></span>
<span data-ttu-id="12d05-129">Az order toouse hello Key Vault API kell olyan hozzáférési jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="12d05-129">In order toouse hello Key Vault API you need an access token.</span></span> <span data-ttu-id="12d05-130">Key Vault ügyfél hello kezeli a Key Vault API toohello, de kell toosupply hívások azt egy függvényt, amely hello szolgáltatáshozzáférési tokent kap.</span><span class="sxs-lookup"><span data-stu-id="12d05-130">hello Key Vault Client handles calls toohello Key Vault API but you need toosupply it with a function that gets hello access token.</span></span>  

<span data-ttu-id="12d05-131">Az alábbiakban az hello kód tooget olyan hozzáférési jogkivonatot az Azure Active Directoryból.</span><span class="sxs-lookup"><span data-stu-id="12d05-131">Following is hello code tooget an access token from Azure Active Directory.</span></span> <span data-ttu-id="12d05-132">Ez a kód bárhol lépjen az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="12d05-132">This code can go anywhere in your application.</span></span> <span data-ttu-id="12d05-133">Lehet például egy Utils tooadd vagy EncryptionHelper osztály.</span><span class="sxs-lookup"><span data-stu-id="12d05-133">I like tooadd a Utils or EncryptionHelper class.</span></span>  

    //add these using statements
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Threading.Tasks;
    using System.Web.Configuration;

    //this is an optional property toohold hello secret after it is retrieved
    public static string EncryptSecret { get; set; }

    //hello method that will be provided toohello KeyVaultClient
    public static async Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(WebConfigurationManager.AppSettings["ClientId"],
                    WebConfigurationManager.AppSettings["ClientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed tooobtain hello JWT token");

        return result.AccessToken;
    }

> [!NOTE]
> <span data-ttu-id="12d05-134">Egy ügyfél-azonosító és a titkos ügyfélkódot használata a hello legegyszerűbb módja tooauthenticate egy Azure AD-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="12d05-134">Using a Client ID and Client Secret is hello easiest way tooauthenticate an Azure AD application.</span></span> <span data-ttu-id="12d05-135">És használja azt a webes alkalmazásban és a kulcskezelést teljesebb körű vezérlése kizárás lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="12d05-135">And using it in your web application allows for a separation of duties and more control over your key management.</span></span> <span data-ttu-id="12d05-136">De azt támaszkodnak hello Ügyfélkulcs és a konfigurációs beállításaiban, amely bizonyos hello titkos kulcsot, amelyet az tooprotect a beállításokban üzembe, a kockázatos lehet.</span><span class="sxs-lookup"><span data-stu-id="12d05-136">But it does rely on putting hello Client Secret in your configuration settings which for some can be as risky as putting hello secret that you want tooprotect in your configuration settings.</span></span> <span data-ttu-id="12d05-137">Lásd az alábbi hogyan toouse egy ügyfél-Azonosítót és az ügyfél-azonosító és a titkos ügyfélkódot tooauthenticate helyett hello Azure AD-alkalmazást a leírását.</span><span class="sxs-lookup"><span data-stu-id="12d05-137">See below for a discussion on how toouse a Client ID and Certificate instead of Client ID and Client Secret tooauthenticate hello Azure AD application.</span></span>
> 
> 

## <span data-ttu-id="12d05-138"><a id="appstart"></a>Az alkalmazás lépések hello titkos beolvasása</span><span class="sxs-lookup"><span data-stu-id="12d05-138"><a id="appstart"></a>Retrieve hello secret on Application Start</span></span>
<span data-ttu-id="12d05-139">Most azt kell toocall hello Key Vault API-kód és a hello titkos kulcs beolvasása.</span><span class="sxs-lookup"><span data-stu-id="12d05-139">Now we need code toocall hello Key Vault API and retrieve hello secret.</span></span> <span data-ttu-id="12d05-140">hello alábbira elhelyezheti bárhol, amíg azt nevezzük, mielőtt toouse kell azt.</span><span class="sxs-lookup"><span data-stu-id="12d05-140">hello following code can be put anywhere as long as it is called before you need toouse it.</span></span> <span data-ttu-id="12d05-141">Ezzel a kóddal rendelkezik I hello alkalmazás indítás esemény hello Global.asax helyezze el, hogy az indítási egyszer fut, és elérhetővé válnak elérhetővé válik hello alkalmazás titkos kulcs hello.</span><span class="sxs-lookup"><span data-stu-id="12d05-141">I have put this code in hello Application Start event in hello Global.asax so that it runs once on start and makes hello secret available for hello application.</span></span>

    //add these using statements
    using Microsoft.Azure.KeyVault;
    using System.Web.Configuration;

    // I put my GetToken method in a Utils class. Change for wherever you placed your method.
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

    var sec = await kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]);

    //I put a variable in a Utils class toohold hello secret for general  application use.
    Utils.EncryptSecret = sec.Value;



## <span data-ttu-id="12d05-142"><a id="portalsettings"></a>Adja hozzá alkalmazásbeállításokat hello Azure Portal (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="12d05-142"><a id="portalsettings"></a>Add App Settings in hello Azure Portal (optional)</span></span>
<span data-ttu-id="12d05-143">Ha az Azure Web Apps hello Azure Portal most hello AppSettings hello tényleges értékeket is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="12d05-143">If you have an Azure Web App you can now add hello actual values for hello AppSettings in hello Azure Portal.</span></span> <span data-ttu-id="12d05-144">Ezzel a hello tényleges értékek nem lesz a hello Web.config fájlban, de a védett hello külön hozzáférés-vezérlés képességeinek amelyhez rendelkezik portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="12d05-144">By doing this, hello actual values will not be in hello web.config but protected via hello Portal where you have separate access control capabilities.</span></span> <span data-ttu-id="12d05-145">Ezeket az értékeket a Web.config fájlban megadott hello értékek kerülnek behelyettesítésre. Győződjön meg arról, hogy hello nevek vannak hello azonos.</span><span class="sxs-lookup"><span data-stu-id="12d05-145">These values will be substituted for hello values that you entered in your web.config. Make sure that hello names are hello same.</span></span>

![Azure portál megjeleníti-Alkalmazásbeállítások][1]

## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a><span data-ttu-id="12d05-147">A hitelesítést egy tanúsítványt a titkos Ügyfélkulcsot helyett</span><span class="sxs-lookup"><span data-stu-id="12d05-147">Authenticate with a Certificate instead of a Client Secret</span></span>
<span data-ttu-id="12d05-148">Egy másik módja tooauthenticate egy Azure AD-alkalmazást az ügyfél-azonosító és a titkos Ügyfélkulcs egy ügyfél-Azonosítót és a tanúsítvány használatával.</span><span class="sxs-lookup"><span data-stu-id="12d05-148">Another way tooauthenticate an Azure AD application is by using a Client ID and a Certificate instead of a Client ID and Client Secret.</span></span> <span data-ttu-id="12d05-149">Következő lépések toouse az Azure Web Apps lévő egyik tanúsítvány vannak hello:</span><span class="sxs-lookup"><span data-stu-id="12d05-149">Following are hello steps toouse a Certificate in an Azure Web App:</span></span>

1. <span data-ttu-id="12d05-150">Futtasson, vagy hozzon létre egy tanúsítványt</span><span class="sxs-lookup"><span data-stu-id="12d05-150">Get or Create a Certificate</span></span>
2. <span data-ttu-id="12d05-151">Egy Azure AD-alkalmazást hello tanúsítvány társítása</span><span class="sxs-lookup"><span data-stu-id="12d05-151">Associate hello Certificate with an Azure AD application</span></span>
3. <span data-ttu-id="12d05-152">Kód tooyour webalkalmazás toouse hello tanúsítvány hozzáadása</span><span class="sxs-lookup"><span data-stu-id="12d05-152">Add code tooyour Web App toouse hello Certificate</span></span>
4. <span data-ttu-id="12d05-153">A tanúsítvány tooyour webalkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="12d05-153">Add a Certificate tooyour Web App</span></span>

<span data-ttu-id="12d05-154">**Futtasson, vagy hozzon létre egy tanúsítványt** teszttanúsítványt használunk a célokra.</span><span class="sxs-lookup"><span data-stu-id="12d05-154">**Get or Create a Certificate** For our purposes we will make a test certificate.</span></span> <span data-ttu-id="12d05-155">Az alábbiakban néhány parancsok használható egy Developer-parancssorból toocreate a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="12d05-155">Here are a couple of commands that you can use in a Developer Command Prompt toocreate a certificate.</span></span> <span data-ttu-id="12d05-156">Azt szeretné, hogy a létrehozott hello cert fájlok directory toowhere módosítása.</span><span class="sxs-lookup"><span data-stu-id="12d05-156">Change directory toowhere you want hello cert files created.</span></span>  <span data-ttu-id="12d05-157">A nyitó és záró dátum hello tanúsítvány hello, használja továbbá hello aktuális dátum plusz 1 év.</span><span class="sxs-lookup"><span data-stu-id="12d05-157">Also, for hello beginning and ending date of hello certificate, use hello current date plus 1 year.</span></span>

    makecert -sv mykey.pvk -n "cn=KVWebApp" KVWebApp.cer -b 03/07/2017 -e 03/07/2018 -r
    pvk2pfx -pvk mykey.pvk -spc KVWebApp.cer -pfx KVWebApp.pfx -po test123

<span data-ttu-id="12d05-158">Jegyezze fel a hello befejezés dátumára és hello .pfx hello jelszavát (ebben a példában: 07/31/2016 és test123).</span><span class="sxs-lookup"><span data-stu-id="12d05-158">Make note of hello end date and hello password for hello .pfx (in this example: 07/31/2016 and test123).</span></span> <span data-ttu-id="12d05-159">Szüksége lesz rájuk az alábbi.</span><span class="sxs-lookup"><span data-stu-id="12d05-159">You will need them below.</span></span>

<span data-ttu-id="12d05-160">Teszttanúsítványt létrehozásával kapcsolatos további információkért lásd: [Útmutató: A saját tesztelése tanúsítvány létrehozása](https://msdn.microsoft.com/library/ff699202.aspx)</span><span class="sxs-lookup"><span data-stu-id="12d05-160">For more information on creating a test certificate, see [How to: Create Your Own Test Certificate](https://msdn.microsoft.com/library/ff699202.aspx)</span></span>

<span data-ttu-id="12d05-161">**Egy Azure AD-alkalmazást a társítás hello tanúsítvány** most, hogy rendelkezik egy tanúsítvánnyal, tooassociate kell azt egy Azure AD-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="12d05-161">**Associate hello Certificate with an Azure AD application** Now that you have a certificate, you need tooassociate it with an Azure AD application.</span></span> <span data-ttu-id="12d05-162">Jelenleg hello Azure-portál nem támogatja a munkafolyamat; Ez a PowerShell használatával elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="12d05-162">Presently, hello Azure Portal does not support this workflow; this can be completed through PowerShell.</span></span> <span data-ttu-id="12d05-163">Futtassa a következő parancsok tooassoicate hello tanúsítvány hello Azure AD alkalmazás hello:</span><span class="sxs-lookup"><span data-stu-id="12d05-163">Run hello following commands tooassoicate hello certificate with hello Azure AD application:</span></span>

    $x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
    $x509.Import("C:\data\KVWebApp.cer")
    $credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())

    # If you used different dates for makecert then adjust these values
    $now = [System.DateTime]::Now
    $yearfromnow = $now.AddYears(1)

    $adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -CertValue $credValue -StartDate $now -EndDate $yearfromnow

    $sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName $sp.ServicePrincipalName -PermissionsToSecrets all -ResourceGroupName 'contosorg'

    # get hello thumbprint toouse in your app settings
    $x509.Thumbprint

<span data-ttu-id="12d05-164">Ezek a parancsok futtatása után az Azure AD hello alkalmazás tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="12d05-164">After you have run these commands, you can see hello application in Azure AD.</span></span> <span data-ttu-id="12d05-165">Keresésekor, győződjön meg arról választja "A vállalat tulajdonában lévő alkalmazások" helyett "Alkalmazások a vállalat használ" hello keresés párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="12d05-165">When searching, ensure you select "Applications my company owns" instead of "Applications my company uses" in hello search dialog.</span></span>

<span data-ttu-id="12d05-166">További információ az Azure AD-alkalmazás objektumának és szolgáltatásnév objektumok toolearn lásd [alkalmazás és szolgáltatás egyszerű objektumok](../active-directory/active-directory-application-objects.md)</span><span class="sxs-lookup"><span data-stu-id="12d05-166">toolearn more about Azure AD Application Objects and ServicePrincipal Objects, see [Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md)</span></span>

<span data-ttu-id="12d05-167">**Adja hozzá a kódot tooyour webalkalmazás toouse hello tanúsítvány** most adjuk hozzá kód tooyour webalkalmazás tooaccess hello cert és a hitelesítéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="12d05-167">**Add code tooyour Web App toouse hello Certificate** Now we will add code tooyour Web App tooaccess hello cert and use it for authentication.</span></span>

<span data-ttu-id="12d05-168">Először nincs kód tooaccess hello cert.</span><span class="sxs-lookup"><span data-stu-id="12d05-168">First there is code tooaccess hello cert.</span></span>

    public static class CertificateHelper
    {
        public static X509Certificate2 FindCertificateByThumbprint(string findValue)
        {
            X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
            try
            {
                store.Open(OpenFlags.ReadOnly);
                X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByThumbprint,
                    findValue, false); // Don't validate certs, since hello test root isn't installed.
                if (col == null || col.Count == 0)
                    return null;
                return col[0];
            }
            finally
            {
                store.Close();
            }
        }
    }


<span data-ttu-id="12d05-169">Vegye figyelembe, hogy hello StoreLocation – CurrentUser LocalMachine helyett.</span><span class="sxs-lookup"><span data-stu-id="12d05-169">Note that hello StoreLocation is CurrentUser instead of LocalMachine.</span></span> <span data-ttu-id="12d05-170">És, hogy "false" toohello Megadja azt metódus található, mert a vizsgálati cert használjuk.</span><span class="sxs-lookup"><span data-stu-id="12d05-170">And that we are supplying 'false' toohello Find method because we are using a test cert.</span></span>

<span data-ttu-id="12d05-171">Ezután van hello CertificateHelper használ, és létrehoz egy ClientAssertionCertificate, amelyre szükség van hitelesítési kódját.</span><span class="sxs-lookup"><span data-stu-id="12d05-171">Next is code that uses hello CertificateHelper and creates a ClientAssertionCertificate which is needed for authentication.</span></span>

    public static ClientAssertionCertificate AssertionCert { get; set; }

    public static void GetCert()
    {
        var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
        AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
    }


<span data-ttu-id="12d05-172">Itt található hello új kódot tooget hello hozzáférési jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="12d05-172">Here is hello new code tooget hello access token.</span></span> <span data-ttu-id="12d05-173">Ez a felváltja a fenti hello GetToken módszert.</span><span class="sxs-lookup"><span data-stu-id="12d05-173">This replaces hello GetToken method above.</span></span> <span data-ttu-id="12d05-174">I adott azt egy másik nevet kényelmét szolgálja.</span><span class="sxs-lookup"><span data-stu-id="12d05-174">I have given it a different name for convenience.</span></span>

    public static async Task<string> GetAccessToken(string authority, string resource, string scope)
    {
        var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
        var result = await context.AcquireTokenAsync(resource, AssertionCert);
        return result.AccessToken;
    }

<span data-ttu-id="12d05-175">I rendelkezik helyezték összes ezt a kódot a webalkalmazás projekt Utils osztály a használat megkönnyítése érdekében.</span><span class="sxs-lookup"><span data-stu-id="12d05-175">I have put all of this code into my Web App project's Utils class for ease of use.</span></span>

<span data-ttu-id="12d05-176">hello utolsó kód módosítása hello Application_Start módszer van.</span><span class="sxs-lookup"><span data-stu-id="12d05-176">hello last code change is in hello Application_Start method.</span></span> <span data-ttu-id="12d05-177">Először toocall hello GetCert() metódus tooload hello ClientAssertionCertificate kell.</span><span class="sxs-lookup"><span data-stu-id="12d05-177">First we need toocall hello GetCert() method tooload hello ClientAssertionCertificate.</span></span> <span data-ttu-id="12d05-178">És majd módosítjuk hello visszahívási metódus, amely azt adja meg egy új KeyVaultClient létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="12d05-178">And then we change hello callback method that we supply when creating a new KeyVaultClient.</span></span> <span data-ttu-id="12d05-179">Vegye figyelembe, hogy ez a felváltja hello kódot, amely felett volt.</span><span class="sxs-lookup"><span data-stu-id="12d05-179">Note that this replaces hello code that we had above.</span></span>

    Utils.GetCert();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));


<span data-ttu-id="12d05-180">**Vegye fel a tanúsítvány tooyour webalkalmazás hello Azure portálon keresztül** hozzáadása egy tanúsítvány tooyour webalkalmazás egyszerű lépésből áll.</span><span class="sxs-lookup"><span data-stu-id="12d05-180">**Add a Certificate tooyour Web App through hello Azure Portal** Adding a Certificate tooyour Web App is a simple two-step process.</span></span> <span data-ttu-id="12d05-181">Először toohello Azure portálon lépjen, és keresse meg a webes alkalmazás tooyour.</span><span class="sxs-lookup"><span data-stu-id="12d05-181">First, go toohello Azure Portal and navigate tooyour Web App.</span></span> <span data-ttu-id="12d05-182">A webalkalmazás hello beállítási paneljén kattintson "egyéni tartományok és SSL" hello bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="12d05-182">On hello Settings blade for your Web App, click on hello entry for "Custom domains and SSL".</span></span> <span data-ttu-id="12d05-183">A hello panelen megjelenő meg kell tudni tooupload hello KVWebApp.pfx a fenti létrehozott tanúsítvány, győződjön meg arról, hogy ne felejtse el hello jelszót hello pfx.</span><span class="sxs-lookup"><span data-stu-id="12d05-183">On hello blade that opens you will be able tooupload hello Certificate that you created above, KVWebApp.pfx, make sure that you remember hello password for hello pfx.</span></span>

![A tanúsítvány tooa webes alkalmazás hozzáadása a hello Azure portál][2]

<span data-ttu-id="12d05-185">hello dolog, hogy kell-e toodo egy alkalmazás-beállítás tooyour hello neve webhely rendelkező webalkalmazás tooadd\_terhelés\_TANÚSÍTVÁNYOKAT és a *.</span><span class="sxs-lookup"><span data-stu-id="12d05-185">hello last thing that you need toodo is tooadd an Application Setting tooyour Web App that has hello name WEBSITE\_LOAD\_CERTIFICATES and a value of *.</span></span> <span data-ttu-id="12d05-186">Ezzel biztosíthatja, hogy minden tanúsítvány vannak betöltve.</span><span class="sxs-lookup"><span data-stu-id="12d05-186">This will ensure that all Certificates are loaded.</span></span> <span data-ttu-id="12d05-187">Ha tooload csak hello tanúsítványokat, amelyeket már feltöltött, majd megadhatja, hogy az ujjlenyomatok vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="12d05-187">If you wanted tooload only hello Certificates that you have uploaded, then you can enter a comma-separated list of their thumbprints.</span></span>

<span data-ttu-id="12d05-188">egy tanúsítvány tooa Web App alkalmazásban hozzáadásáról további toolearn lásd [tanúsítványok használata az Azure-webhelyek alkalmazásokban](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)</span><span class="sxs-lookup"><span data-stu-id="12d05-188">toolearn more about adding a Certificate tooa Web App, see [Using Certificates in Azure Websites Applications](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)</span></span>

<span data-ttu-id="12d05-189">**Adja hozzá a titkos kulcsot egy tanúsítvány tooKey tároló** helyett a tanúsítvány toohello Web App service közvetlenül feltöltését, tárolja Key Vault valamilyen titkos adatot, és telepítse onnan.</span><span class="sxs-lookup"><span data-stu-id="12d05-189">**Add a Certificate tooKey Vault as a secret** Instead of uploading your certificate toohello Web App service directly, you can store it in Key Vault as a secret and deploy it from there.</span></span> <span data-ttu-id="12d05-190">Ez az egy kétlépéses folyamat a következő blogbejegyzésben hello [telepítése Azure Web App tanúsítványt Key Vault keresztül](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)</span><span class="sxs-lookup"><span data-stu-id="12d05-190">This is a two-step process that is outlined in hello following blog post, [Deploying Azure Web App Certificate through Key Vault](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)</span></span>

## <span data-ttu-id="12d05-191"><a id="next"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="12d05-191"><a id="next"></a>Next steps</span></span>
<span data-ttu-id="12d05-192">Programozási hivatkozások: [Azure Key Vault C# ügyfél API-referencia](https://msdn.microsoft.com/library/azure/dn903628.aspx).</span><span class="sxs-lookup"><span data-stu-id="12d05-192">For programming references, see [Azure Key Vault C# Client API Reference](https://msdn.microsoft.com/library/azure/dn903628.aspx).</span></span>

<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
