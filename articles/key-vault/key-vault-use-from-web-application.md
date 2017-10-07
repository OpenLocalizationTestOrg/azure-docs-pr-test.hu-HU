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
# <a name="use-azure-key-vault-from-a-web-application"></a>Használja az Azure Key Vault-webalkalmazások
## <a name="introduction"></a>Bevezetés
Az oktatóanyag toohelp megtudhatja, hogyan toouse Azure kulcsot tároló az Azure-webalkalmazások használja. Végigvezeti a titkos kulcs eléréséhez, hogy a webes alkalmazás használható az Azure Key Vault hello folyamatán.

**Becsült idő toocomplete:** 15 perc

Áttekintést az Azure Key Vaultról a [What is Azure Key Vault?](key-vault-whatis.md) (Mi az Azure Key Vault?) című cikkben talál.

## <a name="prerequisites"></a>Előfeltételek
toocomplete ebben az oktatóanyagban rendelkeznie kell a következő hello:

* Egy Azure Key Vault az URI tooa titkos kulcs
* Egy ügyfél-Azonosítót és egy Ügyfélkulcsot webalkalmazás regisztrálva az Azure Active Directoryban, amely rendelkezik Key Vault hozzáférés tooyour
* A webalkalmazás. Fogja azt megjelenítő hello lépéseket egy ASP.NET MVC alkalmazás üzembe helyezett Azure-bA egy webalkalmazást.

> [!NOTE]
> Fontos, hogy végrehajtotta hello felsorolt [Ismerkedés az Azure Key Vault](key-vault-get-started.md) ehhez az oktatóanyaghoz, hogy hello URI tooa titkos kulcs és a hello ügyfél-azonosító és a titkos ügyfélkódot webalkalmazás.
> 
> 

hello webalkalmazás, amelyek hozzáférhetnek a Key Vault hello hello egy regisztrált az Azure Active Directoryban, és megkapta-e hozzáférési tooyour Key Vault. Ha ez nem hello helyzet, lépjen vissza az alkalmazás hello első lépéseket bemutató oktatóanyaghoz tooRegister, és ismételje meg a felsorolt hello lépéseket.

Ez az oktatóanyag célja webfejlesztőknek, amely az Azure-webalkalmazások létrehozása hello alapok elsajátítása. Azure Web Apps kapcsolatos további információkért lásd: [webalkalmazások áttekintése](../app-service-web/app-service-web-overview.md).

## <a id="packages"></a>Adja hozzá a Nuget-csomagok
Két csomagot, amelyet a webes alkalmazás a toohave telepítve van.

* Active Directory Authentication Library - Azure Active Directoryban való interakció és az identitásfelügyelet metódust tartalmaz.
* Az Azure Key Vault Library - kommunikáció az Azure Key Vault metódust tartalmaz.

Mindkét ezeket a csomagokat telepítheti hello Csomagkezelő konzol hello Install-Package parancs használatával.

    // this is currently hello latest stable version of ADAL
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault


## <a id="webconfig"></a>Web.Config módosítása
Nincsenek három-alkalmazásbeállításokat, amelyek a következőképpen toobe hozzáadott toohello web.config fájl szükséges.

    <!-- ClientId and ClientSecret refer toohello web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is hello URI for hello secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />


Ha toohost az Azure Web Apps, az alkalmazás nem fog, majd adja hozzá hello tényleges ClientId, a titkos Ügyfélkulcs és a titkos kulcs URI értékek toohello web.config. Ellenkező esetben hagyja ezeket az üres értékeket, mert bővítjük hello tényleges értékek hello Azure portál egy további szintű biztonság a.

## <a id="gettoken"></a>Az Access Token módszer tooGet hozzáadása
Az order toouse hello Key Vault API kell olyan hozzáférési jogkivonatot. Key Vault ügyfél hello kezeli a Key Vault API toohello, de kell toosupply hívások azt egy függvényt, amely hello szolgáltatáshozzáférési tokent kap.  

Az alábbiakban az hello kód tooget olyan hozzáférési jogkivonatot az Azure Active Directoryból. Ez a kód bárhol lépjen az alkalmazásban. Lehet például egy Utils tooadd vagy EncryptionHelper osztály.  

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
> Egy ügyfél-azonosító és a titkos ügyfélkódot használata a hello legegyszerűbb módja tooauthenticate egy Azure AD-alkalmazást. És használja azt a webes alkalmazásban és a kulcskezelést teljesebb körű vezérlése kizárás lehetővé teszi. De azt támaszkodnak hello Ügyfélkulcs és a konfigurációs beállításaiban, amely bizonyos hello titkos kulcsot, amelyet az tooprotect a beállításokban üzembe, a kockázatos lehet. Lásd az alábbi hogyan toouse egy ügyfél-Azonosítót és az ügyfél-azonosító és a titkos ügyfélkódot tooauthenticate helyett hello Azure AD-alkalmazást a leírását.
> 
> 

## <a id="appstart"></a>Az alkalmazás lépések hello titkos beolvasása
Most azt kell toocall hello Key Vault API-kód és a hello titkos kulcs beolvasása. hello alábbira elhelyezheti bárhol, amíg azt nevezzük, mielőtt toouse kell azt. Ezzel a kóddal rendelkezik I hello alkalmazás indítás esemény hello Global.asax helyezze el, hogy az indítási egyszer fut, és elérhetővé válnak elérhetővé válik hello alkalmazás titkos kulcs hello.

    //add these using statements
    using Microsoft.Azure.KeyVault;
    using System.Web.Configuration;

    // I put my GetToken method in a Utils class. Change for wherever you placed your method.
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

    var sec = await kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]);

    //I put a variable in a Utils class toohold hello secret for general  application use.
    Utils.EncryptSecret = sec.Value;



## <a id="portalsettings"></a>Adja hozzá alkalmazásbeállításokat hello Azure Portal (nem kötelező)
Ha az Azure Web Apps hello Azure Portal most hello AppSettings hello tényleges értékeket is hozzáadhat. Ezzel a hello tényleges értékek nem lesz a hello Web.config fájlban, de a védett hello külön hozzáférés-vezérlés képességeinek amelyhez rendelkezik portálon keresztül. Ezeket az értékeket a Web.config fájlban megadott hello értékek kerülnek behelyettesítésre. Győződjön meg arról, hogy hello nevek vannak hello azonos.

![Azure portál megjeleníti-Alkalmazásbeállítások][1]

## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a>A hitelesítést egy tanúsítványt a titkos Ügyfélkulcsot helyett
Egy másik módja tooauthenticate egy Azure AD-alkalmazást az ügyfél-azonosító és a titkos Ügyfélkulcs egy ügyfél-Azonosítót és a tanúsítvány használatával. Következő lépések toouse az Azure Web Apps lévő egyik tanúsítvány vannak hello:

1. Futtasson, vagy hozzon létre egy tanúsítványt
2. Egy Azure AD-alkalmazást hello tanúsítvány társítása
3. Kód tooyour webalkalmazás toouse hello tanúsítvány hozzáadása
4. A tanúsítvány tooyour webalkalmazás hozzáadása

**Futtasson, vagy hozzon létre egy tanúsítványt** teszttanúsítványt használunk a célokra. Az alábbiakban néhány parancsok használható egy Developer-parancssorból toocreate a tanúsítványt. Azt szeretné, hogy a létrehozott hello cert fájlok directory toowhere módosítása.  A nyitó és záró dátum hello tanúsítvány hello, használja továbbá hello aktuális dátum plusz 1 év.

    makecert -sv mykey.pvk -n "cn=KVWebApp" KVWebApp.cer -b 03/07/2017 -e 03/07/2018 -r
    pvk2pfx -pvk mykey.pvk -spc KVWebApp.cer -pfx KVWebApp.pfx -po test123

Jegyezze fel a hello befejezés dátumára és hello .pfx hello jelszavát (ebben a példában: 07/31/2016 és test123). Szüksége lesz rájuk az alábbi.

Teszttanúsítványt létrehozásával kapcsolatos további információkért lásd: [Útmutató: A saját tesztelése tanúsítvány létrehozása](https://msdn.microsoft.com/library/ff699202.aspx)

**Egy Azure AD-alkalmazást a társítás hello tanúsítvány** most, hogy rendelkezik egy tanúsítvánnyal, tooassociate kell azt egy Azure AD-alkalmazást. Jelenleg hello Azure-portál nem támogatja a munkafolyamat; Ez a PowerShell használatával elvégezhető. Futtassa a következő parancsok tooassoicate hello tanúsítvány hello Azure AD alkalmazás hello:

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

Ezek a parancsok futtatása után az Azure AD hello alkalmazás tekintheti meg. Keresésekor, győződjön meg arról választja "A vállalat tulajdonában lévő alkalmazások" helyett "Alkalmazások a vállalat használ" hello keresés párbeszédpanelen.

További információ az Azure AD-alkalmazás objektumának és szolgáltatásnév objektumok toolearn lásd [alkalmazás és szolgáltatás egyszerű objektumok](../active-directory/active-directory-application-objects.md)

**Adja hozzá a kódot tooyour webalkalmazás toouse hello tanúsítvány** most adjuk hozzá kód tooyour webalkalmazás tooaccess hello cert és a hitelesítéshez használandó.

Először nincs kód tooaccess hello cert.

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


Vegye figyelembe, hogy hello StoreLocation – CurrentUser LocalMachine helyett. És, hogy "false" toohello Megadja azt metódus található, mert a vizsgálati cert használjuk.

Ezután van hello CertificateHelper használ, és létrehoz egy ClientAssertionCertificate, amelyre szükség van hitelesítési kódját.

    public static ClientAssertionCertificate AssertionCert { get; set; }

    public static void GetCert()
    {
        var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
        AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
    }


Itt található hello új kódot tooget hello hozzáférési jogkivonat. Ez a felváltja a fenti hello GetToken módszert. I adott azt egy másik nevet kényelmét szolgálja.

    public static async Task<string> GetAccessToken(string authority, string resource, string scope)
    {
        var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
        var result = await context.AcquireTokenAsync(resource, AssertionCert);
        return result.AccessToken;
    }

I rendelkezik helyezték összes ezt a kódot a webalkalmazás projekt Utils osztály a használat megkönnyítése érdekében.

hello utolsó kód módosítása hello Application_Start módszer van. Először toocall hello GetCert() metódus tooload hello ClientAssertionCertificate kell. És majd módosítjuk hello visszahívási metódus, amely azt adja meg egy új KeyVaultClient létrehozásakor. Vegye figyelembe, hogy ez a felváltja hello kódot, amely felett volt.

    Utils.GetCert();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));


**Vegye fel a tanúsítvány tooyour webalkalmazás hello Azure portálon keresztül** hozzáadása egy tanúsítvány tooyour webalkalmazás egyszerű lépésből áll. Először toohello Azure portálon lépjen, és keresse meg a webes alkalmazás tooyour. A webalkalmazás hello beállítási paneljén kattintson "egyéni tartományok és SSL" hello bejegyzés. A hello panelen megjelenő meg kell tudni tooupload hello KVWebApp.pfx a fenti létrehozott tanúsítvány, győződjön meg arról, hogy ne felejtse el hello jelszót hello pfx.

![A tanúsítvány tooa webes alkalmazás hozzáadása a hello Azure portál][2]

hello dolog, hogy kell-e toodo egy alkalmazás-beállítás tooyour hello neve webhely rendelkező webalkalmazás tooadd\_terhelés\_TANÚSÍTVÁNYOKAT és a *. Ezzel biztosíthatja, hogy minden tanúsítvány vannak betöltve. Ha tooload csak hello tanúsítványokat, amelyeket már feltöltött, majd megadhatja, hogy az ujjlenyomatok vesszővel tagolt listája.

egy tanúsítvány tooa Web App alkalmazásban hozzáadásáról további toolearn lásd [tanúsítványok használata az Azure-webhelyek alkalmazásokban](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)

**Adja hozzá a titkos kulcsot egy tanúsítvány tooKey tároló** helyett a tanúsítvány toohello Web App service közvetlenül feltöltését, tárolja Key Vault valamilyen titkos adatot, és telepítse onnan. Ez az egy kétlépéses folyamat a következő blogbejegyzésben hello [telepítése Azure Web App tanúsítványt Key Vault keresztül](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)

## <a id="next"></a>Következő lépések
Programozási hivatkozások: [Azure Key Vault C# ügyfél API-referencia](https://msdn.microsoft.com/library/azure/dn903628.aspx).

<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
