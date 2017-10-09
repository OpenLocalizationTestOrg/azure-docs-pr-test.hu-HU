---
title: "egy ILB ASE használata Azure Resource Manager-sablonok aaaHow tooCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan tölthető be a toocreate egy belső terheléselosztó ASE Azure Resource Manager-sablonok használatával."
services: app-service
documentationcenter: 
author: stefsch
manager: nirma
editor: 
ms.assetid: 091decb6-b0de-42a1-9f2f-c18d9b2e67df
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: stefsch
ms.openlocfilehash: 16db20eccc232ccc73107fcc8291de180fb2a323
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-an-ilb-ase-using-azure-resource-manager-templates"></a>Hogyan tooCreate egy ILB ASE használata Azure Resource Manager-sablonok

> [!NOTE] 
> Ez a cikk hello App Service Environment-környezet v1 tárgya. App Service-környezet, amely könnyebben toouse, és nagyobb teljesítményű infrastruktúra futó hello újabb verziója van telepítve. több hello új verzióval kapcsolatos kezdődnie hello toolearn [bemutatása toohello App Service Environment-környezet](../app-service/app-service-environment/intro.md).
>

## <a name="overview"></a>Áttekintés
App Service-környezetek nyilvános virtuális IP-címhez helyett egy virtuális hálózat belső cím hozhatók létre.  Ez a belső cím egy Azure összetevő hello belső terheléselosztón (ILB) nevű biztosítja.  Egy ILB ASE hello Azure portál segítségével hozhatók létre.  Azt is létrehozhatók automation vállalja Azure Resource Manager-sablonok használatával.  Ez a cikk végigvezeti a hello lépéseket, és szintaxis szükséges toocreate egy ILB ASE Azure Resource Manager-sablonok.

Három lépésben történik egy ILB ASE létrehozásának automatizálása részt:

1. Első hello alap ASE nyilvános virtuális IP-címhez helyett egy belső terheléselosztói címet a virtuális hálózat jön létre.  Ez a lépés részeként a gyökértartomány neve toohello ILB ASE van hozzárendelve.
2. Miután hello ILB ASE jön létre, az SSL-tanúsítvány feltöltése.  
3. hello feltöltött SSL-tanúsítvány van explicit módon hozzárendelt toohello ILB ASE az "alapértelmezett" SSL-tanúsítványt.  Az SSL-tanúsítvány használandó SSL forgalom tooapps a hello ILB ASE amikor hello alkalmazások kiiktatása hello közös legfelső szintű tartomány hozzárendelt toohello ASE (pl. https://someapp.mycustomrootcomain.com) használatával

## <a name="creating-hello-base-ilb-ase"></a>Hello Base ILB ASE létrehozása
Egy példa Azure Resource Manager-sablon és a társított paraméterek fájlt a Githubon érhetők [Itt][quickstartilbasecreate].

A legtöbb hello hello paraméterek *azuredeploy.parameters.json* fájl vannak közös toocreating mindkét ILB ASEs, valamint ASEs tooa nyilvános VIP kötött.  kimenő paramétereinek Megjegyzés hívások hello listán, illetve hogy egyediek, egy ILB ASE létrehozásakor:

* *interalLoadBalancingMode*: A legtöbb esetben állítsa be a too3, ami azt jelenti, hogy mindkét HTTP/HTTPS-forgalom 80/443-as porton, és hello vezérlő/adatai portok figyelt hello ASE tooby hello FTP szolgáltatás, a rendszer kötött tooan ILB rendeli hozzá virtuális hálózati belső címe.  Ha ez inkább tulajdonsága too2, akkor csak hello FTP-szolgáltatás (vezérlő és az adatokat egyaránt csatornák) portok társítani cím tooan ILB, amíg hello HTTP/HTTPS-forgalmat a hello nyilvános VIP marad.
* *dnsSuffix*: Ez a paraméter határozza meg a hello alapértelmezett gyökértartomány, amely toohello ASE hozzá lesz rendelve.  Az Azure App Service változata nyilvános hello, hello alapértelmezett gyökértartomány minden webes alkalmazások nem *azurewebsites.net*.  Mivel egy ILB ASE belső tooa az ügyfél virtuális hálózathoz, azt nem győződjön logika toouse hello nyilvános szolgáltatás alapértelmezett legfelső szintű tartomány.  Ehelyett egy ILB ASE kell egy alapértelmezett gyökértartomány legjobb a vállalat belső virtuális hálózaton belüli használatra.  Például egy kitalált, Contoso Corporation használhatja az alapértelmezett legfelső szintű tartomány *belső contoso.com* alkalmazásokat, amelyek a tervezett tooonly lehet feloldható és a Contoso virtuális hálózaton belülről érhetők el. 
* *ipSslAddressCount*: Ez a paraméter a rendszer automatikusan az alapértelmezett tooa értékének 0-ra a hello *azuredeploy.json* fájlhoz, mert ILB ASEs csak egyetlen ILB címmel rendelkezik.  Nincsenek a egy ILB ASE explicit IP-SSL címek, és ezért hello egy ILB ASE IP-SSL-címkészletet kell beállítani toozero, ellenkező esetben a létesítési hiba történik. 

Egyszer hello *azuredeploy.parameters.json* fájl ki van töltve egy ILB ASE ILB ASE majd segítségével hozhatók létre a következő kódrészletet Powershell hello hello számára.  Módosítsa a hello fájl elérési utak toomatch hello Azure Resource Manager sablon fájlok hol található a számítógépen.  Továbbá ne feledje toosupply hello Azure Resource Manager deployment neve és az erőforráscsoport neve a saját értékeit.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"

    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Miután hello Azure Resource Manager sablon beküldött hello ILB ASE toobe létrehozott néhány órát fog igénybe venni.  Hello létrehozása után hello ILB ASE jelennek meg a hello portál UX hello előfizetés hello telepítési kiváltó hello App Service Environment-környezetek listájában.

## <a name="uploading-and-configuring-hello-default-ssl-certificate"></a>Fel- és SSL-tanúsítvány "Alapértelmezett" hello konfigurálása
Egyszer hello ILB ASE jön létre, egy SSL-tanúsítványt úgy kell társított hello mértékéig, hello "alapértelmezett" SSL tanúsítvány használható SSL-kapcsolatok tooapps létrehozásához.  Van folytatása hello elméleti Contoso Corporation például, ha hello ASE tartozó alapértelmezett DNS-utótag *belső contoso.com*, majd a kapcsolat túl*https://some-random-app.internal-contoso.com*érvényes SSL-tanúsítvány szükséges **.internal-contoso.com*. 

Nincsenek különböző módokon tooobtain érvényes SSL-tanúsítvány többek között a belső hitelesítésszolgáltatók, egy tanúsítvány beszerzése egy külső kiállítótól érkező és egy önaláírt tanúsítványt használ.  Hello SSL-tanúsítvány hello forrását, függetlenül hello következő tanúsítvány attribútumokkal kell toobe megfelelően konfigurálva:

* *Tulajdonos*: Ez az attribútum túl be kell állítani **.your-gyökér-tartományi-here.com*
* *Tulajdonos alternatív neve*: ennek az attribútumnak kell tartalmaznia a **.your-gyökér-tartományi-here.com*, és **.scm.your-gyökér-tartományi-here.com*.  hello hello második bejegyzés oka, hogy minden alkalmazáshoz társított SCM/Kudu hely hello űrlap cím használatával fog történni SSL-kapcsolatok toohello *your-app-name.scm.your-root-domain-here.com*.

Egy érvényes SSL-tanúsítvánnyal aktuális két további előkészítő lépések szükségesek.  hello SSL-tanúsítványt kell toobe konvertálni/egy .pfx fájlba menti.  Ne feledje, hogy hello .pfx fájl szüksége lesz az összes köztes tooinclude és a legfelső szintű tanúsítványok, és jelszóval védett toobe kell.

Majd hello eredő .pfx fájlnak kell toobe Base64 kódolású karakterlánc alakítja át, mert az Azure Resource Manager-sablonnal hello SSL-tanúsítvány lesz feltöltve.  Mivel az Azure Resource Manager-sablonok szövegfájlok, hello .pfx fájlba kell konvertálni a Base64 kódolású karakterlánc, azt is meg lehet adni hello sablon paraméterként toobe.

hello Powershell kódrészletben látható példa önaláírt tanúsítvány létrehozása, hello tanúsítvány exportálása egy .pfx fájlba történő konvertálásakor hello .pfx fájl a base64 kódolású karakterlánc, majd mentse a hello base64 kódolású karakterlánc tooa külön fájlt.  Powershell-kódjába hello base64 kódolást volt módosítani a hello [Powershell parancsfájlok Blog][examplebase64encoding].

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.pfx"
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     

    $fileContentBytes = get-content -encoding byte $fileName
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    $fileContentEncoded | set-content ($fileName + ".b64")

Miután hello SSL-tanúsítvány sikeresen létrejött, és a konvertált tooa base64 kódolású karakterlánc, például Azure Resource Manager-sablon a Githubon hello [hello alapértelmezett SSL-tanúsítvány konfigurálása] [ configuringDefaultSSLCertificate] is használható.

paramétereket a hello hello *azuredeploy.parameters.json* fájlt az alábbiak:

* *appServiceEnvironmentName*: hello konfigurált ILB ASE hello nevét.
* *existingAseLocation*: szöveges karakterláncot tartalmazó hello Azure-régió, ahol hello ILB ASE telepítve lett.  Például: "Déli középső Régiójában".
* *pfxBlobString*: hello based64 kódolású karakterlánc-ábrázolása hello .pfx fájl.  Korábban bemutatott hello kódrészletet használja, akkor volna hello karakterlánc szerepel a "exportedcert.pfx.b64" másolja és illessze be hello hello értékeként *pfxBlobString* attribútum.
* *jelszó*: hello használt jelszó toosecure hello .pfx fájlt.
* *certificateThumbprint*: hello tanúsítvány ujjlenyomata.  Ha ez az érték lekérése Powershell (pl. *$certificate. Ujjlenyomat* hello a korábbi kódrészletet), használhatja a hello értékével megegyező-van.  Azonban ha hello értéket másol hello Windows tanúsítvány párbeszédpanel, ne felejtse el toostrip kimenő hello idegen szóközöket.  Hello *certificateThumbprint* hasonlóan kell kinéznie: AF3143EB61D43F6727842115BB7F17BBCECAECAE
* *certificateName*: egy rövid karakterlánc-azonosító saját tooidentity hello tanúsítványt használja.  hello neve hello Azure Resource Manager azonosítójának részeként használatos hello *Microsoft.Web/certificates* hello SSL-tanúsítvány képviselő entitás.  hello neve **kell** végződhet a következő utótag hello: \_yourASENameHere_InternalLoadBalancingASE.  Azt jelzi, hogy a tanúsítvány hello használt biztonságossá tétele az ILB-kompatibilis ASE ez utótag hello portal használja.

Rövidített például *azuredeploy.parameters.json* alább találja:

    {
         "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json",
         "contentVersion": "1.0.0.0",
         "parameters": {
              "appServiceEnvironmentName": {
                   "value": "yourASENameHere"
              },
              "existingAseLocation": {
                   "value": "East US 2"
              },
              "pfxBlobString": {
                   "value": "MIIKcAIBAz...snip...snip...pkCAgfQ"
              },
              "password": {
                   "value": "PASSWORDGOESHERE"
              },
              "certificateThumbprint": {
                   "value": "AF3143EB61D43F6727842115BB7F17BBCECAECAE"
              },
              "certificateName": {
                   "value": "DefaultCertificateFor_yourASENameHere_InternalLoadBalancingASE"
              }
         }
    }

Egyszer hello *azuredeploy.parameters.json* fájl ki van töltve, hello alapértelmezett SSL-tanúsítvány a következő kódrészletet Powershell hello használatával konfigurálható.  Módosítsa a hello fájl elérési utak toomatch hello Azure Resource Manager sablon fájlok hol található a számítógépen.  Továbbá ne feledje toosupply hello Azure Resource Manager deployment neve és az erőforráscsoport neve a saját értékeit.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"

    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Miután hello Azure Resource Manager sablon beküldött nagyjából a ASE előtér-tooapply hello változásonkénti negyven perc percet vesz igénybe.  Például az alapértelmezett érték használatával két elülső vége méretű ASE hello sablon lépnek egy óra és 20 perc toocomplete körül.  Hello sablon futása közben hello ASE nem lesz képes tooscaled.  

Hello sablon befejeztét követően hello ILB ASE alkalmazások HTTPS-KAPCSOLATON keresztül is elérhetők, és hello kapcsolatok megfelelően történik hello alapértelmezett SSL-tanúsítvány.  hello alapértelmezett SSL-tanúsítvány fog használni, amikor hello ILB ASE alkalmazások kombinációjából hello alkalmazásnév plus hello alapértelmezett állomásnév tárgyalja.  Például *https://mycustomapp.internal-contoso.com* hello alapértelmezett SSL-tanúsítványt használna **.internal-contoso.com*.

Azonban hasonlóan hello nyilvános több-bérlős szolgáltatást futó alkalmazások, a fejlesztők is konfigurálja a egyéni állomásneveket, az egyes alkalmazásokkal, és majd konfigurálja az egyedi SNI SSL-tanúsítványok kötései egyes alkalmazások esetében.  

## <a name="getting-started"></a>Bevezetés
Lásd az App Service-környezetek lépései tooget [bemutatása tooApp Service-környezet](app-service-app-service-environment-intro.md)

Összes cikket, és hogyan-a következőre az App Service Environment-környezetek érhetők el hello [alkalmazásszolgáltatási környezetek – fontos fájl](../app-service/app-service-app-service-environments-readme.md).

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[quickstartilbasecreate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-create/
[examplebase64encoding]: http://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/ 

