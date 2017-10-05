---
title: "Használja az Azure Key Vault-webalkalmazások |} Microsoft Docs"
description: "Ez az oktatóanyag segítségével megtudhatja, hogyan használható az Azure Key Vault egy webalkalmazás."
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
ms.openlocfilehash: d095bcfe37baefa90cf79bb48bff3f703ce1dad7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-key-vault-from-a-web-application"></a><span data-ttu-id="27e7c-103">Használja az Azure Key Vault-webalkalmazások</span><span class="sxs-lookup"><span data-stu-id="27e7c-103">Use Azure Key Vault from a Web Application</span></span>
## <a name="introduction"></a><span data-ttu-id="27e7c-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="27e7c-104">Introduction</span></span>
<span data-ttu-id="27e7c-105">Ez az oktatóanyag segítségével az Azure Key Vault használata egy webalkalmazás az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="27e7c-105">Use this tutorial to help you learn how to use Azure Key Vault from a web application in Azure.</span></span> <span data-ttu-id="27e7c-106">Végigvezeti a titkos kulcs eléréséhez, hogy a webes alkalmazás használható az Azure Key Vault folyamatán.</span><span class="sxs-lookup"><span data-stu-id="27e7c-106">It walks you through the process of accessing a secret from an Azure Key Vault so that it can be used in your web application.</span></span>

<span data-ttu-id="27e7c-107">**Oktatóanyag áttekintésének várható időtartama:** 15 perc</span><span class="sxs-lookup"><span data-stu-id="27e7c-107">**Estimated time to complete:** 15 minutes</span></span>

<span data-ttu-id="27e7c-108">Áttekintést az Azure Key Vaultról a [What is Azure Key Vault?](key-vault-whatis.md) (Mi az Azure Key Vault?) című cikkben talál.</span><span class="sxs-lookup"><span data-stu-id="27e7c-108">For overview information about Azure Key Vault, see [What is Azure Key Vault?](key-vault-whatis.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="27e7c-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="27e7c-109">Prerequisites</span></span>
<span data-ttu-id="27e7c-110">Az oktatóanyag teljesítéséhez szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="27e7c-110">To complete this tutorial, you must have the following:</span></span>

* <span data-ttu-id="27e7c-111">Egy URI-t az Azure Key Vault a titkos kulcs</span><span class="sxs-lookup"><span data-stu-id="27e7c-111">A URI to a secret in an Azure Key Vault</span></span>
* <span data-ttu-id="27e7c-112">Az Azure Active Directoryval, a kulcstároló eléréséhez használt regisztrált egy ügyfél-Azonosítót és egy Ügyfélkulcsot webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="27e7c-112">A Client ID and a Client Secret for a web application registered with Azure Active Directory that has access to your Key Vault</span></span>
* <span data-ttu-id="27e7c-113">A webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="27e7c-113">A web application.</span></span> <span data-ttu-id="27e7c-114">Fogja azt megjelenítő webalkalmazásként Azure szolgáltatásba telepített ASP.NET MVC alkalmazásnak az lépéseit.</span><span class="sxs-lookup"><span data-stu-id="27e7c-114">We will be showing the steps for an ASP.NET MVC application deployed in Azure as a Web App.</span></span>

> [!NOTE]
> <span data-ttu-id="27e7c-115">Fontos, hogy végrehajtotta a felsorolt [Ismerkedés az Azure Key Vault](key-vault-get-started.md) ehhez az oktatóanyaghoz, hogy az URI-t a titkos kulcs és az ügyfél-azonosító és a titkos ügyfélkódot webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="27e7c-115">It is essential that you have completed the steps listed in [Get Started with Azure Key Vault](key-vault-get-started.md) for this tutorial so that you have the URI to a secret and the Client ID and Client Secret for a web application.</span></span>
> 
> 

<span data-ttu-id="27e7c-116">A webes alkalmazás, amely a Key Vault hozzáférhetnek az Azure Active Directoryban regisztrálva van, és megkapta-e hozzáféréssel a Key Vault a.</span><span class="sxs-lookup"><span data-stu-id="27e7c-116">The web application that will be accessing the Key Vault is the one that is registered in Azure Active Directory and has been given access to your Key Vault.</span></span> <span data-ttu-id="27e7c-117">Ha nem ez a helyzet, vissza regisztrálása a az alkalmazás az első lépéseket bemutató oktatóanyaghoz, és ismételje meg a ismertetett lépéseket.</span><span class="sxs-lookup"><span data-stu-id="27e7c-117">If this is not the case, go back to Register an Application in the Get Started tutorial and repeat the steps listed.</span></span>

<span data-ttu-id="27e7c-118">Ez az oktatóanyag célja alapjainak webalkalmazások létrehozása az Azure webalkalmazás-fejlesztőknek.</span><span class="sxs-lookup"><span data-stu-id="27e7c-118">This tutorial is designed for web developers that understand the basics of creating web applications on Azure.</span></span> <span data-ttu-id="27e7c-119">Azure Web Apps kapcsolatos további információkért lásd: [webalkalmazások áttekintése](../app-service-web/app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="27e7c-119">For more information about Azure Web Apps, see [Web Apps overview](../app-service-web/app-service-web-overview.md).</span></span>

## <span data-ttu-id="27e7c-120"><a id="packages"></a>Adja hozzá a Nuget-csomagok</span><span class="sxs-lookup"><span data-stu-id="27e7c-120"><a id="packages"></a>Add Nuget Packages</span></span>
<span data-ttu-id="27e7c-121">Két csomagot, amelyet a webes alkalmazás telepítve van.</span><span class="sxs-lookup"><span data-stu-id="27e7c-121">There are two packages that your web application needs to have installed.</span></span>

* <span data-ttu-id="27e7c-122">Active Directory Authentication Library - Azure Active Directoryban való interakció és az identitásfelügyelet metódust tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="27e7c-122">Active Directory Authentication Library - contains methods for interacting with Azure Active Directory and managing user identity</span></span>
* <span data-ttu-id="27e7c-123">Az Azure Key Vault Library - kommunikáció az Azure Key Vault metódust tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="27e7c-123">Azure Key Vault Library - contains methods for interacting with Azure Key Vault</span></span>

<span data-ttu-id="27e7c-124">Mindkét ezeket a csomagokat is telepíthető, a Package Manager konzolon az Install-Package paranccsal.</span><span class="sxs-lookup"><span data-stu-id="27e7c-124">Both of these packages can be installed using the Package Manager Console using the Install-Package command.</span></span>

    // this is currently the latest stable version of ADAL
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault


## <span data-ttu-id="27e7c-125"><a id="webconfig"></a>Web.Config módosítása</span><span class="sxs-lookup"><span data-stu-id="27e7c-125"><a id="webconfig"></a>Modify Web.Config</span></span>
<span data-ttu-id="27e7c-126">Nincsenek három-alkalmazásbeállításokat, amelyek hozzá kell adni a web.config fájlban az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="27e7c-126">There are three application settings that need to be added to the web.config file as follows.</span></span>

    <!-- ClientId and ClientSecret refer to the web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is the URI for the secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />


<span data-ttu-id="27e7c-127">Ha nem kívánja futtatni, Azure Web Apps, majd adjon a tényleges ClientId, a titkos Ügyfélkulcs és a titkos kulcs URI-értékek a Web.config fájlban.</span><span class="sxs-lookup"><span data-stu-id="27e7c-127">If you are not going to host your application as an Azure Web App, then you should add the actual ClientId, Client Secret, and Secret URI values to the web.config.</span></span> <span data-ttu-id="27e7c-128">Egyéb hagyja ki ezeket az üres értékeket, mert bővítjük a tényleges értékek az Azure-portálon egy további biztonsági szintet.</span><span class="sxs-lookup"><span data-stu-id="27e7c-128">Otherwise leave these dummy values because we will be adding the actual values in the Azure Portal for an additional level of security.</span></span>

## <span data-ttu-id="27e7c-129"><a id="gettoken"></a>Módszer segítségével Access Token hozzáadása</span><span class="sxs-lookup"><span data-stu-id="27e7c-129"><a id="gettoken"></a>Add Method to Get an Access Token</span></span>
<span data-ttu-id="27e7c-130">A Key Vault API használatához szükséges olyan hozzáférési jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="27e7c-130">In order to use the Key Vault API you need an access token.</span></span> <span data-ttu-id="27e7c-131">A Key Vault ügyfél kezeli a Key Vault API-hívások, de kell megadnia, az egy függvénynek, amely lekérdezi a hozzáférési jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="27e7c-131">The Key Vault Client handles calls to the Key Vault API but you need to supply it with a function that gets the access token.</span></span>  

<span data-ttu-id="27e7c-132">Az alábbiakban olvashatja a kódot, és szerezze be a hozzáférési tokent az Azure Active Directoryból.</span><span class="sxs-lookup"><span data-stu-id="27e7c-132">Following is the code to get an access token from Azure Active Directory.</span></span> <span data-ttu-id="27e7c-133">Ez a kód bárhol lépjen az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="27e7c-133">This code can go anywhere in your application.</span></span> <span data-ttu-id="27e7c-134">Szeretnék Utils vagy EncryptionHelper osztályt.</span><span class="sxs-lookup"><span data-stu-id="27e7c-134">I like to add a Utils or EncryptionHelper class.</span></span>  

    //add these using statements
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Threading.Tasks;
    using System.Web.Configuration;

    //this is an optional property to hold the secret after it is retrieved
    public static string EncryptSecret { get; set; }

    //the method that will be provided to the KeyVaultClient
    public static async Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(WebConfigurationManager.AppSettings["ClientId"],
                    WebConfigurationManager.AppSettings["ClientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");

        return result.AccessToken;
    }

> [!NOTE]
> <span data-ttu-id="27e7c-135">Egy ügyfél-azonosító és a titkos ügyfélkódot használata a hitelesítéshez az Azure AD-alkalmazást a legegyszerűbben.</span><span class="sxs-lookup"><span data-stu-id="27e7c-135">Using a Client ID and Client Secret is the easiest way to authenticate an Azure AD application.</span></span> <span data-ttu-id="27e7c-136">És használja azt a webes alkalmazásban és a kulcskezelést teljesebb körű vezérlése kizárás lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="27e7c-136">And using it in your web application allows for a separation of duties and more control over your key management.</span></span> <span data-ttu-id="27e7c-137">De azt alapulnak, hogy a titkos Ügyfélkulcs a konfigurációs beállításokat, amely bizonyos lehet a kockázatos, a titkos kulcsot, amely a konfigurációs beállításokat a védeni kívánt üzembe helyezésének.</span><span class="sxs-lookup"><span data-stu-id="27e7c-137">But it does rely on putting the Client Secret in your configuration settings which for some can be as risky as putting the secret that you want to protect in your configuration settings.</span></span> <span data-ttu-id="27e7c-138">Lásd az alábbi egy ügyfél-azonosító és a tanúsítvány használatáról helyett az ügyfél-azonosító és a titkos ügyfélkódot hitelesítéséhez az Azure AD-alkalmazás leírását.</span><span class="sxs-lookup"><span data-stu-id="27e7c-138">See below for a discussion on how to use a Client ID and Certificate instead of Client ID and Client Secret to authenticate the Azure AD application.</span></span>
> 
> 

## <span data-ttu-id="27e7c-139"><a id="appstart"></a>A titkos kulcsot, az alkalmazás lépések beolvasása</span><span class="sxs-lookup"><span data-stu-id="27e7c-139"><a id="appstart"></a>Retrieve the secret on Application Start</span></span>
<span data-ttu-id="27e7c-140">Most szükséges kód a Key Vault API és a titkos kulcs beolvasása.</span><span class="sxs-lookup"><span data-stu-id="27e7c-140">Now we need code to call the Key Vault API and retrieve the secret.</span></span> <span data-ttu-id="27e7c-141">Az alábbi kód bárhova helyezhető, mindaddig, amíg előtt kell használnia az nevezik.</span><span class="sxs-lookup"><span data-stu-id="27e7c-141">The following code can be put anywhere as long as it is called before you need to use it.</span></span> <span data-ttu-id="27e7c-142">Ezzel a kóddal rendelkezik I az alkalmazás indítás esemény a Global.asax helyezze el, hogy a kezdőlapon egyszer fut, és elérhetővé teszi a titkos kulcsot az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="27e7c-142">I have put this code in the Application Start event in the Global.asax so that it runs once on start and makes the secret available for the application.</span></span>

    //add these using statements
    using Microsoft.Azure.KeyVault;
    using System.Web.Configuration;

    // I put my GetToken method in a Utils class. Change for wherever you placed your method.
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

    var sec = await kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]);

    //I put a variable in a Utils class to hold the secret for general  application use.
    Utils.EncryptSecret = sec.Value;



## <span data-ttu-id="27e7c-143"><a id="portalsettings"></a>Alkalmazásbeállítások hozzáadása az Azure portálon (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="27e7c-143"><a id="portalsettings"></a>Add App Settings in the Azure Portal (optional)</span></span>
<span data-ttu-id="27e7c-144">Ha az Azure Web Apps most értékeket is hozzáadhat a tényleges számára az AppSettings az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="27e7c-144">If you have an Azure Web App you can now add the actual values for the AppSettings in the Azure Portal.</span></span> <span data-ttu-id="27e7c-145">Ezzel a tényleges értékek nem lesz a Web.config fájlban, de a portálon, melyekben külön hozzáférés-vezérlés képességeinek védett.</span><span class="sxs-lookup"><span data-stu-id="27e7c-145">By doing this, the actual values will not be in the web.config but protected via the Portal where you have separate access control capabilities.</span></span> <span data-ttu-id="27e7c-146">Ezeket az értékeket a Web.config fájlban megadott értékek kerülnek behelyettesítésre.</span><span class="sxs-lookup"><span data-stu-id="27e7c-146">These values will be substituted for the values that you entered in your web.config.</span></span> <span data-ttu-id="27e7c-147">Győződjön meg arról, hogy a nevek megegyeznek-e.</span><span class="sxs-lookup"><span data-stu-id="27e7c-147">Make sure that the names are the same.</span></span>

![Azure portál megjeleníti-Alkalmazásbeállítások][1]

## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a><span data-ttu-id="27e7c-149">A hitelesítést egy tanúsítványt a titkos Ügyfélkulcsot helyett</span><span class="sxs-lookup"><span data-stu-id="27e7c-149">Authenticate with a Certificate instead of a Client Secret</span></span>
<span data-ttu-id="27e7c-150">Egy Azure AD-alkalmazást hitelesítéséhez egy másik úgy, hogy egy ügyfél-Azonosítót és a tanúsítvány helyett egy ügyfél-azonosító és a titkos ügyfélkódot.</span><span class="sxs-lookup"><span data-stu-id="27e7c-150">Another way to authenticate an Azure AD application is by using a Client ID and a Certificate instead of a Client ID and Client Secret.</span></span> <span data-ttu-id="27e7c-151">A tanúsítvány használata az Azure Web Apps lépései a következők:</span><span class="sxs-lookup"><span data-stu-id="27e7c-151">Following are the steps to use a Certificate in an Azure Web App:</span></span>

1. <span data-ttu-id="27e7c-152">Futtasson, vagy hozzon létre egy tanúsítványt</span><span class="sxs-lookup"><span data-stu-id="27e7c-152">Get or Create a Certificate</span></span>
2. <span data-ttu-id="27e7c-153">A tanúsítvány társítása egy Azure AD-alkalmazást</span><span class="sxs-lookup"><span data-stu-id="27e7c-153">Associate the Certificate with an Azure AD application</span></span>
3. <span data-ttu-id="27e7c-154">Kód felvétele a webalkalmazás a tanúsítvány használatára</span><span class="sxs-lookup"><span data-stu-id="27e7c-154">Add code to your Web App to use the Certificate</span></span>
4. <span data-ttu-id="27e7c-155">Tanúsítvány hozzáadása a webalkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="27e7c-155">Add a Certificate to your Web App</span></span>

<span data-ttu-id="27e7c-156">**Futtasson, vagy hozzon létre egy tanúsítványt** teszttanúsítványt használunk a célokra.</span><span class="sxs-lookup"><span data-stu-id="27e7c-156">**Get or Create a Certificate** For our purposes we will make a test certificate.</span></span> <span data-ttu-id="27e7c-157">Az alábbiakban néhány parancsokat, amelyek a fejlesztői parancssor segítségével hozzon létre egy tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="27e7c-157">Here are a couple of commands that you can use in a Developer Command Prompt to create a certificate.</span></span> <span data-ttu-id="27e7c-158">Módosítsa a könyvtárat, ahová a létrehozott tanúsítvány fájlok.</span><span class="sxs-lookup"><span data-stu-id="27e7c-158">Change directory to where you want the cert files created.</span></span>  <span data-ttu-id="27e7c-159">Emellett a kezdő és záró dátumot a tanúsítvány, használja az aktuális dátum plusz 1 év.</span><span class="sxs-lookup"><span data-stu-id="27e7c-159">Also, for the beginning and ending date of the certificate, use the current date plus 1 year.</span></span>

    makecert -sv mykey.pvk -n "cn=KVWebApp" KVWebApp.cer -b 03/07/2017 -e 03/07/2018 -r
    pvk2pfx -pvk mykey.pvk -spc KVWebApp.cer -pfx KVWebApp.pfx -po test123

<span data-ttu-id="27e7c-160">Jegyezze fel a záró dátum és a jelszót a .pfx (ebben a példában: 07/31/2016 és test123).</span><span class="sxs-lookup"><span data-stu-id="27e7c-160">Make note of the end date and the password for the .pfx (in this example: 07/31/2016 and test123).</span></span> <span data-ttu-id="27e7c-161">Szüksége lesz rájuk az alábbi.</span><span class="sxs-lookup"><span data-stu-id="27e7c-161">You will need them below.</span></span>

<span data-ttu-id="27e7c-162">Teszttanúsítványt létrehozásával kapcsolatos további információkért lásd: [Útmutató: A saját tesztelése tanúsítvány létrehozása](https://msdn.microsoft.com/library/ff699202.aspx)</span><span class="sxs-lookup"><span data-stu-id="27e7c-162">For more information on creating a test certificate, see [How to: Create Your Own Test Certificate](https://msdn.microsoft.com/library/ff699202.aspx)</span></span>

<span data-ttu-id="27e7c-163">**A tanúsítvány társítása egy Azure AD-alkalmazást** most, hogy rendelkezik egy tanúsítvánnyal, szeretné-e társíthatja egy Azure AD-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="27e7c-163">**Associate the Certificate with an Azure AD application** Now that you have a certificate, you need to associate it with an Azure AD application.</span></span> <span data-ttu-id="27e7c-164">Jelenleg az Azure portál nem támogatja a munkafolyamat; Ez a PowerShell használatával elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="27e7c-164">Presently, the Azure Portal does not support this workflow; this can be completed through PowerShell.</span></span> <span data-ttu-id="27e7c-165">Futtassa a következő parancsok futtatásával assoicate a tanúsítvány az Azure AD-alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="27e7c-165">Run the following commands to assoicate the certificate with the Azure AD application:</span></span>

    $x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
    $x509.Import("C:\data\KVWebApp.cer")
    $credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())

    # If you used different dates for makecert then adjust these values
    $now = [System.DateTime]::Now
    $yearfromnow = $now.AddYears(1)

    $adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -CertValue $credValue -StartDate $now -EndDate $yearfromnow

    $sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName $sp.ServicePrincipalName -PermissionsToSecrets all -ResourceGroupName 'contosorg'

    # get the thumbprint to use in your app settings
    $x509.Thumbprint

<span data-ttu-id="27e7c-166">Ezek a parancsok futtatása után megtekintheti az alkalmazást az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="27e7c-166">After you have run these commands, you can see the application in Azure AD.</span></span> <span data-ttu-id="27e7c-167">Ha keres, győződjön meg arról, "A vállalat tulajdonában lévő alkalmazások" helyett "Alkalmazások a vállalat használ" a keresés párbeszédpanelen válasszon.</span><span class="sxs-lookup"><span data-stu-id="27e7c-167">When searching, ensure you select "Applications my company owns" instead of "Applications my company uses" in the search dialog.</span></span>

<span data-ttu-id="27e7c-168">Az Azure AD-alkalmazás objektumának és szolgáltatásnév objektumok kapcsolatos további információkért lásd: [alkalmazás és szolgáltatás egyszerű objektumok](../active-directory/active-directory-application-objects.md)</span><span class="sxs-lookup"><span data-stu-id="27e7c-168">To learn more about Azure AD Application Objects and ServicePrincipal Objects, see [Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md)</span></span>

<span data-ttu-id="27e7c-169">**Adja hozzá a kódot a tanúsítványt használja a webalkalmazáshoz** most hozzáadunk kód a webalkalmazásban a cert eléréséhez, és a hitelesítéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="27e7c-169">**Add code to your Web App to use the Certificate** Now we will add code to your Web App to access the cert and use it for authentication.</span></span>

<span data-ttu-id="27e7c-170">Először nincs kódot, amely a tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="27e7c-170">First there is code to access the cert.</span></span>

    public static class CertificateHelper
    {
        public static X509Certificate2 FindCertificateByThumbprint(string findValue)
        {
            X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
            try
            {
                store.Open(OpenFlags.ReadOnly);
                X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByThumbprint,
                    findValue, false); // Don't validate certs, since the test root isn't installed.
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


<span data-ttu-id="27e7c-171">Vegye figyelembe, hogy a StoreLocation – CurrentUser LocalMachine helyett.</span><span class="sxs-lookup"><span data-stu-id="27e7c-171">Note that the StoreLocation is CurrentUser instead of LocalMachine.</span></span> <span data-ttu-id="27e7c-172">És, hogy azt is biztosítja "false", a keresés metódus, mert a vizsgálati cert használjuk.</span><span class="sxs-lookup"><span data-stu-id="27e7c-172">And that we are supplying 'false' to the Find method because we are using a test cert.</span></span>

<span data-ttu-id="27e7c-173">Ezután van, amely a CertificateHelper használ, és létrehoz egy ClientAssertionCertificate, amelyre szükség van hitelesítési kódot.</span><span class="sxs-lookup"><span data-stu-id="27e7c-173">Next is code that uses the CertificateHelper and creates a ClientAssertionCertificate which is needed for authentication.</span></span>

    public static ClientAssertionCertificate AssertionCert { get; set; }

    public static void GetCert()
    {
        var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
        AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
    }


<span data-ttu-id="27e7c-174">Ez az új kód segítségével kérheti le a hozzáférési jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="27e7c-174">Here is the new code to get the access token.</span></span> <span data-ttu-id="27e7c-175">Ez a felváltja a fenti GetToken metódust.</span><span class="sxs-lookup"><span data-stu-id="27e7c-175">This replaces the GetToken method above.</span></span> <span data-ttu-id="27e7c-176">I adott azt egy másik nevet kényelmét szolgálja.</span><span class="sxs-lookup"><span data-stu-id="27e7c-176">I have given it a different name for convenience.</span></span>

    public static async Task<string> GetAccessToken(string authority, string resource, string scope)
    {
        var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
        var result = await context.AcquireTokenAsync(resource, AssertionCert);
        return result.AccessToken;
    }

<span data-ttu-id="27e7c-177">I rendelkezik helyezték összes ezt a kódot a webalkalmazás projekt Utils osztály a használat megkönnyítése érdekében.</span><span class="sxs-lookup"><span data-stu-id="27e7c-177">I have put all of this code into my Web App project's Utils class for ease of use.</span></span>

<span data-ttu-id="27e7c-178">Az utolsó kódváltoztatást az Application_Start metódus van.</span><span class="sxs-lookup"><span data-stu-id="27e7c-178">The last code change is in the Application_Start method.</span></span> <span data-ttu-id="27e7c-179">Először igazolnia kell betölteni a ClientAssertionCertificate GetCert() metódust.</span><span class="sxs-lookup"><span data-stu-id="27e7c-179">First we need to call the GetCert() method to load the ClientAssertionCertificate.</span></span> <span data-ttu-id="27e7c-180">És majd a visszahívási metódus, amely azt adja meg, amikor hoz létre egy új KeyVaultClient módosítjuk.</span><span class="sxs-lookup"><span data-stu-id="27e7c-180">And then we change the callback method that we supply when creating a new KeyVaultClient.</span></span> <span data-ttu-id="27e7c-181">Vegye figyelembe, hogy ez a felváltja a kódot, amely felett volt.</span><span class="sxs-lookup"><span data-stu-id="27e7c-181">Note that this replaces the code that we had above.</span></span>

    Utils.GetCert();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));


<span data-ttu-id="27e7c-182">**A tanúsítvány felvétele a webalkalmazás az Azure portálon keresztül** egyszerű lépésből áll a tanúsítvány felvétele a webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="27e7c-182">**Add a Certificate to your Web App through the Azure Portal** Adding a Certificate to your Web App is a simple two-step process.</span></span> <span data-ttu-id="27e7c-183">Először az Azure-portálon végezhető, és keresse meg a webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="27e7c-183">First, go to the Azure Portal and navigate to your Web App.</span></span> <span data-ttu-id="27e7c-184">A webalkalmazás beállítási paneljén kattintson az "egyéni tartományok és SSL" bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="27e7c-184">On the Settings blade for your Web App, click on the entry for "Custom domains and SSL".</span></span> <span data-ttu-id="27e7c-185">A panelen megjelenő meg fogja tudni töltse fel a fenti KVWebApp.pfx létrehozott, győződjön meg arról, hogy ne felejtse el a jelszót a PFX.</span><span class="sxs-lookup"><span data-stu-id="27e7c-185">On the blade that opens you will be able to upload the Certificate that you created above, KVWebApp.pfx, make sure that you remember the password for the pfx.</span></span>

![A tanúsítvány felvétele egy webalkalmazást az Azure-portálon][2]

<span data-ttu-id="27e7c-187">A legutolsó dolog, hogy végre kell hajtani, hogy hozzáadjon egy alkalmazásbeállítást a webes alkalmazás, amelynek a neve webhely\_terhelés\_TANÚSÍTVÁNYOKAT és a *.</span><span class="sxs-lookup"><span data-stu-id="27e7c-187">The last thing that you need to do is to add an Application Setting to your Web App that has the name WEBSITE\_LOAD\_CERTIFICATES and a value of *.</span></span> <span data-ttu-id="27e7c-188">Ezzel biztosíthatja, hogy minden tanúsítvány vannak betöltve.</span><span class="sxs-lookup"><span data-stu-id="27e7c-188">This will ensure that all Certificates are loaded.</span></span> <span data-ttu-id="27e7c-189">Ha csak a feltöltött tanúsítványok betölteni, ezután megadhatja az ujjlenyomatok vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="27e7c-189">If you wanted to load only the Certificates that you have uploaded, then you can enter a comma-separated list of their thumbprints.</span></span>

<span data-ttu-id="27e7c-190">A tanúsítvány felvétele a webes alkalmazás kapcsolatos további információkért lásd: [tanúsítványok használata az Azure-webhelyek alkalmazásokban](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)</span><span class="sxs-lookup"><span data-stu-id="27e7c-190">To learn more about adding a Certificate to a Web App, see [Using Certificates in Azure Websites Applications](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)</span></span>

<span data-ttu-id="27e7c-191">**A tanúsítvány felvétele a Key Vault egy titkos kulcsként** helyett a tanúsítványt közvetlenül a Web App service feltölteni, tárolja Key Vault valamilyen titkos adatot, és telepítse onnan.</span><span class="sxs-lookup"><span data-stu-id="27e7c-191">**Add a Certificate to Key Vault as a secret** Instead of uploading your certificate to the Web App service directly, you can store it in Key Vault as a secret and deploy it from there.</span></span> <span data-ttu-id="27e7c-192">Ez az egy kétlépéses folyamat a következő blogbejegyzésben [telepítése Azure Web App tanúsítványt Key Vault keresztül](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)</span><span class="sxs-lookup"><span data-stu-id="27e7c-192">This is a two-step process that is outlined in the following blog post, [Deploying Azure Web App Certificate through Key Vault](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)</span></span>

## <span data-ttu-id="27e7c-193"><a id="next"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="27e7c-193"><a id="next"></a>Next steps</span></span>
<span data-ttu-id="27e7c-194">Programozási hivatkozások: [Azure Key Vault C# ügyfél API-referencia](https://msdn.microsoft.com/library/azure/dn903628.aspx).</span><span class="sxs-lookup"><span data-stu-id="27e7c-194">For programming references, see [Azure Key Vault C# Client API Reference](https://msdn.microsoft.com/library/azure/dn903628.aspx).</span></span>

<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
