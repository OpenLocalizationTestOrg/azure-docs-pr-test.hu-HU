---
title: "végpont kulcs elforgatás és naplózás mentése az Azure Key Vault aaaSet |} Microsoft Docs"
description: "A kulcs elforgatás és figyelési kulcstároló naplóit beolvasása beállítása hogyan-tootoohelp használja."
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
ms.openlocfilehash: e0c393873077e3b91adc9fa7f39128bc1b6abe26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-azure-key-vault-with-end-to-end-key-rotation-and-auditing"></a><span data-ttu-id="f2578-103">Az Azure Key Vault beállítása végpontok közötti kulcsforgatással és auditálással</span><span class="sxs-lookup"><span data-stu-id="f2578-103">Set up Azure Key Vault with end-to-end key rotation and auditing</span></span>
## <a name="introduction"></a><span data-ttu-id="f2578-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="f2578-104">Introduction</span></span>
<span data-ttu-id="f2578-105">Miután létrehozta a kulcstároló, a tároló toostore használ a kulcsok és titkos képes toostart fogja.</span><span class="sxs-lookup"><span data-stu-id="f2578-105">After creating your key vault, you will be able toostart using that vault toostore your keys and secrets.</span></span> <span data-ttu-id="f2578-106">Az alkalmazások toopersist már nem szükséges, a kulcsok vagy titkos, de ahelyett, hogy fog igényelni őket hello kulcs tárolóból igény szerint.</span><span class="sxs-lookup"><span data-stu-id="f2578-106">Your applications no longer need toopersist your keys or secrets, but rather will request them from hello key vault as needed.</span></span> <span data-ttu-id="f2578-107">Ez lehetővé teszi tooupdate kulcsok és titkos hello viselkedést az alkalmazás, így akár a kulcs és a titkos felügyeleti lehetőségek széles választékát befolyásolása nélkül.</span><span class="sxs-lookup"><span data-stu-id="f2578-107">This allows you tooupdate keys and secrets without affecting hello behavior of your application, which opens up a breadth of possibilities around your key and secret management.</span></span>

<span data-ttu-id="f2578-108">Ez a cikk útmutatást keresztül az Azure Key Vault toostore használatának példája egy titkos kulcsot, ebben az esetben egy Azure Storage-fiók kulcsot, amely egy alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="f2578-108">This article walks through an example of using Azure Key Vault toostore a secret, in this case an Azure Storage Account key that is accessed by an application.</span></span> <span data-ttu-id="f2578-109">Is egy ütemezett elforgatási szögét a tárfiók kulcsa végrehajtását mutatja be.</span><span class="sxs-lookup"><span data-stu-id="f2578-109">It also demonstrates implementation of a scheduled rotation of that storage account key.</span></span> <span data-ttu-id="f2578-110">Végül azt végigvezeti az egyes hogyan toomonitor hello kulcstároló naplókat, és riasztást, ha a nem várt kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="f2578-110">Finally, it walks through a demonstration of how toomonitor hello key vault audit logs and raise alerts when unexpected requests are made.</span></span>

> [!NOTE]
> <span data-ttu-id="f2578-111">Ebben az oktatóanyagban nincs tervezett tooexplain részletes hello kezdeti telepítése a kulcstartót.</span><span class="sxs-lookup"><span data-stu-id="f2578-111">This tutorial is not intended tooexplain in detail hello initial setup of your key vault.</span></span> <span data-ttu-id="f2578-112">Ezekről a [Get started with Azure Key Vault](key-vault-get-started.md) (Bevezetés az Azure Key Vault használatába) című cikkben találhat információt.</span><span class="sxs-lookup"><span data-stu-id="f2578-112">For this information, see [Get started with Azure Key Vault](key-vault-get-started.md).</span></span> <span data-ttu-id="f2578-113">Platformfüggetlen parancssori felületre vonatkozó utasításokat lásd: [kezelése Key Vault parancssori felület használatával](key-vault-manage-with-cli2.md).</span><span class="sxs-lookup"><span data-stu-id="f2578-113">For Cross-Platform Command-Line Interface instructions, see [Manage Key Vault using CLI](key-vault-manage-with-cli2.md).</span></span>
>
>

## <a name="set-up-key-vault"></a><span data-ttu-id="f2578-114">A Key Vault beállítása</span><span class="sxs-lookup"><span data-stu-id="f2578-114">Set up Key Vault</span></span>
<span data-ttu-id="f2578-115">egy alkalmazás titkos kulcs tooretrieve tooenable a kulcstároló, először hello titkos kulcs létrehozása, és töltse fel az tooyour tárolóban.</span><span class="sxs-lookup"><span data-stu-id="f2578-115">tooenable an application tooretrieve a secret from Key Vault, you must first create hello secret and upload it tooyour vault.</span></span> <span data-ttu-id="f2578-116">Ehhez az Azure PowerShell-munkamenet indítása, aláírást tooyour az Azure-fiók a hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="f2578-116">This can be accomplished by starting an Azure PowerShell session and signing in tooyour Azure account with hello following command:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="f2578-117">Hello előugró böngészőablakban adja meg a Azure-fiók felhasználói nevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="f2578-117">In hello pop-up browser window, enter your Azure account user name and password.</span></span> <span data-ttu-id="f2578-118">PowerShell beolvassa az ehhez a fiókhoz társított összes hello-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="f2578-118">PowerShell will get all hello subscriptions that are associated with this account.</span></span> <span data-ttu-id="f2578-119">PowerShell használ hello alapértelmezés szerint az elsőt.</span><span class="sxs-lookup"><span data-stu-id="f2578-119">PowerShell uses hello first one by default.</span></span>

<span data-ttu-id="f2578-120">Ha több előfizetéssel rendelkezik, lehetséges, hogy a kulcstartót hello egyet, de a használt toocreate toospecify.</span><span class="sxs-lookup"><span data-stu-id="f2578-120">If you have multiple subscriptions, you might have toospecify hello one that was used toocreate your key vault.</span></span> <span data-ttu-id="f2578-121">Adja meg a fiókhoz toosee hello előfizetések a következő hello:</span><span class="sxs-lookup"><span data-stu-id="f2578-121">Enter hello following toosee hello subscriptions for your account:</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="f2578-122">Adja meg a toospecify hello előfizetés meg fogja naplózás, hello kulcstároló társított:</span><span class="sxs-lookup"><span data-stu-id="f2578-122">toospecify hello subscription that's associated with hello key vault you will be logging, enter:</span></span>

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID>
```

<span data-ttu-id="f2578-123">Mivel ez a cikk bemutatja, hogy a tárfiók kulcsára, titkos kulcs tárolása, ha előbb telepítik azokra a tárfiók kulcsára.</span><span class="sxs-lookup"><span data-stu-id="f2578-123">Because this article demonstrates storing a storage account key as a secret, you must get that storage account key.</span></span>

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

<span data-ttu-id="f2578-124">A titkos kulcsot (ebben az esetben a tárfiók kulcsára) beolvasása, után kell alakítani, hogy tooa biztonságos karakterláncot, és majd titkos kulcs létrehozása ezt az értéket a key vaultban lévő.</span><span class="sxs-lookup"><span data-stu-id="f2578-124">After retrieving your secret (in this case, your storage account key), you must convert that tooa secure string and then create a secret with that value in your key vault.</span></span>

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```
<span data-ttu-id="f2578-125">A következő beolvasása hello URI létrehozott hello titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="f2578-125">Next, get hello URI for hello secret you created.</span></span> <span data-ttu-id="f2578-126">Ez szolgál egy későbbi lépésben a titkos kód hello kulcstároló tooretrieve hívásakor.</span><span class="sxs-lookup"><span data-stu-id="f2578-126">This is used in a later step when you call hello key vault tooretrieve your secret.</span></span> <span data-ttu-id="f2578-127">Futtassa a következő PowerShell-paranccsal hello, és jegyezze fel a hello azonosító értéke, amely hello titkos kulcs URI:</span><span class="sxs-lookup"><span data-stu-id="f2578-127">Run hello following PowerShell command and make note of hello ID value, which is hello secret URI:</span></span>

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

## <a name="set-up-hello-application"></a><span data-ttu-id="f2578-128">Hello alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="f2578-128">Set up hello application</span></span>
<span data-ttu-id="f2578-129">Most, hogy a titkos kulcs tárolása, kód tooretrieve használja, és használja azt.</span><span class="sxs-lookup"><span data-stu-id="f2578-129">Now that you have a secret stored, you can use code tooretrieve and use it.</span></span> <span data-ttu-id="f2578-130">Van néhány lépést szükséges tooachieve ez.</span><span class="sxs-lookup"><span data-stu-id="f2578-130">There are a few steps required tooachieve this.</span></span> <span data-ttu-id="f2578-131">hello első és legfontosabb lépés az alkalmazás regisztrálása az Azure Active Directoryban, majd közli az alkalmazással kapcsolatos adatok Key Vault, így engedélyezheti, hogy az alkalmazás érkező kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="f2578-131">hello first and most important step is registering your application with Azure Active Directory and then telling Key Vault your application information so that it can allow requests from your application.</span></span>

> [!NOTE]
> <span data-ttu-id="f2578-132">Az alkalmazás kell létrehozni a hello ugyanaz, mint a kulcstárolót Azure Active Directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="f2578-132">Your application must be created on hello same Azure Active Directory tenant as your key vault.</span></span>
>
>

<span data-ttu-id="f2578-133">Nyissa meg az Azure Active Directory hello alkalmazások lapon.</span><span class="sxs-lookup"><span data-stu-id="f2578-133">Open hello applications tab of Azure Active Directory.</span></span>

![Nyissa meg az alkalmazások az Azure Active Directoryban](./media/keyvault-keyrotation/AzureAD_Header.png)

<span data-ttu-id="f2578-135">Válasszon **hozzáadása** tooadd egy alkalmazás tooyour Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="f2578-135">Choose **ADD** tooadd an application tooyour Azure Active Directory.</span></span>

![A Hozzáadás gombra](./media/keyvault-keyrotation/Azure_AD_AddApp.png)

<span data-ttu-id="f2578-137">Alkalmazás típusú hello hagyja **WEB APPLICATION AND/OR WEB API** és nevezze el az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f2578-137">Leave hello application type as **WEB APPLICATION AND/OR WEB API** and give your application a name.</span></span>

![Nevű hello alkalmazás](./media/keyvault-keyrotation/AzureAD_NewApp1.png)

<span data-ttu-id="f2578-139">Adja meg az alkalmazás egy **SIGN-ON URL** és egy **APP ID URI**.</span><span class="sxs-lookup"><span data-stu-id="f2578-139">Give your application a **SIGN-ON URL** and an **APP ID URI**.</span></span> <span data-ttu-id="f2578-140">Ebben a bemutatóban bármilyen is lehetnek, és azokat később módosítható, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="f2578-140">These can be anything you want for this demo, and they can be changed later if needed.</span></span>

![Adja meg a szükséges URI-azonosítók](./media/keyvault-keyrotation/AzureAD_NewApp2.png)

<span data-ttu-id="f2578-142">Hello alkalmazás hozzáadása az Active Directory tooAzure után jut hello alkalmazás oldalhoz.</span><span class="sxs-lookup"><span data-stu-id="f2578-142">After hello application is added tooAzure Active Directory, you will be brought into hello application page.</span></span> <span data-ttu-id="f2578-143">Kattintson a hello **konfigurálása** lapon, majd keresse meg és hello másolása **ügyfél-azonosító** érték.</span><span class="sxs-lookup"><span data-stu-id="f2578-143">Click hello **Configure** tab and then find and copy hello **Client ID** value.</span></span> <span data-ttu-id="f2578-144">Jegyezze fel az ügyfél-azonosító hello későbbi lépéseire.</span><span class="sxs-lookup"><span data-stu-id="f2578-144">Make note of hello client ID for later steps.</span></span>

<span data-ttu-id="f2578-145">Ezt követően az alkalmazás kulcs létrehozása, az Azure Active Directory kommunikálhat.</span><span class="sxs-lookup"><span data-stu-id="f2578-145">Next, generate a key for your application so it can interact with your Azure Active Directory.</span></span> <span data-ttu-id="f2578-146">Ez a hello hozhat létre **kulcsok** hello szakasz **konfigurációs** lapon. Jegyezze fel az újonnan létrehozott hello kulcs használható az Azure Active Directory-alkalmazás egy későbbi lépésben.</span><span class="sxs-lookup"><span data-stu-id="f2578-146">You can create this under hello **Keys** section in hello **Configuration** tab. Make note of hello newly generated key from your Azure Active Directory application for use in a later step.</span></span>

![Az Azure Active Directory-alkalmazás kulcsok](./media/keyvault-keyrotation/Azure_AD_AppKeys.png)

<span data-ttu-id="f2578-148">Mielőtt bármely hívásokat az alkalmazásból történő hello kulcstároló létrehozása, a hello kulcstároló kérje meg az alkalmazásról és az engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="f2578-148">Before establishing any calls from your application into hello key vault, you must tell hello key vault about your application and its permissions.</span></span> <span data-ttu-id="f2578-149">hello következő parancsnak hello tároló nevére és hello ügyfél-azonosító az Azure Active Directory-alkalmazás és biztosít **beolvasása** hozzáférés tooyour kulcstároló hello alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="f2578-149">hello following command takes hello vault name and hello client ID from your Azure Active Directory app and grants **Get** access tooyour key vault for hello application.</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

<span data-ttu-id="f2578-150">Ekkor készen áll az alkalmazás hívások felépítése toostart áll.</span><span class="sxs-lookup"><span data-stu-id="f2578-150">At this point, you are ready toostart building your application calls.</span></span> <span data-ttu-id="f2578-151">Az alkalmazás az Azure Key Vault és az Azure Active Directory hello NuGet-csomagok szükséges toointeract kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="f2578-151">In your application, you must install hello NuGet packages required toointeract with Azure Key Vault and Azure Active Directory.</span></span> <span data-ttu-id="f2578-152">Hello Visual Studio Csomagkezelő konzolról adja meg a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="f2578-152">From hello Visual Studio Package Manager console, enter hello following commands.</span></span> <span data-ttu-id="f2578-153">: Ez a cikk hello írását, hello hello Azure Active Directory-csomag nem 3.10.305231913, így előfordulhat, hogy szeretné, hogy tooconfirm hello legújabb verzióját, és ennek megfelelően frissülnek.</span><span class="sxs-lookup"><span data-stu-id="f2578-153">At hello writing of this article, hello current version of hello Azure Active Directory package is 3.10.305231913, so you might want tooconfirm hello latest version and update accordingly.</span></span>

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

<span data-ttu-id="f2578-154">Az alkalmazás kódjában hozzon létre egy osztály toohold hello hitelesítési módszer választása az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="f2578-154">In your application code, create a class toohold hello method for your Azure Active Directory authentication.</span></span> <span data-ttu-id="f2578-155">Ebben a példában az adott osztály neve **Utils**.</span><span class="sxs-lookup"><span data-stu-id="f2578-155">In this example, that class is called **Utils**.</span></span> <span data-ttu-id="f2578-156">Adja hozzá hello következő using utasítást:</span><span class="sxs-lookup"><span data-stu-id="f2578-156">Add hello following using statement:</span></span>

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

<span data-ttu-id="f2578-157">Ezután adja hozzá a hello metódus tooretrieve hello JWT jogkivonat követően az Azure Active Directoryból.</span><span class="sxs-lookup"><span data-stu-id="f2578-157">Next, add hello following method tooretrieve hello JWT token from Azure Active Directory.</span></span> <span data-ttu-id="f2578-158">A karbantartási követelmények érdemes lehet az toomove hello kódolt karakterlánc-értékek be a webhely vagy alkalmazás konfigurációjának.</span><span class="sxs-lookup"><span data-stu-id="f2578-158">For maintainability, you may want toomove hello hard-coded string values into your web or application configuration.</span></span>

```csharp
public async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);

    ClientCredential clientCred = new ClientCredential("<AzureADApplicationClientID>","<AzureADApplicationClientKey>");

    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)

    throw new InvalidOperationException("Failed tooobtain hello JWT token");

    return result.AccessToken;
}
```

<span data-ttu-id="f2578-159">Hello szükséges kód toocall Key Vault hozzáadása, és a titkos érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="f2578-159">Add hello necessary code toocall Key Vault and retrieve your secret value.</span></span> <span data-ttu-id="f2578-160">Először hozzá kell adnia hello következő using utasítást:</span><span class="sxs-lookup"><span data-stu-id="f2578-160">First you must add hello following using statement:</span></span>

```csharp
using Microsoft.Azure.KeyVault;
```

<span data-ttu-id="f2578-161">Hello metódus hívások tooinvoke Key Vault hozzáadása, és a titkos kulcs beolvasása.</span><span class="sxs-lookup"><span data-stu-id="f2578-161">Add hello method calls tooinvoke Key Vault and retrieve your secret.</span></span> <span data-ttu-id="f2578-162">Ezzel a módszerrel hello titkos kulcs URI, amelyet az előző lépésben mentett adja meg.</span><span class="sxs-lookup"><span data-stu-id="f2578-162">In this method, you provide hello secret URI that you saved in a previous step.</span></span> <span data-ttu-id="f2578-163">Vegye figyelembe a hello hello használata **GetToken** hello metódusnak **Utils** korábban létrehozott osztályt.</span><span class="sxs-lookup"><span data-stu-id="f2578-163">Note hello use of hello **GetToken** method from hello **Utils** class created previously.</span></span>

```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

<span data-ttu-id="f2578-164">Az alkalmazás futtatásakor meg kell most kell hitelesítő tooAzure Active Directory és majd a titkos értékének beolvasása az Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="f2578-164">When you run your application, you should now be authenticating tooAzure Active Directory and then retrieving your secret value from Azure Key Vault.</span></span>

## <a name="key-rotation-using-azure-automation"></a><span data-ttu-id="f2578-165">Azure Automation használatával kulcs Elforgatás</span><span class="sxs-lookup"><span data-stu-id="f2578-165">Key rotation using Azure Automation</span></span>
<span data-ttu-id="f2578-166">Az Azure Key Vault titkok, tárolt értékek Elforgatás stratégiája megvalósításának számos lehetőség áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="f2578-166">There are various options for implementing a rotation strategy for values you store as Azure Key Vault secrets.</span></span> <span data-ttu-id="f2578-167">Titkos kulcsok forgatható el kézi folyamat részeként, akkor lehetséges, hogy forgatható programozott módon API-hívásokkal, vagy előfordulhat, hogy forgatható vállalja egy automatizálási parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="f2578-167">Secrets can be rotated as part of a manual process, they may be rotated programmatically by using API calls, or they may be rotated by way of an Automation script.</span></span> <span data-ttu-id="f2578-168">Ez a cikk hello célokra fogja az Azure PowerShell együttesen Azure Automation toochange egy Azure Storage-fiók hozzáférési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="f2578-168">For hello purposes of this article, you will be using Azure PowerShell combined with Azure Automation toochange an Azure Storage Account access key.</span></span> <span data-ttu-id="f2578-169">A kulcstároló titkos kulcsot az új kulcs majd frissíti.</span><span class="sxs-lookup"><span data-stu-id="f2578-169">You will then update a key vault secret with that new key.</span></span>

<span data-ttu-id="f2578-170">tooallow Azure Automation tooset titkos értékek key vaultban lévő, ha előbb telepítik azokra hello ügyfél-azonosító AzureRunAsConnection, az Azure Automation-példányt létrejöttekor létrehozott nevű hello kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="f2578-170">tooallow Azure Automation tooset secret values in your key vault, you must get hello client ID for hello connection named AzureRunAsConnection, which was created when you established your Azure Automation instance.</span></span> <span data-ttu-id="f2578-171">Válassza ezt az Azonosítót található **eszközök** az Azure Automation-példányból.</span><span class="sxs-lookup"><span data-stu-id="f2578-171">You can find this ID by choosing **Assets** from your Azure Automation instance.</span></span> <span data-ttu-id="f2578-172">Ott, válasszon **kapcsolatok** , és válassza a hello **AzureRunAsConnection** szolgáltatás elvet.</span><span class="sxs-lookup"><span data-stu-id="f2578-172">From there, you choose **Connections** and then select hello **AzureRunAsConnection** service principle.</span></span> <span data-ttu-id="f2578-173">Jegyezze fel a hello **Alkalmazásazonosító**.</span><span class="sxs-lookup"><span data-stu-id="f2578-173">Take note of hello **Application ID**.</span></span>

![Azure Automation ügyfél-azonosítója](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

<span data-ttu-id="f2578-175">A **eszközök**, válassza a **modulok**.</span><span class="sxs-lookup"><span data-stu-id="f2578-175">In **Assets**, choose **Modules**.</span></span> <span data-ttu-id="f2578-176">A **modulok**, jelölje be **gyűjteménye**, majd keresse meg és **importálása** frissített verziói egyes hello modulok a következő:</span><span class="sxs-lookup"><span data-stu-id="f2578-176">From **Modules**, select **Gallery**, and then search for and **Import** updated versions of each of hello following modules:</span></span>

    Azure
    Azure.Storage
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage


> [!NOTE]
> <span data-ttu-id="f2578-177">: Ez a cikk hello írását, csak hello szükséges korábban feljegyzett modulok toobe frissítve a következő parancsfájl hello.</span><span class="sxs-lookup"><span data-stu-id="f2578-177">At hello writing of this article, only hello previously noted modules needed toobe updated for hello following script.</span></span> <span data-ttu-id="f2578-178">Ha talál meg, hogy az automation-feladat meghiúsul, győződjön meg arról, hogy importálta-e minden szükséges modulokat és függőségi viszonyaikat.</span><span class="sxs-lookup"><span data-stu-id="f2578-178">If you find that your automation job is failing, confirm that you have imported all necessary modules and their dependencies.</span></span>
>
>

<span data-ttu-id="f2578-179">Az Azure Automation-kapcsolat beolvasása hello Alkalmazásazonosító, után kell közli a kulcstartót, hogy rendelkezik-e az alkalmazás hozzáférési tooupdate titkokat a tárolóban lévő állapottal.</span><span class="sxs-lookup"><span data-stu-id="f2578-179">After you have retrieved hello application ID for your Azure Automation connection, you must tell your key vault that this application has access tooupdate secrets in your vault.</span></span> <span data-ttu-id="f2578-180">Ehhez a következő PowerShell-paranccsal hello:</span><span class="sxs-lookup"><span data-stu-id="f2578-180">This can be accomplished with hello following PowerShell command:</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

<span data-ttu-id="f2578-181">Válassza ki, **Runbookok** az Azure Automation-példány, és válassza ki azt a **hozzáadása egy Runbook**.</span><span class="sxs-lookup"><span data-stu-id="f2578-181">Next, select **Runbooks** under your Azure Automation instance, and then select **Add a Runbook**.</span></span> <span data-ttu-id="f2578-182">Kattintson a **Gyors létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f2578-182">Select **Quick Create**.</span></span> <span data-ttu-id="f2578-183">A runbook neve, és válassza ki **PowerShell** hello runbook típusként.</span><span class="sxs-lookup"><span data-stu-id="f2578-183">Name your runbook and select **PowerShell** as hello runbook type.</span></span> <span data-ttu-id="f2578-184">Hello beállítás tooadd tartozik leírás.</span><span class="sxs-lookup"><span data-stu-id="f2578-184">You have hello option tooadd a description.</span></span> <span data-ttu-id="f2578-185">Végezetül kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="f2578-185">Finally, click **Create**.</span></span>

![Runbook létrehozása](./media/keyvault-keyrotation/Create_Runbook.png)

<span data-ttu-id="f2578-187">Illessze be a következő PowerShell-parancsfájl hello szerkesztő ablaktáblában az új runbook hello:</span><span class="sxs-lookup"><span data-stu-id="f2578-187">Paste hello following PowerShell script in hello editor pane for your new runbook:</span></span>

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get hello connection "AzureRunAsConnection "
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in tooAzure..."
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

#Optionally you may set hello following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for hello storage account
New-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

<span data-ttu-id="f2578-188">Hello szerkesztő ablaktáblán válassza ki a **teszt ablaktábla** tootest a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="f2578-188">From hello editor pane, choose **Test pane** tootest your script.</span></span> <span data-ttu-id="f2578-189">Miután hello parancsfájl hiba nélkül fut-e, kijelölheti a **közzététel**, és vissza a hello runbook konfigurációs ablaktáblán hello runbook ütemezés szerint alkalmazhatja.</span><span class="sxs-lookup"><span data-stu-id="f2578-189">Once hello script is running without error, you can select **Publish**, and then you can apply a schedule for hello runbook back in hello runbook configuration pane.</span></span>

## <a name="key-vault-auditing-pipeline"></a><span data-ttu-id="f2578-190">Key Vault naplózási folyamat</span><span class="sxs-lookup"><span data-stu-id="f2578-190">Key Vault auditing pipeline</span></span>
<span data-ttu-id="f2578-191">Kulcstároló beállításakor bekapcsolása toocollect toohello kulcstároló hozzáférési kérelmet a naplófájlokat.</span><span class="sxs-lookup"><span data-stu-id="f2578-191">When you set up a key vault, you can turn on auditing toocollect logs on access requests made toohello key vault.</span></span> <span data-ttu-id="f2578-192">Ezek a naplók a kijelölt Azure Storage-fiókban tárolt, és figyeli, és elemezni, lekért.</span><span class="sxs-lookup"><span data-stu-id="f2578-192">These logs are stored in a designated Azure Storage account and can be pulled out, monitored, and analyzed.</span></span> <span data-ttu-id="f2578-193">hello alábbi forgatókönyvet használja az Azure functions, az Azure logic apps és kulcstároló naplózási naplók toocreate egy folyamat toosend egy e-mailt Ha egy alkalmazás, amely egyezik a hello Alkalmazásazonosító hello webalkalmazás olvas be a titkos kulcsok hello tárolóból.</span><span class="sxs-lookup"><span data-stu-id="f2578-193">hello following scenario uses Azure functions, Azure logic apps, and key vault audit logs toocreate a pipeline toosend an email when an app that does match hello app ID of hello web app retrieves secrets from hello vault.</span></span>

<span data-ttu-id="f2578-194">Először engedélyeznie kell a kulcstartót bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="f2578-194">First, you must enable logging on your key vault.</span></span> <span data-ttu-id="f2578-195">Ez a következő PowerShell-parancsok hello keresztül végezhető (teljes részletei láthatók [kulcs-tároló-naplózás](key-vault-logging.md)):</span><span class="sxs-lookup"><span data-stu-id="f2578-195">This can be done via hello following PowerShell commands (full details can be seen at [key-vault-logging](key-vault-logging.md)):</span></span>

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>'
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

<span data-ttu-id="f2578-196">Miután engedélyezve van, az auditnaplókat a kijelölt tárfiókkal hello gyűjtése start.</span><span class="sxs-lookup"><span data-stu-id="f2578-196">After this is enabled, audit logs start collecting into hello designated storage account.</span></span> <span data-ttu-id="f2578-197">Ezek a naplók tartalmaz arról, hogyan és mikor érhetők el a kulcstárolót, és ki eseményeket.</span><span class="sxs-lookup"><span data-stu-id="f2578-197">These logs contain events about how and when your key vaults are accessed, and by whom.</span></span>

> [!NOTE]
> <span data-ttu-id="f2578-198">Elérheti a naplóinformációkat hello kulcstároló műveletet követően 10 percet.</span><span class="sxs-lookup"><span data-stu-id="f2578-198">You can access your logging information 10 minutes after hello key vault operation.</span></span> <span data-ttu-id="f2578-199">Általában lesz gyorsabb, mint ez.</span><span class="sxs-lookup"><span data-stu-id="f2578-199">It will usually be quicker than this.</span></span>
>
>

<span data-ttu-id="f2578-200">következő lépés hello túl van[hozzon létre egy Azure Service Bus-üzenetsorba](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span><span class="sxs-lookup"><span data-stu-id="f2578-200">hello next step is too[create an Azure Service Bus queue](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span></span> <span data-ttu-id="f2578-201">Ez azért, ahol a kulcstartót naplók leküldött vannak.</span><span class="sxs-lookup"><span data-stu-id="f2578-201">This is where key vault audit logs are pushed.</span></span> <span data-ttu-id="f2578-202">Ha hello naplózási üzenetek hello várólista, a hello logikai alkalmazás felveszi őket, és kezelje őket.</span><span class="sxs-lookup"><span data-stu-id="f2578-202">When hello audit log messages are in hello queue, hello logic app picks them up and acts on them.</span></span> <span data-ttu-id="f2578-203">Hozzon létre egy service bus hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="f2578-203">Create a service bus with hello following steps:</span></span>

1. <span data-ttu-id="f2578-204">Hozzon létre egy Service Bus-névtér (Ha már rendelkezik egy használni kívánt ehhez toouse kihagyása tooStep 2).</span><span class="sxs-lookup"><span data-stu-id="f2578-204">Create a Service Bus namespace (if you already have one that you want toouse for this, skip tooStep 2).</span></span>
2. <span data-ttu-id="f2578-205">Toohello a service bus hello Azure-portálon, és jelölje be hello névtér toocreate hello várólista a Tallózás gombra.</span><span class="sxs-lookup"><span data-stu-id="f2578-205">Browse toohello service bus in hello Azure portal and select hello namespace you want toocreate hello queue in.</span></span>
3. <span data-ttu-id="f2578-206">Válassza ki **új** válassza **Service Bus > várólista** és írja be a szükséges hello adatait.</span><span class="sxs-lookup"><span data-stu-id="f2578-206">Select **New** and choose **Service Bus > Queue** and enter hello required details.</span></span>
4. <span data-ttu-id="f2578-207">Hello névtér kiválasztásával, majd válassza ki a hello Service Bus kapcsolati információit **kapcsolatadatok**.</span><span class="sxs-lookup"><span data-stu-id="f2578-207">Select hello Service Bus connection information by choosing hello namespace and clicking **Connection Information**.</span></span> <span data-ttu-id="f2578-208">A következő szakaszban hello szüksége lesz ezt az információt.</span><span class="sxs-lookup"><span data-stu-id="f2578-208">You will need this information for hello next section.</span></span>

<span data-ttu-id="f2578-209">Ezt követően [egy Azure-függvény létrehozása](../azure-functions/functions-create-first-azure-function.md) toopoll kulcstároló naplóit belül hello tárfiók, és új események átvételéhez.</span><span class="sxs-lookup"><span data-stu-id="f2578-209">Next, [create an Azure function](../azure-functions/functions-create-first-azure-function.md) toopoll key vault logs within hello storage account and pick up new events.</span></span> <span data-ttu-id="f2578-210">Ez lesz az ütemezés szerint kiváltó függvényt.</span><span class="sxs-lookup"><span data-stu-id="f2578-210">This will be a function that is triggered on a schedule.</span></span>

<span data-ttu-id="f2578-211">Válasszon egy Azure függvény toocreate **új > függvény App** hello Azure-portálon a.</span><span class="sxs-lookup"><span data-stu-id="f2578-211">toocreate an Azure function, choose **New > Function App** in hello Azure portal.</span></span> <span data-ttu-id="f2578-212">A létrehozás során egy meglévő üzemeltetési terv használja, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="f2578-212">During creation, you can use an existing hosting plan or create a new one.</span></span> <span data-ttu-id="f2578-213">Sikerült is választhat dinamikus üzemeltetéséhez.</span><span class="sxs-lookup"><span data-stu-id="f2578-213">You could also opt for dynamic hosting.</span></span> <span data-ttu-id="f2578-214">További részleteket a beállításokat tartalmazó függvény található [hogyan tooscale Azure Functions](../azure-functions/functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="f2578-214">More details on Function hosting options can be found at [How tooscale Azure Functions](../azure-functions/functions-scale.md).</span></span>

<span data-ttu-id="f2578-215">Hello Azure függvény létrehozásakor nyissa meg a tooit, és válasszon egy számlálót függvény és C\#.</span><span class="sxs-lookup"><span data-stu-id="f2578-215">When hello Azure function is created, navigate tooit and choose a timer function and C\#.</span></span> <span data-ttu-id="f2578-216">Kattintson a **Ez a függvény létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="f2578-216">Then click **Create this function**.</span></span>

![Az Azure Functions lépések panelen](./media/keyvault-keyrotation/Azure_Functions_Start.png)

<span data-ttu-id="f2578-218">A hello **Develop** lapján hello következő hello run.csx kód cseréje:</span><span class="sxs-lookup"><span data-stu-id="f2578-218">On hello **Develop** tab, replace hello run.csx code with hello following:</span></span>

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
            log.Verbose($"Sync point file didnt have a date. Setting toonow.");
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

            //required tooorder by time as they may not be in hello file
            var results = ((IEnumerable<dynamic>) dynJson.records).OrderBy(p => p.time);

            foreach (var jsonItem in results)
            {
                DateTime dt = Convert.ToDateTime(jsonItem.time);

                if(dt>dtPrev){
                    log.Info($"{jsonItem.ToString()}");

                    var payloadStream = new MemoryStream(Encoding.UTF8.GetBytes(jsonItem.ToString()));
                    //When sending tooServiceBus, use hello payloadStream and set keeporiginal tootrue
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

    //Generate hello shared access signature on hello container, setting hello constraints directly on hello signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return hello URI string for hello container, including hello SAS token.
    return blob.Uri + sasBlobToken;
}
```


> [!NOTE]
> <span data-ttu-id="f2578-219">Győződjön meg arról, hogy tooreplace hello változók hello megelőző kód toopoint tooyour tárfiókot, ahol hello kulcstároló naplóit íródtak, korábban létrehozott hello a service bus, és a megadott elérési út toohello kulcstároló tárolási naplóit hello.</span><span class="sxs-lookup"><span data-stu-id="f2578-219">Make sure tooreplace hello variables in hello preceding code toopoint tooyour storage account where hello key vault logs are written, hello service bus you created earlier, and hello specific path toohello key vault storage logs.</span></span>
>
>

<span data-ttu-id="f2578-220">hello funkció szerzi be hello legújabb naplófájl hello tárfiókból ahol hello kulcstároló írja a naplókat, Markoló hello legújabb események a fájl, és a Service Bus-üzenetsorba tooa leküldéses értesítések.</span><span class="sxs-lookup"><span data-stu-id="f2578-220">hello function picks up hello latest log file from hello storage account where hello key vault logs are written, grabs hello latest events from that file, and pushes them tooa Service Bus queue.</span></span> <span data-ttu-id="f2578-221">Mivel egyetlen fájl rendelkezhet több esemény, készítsen hello függvény is ellenőrzi, hogy az utolsó esemény hello mentése kivételezett toodetermine hello időbélyegzőjét sync.txt fájlt.</span><span class="sxs-lookup"><span data-stu-id="f2578-221">Since a single file could have multiple events, you should create a sync.txt file that hello function also looks at toodetermine hello time stamp of hello last event that was picked up.</span></span> <span data-ttu-id="f2578-222">Ez biztosítja, hogy nem leküldéses hello ugyanarra az eseményre több alkalommal.</span><span class="sxs-lookup"><span data-stu-id="f2578-222">This ensures that you don't push hello same event multiple times.</span></span> <span data-ttu-id="f2578-223">A sync.txt fájl tartalmaz egy Timestamp típusú hello utolsó észlelt esemény.</span><span class="sxs-lookup"><span data-stu-id="f2578-223">This sync.txt file contains a timestamp for hello last encountered event.</span></span> <span data-ttu-id="f2578-224">hello naplókat, ha be van töltve, van rendezve toobe hello időbélyeg tooensure megfelelő sorrendben alapján.</span><span class="sxs-lookup"><span data-stu-id="f2578-224">hello logs, when loaded, have toobe sorted based on hello timestamp tooensure they are ordered correctly.</span></span>

<span data-ttu-id="f2578-225">Ennél a függvénynél néhány további szalagtár szerepel, amely nem használható az Azure Functions hello mezőben kívüli jelenleg hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="f2578-225">For this function, we reference a couple of additional libraries that are not available out of hello box in Azure Functions.</span></span> <span data-ttu-id="f2578-226">tooinclude ezeket, igazolnia kell, hogy az Azure Functions toopull őket NuGet segítségével.</span><span class="sxs-lookup"><span data-stu-id="f2578-226">tooinclude these, we need Azure Functions toopull them using NuGet.</span></span> <span data-ttu-id="f2578-227">Válassza ki a hello **fájlok megtekintése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="f2578-227">Choose hello **View Files** option.</span></span>

![Fájlok beállítás megtekintése](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

<span data-ttu-id="f2578-229">És adja hozzá a következő tartalommal project.json nevű fájlba:</span><span class="sxs-lookup"><span data-stu-id="f2578-229">And add a file called project.json with following content:</span></span>

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
<span data-ttu-id="f2578-230">Akkor **mentése**, az Azure Functions hello szükséges bináris fájlokat tölti le.</span><span class="sxs-lookup"><span data-stu-id="f2578-230">Upon **Save**, Azure Functions will download hello required binaries.</span></span>

<span data-ttu-id="f2578-231">Váltás toohello **integráció** lapon és hello időzítő paraméter adjon meg egy kifejező nevet toouse hello függvényen belül.</span><span class="sxs-lookup"><span data-stu-id="f2578-231">Switch toohello **Integrate** tab and give hello timer parameter a meaningful name toouse within hello function.</span></span> <span data-ttu-id="f2578-232">A kód megelőző hello, hello időzítő toobe nevű vár *myTimer*.</span><span class="sxs-lookup"><span data-stu-id="f2578-232">In hello preceding code, it expects hello timer toobe called *myTimer*.</span></span> <span data-ttu-id="f2578-233">Adjon meg egy [CRON-kifejezés](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) az alábbiak szerint: 0 \* \* \* \* \* hello időzítő, amely újraindítja a hello függvény toorun percenként egyszer.</span><span class="sxs-lookup"><span data-stu-id="f2578-233">Specify a [CRON expression](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) as follows: 0 \* \* \* \* \* for hello timer that will cause hello function toorun once a minute.</span></span>

<span data-ttu-id="f2578-234">A hello azonos **integráció** lapon maradva adja hozzá a hello típusú bemeneti **Azure Blob Storage**.</span><span class="sxs-lookup"><span data-stu-id="f2578-234">On hello same **Integrate** tab, add an input of hello type **Azure Blob Storage**.</span></span> <span data-ttu-id="f2578-235">Ez fog mutatni toohello sync.txt hello tekintett meg hello függvény utolsó esemény időbélyegzője hello tartalmazó fájlt.</span><span class="sxs-lookup"><span data-stu-id="f2578-235">This will point toohello sync.txt file that contains hello timestamp of hello last event looked at by hello function.</span></span> <span data-ttu-id="f2578-236">Ez lesz hello paraméter neve hello függvényen belül használható.</span><span class="sxs-lookup"><span data-stu-id="f2578-236">This will be available within hello function by hello parameter name.</span></span> <span data-ttu-id="f2578-237">A kód megelőző hello, hello Azure Blob Storage bemenetet vár hello paraméter neve toobe *inputBlob*.</span><span class="sxs-lookup"><span data-stu-id="f2578-237">In hello preceding code, hello Azure Blob Storage input expects hello parameter name toobe *inputBlob*.</span></span> <span data-ttu-id="f2578-238">A tárfiók hello hello sync.txt fájl tároló választható (lehet hello ugyanabban vagy egy másik tárolási fiókot).</span><span class="sxs-lookup"><span data-stu-id="f2578-238">Choose hello storage account where hello sync.txt file will reside (it could be hello same or a different storage account).</span></span> <span data-ttu-id="f2578-239">Hello elérési út mezőbe adjon meg a hello elérési utat, ahol él, hello fájl hello formátum {container-name}/path/to/sync.txt.</span><span class="sxs-lookup"><span data-stu-id="f2578-239">In hello path field, provide hello path where hello file lives in hello format {container-name}/path/to/sync.txt.</span></span>

<span data-ttu-id="f2578-240">Adja hozzá egy kimeneti hello típusú *Azure Blob Storage* kimeneti.</span><span class="sxs-lookup"><span data-stu-id="f2578-240">Add an output of hello type *Azure Blob Storage* output.</span></span> <span data-ttu-id="f2578-241">Ez fog mutatni hello bemeneti megadott toohello sync.txt fájlt.</span><span class="sxs-lookup"><span data-stu-id="f2578-241">This will point toohello sync.txt file you defined in hello input.</span></span> <span data-ttu-id="f2578-242">Ez használható az hello függvény toowrite hello hello utolsó esemény időbélyegzője tekintett meg.</span><span class="sxs-lookup"><span data-stu-id="f2578-242">This is used by hello function toowrite hello timestamp of hello last event looked at.</span></span> <span data-ttu-id="f2578-243">hello előző kód várja a paraméter toobe nevű *outputBlob*.</span><span class="sxs-lookup"><span data-stu-id="f2578-243">hello preceding code expects this parameter toobe called *outputBlob*.</span></span>

<span data-ttu-id="f2578-244">Ezen a ponton hello függvény készen áll.</span><span class="sxs-lookup"><span data-stu-id="f2578-244">At this point, hello function is ready.</span></span> <span data-ttu-id="f2578-245">Győződjön meg arról, hogy tooswitch hátsó toohello **Develop** lapra, és mentse a hello kódot.</span><span class="sxs-lookup"><span data-stu-id="f2578-245">Make sure tooswitch back toohello **Develop** tab and save hello code.</span></span> <span data-ttu-id="f2578-246">Hello kimeneti ablakban a fordítási hibákat, és ennek megfelelően javítsa ki azokat.</span><span class="sxs-lookup"><span data-stu-id="f2578-246">Check hello output window for any compilation errors and correct them accordingly.</span></span> <span data-ttu-id="f2578-247">Ha hello kód lefordításához, majd hello kódot kell most kell ellenőrzése hello kulcstároló naplóit percenként és bármely új események alakzatot hello küldését definiált Service Bus-üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="f2578-247">If hello code compiles, then hello code should now be checking hello key vault logs every minute and pushing any new events onto hello defined Service Bus queue.</span></span> <span data-ttu-id="f2578-248">Minden indításakor lefutnak hello függvény toohello napló ablakban kiírni naplózási információkat kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="f2578-248">You should see logging information write out toohello log window every time hello function is triggered.</span></span>

### <a name="azure-logic-app"></a><span data-ttu-id="f2578-249">Az Azure Logic Apps alkalmazást</span><span class="sxs-lookup"><span data-stu-id="f2578-249">Azure logic app</span></span>
<span data-ttu-id="f2578-250">Ezután létre kell hoznia egy Azure logikai alkalmazás, amely szerzi be, hogy hello függvény küldését toohello Service Bus-üzenetsorba, hello tartalom elemzi és elküld egy e-mailt az egyező feltétel alapján hello események.</span><span class="sxs-lookup"><span data-stu-id="f2578-250">Next you must create an Azure logic app that picks up hello events that hello function is pushing toohello Service Bus queue, parses hello content, and sends an email based on a condition being matched.</span></span>

<span data-ttu-id="f2578-251">[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md) címen túl**új > logikai alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f2578-251">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md) by going too**New > Logic App**.</span></span>

<span data-ttu-id="f2578-252">Hello logikai alkalmazás létrehozása után nyissa meg a tooit, és válassza a **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="f2578-252">Once hello logic app is created, navigate tooit and choose **edit**.</span></span> <span data-ttu-id="f2578-253">Hello logic app szerkesztő választható **Service Bus-üzenetsorba** , és írja be a Service Bus-hitelesítő adatok tooconnect azt toohello várólista.</span><span class="sxs-lookup"><span data-stu-id="f2578-253">Within hello logic app editor, choose **Service Bus Queue** and enter your Service Bus credentials tooconnect it toohello queue.</span></span>

![Az Azure Logic App Service Bus](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

<span data-ttu-id="f2578-255">Ezután válasszon **feltétel hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="f2578-255">Next choose **Add a condition**.</span></span> <span data-ttu-id="f2578-256">Hello állapotban speciális szerkesztő toohello váltson, és adja meg a következő kódot, APP_ID lecserélését hello hello webalkalmazás tényleges APP_ID:</span><span class="sxs-lookup"><span data-stu-id="f2578-256">In hello condition, switch toohello advanced editor and enter hello following code, replacing APP_ID with hello actual APP_ID of your web app:</span></span>

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

<span data-ttu-id="f2578-257">Ebben a kifejezésben lényegében adja vissza **hamis** Ha hello *appid* hello a beérkező eseményhez (amely hello Service Bus üzenet törzsét hello) nem hello *appid* a hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f2578-257">This expression essentially returns **false** if hello *appid* from hello incoming event (which is hello body of hello Service Bus message) is not hello *appid* of hello app.</span></span>

<span data-ttu-id="f2578-258">Ezután hozzon létre egy műveletet a **Ha nem, nem történik semmi**.</span><span class="sxs-lookup"><span data-stu-id="f2578-258">Now, create an action under **If no, do nothing**.</span></span>

![Az Azure Logic App művelet kiválasztását.](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

<span data-ttu-id="f2578-260">Hello a művelethez, válassza ki a **Office 365 – e-mail küldése**.</span><span class="sxs-lookup"><span data-stu-id="f2578-260">For hello action, choose **Office 365 - send email**.</span></span> <span data-ttu-id="f2578-261">Töltse ki hello mezők toocreate egy e-mailek toosend hello meghatározva feltétel beolvasása **hamis**.</span><span class="sxs-lookup"><span data-stu-id="f2578-261">Fill out hello fields toocreate an email toosend when hello defined condition returns **false**.</span></span> <span data-ttu-id="f2578-262">Ha nem rendelkezik Office 365, sikerült tekinti meg alternatív tooachieve hello ugyanazokat az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="f2578-262">If you do not have Office 365, you could look at alternatives tooachieve hello same results.</span></span>

<span data-ttu-id="f2578-263">Ezen a ponton rendelkezik egy záró tooend-feldolgozási folyamat percenként egyszer új kulcstartó naplók keresi.</span><span class="sxs-lookup"><span data-stu-id="f2578-263">At this point, you have an end tooend pipeline that looks for new key vault audit logs once a minute.</span></span> <span data-ttu-id="f2578-264">Ez a leküldéses értesítések tooa service bus-üzenetsorba megtalálja az új naplók.</span><span class="sxs-lookup"><span data-stu-id="f2578-264">It pushes new logs it finds tooa service bus queue.</span></span> <span data-ttu-id="f2578-265">hello logikai alkalmazás lesz kiváltva, ha egy új üzenet fájljai a hello várólista.</span><span class="sxs-lookup"><span data-stu-id="f2578-265">hello logic app is triggered when a new message lands in hello queue.</span></span> <span data-ttu-id="f2578-266">Ha hello *appid* belül hello esemény nem egyezik meg az alkalmazás hívása hello hello Alkalmazásazonosító, egy e-mailt küld.</span><span class="sxs-lookup"><span data-stu-id="f2578-266">If hello *appid* within hello event does not match hello app ID of hello calling application, it sends an email.</span></span>
