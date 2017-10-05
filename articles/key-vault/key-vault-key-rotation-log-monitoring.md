---
title: "Állítsa be a Azure Key Vault-végpontok közötti fő elforgatás és naplózási |} Microsoft Docs"
description: "Ez az útmutató segítségével rendszerrel legfontosabb rotációjával és figyelési kulcstároló naplóit."
services: key-vault
documentationcenter: 
author: swgriffith
manager: mbaldwin
tags: 
ms.assetid: 9cd7e15e-23b8-41c0-a10a-06e6207ed157
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: jodehavi;stgriffi
ms.openlocfilehash: 38c342802ed687985ac6f84f5a590a1a0dcc6c6a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-azure-key-vault-with-end-to-end-key-rotation-and-auditing"></a><span data-ttu-id="0ef19-103">Az Azure Key Vault beállítása végpontok közötti kulcsforgatással és auditálással</span><span class="sxs-lookup"><span data-stu-id="0ef19-103">Set up Azure Key Vault with end-to-end key rotation and auditing</span></span>
## <a name="introduction"></a><span data-ttu-id="0ef19-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="0ef19-104">Introduction</span></span>
<span data-ttu-id="0ef19-105">Miután létrehozta a kulcstároló, lesz a kulcsok és titkos kulcsok tárolására, hogy a tároló használatának megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="0ef19-105">After creating your key vault, you will be able to start using that vault to store your keys and secrets.</span></span> <span data-ttu-id="0ef19-106">Az alkalmazások többé nem kell megőrizni a kulcsok vagy titkos kulcsok, hanem fog kérni azokat a key vault a igény szerint.</span><span class="sxs-lookup"><span data-stu-id="0ef19-106">Your applications no longer need to persist your keys or secrets, but rather will request them from the key vault as needed.</span></span> <span data-ttu-id="0ef19-107">Ez lehetővé teszi a kulcsok és titkos kulcsok frissítése az alkalmazás, így akár a kulcs és a titkos felügyeleti lehetőségek széles választékát viselkedésének módosítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="0ef19-107">This allows you to update keys and secrets without affecting the behavior of your application, which opens up a breadth of possibilities around your key and secret management.</span></span>

<span data-ttu-id="0ef19-108">Ez a cikk végigvezeti az Azure Key Vault használatával használatával egy titkos kulcsot, ebben az esetben egy Azure Storage-fiók kulcsát, hogy egy alkalmazás egy példát.</span><span class="sxs-lookup"><span data-stu-id="0ef19-108">This article walks through an example of using Azure Key Vault to store a secret, in this case an Azure Storage Account key that is accessed by an application.</span></span> <span data-ttu-id="0ef19-109">Is egy ütemezett elforgatási szögét a tárfiók kulcsa végrehajtását mutatja be.</span><span class="sxs-lookup"><span data-stu-id="0ef19-109">It also demonstrates implementation of a scheduled rotation of that storage account key.</span></span> <span data-ttu-id="0ef19-110">Végül azt végigvezeti bemutatjuk a kulcstartót naplók figyelésére, és riasztást, ha a nem várt kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="0ef19-110">Finally, it walks through a demonstration of how to monitor the key vault audit logs and raise alerts when unexpected requests are made.</span></span>

> [!NOTE]
> <span data-ttu-id="0ef19-111">Ez az oktatóanyag nem célja a részletesen ismertetik a kulcstartót kezdeti telepítése.</span><span class="sxs-lookup"><span data-stu-id="0ef19-111">This tutorial is not intended to explain in detail the initial setup of your key vault.</span></span> <span data-ttu-id="0ef19-112">Ezekről a [Get started with Azure Key Vault](key-vault-get-started.md) (Bevezetés az Azure Key Vault használatába) című cikkben találhat információt.</span><span class="sxs-lookup"><span data-stu-id="0ef19-112">For this information, see [Get started with Azure Key Vault](key-vault-get-started.md).</span></span> <span data-ttu-id="0ef19-113">Platformfüggetlen parancssori felületre vonatkozó utasításokat lásd: [kezelése Key Vault parancssori felület használatával](key-vault-manage-with-cli2.md).</span><span class="sxs-lookup"><span data-stu-id="0ef19-113">For Cross-Platform Command-Line Interface instructions, see [Manage Key Vault using CLI](key-vault-manage-with-cli2.md).</span></span>
>
>

## <a name="set-up-key-vault"></a><span data-ttu-id="0ef19-114">A Key Vault beállítása</span><span class="sxs-lookup"><span data-stu-id="0ef19-114">Set up Key Vault</span></span>
<span data-ttu-id="0ef19-115">Ahhoz, hogy egy alkalmazás titkos kulcs lekérése a Key Vault, először hozzon létre a titkos kulcsot, és töltse fel azt a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="0ef19-115">To enable an application to retrieve a secret from Key Vault, you must first create the secret and upload it to your vault.</span></span> <span data-ttu-id="0ef19-116">Ehhez az Azure PowerShell-munkamenet indítása, a következő paranccsal Azure-fiókjába történő bejelentkezés:</span><span class="sxs-lookup"><span data-stu-id="0ef19-116">This can be accomplished by starting an Azure PowerShell session and signing in to your Azure account with the following command:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="0ef19-117">Az előugró böngészőablakban adja meg az Azure-fiókja felhasználónevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="0ef19-117">In the pop-up browser window, enter your Azure account user name and password.</span></span> <span data-ttu-id="0ef19-118">PowerShell beolvassa az összes olyan előfizetést, ehhez a fiókhoz társított.</span><span class="sxs-lookup"><span data-stu-id="0ef19-118">PowerShell will get all the subscriptions that are associated with this account.</span></span> <span data-ttu-id="0ef19-119">PowerShell alapértelmezés szerint az elsőt használja.</span><span class="sxs-lookup"><span data-stu-id="0ef19-119">PowerShell uses the first one by default.</span></span>

<span data-ttu-id="0ef19-120">Ha több előfizetéssel rendelkezik, akkor előfordulhat, hogy adja meg azt, amelyik a kulcstároló létrehozásához használt.</span><span class="sxs-lookup"><span data-stu-id="0ef19-120">If you have multiple subscriptions, you might have to specify the one that was used to create your key vault.</span></span> <span data-ttu-id="0ef19-121">Adja meg a fiókhoz tartozó előfizetések megjelenítéséhez a következőket:</span><span class="sxs-lookup"><span data-stu-id="0ef19-121">Enter the following to see the subscriptions for your account:</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="0ef19-122">Adja meg az előfizetést, a key vault naplózása akkor van társítva, írja be a következőt:</span><span class="sxs-lookup"><span data-stu-id="0ef19-122">To specify the subscription that's associated with the key vault you will be logging, enter:</span></span>

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID>
```

<span data-ttu-id="0ef19-123">Mivel ez a cikk bemutatja, hogy a tárfiók kulcsára, titkos kulcs tárolása, ha előbb telepítik azokra a tárfiók kulcsára.</span><span class="sxs-lookup"><span data-stu-id="0ef19-123">Because this article demonstrates storing a storage account key as a secret, you must get that storage account key.</span></span>

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

<span data-ttu-id="0ef19-124">A titkos kulcsot (ebben az esetben a tárfiók kulcsára) beolvasása, után kell átalakítani, amely egy biztonságos karakterláncot, és majd titkos kulcs létrehozása ezt az értéket a key vaultban lévő.</span><span class="sxs-lookup"><span data-stu-id="0ef19-124">After retrieving your secret (in this case, your storage account key), you must convert that to a secure string and then create a secret with that value in your key vault.</span></span>

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```
<span data-ttu-id="0ef19-125">Az URI a következő lekérése a létrehozott titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="0ef19-125">Next, get the URI for the secret you created.</span></span> <span data-ttu-id="0ef19-126">Ez szolgál egy későbbi lépésben hívásakor a key vault beolvasni a titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="0ef19-126">This is used in a later step when you call the key vault to retrieve your secret.</span></span> <span data-ttu-id="0ef19-127">A következő PowerShell-parancsot, és jegyezze fel az azonosító értéket, a titkos URI:</span><span class="sxs-lookup"><span data-stu-id="0ef19-127">Run the following PowerShell command and make note of the ID value, which is the secret URI:</span></span>

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

## <a name="set-up-the-application"></a><span data-ttu-id="0ef19-128">Az alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="0ef19-128">Set up the application</span></span>
<span data-ttu-id="0ef19-129">Most, hogy a titkos kulcs tárolása, kód segítségével kérje le, majd használja.</span><span class="sxs-lookup"><span data-stu-id="0ef19-129">Now that you have a secret stored, you can use code to retrieve and use it.</span></span> <span data-ttu-id="0ef19-130">Ennek eléréséhez szükséges néhány lépésből áll.</span><span class="sxs-lookup"><span data-stu-id="0ef19-130">There are a few steps required to achieve this.</span></span> <span data-ttu-id="0ef19-131">Az első és legfontosabb lépés az alkalmazás regisztrálása az Azure Active Directoryban, és majd közli az alkalmazással kapcsolatos adatok Key Vault, így engedélyezheti, hogy az alkalmazás érkező kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="0ef19-131">The first and most important step is registering your application with Azure Active Directory and then telling Key Vault your application information so that it can allow requests from your application.</span></span>

> [!NOTE]
> <span data-ttu-id="0ef19-132">Az alkalmazás léteznie kell a azonos Azure Active Directory-bérlőt, mint a kulcstárolót.</span><span class="sxs-lookup"><span data-stu-id="0ef19-132">Your application must be created on the same Azure Active Directory tenant as your key vault.</span></span>
>
>

<span data-ttu-id="0ef19-133">Nyissa meg az Azure Active Directory az alkalmazások fülre.</span><span class="sxs-lookup"><span data-stu-id="0ef19-133">Open the applications tab of Azure Active Directory.</span></span>

![Nyissa meg az alkalmazások az Azure Active Directoryban](./media/keyvault-keyrotation/AzureAD_Header.png)

<span data-ttu-id="0ef19-135">Válasszon **Hozzáadás** hozzáadhat egy alkalmazást az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="0ef19-135">Choose **ADD** to add an application to your Azure Active Directory.</span></span>

![A Hozzáadás gombra](./media/keyvault-keyrotation/Azure_AD_AddApp.png)

<span data-ttu-id="0ef19-137">Hagyja meg az alkalmazás típusú **WEB APPLICATION AND/OR WEB API** és nevezze el az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0ef19-137">Leave the application type as **WEB APPLICATION AND/OR WEB API** and give your application a name.</span></span>

![Az alkalmazás neve](./media/keyvault-keyrotation/AzureAD_NewApp1.png)

<span data-ttu-id="0ef19-139">Adja meg az alkalmazás egy **SIGN-ON URL** és egy **APP ID URI**.</span><span class="sxs-lookup"><span data-stu-id="0ef19-139">Give your application a **SIGN-ON URL** and an **APP ID URI**.</span></span> <span data-ttu-id="0ef19-140">Ebben a bemutatóban bármilyen is lehetnek, és azokat később módosítható, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="0ef19-140">These can be anything you want for this demo, and they can be changed later if needed.</span></span>

![Adja meg a szükséges URI-azonosítók](./media/keyvault-keyrotation/AzureAD_NewApp2.png)

<span data-ttu-id="0ef19-142">Az alkalmazás Azure Active Directoryba való hozzáadása után jut az alkalmazás oldalhoz.</span><span class="sxs-lookup"><span data-stu-id="0ef19-142">After the application is added to Azure Active Directory, you will be brought into the application page.</span></span> <span data-ttu-id="0ef19-143">Kattintson a **konfigurálása** lapon, majd keresse meg és másolja a **ügyfél-azonosító** érték.</span><span class="sxs-lookup"><span data-stu-id="0ef19-143">Click the **Configure** tab and then find and copy the **Client ID** value.</span></span> <span data-ttu-id="0ef19-144">Jegyezze fel a későbbi lépésekben az ügyfél-azonosító.</span><span class="sxs-lookup"><span data-stu-id="0ef19-144">Make note of the client ID for later steps.</span></span>

<span data-ttu-id="0ef19-145">Ezt követően az alkalmazás kulcs létrehozása, az Azure Active Directory kommunikálhat.</span><span class="sxs-lookup"><span data-stu-id="0ef19-145">Next, generate a key for your application so it can interact with your Azure Active Directory.</span></span> <span data-ttu-id="0ef19-146">Ez alapján hozhat létre a **kulcsok** szakasz a **konfigurációs** fülre.</span><span class="sxs-lookup"><span data-stu-id="0ef19-146">You can create this under the **Keys** section in the **Configuration** tab.</span></span> <span data-ttu-id="0ef19-147">Jegyezze fel az újonnan létrehozott kulcs használható az Azure Active Directory-alkalmazás egy későbbi lépésben.</span><span class="sxs-lookup"><span data-stu-id="0ef19-147">Make note of the newly generated key from your Azure Active Directory application for use in a later step.</span></span>

![Az Azure Active Directory-alkalmazás kulcsok](./media/keyvault-keyrotation/Azure_AD_AppKeys.png)

<span data-ttu-id="0ef19-149">Mielőtt bármely hívást az alkalmazás a kulcstartót létrehozó, a a key vault kérje meg az alkalmazásról és az engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="0ef19-149">Before establishing any calls from your application into the key vault, you must tell the key vault about your application and its permissions.</span></span> <span data-ttu-id="0ef19-150">A következő parancs időt vesz igénybe, a tároló neve és az ügyfél-Azonosítóját az Azure Active Directory-alkalmazás és biztosít **beolvasása** a kulcstartót eléréséhez alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="0ef19-150">The following command takes the vault name and the client ID from your Azure Active Directory app and grants **Get** access to your key vault for the application.</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

<span data-ttu-id="0ef19-151">Ekkor készen áll az alkalmazás hívások készítéséhez.</span><span class="sxs-lookup"><span data-stu-id="0ef19-151">At this point, you are ready to start building your application calls.</span></span> <span data-ttu-id="0ef19-152">Az alkalmazás telepítenie kell a NuGet-csomagok működjön együtt az Azure Key Vault és az Azure Active Directory szükséges.</span><span class="sxs-lookup"><span data-stu-id="0ef19-152">In your application, you must install the NuGet packages required to interact with Azure Key Vault and Azure Active Directory.</span></span> <span data-ttu-id="0ef19-153">A Visual Studio Csomagkezelő konzolról adja meg a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="0ef19-153">From the Visual Studio Package Manager console, enter the following commands.</span></span> <span data-ttu-id="0ef19-154">: Ez a cikk írásának pillanatában a az Azure Active Directory-csomag nem 3.10.305231913, így előfordulhat, hogy a legújabb verzióra, és ennek megfelelően szeretné.</span><span class="sxs-lookup"><span data-stu-id="0ef19-154">At the writing of this article, the current version of the Azure Active Directory package is 3.10.305231913, so you might want to confirm the latest version and update accordingly.</span></span>

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

<span data-ttu-id="0ef19-155">Az alkalmazás kódjában hozzon létre egy osztályt, ahhoz, hogy az az Azure Active Directory hitelesítési módszert.</span><span class="sxs-lookup"><span data-stu-id="0ef19-155">In your application code, create a class to hold the method for your Azure Active Directory authentication.</span></span> <span data-ttu-id="0ef19-156">Ebben a példában az adott osztály neve **Utils**.</span><span class="sxs-lookup"><span data-stu-id="0ef19-156">In this example, that class is called **Utils**.</span></span> <span data-ttu-id="0ef19-157">Adja hozzá a következő using utasítást:</span><span class="sxs-lookup"><span data-stu-id="0ef19-157">Add the following using statement:</span></span>

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

<span data-ttu-id="0ef19-158">Ezután adja hozzá a következő metódust a JWT jogkivonat beolvasása az Azure Active Directoryból.</span><span class="sxs-lookup"><span data-stu-id="0ef19-158">Next, add the following method to retrieve the JWT token from Azure Active Directory.</span></span> <span data-ttu-id="0ef19-159">A karbantartási követelmények érdemes lehet a kódolt karakterlánc-értékek áthelyezi a webhely vagy alkalmazás konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="0ef19-159">For maintainability, you may want to move the hard-coded string values into your web or application configuration.</span></span>

```csharp
public async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);

    ClientCredential clientCred = new ClientCredential("<AzureADApplicationClientID>","<AzureADApplicationClientKey>");

    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)

    throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

<span data-ttu-id="0ef19-160">Adja hozzá a szükséges kódot Key Vault és a titkos érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="0ef19-160">Add the necessary code to call Key Vault and retrieve your secret value.</span></span> <span data-ttu-id="0ef19-161">Először hozzá kell adnia a következő using utasítást:</span><span class="sxs-lookup"><span data-stu-id="0ef19-161">First you must add the following using statement:</span></span>

```csharp
using Microsoft.Azure.KeyVault;
```

<span data-ttu-id="0ef19-162">Adja hozzá a metódushívások Key Vault meghívni, és a titkos kulcs beolvasása.</span><span class="sxs-lookup"><span data-stu-id="0ef19-162">Add the method calls to invoke Key Vault and retrieve your secret.</span></span> <span data-ttu-id="0ef19-163">Ez a módszer biztosítja a titkos kulcsot, amelyet az előző lépésben mentett URI.</span><span class="sxs-lookup"><span data-stu-id="0ef19-163">In this method, you provide the secret URI that you saved in a previous step.</span></span> <span data-ttu-id="0ef19-164">Vegye figyelembe a használatát a **GetToken** metódust a **Utils** korábban létrehozott osztályt.</span><span class="sxs-lookup"><span data-stu-id="0ef19-164">Note the use of the **GetToken** method from the **Utils** class created previously.</span></span>

```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

<span data-ttu-id="0ef19-165">Az alkalmazás futtatásakor kell hitelesítéséhez az Azure Active Directory és a titkos érték majd lekérése az Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="0ef19-165">When you run your application, you should now be authenticating to Azure Active Directory and then retrieving your secret value from Azure Key Vault.</span></span>

## <a name="key-rotation-using-azure-automation"></a><span data-ttu-id="0ef19-166">Azure Automation használatával kulcs Elforgatás</span><span class="sxs-lookup"><span data-stu-id="0ef19-166">Key rotation using Azure Automation</span></span>
<span data-ttu-id="0ef19-167">Az Azure Key Vault titkok, tárolt értékek Elforgatás stratégiája megvalósításának számos lehetőség áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="0ef19-167">There are various options for implementing a rotation strategy for values you store as Azure Key Vault secrets.</span></span> <span data-ttu-id="0ef19-168">Titkos kulcsok forgatható el kézi folyamat részeként, akkor lehetséges, hogy forgatható programozott módon API-hívásokkal, vagy előfordulhat, hogy forgatható vállalja egy automatizálási parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="0ef19-168">Secrets can be rotated as part of a manual process, they may be rotated programmatically by using API calls, or they may be rotated by way of an Automation script.</span></span> <span data-ttu-id="0ef19-169">Ez a cikk alkalmazásában fog használni az Azure Automation szolgáltatásban, együttesen Azure PowerShell módosítása az Azure Storage-fiók hozzáférési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="0ef19-169">For the purposes of this article, you will be using Azure PowerShell combined with Azure Automation to change an Azure Storage Account access key.</span></span> <span data-ttu-id="0ef19-170">A kulcstároló titkos kulcsot az új kulcs majd frissíti.</span><span class="sxs-lookup"><span data-stu-id="0ef19-170">You will then update a key vault secret with that new key.</span></span>

<span data-ttu-id="0ef19-171">Ahhoz, hogy az Azure Automation key vaultban lévő titkos értékeinek beállításához, ha előbb telepítik azokra az ügyfél-azonosító nevű AzureRunAsConnection, az Azure Automation-példányt létrejöttekor létrehozott.</span><span class="sxs-lookup"><span data-stu-id="0ef19-171">To allow Azure Automation to set secret values in your key vault, you must get the client ID for the connection named AzureRunAsConnection, which was created when you established your Azure Automation instance.</span></span> <span data-ttu-id="0ef19-172">Válassza ezt az Azonosítót található **eszközök** az Azure Automation-példányból.</span><span class="sxs-lookup"><span data-stu-id="0ef19-172">You can find this ID by choosing **Assets** from your Azure Automation instance.</span></span> <span data-ttu-id="0ef19-173">Ott, válasszon **kapcsolatok** , és válassza a **AzureRunAsConnection** szolgáltatás elvet.</span><span class="sxs-lookup"><span data-stu-id="0ef19-173">From there, you choose **Connections** and then select the **AzureRunAsConnection** service principle.</span></span> <span data-ttu-id="0ef19-174">Vegye figyelembe a **Alkalmazásazonosító**.</span><span class="sxs-lookup"><span data-stu-id="0ef19-174">Take note of the **Application ID**.</span></span>

![Azure Automation ügyfél-azonosítója](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

<span data-ttu-id="0ef19-176">A **eszközök**, válassza a **modulok**.</span><span class="sxs-lookup"><span data-stu-id="0ef19-176">In **Assets**, choose **Modules**.</span></span> <span data-ttu-id="0ef19-177">A **modulok**, jelölje be **gyűjtemény**, majd keresse meg és **importálási** frissített verziói, a következő modulok mindegyikének:</span><span class="sxs-lookup"><span data-stu-id="0ef19-177">From **Modules**, select **Gallery**, and then search for and **Import** updated versions of each of the following modules:</span></span>

    Azure
    Azure.Storage
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage


> [!NOTE]
> <span data-ttu-id="0ef19-178">Ez a cikk írásának pillanatában, csak a korábban feljegyzett modulok frissítenie kell a következő parancsfájl szükséges.</span><span class="sxs-lookup"><span data-stu-id="0ef19-178">At the writing of this article, only the previously noted modules needed to be updated for the following script.</span></span> <span data-ttu-id="0ef19-179">Ha talál meg, hogy az automation-feladat meghiúsul, győződjön meg arról, hogy importálta-e minden szükséges modulokat és függőségi viszonyaikat.</span><span class="sxs-lookup"><span data-stu-id="0ef19-179">If you find that your automation job is failing, confirm that you have imported all necessary modules and their dependencies.</span></span>
>
>

<span data-ttu-id="0ef19-180">Után az alkalmazás-azonosítója az Azure Automation-kapcsolat, meg kell, hogy a kulcstároló, hogy az alkalmazás fér hozzá a tárolóban lévő titkos kulcsok frissítése.</span><span class="sxs-lookup"><span data-stu-id="0ef19-180">After you have retrieved the application ID for your Azure Automation connection, you must tell your key vault that this application has access to update secrets in your vault.</span></span> <span data-ttu-id="0ef19-181">Ehhez a következő PowerShell-paranccsal:</span><span class="sxs-lookup"><span data-stu-id="0ef19-181">This can be accomplished with the following PowerShell command:</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

<span data-ttu-id="0ef19-182">Válassza ki, **Runbookok** az Azure Automation-példány, és válassza ki azt a **hozzáadása egy Runbook**.</span><span class="sxs-lookup"><span data-stu-id="0ef19-182">Next, select **Runbooks** under your Azure Automation instance, and then select **Add a Runbook**.</span></span> <span data-ttu-id="0ef19-183">Kattintson a **Gyors létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="0ef19-183">Select **Quick Create**.</span></span> <span data-ttu-id="0ef19-184">A runbook neve, és válassza ki **PowerShell** a runbook típusa.</span><span class="sxs-lookup"><span data-stu-id="0ef19-184">Name your runbook and select **PowerShell** as the runbook type.</span></span> <span data-ttu-id="0ef19-185">Lehetősége van a adjon meg egy leírást.</span><span class="sxs-lookup"><span data-stu-id="0ef19-185">You have the option to add a description.</span></span> <span data-ttu-id="0ef19-186">Végezetül kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="0ef19-186">Finally, click **Create**.</span></span>

![Runbook létrehozása](./media/keyvault-keyrotation/Create_Runbook.png)

<span data-ttu-id="0ef19-188">Illessze be a következő PowerShell-parancsfájlt az új runbook szerkesztő ablaktáblában:</span><span class="sxs-lookup"><span data-stu-id="0ef19-188">Paste the following PowerShell script in the editor pane for your new runbook:</span></span>

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get the connection "AzureRunAsConnection "
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in to Azure..."
    Add-AzureRmAccount `
        -ServicePrincipal `
        -TenantId $servicePrincipalConnection.TenantId `
        -ApplicationId $servicePrincipalConnection.ApplicationId `
        -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
    "Login complete."
}
catch {
    if (!$servicePrincipalConnection)
    {
        $ErrorMessage = "Connection $connectionName not found."
        throw $ErrorMessage
    } else{
        Write-Error -Message $_.Exception
        throw $_.Exception
    }
}

#Optionally you may set the following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for the storage account
New-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

<span data-ttu-id="0ef19-189">A szerkesztő ablaktáblában válassza **teszt ablaktábla** tesztelni a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="0ef19-189">From the editor pane, choose **Test pane** to test your script.</span></span> <span data-ttu-id="0ef19-190">Miután a parancsfájl hiba nélkül fut-e, kijelölheti a **közzététel**, és újra a runbook konfigurációs ablaktábla a runbook ütemezés szerint alkalmazhatja.</span><span class="sxs-lookup"><span data-stu-id="0ef19-190">Once the script is running without error, you can select **Publish**, and then you can apply a schedule for the runbook back in the runbook configuration pane.</span></span>

## <a name="key-vault-auditing-pipeline"></a><span data-ttu-id="0ef19-191">Key Vault naplózási folyamat</span><span class="sxs-lookup"><span data-stu-id="0ef19-191">Key Vault auditing pipeline</span></span>
<span data-ttu-id="0ef19-192">Kulcstároló beállításakor bekapcsolása gyűjtött naplók a hozzáférési kérelmeket a key vault naplózását.</span><span class="sxs-lookup"><span data-stu-id="0ef19-192">When you set up a key vault, you can turn on auditing to collect logs on access requests made to the key vault.</span></span> <span data-ttu-id="0ef19-193">Ezek a naplók a kijelölt Azure Storage-fiókban tárolt, és figyeli, és elemezni, lekért.</span><span class="sxs-lookup"><span data-stu-id="0ef19-193">These logs are stored in a designated Azure Storage account and can be pulled out, monitored, and analyzed.</span></span> <span data-ttu-id="0ef19-194">Az alábbi forgatókönyvet az Azure functions az Azure logic apps és kulcstároló-naplók segítségével hozzon létre egy folyamatot, az e-mailt küld, ha olyan alkalmazás, amelynek felel meg az alkalmazás Azonosítóját a webalkalmazás titkok lekéri a tárolóból.</span><span class="sxs-lookup"><span data-stu-id="0ef19-194">The following scenario uses Azure functions, Azure logic apps, and key vault audit logs to create a pipeline to send an email when an app that does match the app ID of the web app retrieves secrets from the vault.</span></span>

<span data-ttu-id="0ef19-195">Először engedélyeznie kell a kulcstartót bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="0ef19-195">First, you must enable logging on your key vault.</span></span> <span data-ttu-id="0ef19-196">Ez a következő PowerShell-parancsok segítségével végezhető (teljes részletei láthatók [kulcs-tároló-naplózás](key-vault-logging.md)):</span><span class="sxs-lookup"><span data-stu-id="0ef19-196">This can be done via the following PowerShell commands (full details can be seen at [key-vault-logging](key-vault-logging.md)):</span></span>

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>'
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

<span data-ttu-id="0ef19-197">Miután engedélyezve van, az auditnaplókat indítása a kijelölt tárfiókkal való gyűjtésére.</span><span class="sxs-lookup"><span data-stu-id="0ef19-197">After this is enabled, audit logs start collecting into the designated storage account.</span></span> <span data-ttu-id="0ef19-198">Ezek a naplók tartalmaz arról, hogyan és mikor érhetők el a kulcstárolót, és ki eseményeket.</span><span class="sxs-lookup"><span data-stu-id="0ef19-198">These logs contain events about how and when your key vaults are accessed, and by whom.</span></span>

> [!NOTE]
> <span data-ttu-id="0ef19-199">Elérheti a naplóinformációkat 10 perccel a kulcstartót művelet után.</span><span class="sxs-lookup"><span data-stu-id="0ef19-199">You can access your logging information 10 minutes after the key vault operation.</span></span> <span data-ttu-id="0ef19-200">Általában lesz gyorsabb, mint ez.</span><span class="sxs-lookup"><span data-stu-id="0ef19-200">It will usually be quicker than this.</span></span>
>
>

<span data-ttu-id="0ef19-201">A következő lépés [hozzon létre egy Azure Service Bus-üzenetsorba](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span><span class="sxs-lookup"><span data-stu-id="0ef19-201">The next step is to [create an Azure Service Bus queue](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span></span> <span data-ttu-id="0ef19-202">Ez azért, ahol a kulcstartót naplók leküldött vannak.</span><span class="sxs-lookup"><span data-stu-id="0ef19-202">This is where key vault audit logs are pushed.</span></span> <span data-ttu-id="0ef19-203">Ha a naplózási üzenetek a várólista, a logikai alkalmazás felveszi őket, és kezelje őket.</span><span class="sxs-lookup"><span data-stu-id="0ef19-203">When the audit log messages are in the queue, the logic app picks them up and acts on them.</span></span> <span data-ttu-id="0ef19-204">Hozzon létre egy service bus az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="0ef19-204">Create a service bus with the following steps:</span></span>

1. <span data-ttu-id="0ef19-205">Service Bus-névtér létrehozása (Ha már rendelkezik egy, a, folytassa a 2. lépésben használni kívánt).</span><span class="sxs-lookup"><span data-stu-id="0ef19-205">Create a Service Bus namespace (if you already have one that you want to use for this, skip to Step 2).</span></span>
2. <span data-ttu-id="0ef19-206">Keresse meg a service bus az Azure portálon, és válassza ki a létrehozandó sorból névteret.</span><span class="sxs-lookup"><span data-stu-id="0ef19-206">Browse to the service bus in the Azure portal and select the namespace you want to create the queue in.</span></span>
3. <span data-ttu-id="0ef19-207">Válassza ki **új** válassza **Service Bus > várólista** , és írja be a szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="0ef19-207">Select **New** and choose **Service Bus > Queue** and enter the required details.</span></span>
4. <span data-ttu-id="0ef19-208">Válassza ki a Service Bus kapcsolati információit a névtér kiválasztásával, majd **kapcsolatadatok**.</span><span class="sxs-lookup"><span data-stu-id="0ef19-208">Select the Service Bus connection information by choosing the namespace and clicking **Connection Information**.</span></span> <span data-ttu-id="0ef19-209">Szüksége lesz ezt az információt a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="0ef19-209">You will need this information for the next section.</span></span>

<span data-ttu-id="0ef19-210">Ezt követően [egy Azure-függvény létrehozása](../azure-functions/functions-create-first-azure-function.md) kérdezze le a kulcstároló naplóit a tárfiókon belül, és új események átvételéhez.</span><span class="sxs-lookup"><span data-stu-id="0ef19-210">Next, [create an Azure function](../azure-functions/functions-create-first-azure-function.md) to poll key vault logs within the storage account and pick up new events.</span></span> <span data-ttu-id="0ef19-211">Ez lesz az ütemezés szerint kiváltó függvényt.</span><span class="sxs-lookup"><span data-stu-id="0ef19-211">This will be a function that is triggered on a schedule.</span></span>

<span data-ttu-id="0ef19-212">Egy Azure-függvény létrehozása, válassza a **új > függvény App** az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="0ef19-212">To create an Azure function, choose **New > Function App** in the Azure portal.</span></span> <span data-ttu-id="0ef19-213">A létrehozás során egy meglévő üzemeltetési terv használja, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="0ef19-213">During creation, you can use an existing hosting plan or create a new one.</span></span> <span data-ttu-id="0ef19-214">Sikerült is választhat dinamikus üzemeltetéséhez.</span><span class="sxs-lookup"><span data-stu-id="0ef19-214">You could also opt for dynamic hosting.</span></span> <span data-ttu-id="0ef19-215">További részleteket a beállításokat tartalmazó függvény található [az Azure Functions méretezése](../azure-functions/functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="0ef19-215">More details on Function hosting options can be found at [How to scale Azure Functions](../azure-functions/functions-scale.md).</span></span>

<span data-ttu-id="0ef19-216">Az Azure-függvény létrehozása esetén keresse meg a fájlt, és válassza a időzítőt, függvény és C\#.</span><span class="sxs-lookup"><span data-stu-id="0ef19-216">When the Azure function is created, navigate to it and choose a timer function and C\#.</span></span> <span data-ttu-id="0ef19-217">Kattintson a **Ez a függvény létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="0ef19-217">Then click **Create this function**.</span></span>

![Az Azure Functions lépések panelen](./media/keyvault-keyrotation/Azure_Functions_Start.png)

<span data-ttu-id="0ef19-219">Az a **Develop** lapra, cserélje ki a run.csx kódot a következőre:</span><span class="sxs-lookup"><span data-stu-id="0ef19-219">On the **Develop** tab, replace the run.csx code with the following:</span></span>

```csharp
#r "Newtonsoft.Json"

using System;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.ServiceBus.Messaging;
using System.Text;

public static void Run(TimerInfo myTimer, TextReader inputBlob, TextWriter outputBlob, TraceWriter log)
{
    log.Info("Starting");

    CloudStorageAccount sourceStorageAccount = new CloudStorageAccount(new StorageCredentials("<STORAGE_ACCOUNT_NAME>", "<STORAGE_ACCOUNT_KEY>"), true);

    CloudBlobClient sourceCloudBlobClient = sourceStorageAccount.CreateCloudBlobClient();

    var connectionString = "<SERVICE_BUS_CONNECTION_STRING>";
    var queueName = "<SERVICE_BUS_QUEUE_NAME>";

    var sbClient = QueueClient.CreateFromConnectionString(connectionString, queueName);

    DateTime dtPrev = DateTime.UtcNow;
    if(inputBlob != null)
    {
        var txt = inputBlob.ReadToEnd();

        if(!string.IsNullOrEmpty(txt))
        {
            dtPrev = DateTime.Parse(txt);
            log.Verbose($"SyncPoint: {dtPrev.ToString("O")}");
        }
        else
        {
            dtPrev = DateTime.UtcNow;
            log.Verbose($"Sync point file didnt have a date. Setting to now.");
        }
    }

    var now = DateTime.UtcNow;

    string blobPrefix = "insights-logs-auditevent/resourceId=/SUBSCRIPTIONS/<SUBSCRIPTION_ID>/RESOURCEGROUPS/<RESOURCE_GROUP_NAME>/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/<KEY_VAULT_NAME>/y=" + now.Year +"/m="+now.Month.ToString("D2")+"/d="+ (now.Day).ToString("D2")+"/h="+(now.Hour).ToString("D2")+"/m=00/";

    log.Info($"Scanning:  {blobPrefix}");

    IEnumerable<IListBlobItem> blobs = sourceCloudBlobClient.ListBlobs(blobPrefix, true);

    log.Info($"found {blobs.Count()} blobs");

    foreach(var item in blobs)
    {
        if (item is CloudBlockBlob)
        {
            CloudBlockBlob blockBlob = (CloudBlockBlob)item;

            log.Info($"Syncing: {item.Uri}");

            string sharedAccessUri = GetContainerSasUri(blockBlob);

            CloudBlockBlob sourceBlob = new CloudBlockBlob(new Uri(sharedAccessUri));

            string text;
            using (var memoryStream = new MemoryStream())
            {
                sourceBlob.DownloadToStream(memoryStream);
                text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
            }

            dynamic dynJson = JsonConvert.DeserializeObject(text);

            //required to order by time as they may not be in the file
            var results = ((IEnumerable<dynamic>) dynJson.records).OrderBy(p => p.time);

            foreach (var jsonItem in results)
            {
                DateTime dt = Convert.ToDateTime(jsonItem.time);

                if(dt>dtPrev){
                    log.Info($"{jsonItem.ToString()}");

                    var payloadStream = new MemoryStream(Encoding.UTF8.GetBytes(jsonItem.ToString()));
                    //When sending to ServiceBus, use the payloadStream and set keeporiginal to true
                    var message = new BrokeredMessage(payloadStream, true);
                    sbClient.Send(message);
                    dtPrev = dt;
                }
            }
        }
    }
    outputBlob.Write(dtPrev.ToString("o"));
}

static string GetContainerSasUri(CloudBlockBlob blob)
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

    sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

    //Generate the shared access signature on the container, setting the constraints directly on the signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```


> [!NOTE]
> <span data-ttu-id="0ef19-220">Feltétlenül cserélje le a változók az előző kódban, hogy a tárfiók mutasson, amelyben a kulcstároló naplóit íródtak, a korábban létrehozott service bus és a megadott elérési útját a kulcstároló tárolási naplóit.</span><span class="sxs-lookup"><span data-stu-id="0ef19-220">Make sure to replace the variables in the preceding code to point to your storage account where the key vault logs are written, the service bus you created earlier, and the specific path to the key vault storage logs.</span></span>
>
>

<span data-ttu-id="0ef19-221">A függvény szerzi be a legújabb naplófájl a tárfiók a kulcstároló naplóit írt ahol, a fájl legújabb események grabs és leküldéses értesítések azokat a Service Bus-üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="0ef19-221">The function picks up the latest log file from the storage account where the key vault logs are written, grabs the latest events from that file, and pushes them to a Service Bus queue.</span></span> <span data-ttu-id="0ef19-222">Mivel egyetlen fájl rendelkezhet több esemény, készítsen egy sync.txt fájlt, amely a függvény is ellenőrzi, hogy az kivételezett be a legutóbbi esemény időbélyege meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="0ef19-222">Since a single file could have multiple events, you should create a sync.txt file that the function also looks at to determine the time stamp of the last event that was picked up.</span></span> <span data-ttu-id="0ef19-223">Ez biztosítja, hogy nem leküldéses ugyanarra az eseményre több alkalommal.</span><span class="sxs-lookup"><span data-stu-id="0ef19-223">This ensures that you don't push the same event multiple times.</span></span> <span data-ttu-id="0ef19-224">Ez a sync.txt fájl tartalmazza a legutóbbi észlelt esemény időbélyeg.</span><span class="sxs-lookup"><span data-stu-id="0ef19-224">This sync.txt file contains a timestamp for the last encountered event.</span></span> <span data-ttu-id="0ef19-225">A naplókat, ha be van töltve, a megfelelő sorrendben biztosításához időbélyeg alapján rendezni kell.</span><span class="sxs-lookup"><span data-stu-id="0ef19-225">The logs, when loaded, have to be sorted based on the timestamp to ensure they are ordered correctly.</span></span>

<span data-ttu-id="0ef19-226">Ennél a függvénynél néhány további szalagtár szerepel, amely nem használható az Azure Functions mezőben kívüli jelenleg hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="0ef19-226">For this function, we reference a couple of additional libraries that are not available out of the box in Azure Functions.</span></span> <span data-ttu-id="0ef19-227">Az ezen, igazolnia kell őket az Azure Functions NuGet segítségével.</span><span class="sxs-lookup"><span data-stu-id="0ef19-227">To include these, we need Azure Functions to pull them using NuGet.</span></span> <span data-ttu-id="0ef19-228">Válassza ki a **fájlok megtekintése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="0ef19-228">Choose the **View Files** option.</span></span>

![Fájlok beállítás megtekintése](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

<span data-ttu-id="0ef19-230">És adja hozzá a következő tartalommal project.json nevű fájlba:</span><span class="sxs-lookup"><span data-stu-id="0ef19-230">And add a file called project.json with following content:</span></span>

```json
    {
      "frameworks": {
        "net46":{
          "dependencies": {
                "WindowsAzure.Storage": "7.0.0",
                "WindowsAzure.ServiceBus":"3.2.2"
          }
        }
       }
    }
```
<span data-ttu-id="0ef19-231">Akkor **mentése**, az Azure Functions fogja letölteni a szükséges bináris fájlokat.</span><span class="sxs-lookup"><span data-stu-id="0ef19-231">Upon **Save**, Azure Functions will download the required binaries.</span></span>

<span data-ttu-id="0ef19-232">Váltás a **integráció** lapon és a időzítő paraméter adjon meg egy beszédes nevet a a függvényen belül.</span><span class="sxs-lookup"><span data-stu-id="0ef19-232">Switch to the **Integrate** tab and give the timer parameter a meaningful name to use within the function.</span></span> <span data-ttu-id="0ef19-233">Az előző kód hívása időzítő vár *myTimer*.</span><span class="sxs-lookup"><span data-stu-id="0ef19-233">In the preceding code, it expects the timer to be called *myTimer*.</span></span> <span data-ttu-id="0ef19-234">Adjon meg egy [CRON-kifejezés](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) az alábbiak szerint: 0 \* \* \* \* \* az időzítő, amely újraindítja a függvény percenként egyszer futtatásához.</span><span class="sxs-lookup"><span data-stu-id="0ef19-234">Specify a [CRON expression](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) as follows: 0 \* \* \* \* \* for the timer that will cause the function to run once a minute.</span></span>

<span data-ttu-id="0ef19-235">Az azonos **integráció** lapon maradva adja hozzá a típusú bemeneti **Azure Blob Storage**.</span><span class="sxs-lookup"><span data-stu-id="0ef19-235">On the same **Integrate** tab, add an input of the type **Azure Blob Storage**.</span></span> <span data-ttu-id="0ef19-236">Ez a sync.txt fájlt, amely tartalmazza a tekintett meg, amelyet a függvény utolsó esemény időbélyegzője mutasson.</span><span class="sxs-lookup"><span data-stu-id="0ef19-236">This will point to the sync.txt file that contains the timestamp of the last event looked at by the function.</span></span> <span data-ttu-id="0ef19-237">Ez a paraméter neve függvényen belül elérhető lesz.</span><span class="sxs-lookup"><span data-stu-id="0ef19-237">This will be available within the function by the parameter name.</span></span> <span data-ttu-id="0ef19-238">Az előzőekben látható kód az Azure Blob Storage bemenetet vár a paraméternév megadásához, hogy *inputBlob*.</span><span class="sxs-lookup"><span data-stu-id="0ef19-238">In the preceding code, the Azure Blob Storage input expects the parameter name to be *inputBlob*.</span></span> <span data-ttu-id="0ef19-239">Válassza ki a tárfiók, amelyben a sync.txt fájl lesznek tárolva (Ez lehet ugyanaz vagy egy másik tárolási fiókot).</span><span class="sxs-lookup"><span data-stu-id="0ef19-239">Choose the storage account where the sync.txt file will reside (it could be the same or a different storage account).</span></span> <span data-ttu-id="0ef19-240">Az elérési út mezőben adja meg az elérési utat, ahol a formátum {container-name}/path/to/sync.txt a fájl él.</span><span class="sxs-lookup"><span data-stu-id="0ef19-240">In the path field, provide the path where the file lives in the format {container-name}/path/to/sync.txt.</span></span>

<span data-ttu-id="0ef19-241">Adja hozzá egy kimeneti típusú *Azure Blob Storage* kimeneti.</span><span class="sxs-lookup"><span data-stu-id="0ef19-241">Add an output of the type *Azure Blob Storage* output.</span></span> <span data-ttu-id="0ef19-242">Ez a sync.txt fájl a bemeneti megadott mutasson.</span><span class="sxs-lookup"><span data-stu-id="0ef19-242">This will point to the sync.txt file you defined in the input.</span></span> <span data-ttu-id="0ef19-243">Ez használatos a függvény tekintett meg a legutóbbi esemény időbélyege írni.</span><span class="sxs-lookup"><span data-stu-id="0ef19-243">This is used by the function to write the timestamp of the last event looked at.</span></span> <span data-ttu-id="0ef19-244">Az előzőekben látható kód várja a paraméter hívni *outputBlob*.</span><span class="sxs-lookup"><span data-stu-id="0ef19-244">The preceding code expects this parameter to be called *outputBlob*.</span></span>

<span data-ttu-id="0ef19-245">Ezen a ponton a függvény készen áll.</span><span class="sxs-lookup"><span data-stu-id="0ef19-245">At this point, the function is ready.</span></span> <span data-ttu-id="0ef19-246">Ügyeljen arra, hogy váltson vissza a **Develop** lapra, és mentse a kódot.</span><span class="sxs-lookup"><span data-stu-id="0ef19-246">Make sure to switch back to the **Develop** tab and save the code.</span></span> <span data-ttu-id="0ef19-247">A kimeneti ablakban, a fordítási hibákat, és ennek megfelelően javítsa ki azokat.</span><span class="sxs-lookup"><span data-stu-id="0ef19-247">Check the output window for any compilation errors and correct them accordingly.</span></span> <span data-ttu-id="0ef19-248">Ha a kód lefordításához, majd a kódot kell most kell ellenőrzése a kulcstároló naplóit percenként és terjesztése a megadott Service Bus-üzenetsorba, új eseményeket.</span><span class="sxs-lookup"><span data-stu-id="0ef19-248">If the code compiles, then the code should now be checking the key vault logs every minute and pushing any new events onto the defined Service Bus queue.</span></span> <span data-ttu-id="0ef19-249">Minden alkalommal aktiválódik, a függvény a napló ablakban írja naplóinformációk kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="0ef19-249">You should see logging information write out to the log window every time the function is triggered.</span></span>

### <a name="azure-logic-app"></a><span data-ttu-id="0ef19-250">Az Azure Logic Apps alkalmazást</span><span class="sxs-lookup"><span data-stu-id="0ef19-250">Azure logic app</span></span>
<span data-ttu-id="0ef19-251">Ezután létre kell hoznia egy Azure logikai alkalmazás, amely szerzi be az eseményeket, hogy a funkció a Service Bus-üzenetsorba való küldését, elemzi a tartalom és elküld egy e-mailt az egyező feltétel alapján.</span><span class="sxs-lookup"><span data-stu-id="0ef19-251">Next you must create an Azure logic app that picks up the events that the function is pushing to the Service Bus queue, parses the content, and sends an email based on a condition being matched.</span></span>

<span data-ttu-id="0ef19-252">[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md) címen **új > logikai alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0ef19-252">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md) by going to **New > Logic App**.</span></span>

<span data-ttu-id="0ef19-253">A logikai alkalmazás létrehozása után keresse meg a fájlt, és válassza a **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="0ef19-253">Once the logic app is created, navigate to it and choose **edit**.</span></span> <span data-ttu-id="0ef19-254">A logic app szerkesztő választható **Service Bus-üzenetsorba** a várólista csatlakozni a Service Bus hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="0ef19-254">Within the logic app editor, choose **Service Bus Queue** and enter your Service Bus credentials to connect it to the queue.</span></span>

![Az Azure Logic App Service Bus](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

<span data-ttu-id="0ef19-256">Ezután válasszon **feltétel hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="0ef19-256">Next choose **Add a condition**.</span></span> <span data-ttu-id="0ef19-257">A feltételben a speciális szerkesztő váltson, és írja be a következő kódot, a tényleges APP_ID webalkalmazás APP_ID cseréje:</span><span class="sxs-lookup"><span data-stu-id="0ef19-257">In the condition, switch to the advanced editor and enter the following code, replacing APP_ID with the actual APP_ID of your web app:</span></span>

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

<span data-ttu-id="0ef19-258">Ebben a kifejezésben lényegében adja vissza **hamis** Ha a *appid* a bejövő eseménytől (amely a Service Bus üzenet törzsét) nincs a *appid* az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0ef19-258">This expression essentially returns **false** if the *appid* from the incoming event (which is the body of the Service Bus message) is not the *appid* of the app.</span></span>

<span data-ttu-id="0ef19-259">Ezután hozzon létre egy műveletet a **Ha nem, nem történik semmi**.</span><span class="sxs-lookup"><span data-stu-id="0ef19-259">Now, create an action under **If no, do nothing**.</span></span>

![Az Azure Logic App művelet kiválasztását.](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

<span data-ttu-id="0ef19-261">A művelet kiválasztása **Office 365 – e-mail küldése**.</span><span class="sxs-lookup"><span data-stu-id="0ef19-261">For the action, choose **Office 365 - send email**.</span></span> <span data-ttu-id="0ef19-262">Hozzon létre egy e-mailek küldése, ha a megadott feltétel visszaadja a mezők kitöltése **hamis**.</span><span class="sxs-lookup"><span data-stu-id="0ef19-262">Fill out the fields to create an email to send when the defined condition returns **false**.</span></span> <span data-ttu-id="0ef19-263">Ha nem rendelkezik Office 365, sikerült megnézzük alternatívák ugyanaz az eredmény elérése érdekében.</span><span class="sxs-lookup"><span data-stu-id="0ef19-263">If you do not have Office 365, you could look at alternatives to achieve the same results.</span></span>

<span data-ttu-id="0ef19-264">Ezen a ponton rendelkezik egy teljes körű folyamatot, amely új kulcstartó naplók percenként egyszer keresi.</span><span class="sxs-lookup"><span data-stu-id="0ef19-264">At this point, you have an end to end pipeline that looks for new key vault audit logs once a minute.</span></span> <span data-ttu-id="0ef19-265">Úgy találja, az új naplók az leküldi a service bus-üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="0ef19-265">It pushes new logs it finds to a service bus queue.</span></span> <span data-ttu-id="0ef19-266">A logikai alkalmazás lesz kiváltva, ha egy új üzenet a várólistában lévő fájljai.</span><span class="sxs-lookup"><span data-stu-id="0ef19-266">The logic app is triggered when a new message lands in the queue.</span></span> <span data-ttu-id="0ef19-267">Ha a *appid* belül az esemény nem egyezik meg az alkalmazás Azonosítóját a hívó alkalmazás, egy e-mailt küld.</span><span class="sxs-lookup"><span data-stu-id="0ef19-267">If the *appid* within the event does not match the app ID of the calling application, it sends an email.</span></span>
