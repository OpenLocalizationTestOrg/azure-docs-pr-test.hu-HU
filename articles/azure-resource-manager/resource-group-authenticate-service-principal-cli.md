---
title: "az Azure CLI Azure alkalmazás aaaCreate identitást |} Microsoft Docs"
description: "Ismerteti, hogyan szabályozza a toouse Azure CLI toocreate egy Azure Active Directory-alkalmazás és szolgáltatás egyszerű, és azt keresztül férnek hozzá tooresources szerepkörön alapuló hozzáférés biztosítása. Azt illusztrálja, hogyan tooauthenticate alkalmazás jelszóval vagy tanúsítvánnyal."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: c224a189-dd28-4801-b3e3-26991b0eb24d
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: 0d693ec801d4f4d6c24ec420580776e73014b325
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-cli-toocreate-a-service-principal-tooaccess-resources"></a><span data-ttu-id="e1ad0-104">A szolgáltatás egyszerű tooaccess erőforrásainak használatához az Azure CLI toocreate</span><span class="sxs-lookup"><span data-stu-id="e1ad0-104">Use Azure CLI toocreate a service principal tooaccess resources</span></span>

<span data-ttu-id="e1ad0-105">Ha egy alkalmazás vagy parancsfájlt, amelyet a tooaccess erőforrások, hello alkalmazás identitás beállítása, és hitelesíteni hello alkalmazást a saját hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-105">When you have an app or script that needs tooaccess resources, you can set up an identity for hello app and authenticate hello app with its own credentials.</span></span> <span data-ttu-id="e1ad0-106">Ezzel az identitással egyszerű szolgáltatás néven ismert.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-106">This identity is known as a service principal.</span></span> <span data-ttu-id="e1ad0-107">Ez a megközelítés lehetővé teszi:</span><span class="sxs-lookup"><span data-stu-id="e1ad0-107">This approach enables you to:</span></span>

* <span data-ttu-id="e1ad0-108">Engedélyek hozzárendelése saját engedélyek eltérő toohello identitását.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-108">Assign permissions toohello app identity that are different than your own permissions.</span></span> <span data-ttu-id="e1ad0-109">Ezek az engedélyek általában korlátozott tooexactly milyen hello alkalmazásnak kell toodo.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-109">Typically, these permissions are restricted tooexactly what hello app needs toodo.</span></span>
* <span data-ttu-id="e1ad0-110">A tanúsítvány használata a hitelesítéshez, egy felügyelet nélküli parancsfájl végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-110">Use a certificate for authentication when executing an unattended script.</span></span>

<span data-ttu-id="e1ad0-111">Ez a cikk bemutatja, hogyan toouse [Azure CLI 1.0](../cli-install-nodejs.md) tooset be az alkalmazás toorun a saját hitelesítő adatait és identitás.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-111">This article shows you how toouse [Azure CLI 1.0](../cli-install-nodejs.md) tooset up an application toorun under its own credentials and identity.</span></span> <span data-ttu-id="e1ad0-112">Telepítse a legújabb változatát hello [Azure CLI 1.0](../cli-install-nodejs.md) meg arról, hogy a környezet megfelel a cikkben szereplő példák hello toomake.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-112">Install hello latest version of [Azure CLI 1.0](../cli-install-nodejs.md) toomake sure your environment matches hello examples in this article.</span></span>

## <a name="required-permissions"></a><span data-ttu-id="e1ad0-113">Szükséges engedélyek</span><span class="sxs-lookup"><span data-stu-id="e1ad0-113">Required permissions</span></span>
<span data-ttu-id="e1ad0-114">toocomplete Ez a témakör megfelelő engedélyekkel kell rendelkeznie az Azure Active Directory és az Azure-előfizetésében is.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-114">toocomplete this topic, you must have sufficient permissions in both your Azure Active Directory and your Azure subscription.</span></span> <span data-ttu-id="e1ad0-115">Pontosabban tudja toocreate hello Azure Active Directory az alkalmazásban, és hello szolgáltatás egyszerű tooa szerepkör hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-115">Specifically, you must be able toocreate an app in hello Azure Active Directory, and assign hello service principal tooa role.</span></span> 

<span data-ttu-id="e1ad0-116">hello legegyszerűbb módja toocheck hello portálon keresztül van a fiók rendelkezik-e megfelelő engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-116">hello easiest way toocheck whether your account has adequate permissions is through hello portal.</span></span> <span data-ttu-id="e1ad0-117">Lásd [Szükséges jogosultságok ellenőrzése a portálon](resource-group-create-service-principal-portal.md#required-permissions).</span><span class="sxs-lookup"><span data-stu-id="e1ad0-117">See [Check required permission in portal](resource-group-create-service-principal-portal.md#required-permissions).</span></span>

<span data-ttu-id="e1ad0-118">A következő lépésben tooa szakasz bármelyik [jelszó](#create-service-principal-with-password) vagy [tanúsítvány](#create-service-principal-with-certificate) hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-118">Now, proceed tooa section for either [password](#create-service-principal-with-password) or [certificate](#create-service-principal-with-certificate) authentication.</span></span>

## <a name="create-service-principal-with-password"></a><span data-ttu-id="e1ad0-119">Egyszerű szolgáltatásnév létrehozása jelszóval</span><span class="sxs-lookup"><span data-stu-id="e1ad0-119">Create service principal with password</span></span>
<span data-ttu-id="e1ad0-120">Ebben a szakaszban hajtsa végre a hello lépéseket toocreate hello jelszóval AD-alkalmazást, és rendelje hozzá a hello olvasó szerepkört toohello egyszerű.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-120">In this section, you perform hello steps toocreate hello AD application with a password, and assign hello Reader role toohello service principal.</span></span>

1. <span data-ttu-id="e1ad0-121">Jelentkezzen be tooyour fiókjával.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-121">Sign in tooyour account.</span></span>
   
   ```azurecli
   azure login
   ```
2. <span data-ttu-id="e1ad0-122">toocreate app identitást, hello alkalmazás hello nevét és jelszavát, adja meg, ahogy az a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="e1ad0-122">toocreate an app identity, provide hello name of hello app and a password, as shown in hello following command:</span></span>
     
   ```azurecli
   azure ad sp create -n exampleapp -p {your-password}
   ```
     
   <span data-ttu-id="e1ad0-123">hello új egyszerű szolgáltatásnév adja vissza.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-123">hello new service principal is returned.</span></span> <span data-ttu-id="e1ad0-124">Az objektumazonosító hello van szükség, ha a jogosultságokat.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-124">hello Object Id is needed when granting permissions.</span></span> <span data-ttu-id="e1ad0-125">az egyszerű szolgáltatásnevek hello felsorolt hello guid van szükség, ha van bejelentkezve.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-125">hello guid listed with hello Service Principal Names is needed when logging in.</span></span> <span data-ttu-id="e1ad0-126">Ez a guid egy hello hello app ID ugyanazt az értéket. A hello alkalmazásokat, az értéket nem hivatkozott tooas hello `Client ID`.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-126">This guid is hello same value as hello app id. In hello sample applications, this value is referred tooas hello `Client ID`.</span></span> 
     
   ```azurecli
   info:    Executing command ad sp create
     
   Creating application exampleapp
     / Creating service principal for application 7132aca4-1bdb-4238-ad81-996ff91d8db+
     data:    Object Id:               ff863613-e5e2-4a6b-af07-fff6f2de3f4e
     data:    Display Name:            exampleapp
     data:    Service Principal Names:
     data:                             7132aca4-1bdb-4238-ad81-996ff91d8db4
     data:                             https://www.contoso.org/example
     info:    ad sp create command OK
   ```

3. <span data-ttu-id="e1ad0-127">Adja meg a hello szolgáltatás egyszerű engedélyeket az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-127">Grant hello service principal permissions on your subscription.</span></span> <span data-ttu-id="e1ad0-128">A jelen példában adja hozzá hello szolgáltatás egyszerű toohello olvasó szerepkört, amely ad engedélyt tooread hello előfizetésben található összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-128">In this example, you add hello service principal toohello Reader role, which grants permission tooread all resources in hello subscription.</span></span> <span data-ttu-id="e1ad0-129">Más szerepköreivel kapcsolatban, tekintse meg a [RBAC: beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="e1ad0-129">For other roles, see [RBAC: Built-in roles](../active-directory/role-based-access-built-in-roles.md).</span></span> <span data-ttu-id="e1ad0-130">Hello objectid paraméter adja meg, amely hello alkalmazás létrehozásakor használt objektumazonosító hello.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-130">For hello objectid parameter, provide hello Object Id that you used when creating hello application.</span></span> <span data-ttu-id="e1ad0-131">A parancs futtatása előtt engedélyeznie kell a némi időbe, hello új szolgáltatás egyszerű toopropagate egész Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-131">Before running this command, you must allow some time for hello new service principal toopropagate throughout Azure Active Directory.</span></span> <span data-ttu-id="e1ad0-132">Ha manuálisan futtassa a következő parancsokat, általában elegendő idő eltelte feladatok között.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-132">When you run these commands manually, usually enough time has elapsed between tasks.</span></span> <span data-ttu-id="e1ad0-133">Egy parancsfájlban, hozzá kell adnia egy lépés toosleep hello parancsok között (például `sleep 15`).</span><span class="sxs-lookup"><span data-stu-id="e1ad0-133">In a script, you should add a step toosleep between hello commands (like `sleep 15`).</span></span> <span data-ttu-id="e1ad0-134">Ha egy hiba azzal hello egyszerű hello könyvtárban nem létezik, akkor futtassa újra a hello parancsot.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-134">If you see an error stating hello principal does not exist in hello directory, rerun hello command.</span></span>
   
   ```azurecli
   azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/
   ```
   
<span data-ttu-id="e1ad0-135">Ennyi az egész!</span><span class="sxs-lookup"><span data-stu-id="e1ad0-135">That's it!</span></span> <span data-ttu-id="e1ad0-136">Az AD-alkalmazást és egy egyszerű szolgáltatást be vannak állítva.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-136">Your AD application and service principal are set up.</span></span> <span data-ttu-id="e1ad0-137">hello következő szakasz bemutatja, hogyan toolog hello be a hitelesítő adatok Azure parancssori felületen keresztül.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-137">hello next section shows you how toolog in with hello credential through Azure CLI.</span></span> <span data-ttu-id="e1ad0-138">Ha toouse hello hitelesítő kódot alkalmazásában, nem kell toocontinue az ebben a témakörben.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-138">If you want toouse hello credential in your code application, you do not need toocontinue with this topic.</span></span> <span data-ttu-id="e1ad0-139">Toohello is ugorhat [mintaalkalmazást](#sample-applications) példák a jelentkezhetnek be az alkalmazás azonosítóját és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-139">You can jump toohello [Sample applications](#sample-applications) for examples of logging in with your application id and password.</span></span> 

### <a name="provide-credentials-through-azure-cli"></a><span data-ttu-id="e1ad0-140">Adjon meg hitelesítő adatokat az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="e1ad0-140">Provide credentials through Azure CLI</span></span>
<span data-ttu-id="e1ad0-141">Most van szüksége a toolog hello alkalmazás tooperform műveletként.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-141">Now, you need toolog in as hello application tooperform operations.</span></span>

1. <span data-ttu-id="e1ad0-142">Amikor jelentkezik be, és egy egyszerű szolgáltatást, az AD-alkalmazás kell tooprovide hello bérlőazonosító hello könyvtár.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-142">Whenever you sign in as a service principal, you need tooprovide hello tenant id of hello directory for your AD app.</span></span> <span data-ttu-id="e1ad0-143">A bérlő az Azure Active Directory példánya.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-143">A tenant is an instance of Azure Active Directory.</span></span> <span data-ttu-id="e1ad0-144">hello-bérlőazonosító tooretrieve jelenleg hitelesített előfizetési, használja:</span><span class="sxs-lookup"><span data-stu-id="e1ad0-144">tooretrieve hello tenant id for your currently authenticated subscription, use:</span></span>
   
   ```azurecli
   azure account show
   ```
   
   <span data-ttu-id="e1ad0-145">Amely adja vissza:</span><span class="sxs-lookup"><span data-stu-id="e1ad0-145">Which returns:</span></span>
   
   ```azurecli
   info:    Executing command account show
   data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
   data:    ID                          : {guid}
   data:    State                       : Enabled
   data:    Tenant ID                   : {guid}
   data:    Is Default                  : true
   ...
   ```
   
     <span data-ttu-id="e1ad0-146">Tooget hello bérlői azonosító egy másik előfizetés szükséges, ha használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="e1ad0-146">If you need tooget hello tenant id of another subscription, use hello following command:</span></span>
   
   ```azurecli
   azure account show -s {subscription-id}
   ```
2. <span data-ttu-id="e1ad0-147">Ha naplózás tooretrieve hello ügyfél azonosító toouse van szüksége, használja:</span><span class="sxs-lookup"><span data-stu-id="e1ad0-147">If you need tooretrieve hello client id toouse for logging in, use:</span></span>
   
   ```azurecli
   azure ad sp show -c exampleapp --json
   ```
   
     <span data-ttu-id="e1ad0-148">hello érték toouse való bejelentkezéskor hello egyszerű szolgáltatásnevek felsorolt hello guid.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-148">hello value toouse for logging in is hello guid listed in hello service principal names.</span></span>
   
   ```azurecli
   [
     {
       "objectId": "ff863613-e5e2-4a6b-af07-fff6f2de3f4e",
       "objectType": "ServicePrincipal",
       "displayName": "exampleapp",
       "appId": "7132aca4-1bdb-4238-ad81-996ff91d8db4",
       "servicePrincipalNames": [
         "https://www.contoso.org/example",
         "7132aca4-1bdb-4238-ad81-996ff91d8db4"
       ]
     }
   ]
   ```
3. <span data-ttu-id="e1ad0-149">Jelentkezzen be egyszerű hello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-149">Log in as hello service principal.</span></span>
   
   ```azurecli
   azure login -u 7132aca4-1bdb-4238-ad81-996ff91d8db4 --service-principal --tenant {tenant-id}
   ```
   
    <span data-ttu-id="e1ad0-150">Hello jelszó megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-150">You are prompted for hello password.</span></span> <span data-ttu-id="e1ad0-151">Hello AD-alkalmazás létrehozásakor megadott hello jelszót megadnia.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-151">Provide hello password you specified when creating hello AD application.</span></span>
   
   ```azurecli
   info:    Executing command login
   Password: ********
   ```

<span data-ttu-id="e1ad0-152">Most már hitelesítve mint hello szolgáltatás egyszerű létrehozott egyszerű hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-152">You are now authenticated as hello service principal for hello service principal that you created.</span></span>

<span data-ttu-id="e1ad0-153">Azt is megteheti hívhat meg hello parancssori toolog a többi műveletek.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-153">Alternatively, you can invoke REST operations from hello command line toolog in.</span></span> <span data-ttu-id="e1ad0-154">Hello hitelesítési választ, a hozzáférési jogkivonat hello használható egyéb műveletek kérheti le.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-154">From hello authentication response, you can retrieve hello access token for use with other operations.</span></span> <span data-ttu-id="e1ad0-155">Például egy hello hozzáférési jogkivonat beolvasása REST műveleteinek figyelőn, lásd: [generálása egy hozzáférési jogkivonat](resource-manager-rest-api.md#generating-an-access-token).</span><span class="sxs-lookup"><span data-stu-id="e1ad0-155">For an example of retrieving hello access token by invoking REST operations, see [Generating an Access Token](resource-manager-rest-api.md#generating-an-access-token).</span></span>

## <a name="create-service-principal-with-certificate"></a><span data-ttu-id="e1ad0-156">A tanúsítvány egyszerű szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e1ad0-156">Create service principal with certificate</span></span>
<span data-ttu-id="e1ad0-157">Ebben a szakaszban hello lépéseket hajtsa végre:</span><span class="sxs-lookup"><span data-stu-id="e1ad0-157">In this section, you perform hello steps to:</span></span>

* <span data-ttu-id="e1ad0-158">Önaláírt tanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="e1ad0-158">create a self-signed certificate</span></span>
* <span data-ttu-id="e1ad0-159">hello tanúsítványt, és egy egyszerű hello szolgáltatást hello AD-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e1ad0-159">create hello AD application with hello certificate, and hello service principal</span></span>
* <span data-ttu-id="e1ad0-160">hello olvasó szerepkört toohello egyszerű hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="e1ad0-160">assign hello Reader role toohello service principal</span></span>

<span data-ttu-id="e1ad0-161">Ezek a lépések toocomplete, rendelkeznie kell [OpenSSL](http://www.openssl.org/) telepítve.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-161">toocomplete these steps, you must have [OpenSSL](http://www.openssl.org/) installed.</span></span>

1. <span data-ttu-id="e1ad0-162">Hozzon létre egy önaláírt tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-162">Create a self-signed certificate.</span></span>
   
   ```
   openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'
   ```

2. <span data-ttu-id="e1ad0-163">előző lépésben létrehozott két fájlt - privkey.pem és cert.pem hello.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-163">hello preceding step created two files - privkey.pem and cert.pem.</span></span> <span data-ttu-id="e1ad0-164">Hello nyilvános és titkos kulcsok egyesítése egyetlen fájlt.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-164">Combine hello public and private keys into a single file.</span></span>

   ```
   cat privkey.pem cert.pem > examplecert.pem
   ```

3. <span data-ttu-id="e1ad0-165">Nyissa meg hello **examplecert.pem** fájlt, és keressen hello hosszú karaktersorozatot, mely közötti **---BEGIN CERTIFICATE---** és **---vége tanúsítvány---**.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-165">Open hello **examplecert.pem** file and look for hello long sequence of characters between **-----BEGIN CERTIFICATE-----** and **-----END CERTIFICATE-----**.</span></span> <span data-ttu-id="e1ad0-166">Másolja a hello tanúsítványának adatait.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-166">Copy hello certificate data.</span></span> <span data-ttu-id="e1ad0-167">Ezek az adatok továbbítsa paraméterként hello egyszerű szolgáltatás létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-167">You pass this data as a parameter when creating hello service principal.</span></span>

4. <span data-ttu-id="e1ad0-168">Jelentkezzen be tooyour fiókjával.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-168">Sign in tooyour account.</span></span>

   ```azurecli
   azure login
   ```
5. <span data-ttu-id="e1ad0-169">toocreate hello szolgáltatás egyszerű adja hello hello app és hello Tanúsítványadatok, ahogy az a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="e1ad0-169">toocreate hello service principal, provide hello name of hello app and hello certificate data, as shown in hello following command:</span></span>
     
   ```azurecli
   azure ad sp create -n exampleapp --cert-value {certificate data}
   ```
     
   <span data-ttu-id="e1ad0-170">hello új egyszerű szolgáltatásnév adja vissza.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-170">hello new service principal is returned.</span></span> <span data-ttu-id="e1ad0-171">Az objektumazonosító hello van szükség, ha a jogosultságokat.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-171">hello Object Id is needed when granting permissions.</span></span> <span data-ttu-id="e1ad0-172">az egyszerű szolgáltatásnevek hello felsorolt hello guid van szükség, ha van bejelentkezve.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-172">hello guid listed with hello Service Principal Names is needed when logging in.</span></span> <span data-ttu-id="e1ad0-173">Ez a guid egy hello hello app ID ugyanazt az értéket. A hello alkalmazásokat az értéket nem hivatkozott tooas hello ügyfél-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-173">This guid is hello same value as hello app id. In hello sample applications, this value is referred tooas hello Client ID.</span></span> 
     
   ```azurecli
   info:    Executing command ad sp create
     
   Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
     data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
     data:    Display Name:     exampleapp
     data:    Service Principal Names:
     data:                      4fd39843-c338-417d-b549-a545f584a745
     data:                      https://www.contoso.org/example
     info:    ad sp create command OK
   ```
6. <span data-ttu-id="e1ad0-174">Adja meg a hello szolgáltatás egyszerű engedélyeket az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-174">Grant hello service principal permissions on your subscription.</span></span> <span data-ttu-id="e1ad0-175">A jelen példában adja hozzá hello szolgáltatás egyszerű toohello olvasó szerepkört, amely ad engedélyt tooread hello előfizetésben található összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-175">In this example, you add hello service principal toohello Reader role, which grants permission tooread all resources in hello subscription.</span></span> <span data-ttu-id="e1ad0-176">Más szerepköreivel kapcsolatban, tekintse meg a [RBAC: beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="e1ad0-176">For other roles, see [RBAC: Built-in roles](../active-directory/role-based-access-built-in-roles.md).</span></span> <span data-ttu-id="e1ad0-177">Hello objectid paraméter adja meg, amely hello alkalmazás létrehozásakor használt objektumazonosító hello.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-177">For hello objectid parameter, provide hello Object Id that you used when creating hello application.</span></span> <span data-ttu-id="e1ad0-178">A parancs futtatása előtt engedélyeznie kell a némi időbe, hello új szolgáltatás egyszerű toopropagate egész Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-178">Before running this command, you must allow some time for hello new service principal toopropagate throughout Azure Active Directory.</span></span> <span data-ttu-id="e1ad0-179">Ha manuálisan futtassa a következő parancsokat, általában elegendő idő eltelte feladatok között.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-179">When you run these commands manually, usually enough time has elapsed between tasks.</span></span> <span data-ttu-id="e1ad0-180">Egy parancsfájlban, hozzá kell adnia egy lépés toosleep hello parancsok között (például `sleep 15`).</span><span class="sxs-lookup"><span data-stu-id="e1ad0-180">In a script, you should add a step toosleep between hello commands (like `sleep 15`).</span></span> <span data-ttu-id="e1ad0-181">Ha egy hiba azzal hello egyszerű hello könyvtárban nem létezik, akkor futtassa újra a hello parancsot.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-181">If you see an error stating hello principal does not exist in hello directory, rerun hello command.</span></span>
   
   ```azurecli
   azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/
   ```
  
### <a name="provide-certificate-through-automated-azure-cli-script"></a><span data-ttu-id="e1ad0-182">Adja meg a tanúsítvány automatikus Azure CLI-parancsfájl használatával</span><span class="sxs-lookup"><span data-stu-id="e1ad0-182">Provide certificate through automated Azure CLI script</span></span>
<span data-ttu-id="e1ad0-183">Most van szüksége a toolog hello alkalmazás tooperform műveletként.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-183">Now, you need toolog in as hello application tooperform operations.</span></span>

1. <span data-ttu-id="e1ad0-184">Amikor jelentkezik be, és egy egyszerű szolgáltatást, az AD-alkalmazás kell tooprovide hello bérlőazonosító hello könyvtár.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-184">Whenever you sign in as a service principal, you need tooprovide hello tenant id of hello directory for your AD app.</span></span> <span data-ttu-id="e1ad0-185">A bérlő az Azure Active Directory példánya.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-185">A tenant is an instance of Azure Active Directory.</span></span> <span data-ttu-id="e1ad0-186">hello-bérlőazonosító tooretrieve jelenleg hitelesített előfizetési, használja:</span><span class="sxs-lookup"><span data-stu-id="e1ad0-186">tooretrieve hello tenant id for your currently authenticated subscription, use:</span></span>
   
   ```azurecli
   azure account show
   ```
   
   <span data-ttu-id="e1ad0-187">Amely adja vissza:</span><span class="sxs-lookup"><span data-stu-id="e1ad0-187">Which returns:</span></span>
   
   ```azurecli
   info:    Executing command account show
   data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
   data:    ID                          : {guid}
   data:    State                       : Enabled
   data:    Tenant ID                   : {guid}
   data:    Is Default                  : true
   ...
   ```
   
   <span data-ttu-id="e1ad0-188">Tooget hello bérlői azonosító egy másik előfizetés szükséges, ha használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="e1ad0-188">If you need tooget hello tenant id of another subscription, use hello following command:</span></span>
   
   ```azurecli
   azure account show -s {subscription-id}
   ```
2. <span data-ttu-id="e1ad0-189">tooretrieve hello tanúsítvány ujjlenyomatát, és távolítsa el a felesleges karaktereket, használja:</span><span class="sxs-lookup"><span data-stu-id="e1ad0-189">tooretrieve hello certificate thumbprint and remove unneeded characters, use:</span></span>
   
   ```
   openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
   ```
   
   <span data-ttu-id="e1ad0-190">Amely értékét adja vissza egy ujjlenyomat hasonló:</span><span class="sxs-lookup"><span data-stu-id="e1ad0-190">Which returns a thumbprint value similar to:</span></span>
   
   ```
   30996D9CE48A0B6E0CD49DBB9A48059BF9355851
   ```
3. <span data-ttu-id="e1ad0-191">Ha naplózás tooretrieve hello ügyfél azonosító toouse van szüksége, használja:</span><span class="sxs-lookup"><span data-stu-id="e1ad0-191">If you need tooretrieve hello client id toouse for logging in, use:</span></span>
   
   ```azurecli
   azure ad sp show -c exampleapp
   ```
   
   <span data-ttu-id="e1ad0-192">hello érték toouse való bejelentkezéskor hello egyszerű szolgáltatásnevek felsorolt hello guid.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-192">hello value toouse for logging in is hello guid listed in hello service principal names.</span></span>
     
   ```azurecli
   [
     {
       "objectId": "7dbc8265-51ed-4038-8e13-31948c7f4ce7",
       "objectType": "ServicePrincipal",
       "displayName": "exampleapp",
       "appId": "4fd39843-c338-417d-b549-a545f584a745",
       "servicePrincipalNames": [
         "https://www.contoso.org/example",
         "4fd39843-c338-417d-b549-a545f584a745"
       ]
     }
   ]
   ```
4. <span data-ttu-id="e1ad0-193">Jelentkezzen be egyszerű hello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-193">Log in as hello service principal.</span></span>
   
   ```azurecli
   azure login --service-principal --tenant {tenant-id} -u 4fd39843-c338-417d-b549-a545f584a745 --certificate-file C:\certificates\examplecert.pem --thumbprint {thumbprint}
   ```

<span data-ttu-id="e1ad0-194">Most már hitelesítve hello Azure Active Directory-alkalmazást, az egyszerű hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-194">You are now authenticated as hello service principal for hello Azure Active Directory application that you created.</span></span>

## <a name="change-credentials"></a><span data-ttu-id="e1ad0-195">Hitelesítő adatok módosítása</span><span class="sxs-lookup"><span data-stu-id="e1ad0-195">Change credentials</span></span>

<span data-ttu-id="e1ad0-196">AD-alkalmazás, vagy a biztonsági sérülés vagy a hitelesítő adatok érvényessége miatt toochange hello hitelesítő adatait használja `azure ad app set`.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-196">toochange hello credentials for an AD app, either because of a security compromise or a credential expiration, use `azure ad app set`.</span></span>

<span data-ttu-id="e1ad0-197">a jelszó toochange használja:</span><span class="sxs-lookup"><span data-stu-id="e1ad0-197">toochange a password, use:</span></span>

```azurecli
azure ad app set --applicationId 4fd39843-c338-417d-b549-a545f584a745 --password p@ssword
```

<span data-ttu-id="e1ad0-198">egy tanúsítvány érték toochange használja:</span><span class="sxs-lookup"><span data-stu-id="e1ad0-198">toochange a certificate value, use:</span></span>

```azurecli
azure ad app set --applicationId 4fd39843-c338-417d-b549-a545f584a745 --cert-value {certificate data}
```

## <a name="debug"></a><span data-ttu-id="e1ad0-199">Hibakeresés</span><span class="sxs-lookup"><span data-stu-id="e1ad0-199">Debug</span></span>

<span data-ttu-id="e1ad0-200">Egy egyszerű szolgáltatás létrehozásakor a következő hibák hello merülhetnek fel:</span><span class="sxs-lookup"><span data-stu-id="e1ad0-200">You may encounter hello following errors when creating a service principal:</span></span>

* <span data-ttu-id="e1ad0-201">**"Authentication_Unauthorized"** vagy **"előfizetést hello a környezetben található."**</span><span class="sxs-lookup"><span data-stu-id="e1ad0-201">**"Authentication_Unauthorized"** or **"No subscription found in hello context."**</span></span> <span data-ttu-id="e1ad0-202">– Ezt a hibaüzenetet látja, ha a fiók nem rendelkezik hello [szükséges engedélyek](#required-permissions) a hello Azure Active Directory tooregister egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-202">- You see this error when your account does not have hello [required permissions](#required-permissions) on hello Azure Active Directory tooregister an app.</span></span> <span data-ttu-id="e1ad0-203">Általában ezt a hibát látva az Azure Active Directoryban csak rendszergazda felhasználók regisztrálhatják az alkalmazásokat, és a fiók nincs a rendszergazda segítségét. Kérje a rendszergazda tooeither rendelje hozzá, akkor tooan rendszergazdai szerepkör, illetve tooenable felhasználók tooregister alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-203">Typically, you see this error when only admin users in your Azure Active Directory can register apps, and your account is not an admin. Ask your administrator tooeither assign you tooan administrator role, or tooenable users tooregister apps.</span></span>

* <span data-ttu-id="e1ad0-204">A fiók **"Nincs engedély tooperform művelet"Microsoft.Authorization/roleAssignments/write"hatókörben"/Subscriptions/ {guid}"."**  -Ezt a hibaüzenetet látja, ha a fiók nem rendelkezik elegendő engedélyekkel tooassign egy szerepkör tooan identitást.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-204">Your account **"does not have authorization tooperform action 'Microsoft.Authorization/roleAssignments/write' over scope '/subscriptions/{guid}'."** - You see this error when your account does not have sufficient permissions tooassign a role tooan identity.</span></span> <span data-ttu-id="e1ad0-205">Kérje meg az előfizetés rendszergazdája tooadd meg tooUser hozzáférési rendszergazdai szerepkört.</span><span class="sxs-lookup"><span data-stu-id="e1ad0-205">Ask your subscription administrator tooadd you tooUser Access Administrator role.</span></span>

## <a name="sample-applications"></a><span data-ttu-id="e1ad0-206">Mintaalkalmazások</span><span class="sxs-lookup"><span data-stu-id="e1ad0-206">Sample applications</span></span>
<span data-ttu-id="e1ad0-207">Különböző platformokon keresztül hello alkalmazásként naplózás kapcsolatos információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="e1ad0-207">For information about logging in as hello application through different platforms, see:</span></span>

* [<span data-ttu-id="e1ad0-208">.NET</span><span class="sxs-lookup"><span data-stu-id="e1ad0-208">.NET</span></span>](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [<span data-ttu-id="e1ad0-209">Java</span><span class="sxs-lookup"><span data-stu-id="e1ad0-209">Java</span></span>](/java/azure/java-sdk-azure-authenticate)
* [<span data-ttu-id="e1ad0-210">Node.js</span><span class="sxs-lookup"><span data-stu-id="e1ad0-210">Node.js</span></span>](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [<span data-ttu-id="e1ad0-211">Python</span><span class="sxs-lookup"><span data-stu-id="e1ad0-211">Python</span></span>](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [<span data-ttu-id="e1ad0-212">Ruby</span><span class="sxs-lookup"><span data-stu-id="e1ad0-212">Ruby</span></span>](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a><span data-ttu-id="e1ad0-213">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e1ad0-213">Next steps</span></span>
* <span data-ttu-id="e1ad0-214">Az alkalmazás integrálása az Azure erőforrások kezeléséhez részletes lépéseiért lásd: [– fejlesztői útmutató tooauthorization hello Azure Resource Manager API-val rendelkező](resource-manager-api-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="e1ad0-214">For detailed steps on integrating an application into Azure for managing resources, see [Developer's guide tooauthorization with hello Azure Resource Manager API](resource-manager-api-authentication.md).</span></span>
* <span data-ttu-id="e1ad0-215">tooget tanúsítványok és az Azure parancssori felület használatával kapcsolatos további tudnivalókat lásd a [Tanúsítványalapú hitelesítés az Azure Szolgáltatásnevekről Linux parancssorból](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx).</span><span class="sxs-lookup"><span data-stu-id="e1ad0-215">tooget more information about using certificates and Azure CLI, see [Certificate-based authentication with Azure Service Principals from Linux command line](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx).</span></span> 
* <span data-ttu-id="e1ad0-216">Az elérhető műveleteket, lehet megadott vagy toousers megtagadva listájáért lásd: [Azure Resource Manager erőforrás-szolgáltató műveletek](../active-directory/role-based-access-control-resource-provider-operations.md).</span><span class="sxs-lookup"><span data-stu-id="e1ad0-216">For a list of available actions that can be granted or denied toousers, see [Azure Resource Manager Resource Provider operations](../active-directory/role-based-access-control-resource-provider-operations.md).</span></span>
