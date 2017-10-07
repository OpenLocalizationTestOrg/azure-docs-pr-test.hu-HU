---
title: "aaaAdd egyéni tartomány- és SSL tooan Azure web app |} Microsoft Docs"
description: "Ismerje meg, hogyan tooprepare az Azure web app toogo éles adja hozzá a vállalati arculat megjelenítése. Hello egyéni tartomány nevét (kreatív tartomány) tooyour webalkalmazás rendelve, és biztosíthatja egy egyéni SSL-tanúsítvánnyal."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: dc446e0e-0958-48ea-8d99-441d2b947a7c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 03/29/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 2679ed8b2dbbeba0b128c1a3ec01148f97c35342
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-custom-domain-and-ssl-tooan-azure-web-app"></a><span data-ttu-id="2c401-104">Adja hozzá az egyéni tartomány- és SSL tooan Azure-webalkalmazásban</span><span class="sxs-lookup"><span data-stu-id="2c401-104">Add custom domain and SSL tooan Azure web app</span></span>

<span data-ttu-id="2c401-105">Az oktatóanyag bemutatja, hogyan tooquickly egy egyéni tartomány nevét tooyour Azure-webalkalmazásban rendelve, és ezután biztonságos egyéni SSL-tanúsítvánnyal.</span><span class="sxs-lookup"><span data-stu-id="2c401-105">This tutorial shows you how tooquickly map a custom domain name tooyour Azure web app and then secure it with a custom SSL certificate.</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="2c401-106">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="2c401-106">Before you begin</span></span>

<span data-ttu-id="2c401-107">Ez a minta futtatásához telepítse a hello [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) helyileg.</span><span class="sxs-lookup"><span data-stu-id="2c401-107">Before running this sample, install hello [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) locally.</span></span>

<span data-ttu-id="2c401-108">Szükség rendszergazdai hozzáférés toohello DNS konfigurálása lapon a megfelelő tartomány szolgáltatóhoz.</span><span class="sxs-lookup"><span data-stu-id="2c401-108">You also need administrative access toohello DNS configuration page for your respective domain provider.</span></span> <span data-ttu-id="2c401-109">Például tooadd `www.contoso.com`, toobe képes tooconfigure DNS-bejegyzéseket kell `contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="2c401-109">For example, tooadd `www.contoso.com`, you need toobe able tooconfigure DNS entries for `contoso.com`.</span></span>

<span data-ttu-id="2c401-110">Végül kell egy érvényes. PFX-fájl _és_ a kívánt tooupload hello SSL-tanúsítvány jelszavát, majd kötést.</span><span class="sxs-lookup"><span data-stu-id="2c401-110">Lastly, you need a valid .PFX file _and_ its password for hello SSL certificate you want tooupload and bind.</span></span> <span data-ttu-id="2c401-111">Az SSL-tanúsítvány konfigurált toosecure hello egyéni tartománynevet kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2c401-111">This SSL certificate should be configured toosecure hello custom domain name you want.</span></span> <span data-ttu-id="2c401-112">A fenti példa hello, az SSL-tanúsítvány kell biztonságossá tenni `www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="2c401-112">In hello above example, your SSL certificate should secure `www.contoso.com`.</span></span> 

## <a name="step-1---create-an-azure-web-app"></a><span data-ttu-id="2c401-113">1. lépés – Azure-webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="2c401-113">Step 1 - Create an Azure web app</span></span>

### <a name="log-in-tooazure"></a><span data-ttu-id="2c401-114">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="2c401-114">Log in tooAzure</span></span>

<span data-ttu-id="2c401-115">Folyamatban van, most a folyamatban toouse hello Azure CLI 2.0 egy terminálablakot toocreate hello erőforrások a szükséges toohost a Node.js-alkalmazás az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="2c401-115">We are now going toouse hello Azure CLI 2.0 in a terminal window toocreate hello resources needed toohost our Node.js app in Azure.</span></span>  <span data-ttu-id="2c401-116">Jelentkezzen be Azure előfizetés hello tooyour [az bejelentkezési](/cli/azure/#login) parancsot, és kövesse hello képernyőn megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="2c401-116">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span> 

```azurecli 
az login 
``` 
   
### <a name="create-a-resource-group"></a><span data-ttu-id="2c401-117">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="2c401-117">Create a resource group</span></span>   
<span data-ttu-id="2c401-118">Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="2c401-118">Create a resource group with hello [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="2c401-119">Az Azure-erőforráscsoport olyan logikai tároló, amelyben a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat (például webappokat, adatbázisokat és tárfiókokat).</span><span class="sxs-lookup"><span data-stu-id="2c401-119">An Azure resource group is a logical container into which Azure resources like web apps, databases and storage accounts are deployed and managed.</span></span> 

```azurecli
az group create --name myResourceGroup --location westeurope 
```

<span data-ttu-id="2c401-120">toosee milyen lehetséges értékei, akkor is használhat `---location`, használja a hello `az appservice list-locations` Azure CLI parancs.</span><span class="sxs-lookup"><span data-stu-id="2c401-120">toosee what possible values you can use for `---location`, use hello `az appservice list-locations` Azure CLI command.</span></span>

## <a name="create-an-app-service-plan"></a><span data-ttu-id="2c401-121">App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="2c401-121">Create an App Service plan</span></span>

<span data-ttu-id="2c401-122">Hozzon létre egy App Service-csomag hello [az App Service-csomagot hozzon létre](/cli/azure/appservice/plan#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="2c401-122">Create an App Service plan with hello [az appservice plan create](/cli/azure/appservice/plan#create) command.</span></span> 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<span data-ttu-id="2c401-123">hello alábbi példakód létrehozza az App Service-csomag nevű `myAppServicePlan` hello segítségével **alapvető** tarifacsomagra vált.</span><span class="sxs-lookup"><span data-stu-id="2c401-123">hello following example creates an App Service plan named `myAppServicePlan` using hello **Basic** pricing tier.</span></span>

<span data-ttu-id="2c401-124">az App Service-csomagot hozzon létre--name myAppServicePlan--B1 erőforráscsoport myResourceGroup--termékváltozat</span><span class="sxs-lookup"><span data-stu-id="2c401-124">az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1</span></span>

<span data-ttu-id="2c401-125">Hello App Service-csomag létrehozásakor a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="2c401-125">When hello App Service plan has been created, hello Azure CLI shows information similar toohello following example.</span></span> 

```json 
{ 
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan", 
    "kind": "app", 
    "location": "West Europe", 
    "sku": { 
    "capacity": 1, 
    "family": "B", 
    "name": "B1", 
    "tier": "Basic" 
    }, 
    "status": "Ready", 
    "type": "Microsoft.Web/serverfarms" 
} 
``` 

## <a name="create-a-web-app"></a><span data-ttu-id="2c401-126">Webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="2c401-126">Create a web app</span></span>

<span data-ttu-id="2c401-127">Most, hogy az App Service-csomag létrehozása után hozzon létre egy webalkalmazás hello `myAppServicePlan` App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="2c401-127">Now that an App Service plan has been created, create a web app within hello `myAppServicePlan` App Service plan.</span></span> <span data-ttu-id="2c401-128">hello web app ad meg egy üzemeltetési terület toodeploy még a kódot, valamint tooview hello biztosítja egy URL-CÍMÉT, központilag telepített alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2c401-128">hello web app gives your a hosting space toodeploy your code as well as provides a URL for you tooview hello deployed application.</span></span> <span data-ttu-id="2c401-129">Használjon hello [az App Service webalkalmazás létrehozása](/cli/azure/appservice/web#create) parancs toocreate hello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2c401-129">Use hello [az appservice web create](/cli/azure/appservice/web#create) command toocreate hello web app.</span></span> 

<span data-ttu-id="2c401-130">Hello parancs az alábbi, a adjon helyettesítse a saját egyedi alkalmazásnévvel hello megtapasztalhatja `<app_name>` helyőrző.</span><span class="sxs-lookup"><span data-stu-id="2c401-130">In hello command below, please substitute your own unique app name where you see hello `<app_name>` placeholder.</span></span> <span data-ttu-id="2c401-131">A egyedi név, hello nevének kell toobe egyedi az Azure-ban minden alkalmazások között használható hello alapértelmezett tartomány nevét hello webalkalmazás, hello részeként.</span><span class="sxs-lookup"><span data-stu-id="2c401-131">This unique name will be used as hello part of hello default domain name for hello web app, so hello name needs toobe unique across all apps in Azure.</span></span> <span data-ttu-id="2c401-132">Ki kell alakítani az tooyour felhasználók bármilyen egyéni DNS bejegyzés toohello webalkalmazás később is leképezheti.</span><span class="sxs-lookup"><span data-stu-id="2c401-132">You can later map any custom DNS entry toohello web app before you expose it tooyour users.</span></span> 

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan 
```

<span data-ttu-id="2c401-133">Hello webalkalmazás létrehozásakor hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="2c401-133">When hello web app has been created, hello Azure CLI shows information similar toohello following example.</span></span> 

```json 
{ 
    "clientAffinityEnabled": true, 
    "defaultHostName": "<app_name>.azurewebsites.net", 
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/sites/<app_name>", 
    "isDefaultContainer": null, 
    "kind": "app", 
    "location": "West Europe", 
    "name": "<app_name>", 
    "repositorySiteName": "<app_name>", 
    "reserved": true, 
    "resourceGroup": "myResourceGroup", 
    "serverFarmId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan", 
    "state": "Running", 
    "type": "Microsoft.Web/sites", 
} 
```

<span data-ttu-id="2c401-134">A JSON-kimenetét hello `defaultHostName` a webalkalmazás alapértelmezett tartomány nevét jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="2c401-134">From hello JSON output, `defaultHostName` shows your web app's default domain name.</span></span> <span data-ttu-id="2c401-135">A böngészőben nyissa meg a toothis cím.</span><span class="sxs-lookup"><span data-stu-id="2c401-135">In your browser, navigate toothis address.</span></span>

```
http://<app_name>.azurewebsites.net 
``` 
 
![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-created.png)  

## <a name="step-2---configure-dns-mapping"></a><span data-ttu-id="2c401-137">2. lépés – konfigurálja a DNS-hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="2c401-137">Step 2 - Configure DNS mapping</span></span>

<span data-ttu-id="2c401-138">Ebben a lépésben ad hozzá egy leképezése az egy egyéni tartomány tooyour webes alkalmazás alapértelmezett tartomány nevét, `<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="2c401-138">In this step, you add a mapping from a custom domain tooyour web app's default domain name, `<app_name>.azurewebsites.net`.</span></span> <span data-ttu-id="2c401-139">Általában akkor hajtsa végre ezt a lépést a tartomány szolgáltató webhelyéről.</span><span class="sxs-lookup"><span data-stu-id="2c401-139">Typically, you perform this step in your domain provider's website.</span></span> <span data-ttu-id="2c401-140">Minden tartományregisztráló webhely némileg eltér, nézze át a szolgáltató dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="2c401-140">Every domain registrar's website is slightly different, so you should consult your provider's documentation.</span></span> <span data-ttu-id="2c401-141">Azonban az alábbiakban néhány általános irányelveket.</span><span class="sxs-lookup"><span data-stu-id="2c401-141">However, here are some general guidelines.</span></span> 

### <a name="navigate-tootoodns-management-page"></a><span data-ttu-id="2c401-142">Keresse meg a tootooDNS kezelése lap</span><span class="sxs-lookup"><span data-stu-id="2c401-142">Navigate tootooDNS management page</span></span>

<span data-ttu-id="2c401-143">Első lépésként jelentkezzen be tooyour tartomány regisztráló webhelyén.</span><span class="sxs-lookup"><span data-stu-id="2c401-143">First, log in tooyour domain registrar's website.</span></span>  

<span data-ttu-id="2c401-144">Ezt követően hello lapon található DNS-rekordok kezelése.</span><span class="sxs-lookup"><span data-stu-id="2c401-144">Then, find hello page for managing DNS records.</span></span> <span data-ttu-id="2c401-145">Keressen hivatkozásokat vagy feliratú hello hely területek **tartománynév**, **DNS**, vagy **neve kezelő**.</span><span class="sxs-lookup"><span data-stu-id="2c401-145">Look for links or areas of hello site labeled **Domain Name**, **DNS**, or **Name Server Management**.</span></span> <span data-ttu-id="2c401-146">Gyakran, megtalálhatja hello hivatkozás megtekintésével a fiók adatait, és ezután keres, mint egy hivatkozást **a tartományok**.</span><span class="sxs-lookup"><span data-stu-id="2c401-146">Often, you can find hello link by viewing your account information, and then looking for a link such as **My domains**.</span></span>

<span data-ttu-id="2c401-147">Ezen a lapon talál, keresse meg a hivatkozást, amellyel lehetővé teszi, hogy hozzáadása vagy szerkesztése a DNS-rekordokat.</span><span class="sxs-lookup"><span data-stu-id="2c401-147">Once you find this page, look for a link that lets you add or edit DNS records.</span></span> <span data-ttu-id="2c401-148">Lehetséges, hogy egy **zónafájl** vagy **DNS-rekordok** hivatkozásra, vagy egy **speciális konfigurációs** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="2c401-148">This might be a **Zone file** or **DNS Records** link, or an **Advanced configuration** link.</span></span>

### <a name="create-a-cname-record"></a><span data-ttu-id="2c401-149">Hozzon létre egy CNAME rekordot</span><span class="sxs-lookup"><span data-stu-id="2c401-149">Create a CNAME record</span></span>

<span data-ttu-id="2c401-150">Adjon hozzá egy CNAME rekordot, amely leképezhető hello kívánt altartomány neve tooyour webes alkalmazás alapértelmezett tartomány nevét (`<app_name>.azurewebsites.net`, ahol `<app_name>` egyedi név az alkalmazásban).</span><span class="sxs-lookup"><span data-stu-id="2c401-150">Add a CNAME record that maps hello desired subdomain name tooyour web app's default domain name (`<app_name>.azurewebsites.net`, where `<app_name>` is your app's unique name).</span></span>

<span data-ttu-id="2c401-151">A hello `www.contoso.com` például létrehozhat egy olyan CNAME REKORDOT, amely leképezhető hello `www` állomásnév túl`<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="2c401-151">For hello `www.contoso.com` example, you create a CNAME that maps hello `www` hostname too`<app_name>.azurewebsites.net`.</span></span>

## <a name="step-3---configure-hello-custom-domain-on-your-web-app"></a><span data-ttu-id="2c401-152">3. lépés - a webalkalmazás hello egyéni tartománynév konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2c401-152">Step 3 - Configure hello custom domain on your web app</span></span>

<span data-ttu-id="2c401-153">Amikor befejezte a hello állomásnév leképezés konfigurálása a tartomány szolgáltató webhelyéről, most készen áll a tooconfigure hello egyéni tartomány a webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="2c401-153">When you finish configuring hello hostname mapping in your domain provider's website, you're ready tooconfigure hello custom domain on your web app.</span></span> <span data-ttu-id="2c401-154">Használjon hello [hozzáadása az App Service web config állomásnév](/cli/azure/appservice/web/config/hostname#add) parancs tooadd ezt a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="2c401-154">Use hello [az appservice web config hostname add](/cli/azure/appservice/web/config/hostname#add) command tooadd this configuration.</span></span> 

<span data-ttu-id="2c401-155">Hello parancs az alábbi, adjon helyettesítse `<app_name>` egyedi alkalmazásnévvel és < your_custom_domain > hello teljesen minősített egyéni tartomány nevét (pl. `www.contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="2c401-155">In hello command below, please substitute `<app_name>` with your unique app name, and <your_custom_domain> with hello fully qualified custom domain name (e.g. `www.contoso.com`).</span></span> 

```azurecli
az appservice web config hostname add --webapp <app_name> --resource-group myResourceGroup --name <your_custom_domain>
```

<span data-ttu-id="2c401-156">hello egyéni tartomány most már teljes mértékben csatlakoztatott tooyour webalkalmazás áll.</span><span class="sxs-lookup"><span data-stu-id="2c401-156">hello custom domain now is fully mapped tooyour web app.</span></span> <span data-ttu-id="2c401-157">A böngészőben nyissa meg a toohello egyéni tartománynevet.</span><span class="sxs-lookup"><span data-stu-id="2c401-157">In your browser, navigate toohello custom domain name.</span></span> <span data-ttu-id="2c401-158">Példa:</span><span class="sxs-lookup"><span data-stu-id="2c401-158">For example:</span></span>

```
http://www.contoso.com 
``` 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-custom-domain.png)  

## <a name="step-4---bind-a-custom-ssl-certificate-tooyour-web-app"></a><span data-ttu-id="2c401-160">4. lépés: a kötés SSL tanúsítvány tooyour egyéni webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="2c401-160">Step 4 - Bind a custom SSL certificate tooyour web app</span></span>

<span data-ttu-id="2c401-161">Most már rendelkezik egy Azure webalkalmazás hello tartománynévvel kívánt hello böngésző címsorába.</span><span class="sxs-lookup"><span data-stu-id="2c401-161">You now have an Azure web app, with hello domain name you want in hello browser's address bar.</span></span> <span data-ttu-id="2c401-162">Azonban ha manuálisan lép toohello `https://<your_custom_domain>` , akkor a hibaüzenet egy tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="2c401-162">However, if you navigate toohello `https://<your_custom_domain>` now, you get a certificate error.</span></span> 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-cert-error.png)  

<span data-ttu-id="2c401-164">Ez a hiba akkor fordul elő, mert a webalkalmazás még nem rendelkezik SSL-tanúsítvány kötést, amely megfelel az egyéni tartománynevet.</span><span class="sxs-lookup"><span data-stu-id="2c401-164">This error occurs because your web app doesn't yet have an SSL certificate binding that matches your custom domain name.</span></span> <span data-ttu-id="2c401-165">Azonban nem kérek hiba, ha manuálisan lép túl`https://<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="2c401-165">However, you don't get an error if you navigate too`https://<app_name>.azurewebsites.net`.</span></span> <span data-ttu-id="2c401-166">Ennek az az oka az alkalmazást, valamint az összes Azure App Service apps védett hello SSL-tanúsítvánnyal hello `*.azurewebsites.net` helyettesítő tartomány alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="2c401-166">This is because your app, as well as all Azure App Service apps, is secured with hello SSL certificate for hello `*.azurewebsites.net` wildcard domain by default.</span></span> 

<span data-ttu-id="2c401-167">A webalkalmazás tooaccess sorrendben az egyéni tartománynév alapján, az egyéni tartomány toohello webalkalmazás toobind hello SSL-tanúsítvány szükséges.</span><span class="sxs-lookup"><span data-stu-id="2c401-167">In order tooaccess your web app by your custom domain name, you need toobind hello SSL certificate for your custom domain toohello web app.</span></span> <span data-ttu-id="2c401-168">Az ebben a lépésben fogja végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="2c401-168">You will do it in this step.</span></span> 

### <a name="upload-hello-ssl-certificate"></a><span data-ttu-id="2c401-169">Hello SSL-tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="2c401-169">Upload hello SSL certificate</span></span>

<span data-ttu-id="2c401-170">Az egyéni tartomány tooyour webalkalmazás hello SSL-tanúsítvány feltöltése hello segítségével [az App Service web config ssl feltöltés](/cli/azure/appservice/web/config/ssl#upload) parancsot.</span><span class="sxs-lookup"><span data-stu-id="2c401-170">Upload hello SSL certificate for your custom domain tooyour web app by using hello [az appservice web config ssl upload](/cli/azure/appservice/web/config/ssl#upload) command.</span></span>

<span data-ttu-id="2c401-171">Hello parancs az alábbi, adjon helyettesítse `<app_name>` az egyedi alkalmazás nevű `<path_to_ptx_file>` hello elérési tooyour együtt. PFX-fájlt, és `<password>` a tanúsítvány jelszóval.</span><span class="sxs-lookup"><span data-stu-id="2c401-171">In hello command below, please substitute `<app_name>` with your unique app name, `<path_to_ptx_file>` with hello path tooyour .PFX file, and `<password>` with your certificate's password.</span></span> 

```azurecli
az appservice web config ssl upload --name <app_name> --resource-group myResourceGroup --certificate-file <path_to_pfx_file> --certificate-password <password> 
```

<span data-ttu-id="2c401-172">Hello tanúsítvány töltheti fel, ha a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="2c401-172">When hello certificate is uploaded, hello Azure CLI shows information similar toohello following example:</span></span>

```json
{
  "cerBlob": null,
  "expirationDate": "2018-03-29T14:12:57+00:00",
  "friendlyName": "",
  "hostNames": [
    "www.contoso.com"
  ],
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/cert
ificates/9FD1D2D06E2293673E2A8D1CA484A092BD016D00__West Europe_myResourceGroup",
  "issueDate": "2017-03-29T14:12:57+00:00",
  "issuer": "www.contoso.com",
  "keyVaultId": null,
  "keyVaultSecretName": null,
  "keyVaultSecretStatus": "Initialized",
  "kind": null,
  "location": "West Europe",
  "name": "9FD1D2D06E2293673E2A8D1CA484A092BD016D00__West Europe_myResourceGroup",
  "password": null,
 "pfxBlob": null,
  "publicKeyHash": null,
  "resourceGroup": "myResourceGroup",
  "selfLink": null,
  "serverFarmId": null,
  "siteName": null,
  "subjectName": "www.contoso.com",
  "tags": null,
  "thumbprint": "9FD1D2D06E2293673E2A8D1CA484A092BD016D00",
  "type": "Microsoft.Web/certificates",
  "valid": null
}
```

<span data-ttu-id="2c401-173">A JSON-kimenetét hello `thumbprint` a feltöltött tanúsítvány ujjlenyomata látható.</span><span class="sxs-lookup"><span data-stu-id="2c401-173">From hello JSON output, `thumbprint` shows your uploaded certificate's thumbprint.</span></span> <span data-ttu-id="2c401-174">Másolja a következő lépéshez hello értéke.</span><span class="sxs-lookup"><span data-stu-id="2c401-174">Copy its value for hello next step.</span></span>

### <a name="bind-hello-uploaded-ssl-certificate-toohello-web-app"></a><span data-ttu-id="2c401-175">A kötés hello feltöltött SSL-tanúsítvány toohello webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="2c401-175">Bind hello uploaded SSL certificate toohello web app</span></span>

<span data-ttu-id="2c401-176">A webalkalmazás már hello egyéni tartománynevet, és egy SSL-tanúsítvány, amely biztosítja, hogy egyéni tartományt is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2c401-176">Your web app now has hello custom domain name you want, and it also has a SSL certificate that secures that custom domain.</span></span> <span data-ttu-id="2c401-177">hello csak dolog bal oldali toodo toobind hello feltöltött tanúsítvány toohello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2c401-177">hello only thing left toodo is toobind hello uploaded certificate toohello web app.</span></span> <span data-ttu-id="2c401-178">Hello használatával teheti [az App Service web config ssl-kötés](/cli/azure/appservice/web/config/ssl#bind) parancsot.</span><span class="sxs-lookup"><span data-stu-id="2c401-178">You do this by using hello [az appservice web config ssl bind](/cli/azure/appservice/web/config/ssl#bind) command.</span></span>

<span data-ttu-id="2c401-179">Hello parancs az alábbi, adjon helyettesítse `<app_name>` saját egyedi alkalmazás nevét és `<thumbprint-from-previous-output>` az előző parancs hello kapott hello tanúsítvány ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="2c401-179">In hello command below, please substitute `<app_name>` with your unique app name and `<thumbprint-from-previous-output>` with hello certificate thumbprint that you get from hello previous command.</span></span> 

<span data-ttu-id="2c401-180">az App Service web config ssl-kötés--name < alkalmazásnév >--erőforráscsoport myResourceGroup – tanúsítvány-ujjlenyomata < ujjlenyomat-a-előző-kimeneti >--ssl-típus SNI</span><span class="sxs-lookup"><span data-stu-id="2c401-180">az appservice web config ssl bind --name <app_name> --resource-group myResourceGroup --certificate-thumbprint <thumbprint-from-previous-output> --ssl-type SNI</span></span>

<span data-ttu-id="2c401-181">Ha hello tanúsítvány kötött tooyour webalkalmazás, hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="2c401-181">When hello certificate is bound tooyour web app, hello Azure CLI shows information similar toohello following example:</span></span>

<span data-ttu-id="2c401-182">{"availabilityState": "Normál", "clientAffinityEnabled": true, "clientCertEnabled": hamis, "cloningInfo": null, "containerSize": 0, "dailyMemoryTimeQuota": 0, "defaultHostName": "< alkalmazásnév >. azurewebsites.net", "enabled": true, "enabledHostNames": ["www.contoso.com", "< alkalmazásnév >. azurewebsites.net", "< alkalmazásnév >. scm.azurewebsites.net"], "gatewaySiteName": null, a "hostNameSslStates": [{"hostType": "Standard", "name": "< alkalmazásnév >. azurewebsites.net", "sslState": "Letiltott", "ujjlenyomata": null, "toUpdate": null, "virtuális IP-cím": null}, {"hostType": "Tárház", "name": "< alkalmazásnév >. scm.azurewebsites.net", "sslState": "Letiltva" "ujjlenyomata": null, "toUpdate": null, "virtuális IP-cím": null}, {"hostType": "Standard", "name": "www.contoso.com", "sslState": "SniEnabled", "ujjlenyomata": "9FD1D2D06E2293673E2A8D1CA484A092BD016D00", "toUpdate": null, "virtuális IP-cím": null}], "hostNames": ["www.contoso.com", "< alkalmazásnév >. azurewebsites.net"], "hostNamesDisabled": false, "hostingEnvironmentProfile": null, "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/site s / < alkalmazásnév >", "isDefaultContainer": null, a "fajta": "Webalkalmazás", "lastModifiedTimeUtc": "2017-03-29T14:36:18.803333", "hely": "Nyugati Európa", "maxNumberOfWorkers": null, "Mikroszolgáltatási": "false", "név": "< alkalmazásnév >" "outboundIpAddresses": "13.94.143.57,13.94.136.57,40.68.199.146,13.94.138.55,13.94.140.1", "premiumAppDeployed": null, "repositorySiteName": "< alkalmazásnév >", "fenntartott": false, "erőforráscsoport": "contoso.com", "scmSiteAlsoStopped": hamis, "serverFarmId": "/ előfizetések/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/szolgáltatók/Microsof t.Web/serverfarms/myAppServicePlan", "siteConfig": null, a "slotSwapStatus": null, a "állapot": "Fut", "suspendedTill": null, a "címkék": null, "targetSwapSlot": null, a "trafficManagerHostNames": null, "type": "Microsoft.Web/sites", "usageState": "Normál"}</span><span class="sxs-lookup"><span data-stu-id="2c401-182">{ "availabilityState": "Normal", "clientAffinityEnabled": true, "clientCertEnabled": false, "cloningInfo": null, "containerSize": 0, "dailyMemoryTimeQuota": 0, "defaultHostName": "<app_name>.azurewebsites.net", "enabled": true, "enabledHostNames": [ "www.contoso.com", "<app_name>.azurewebsites.net", "<app_name>.scm.azurewebsites.net" ], "gatewaySiteName": null, "hostNameSslStates": [ { "hostType": "Standard", "name": "<app_name>.azurewebsites.net", "sslState": "Disabled", "thumbprint": null, "toUpdate": null, "virtualIp": null }, { "hostType": "Repository", "name": "<app_name>.scm.azurewebsites.net", "sslState": "Disabled", "thumbprint": null, "toUpdate": null, "virtualIp": null }, { "hostType": "Standard", "name": "www.contoso.com", "sslState": "SniEnabled", "thumbprint": "9FD1D2D06E2293673E2A8D1CA484A092BD016D00", "toUpdate": null, "virtualIp": null } ], "hostNames": [ "www.contoso.com", "<app_name>.azurewebsites.net" ], "hostNamesDisabled": false, "hostingEnvironmentProfile": null, "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/site s/<app_name>", "isDefaultContainer": null, "kind": "WebApp", "lastModifiedTimeUtc": "2017-03-29T14:36:18.803333", "location": "West Europe", "maxNumberOfWorkers": null, "microService": "false", "name": "<app_name>", "outboundIpAddresses": "13.94.143.57,13.94.136.57,40.68.199.146,13.94.138.55,13.94.140.1", "premiumAppDeployed": null, "repositorySiteName": "<app_name>", "reserved": false, "resourceGroup": "myResourceGroup", "scmSiteAlsoStopped": false, "serverFarmId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsof t.Web/serverfarms/myAppServicePlan", "siteConfig": null, "slotSwapStatus": null, "state": "Running", "suspendedTill": null, "tags": null, "targetSwapSlot": null, "trafficManagerHostNames": null, "type": "Microsoft.Web/sites", "usageState": "Normal" }</span></span>

<span data-ttu-id="2c401-183">A böngészőben nyissa meg az egyéni tartománynevet tooHTTPS végpontja.</span><span class="sxs-lookup"><span data-stu-id="2c401-183">In your browser, navigate tooHTTPS endpoint of your custom domain name.</span></span> <span data-ttu-id="2c401-184">Példa:</span><span class="sxs-lookup"><span data-stu-id="2c401-184">For example:</span></span>

```
https://www.contoso.com 
``` 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-ssl-success.png)  

## <a name="more-resources"></a><span data-ttu-id="2c401-186">További erőforrások</span><span class="sxs-lookup"><span data-stu-id="2c401-186">More resources</span></span>

<span data-ttu-id="2c401-187">[Vásároljon és egyéni tartománynév beállítása az Azure App Service](custom-dns-web-site-buydomains-web-app.md)
[vásárolnia és SSL-tanúsítvány konfigurálása az Azure App Service](web-sites-purchase-ssl-web-site.md)</span><span class="sxs-lookup"><span data-stu-id="2c401-187">[Buy and Configure a custom domain name in Azure App Service](custom-dns-web-site-buydomains-web-app.md)
[Buy and Configure an SSL Certificate for your Azure App Service](web-sites-purchase-ssl-web-site.md)</span></span>
