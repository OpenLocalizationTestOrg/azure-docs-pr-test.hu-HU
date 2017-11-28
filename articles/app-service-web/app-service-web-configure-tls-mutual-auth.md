---
title: "aaaHow tooConfigure TLS a kölcsönös hitelesítés a webalkalmazás számára"
description: "Ismerje meg, hogyan tooconfigure a webalkalmazás ügyféloldalának toouse Tanúsítványalapú hitelesítés a TLS."
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: jimbe
ms.assetid: cd1d15d3-2d9e-4502-9f11-a306dac4453a
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2016
ms.author: naziml
ms.openlocfilehash: 8aeb9b35058fac50b8b38f6428207ad4a82d8637
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-tls-mutual-authentication-for-web-app"></a><span data-ttu-id="8ebdd-103">Hogyan tooConfigure TLS a kölcsönös hitelesítés a webalkalmazás számára</span><span class="sxs-lookup"><span data-stu-id="8ebdd-103">How tooConfigure TLS Mutual Authentication for Web App</span></span>
## <a name="overview"></a><span data-ttu-id="8ebdd-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="8ebdd-104">Overview</span></span>
<span data-ttu-id="8ebdd-105">Korlátozhatja a hozzáférést tooyour Azure-webalkalmazásban különböző hitelesítési az engedélyezésével.</span><span class="sxs-lookup"><span data-stu-id="8ebdd-105">You can restrict access tooyour Azure web app by enabling different types of authentication for it.</span></span> <span data-ttu-id="8ebdd-106">Egyirányú toodo ezért nem használ tanúsítványt, a TLS/SSL hello kérés esetén tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="8ebdd-106">One way toodo so is tooauthenticate using a client certificate when hello request is over TLS/SSL.</span></span> <span data-ttu-id="8ebdd-107">Ez az eljárás meghívása TLS kölcsönös hitelesítés vagy ügyféltanúsítvány-alapú hitelesítés, és ez a cikk részletesen ismerteti hogyan toosetup a webes alkalmazás toouse ügyféltanúsítvány-alapú hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="8ebdd-107">This mechanism is called TLS mutual authentication or client certificate authentication and this article will detail how toosetup your web app toouse client certificate authentication.</span></span>

> <span data-ttu-id="8ebdd-108">**Megjegyzés:** a webhely HTTP és HTTPS protokollt használó nem fér hozzá, addig nem kap minden ügyfél-tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="8ebdd-108">**Note:** If you access your site over HTTP and not HTTPS, you will not receive any client certificate.</span></span> <span data-ttu-id="8ebdd-109">Így ha az alkalmazás ügyfél-tanúsítványok nem engedélyezze kérelmek tooyour alkalmazás HTTP Protokollon keresztül.</span><span class="sxs-lookup"><span data-stu-id="8ebdd-109">So if your application requires client certificates you should not allow requests tooyour application over HTTP.</span></span>
> 
> 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="configure-web-app-for-client-certificate-authentication"></a><span data-ttu-id="8ebdd-110">Ügyféltanúsítvány-alapú hitelesítés a webalkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8ebdd-110">Configure Web App for Client Certificate Authentication</span></span>
<span data-ttu-id="8ebdd-111">a webes alkalmazás toorequire ügyféltanúsítványok tooadd van szüksége a webalkalmazás beállítását, majd állítsa be tootrue clientCertEnabled hely hello toosetup.</span><span class="sxs-lookup"><span data-stu-id="8ebdd-111">toosetup your web app toorequire client certificates you need tooadd hello clientCertEnabled site setting for your web app and set it tootrue.</span></span> <span data-ttu-id="8ebdd-112">Ez a beállítás már nem érhető el hello kezelési élményt hello portálon keresztül, és a REST API hello szüksége lesz rá használt toobe tooaccomplish.</span><span class="sxs-lookup"><span data-stu-id="8ebdd-112">This setting is not currently available through hello management experience in hello Portal, and hello REST API will need toobe used tooaccomplish this.</span></span>

<span data-ttu-id="8ebdd-113">Használhatja a hello [ARMClient eszköz](https://github.com/projectkudu/ARMClient) toomake azt könnyen toocraft hello REST API-hívás.</span><span class="sxs-lookup"><span data-stu-id="8ebdd-113">You can use hello [ARMClient tool](https://github.com/projectkudu/ARMClient) toomake it easy toocraft hello REST API call.</span></span> <span data-ttu-id="8ebdd-114">Hello eszköz bejelentkezés után a következő parancs tooissue hello lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="8ebdd-114">After you log in with hello tool you will need tooissue hello following command:</span></span>

    ARMClient PUT subscriptions/{Subscription Id}/resourcegroups/{Resource Group Name}/providers/Microsoft.Web/sites/{Website Name}?api-version=2015-04-01 @enableclientcert.json -verbose

<span data-ttu-id="8ebdd-115">cseréje mindent megkeresése és a webalkalmazás vonatkozó információkat, és a fájl létrehozásakor a következő nevű enableclientcert.json a következő JSON hello tartalom:</span><span class="sxs-lookup"><span data-stu-id="8ebdd-115">replacing everything in {} with information for your web app and creating a file called enableclientcert.json with hello following JSON content:</span></span>

    {
        "location": "My Web App Location",
        "properties": {
            "clientCertEnabled": true
        }
    }

<span data-ttu-id="8ebdd-116">Győződjön meg arról a webalkalmazás "hely" toowherever toochange hello értékének például északi középső Régiójában, vagy nyugati USA stb.</span><span class="sxs-lookup"><span data-stu-id="8ebdd-116">Make sure toochange hello value of "location" toowherever your web app is located e.g. North Central US or West US etc.</span></span>

<span data-ttu-id="8ebdd-117">Is használhatja a https://resources.azure.com tooflip hello `clientCertEnabled` tulajdonság túl`true`.</span><span class="sxs-lookup"><span data-stu-id="8ebdd-117">You can also use https://resources.azure.com tooflip hello `clientCertEnabled` property too`true`.</span></span>

> <span data-ttu-id="8ebdd-118">**Megjegyzés:** ARMClient Powershell futtatja, ha szüksége lesz tooescape hello @ jel hello JSON-fájl, a háttérben osztásjelek ".</span><span class="sxs-lookup"><span data-stu-id="8ebdd-118">**Note:** If you run ARMClient from Powershell, you will need tooescape hello @ symbol for hello JSON file with a back tick \`.</span></span>
> 
> 

## <a name="accessing-hello-client-certificate-from-your-web-app"></a><span data-ttu-id="8ebdd-119">Hello ügyfél tanúsítvány a a webes alkalmazás elérése</span><span class="sxs-lookup"><span data-stu-id="8ebdd-119">Accessing hello Client Certificate From Your Web App</span></span>
<span data-ttu-id="8ebdd-120">Ha ASP.NET használ, és konfigurálja az alkalmazás toouse ügyféltanúsítvány-alapú hitelesítés, a hello tanúsítvány hello érhető el **HttpRequest.ClientCertificate** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="8ebdd-120">If you are using ASP.NET and configure your app toouse client certificate authentication, hello certificate will be available through hello **HttpRequest.ClientCertificate** property.</span></span> <span data-ttu-id="8ebdd-121">Más alkalmazás verem hello ügyféltanúsítványt keresztül hello "X-ARR-ClientCert" fejléc base64-kódolású értéket az alkalmazás elérhető lesz.</span><span class="sxs-lookup"><span data-stu-id="8ebdd-121">For other application stacks, hello client cert will be available in your app through a base64 encoded value in hello "X-ARR-ClientCert" request header.</span></span> <span data-ttu-id="8ebdd-122">Az alkalmazás hozzon létre egy tanúsítványt az ezt az értéket, és majd a hitelesítés és engedélyezés céljából az alkalmazás használatával.</span><span class="sxs-lookup"><span data-stu-id="8ebdd-122">Your application can create a certificate from this value and then use it for authentication and authorization purposes in your application.</span></span>

## <a name="special-considerations-for-certificate-validation"></a><span data-ttu-id="8ebdd-123">Különleges szempontok a tanúsítvány érvényesítése</span><span class="sxs-lookup"><span data-stu-id="8ebdd-123">Special Considerations for Certificate Validation</span></span>
<span data-ttu-id="8ebdd-124">hello ügyféltanúsítvány toohello alkalmazás küldött nem halad át egyetlen ellenőrzési hello Azure Web Apps platform.</span><span class="sxs-lookup"><span data-stu-id="8ebdd-124">hello client certificate that is sent toohello application does not go through any validation by hello Azure Web Apps platform.</span></span> <span data-ttu-id="8ebdd-125">Ez a tanúsítvány érvényesítése feladata hello hello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8ebdd-125">Validating this certificate is hello responsibility of hello web app.</span></span> <span data-ttu-id="8ebdd-126">Itt található ASP.NET mintakód, amely ellenőrzi a tanúsítvány tulajdonságai hitelesítési célokra.</span><span class="sxs-lookup"><span data-stu-id="8ebdd-126">Here is sample ASP.NET code that validates certificate properties for authentication purposes.</span></span>

    using System;
    using System.Collections.Specialized;
    using System.Security.Cryptography.X509Certificates;
    using System.Web;

    namespace ClientCertificateUsageSample
    {
        public partial class cert : System.Web.UI.Page
        {
            public string certHeader = "";
            public string errorString = "";
            private X509Certificate2 certificate = null;
            public string certThumbprint = "";
            public string certSubject = "";
            public string certIssuer = "";
            public string certSignatureAlg = "";
            public string certIssueDate = "";
            public string certExpiryDate = "";
            public bool isValidCert = false;

            //
            // Read hello certificate from hello header into an X509Certificate2 object
            // Display properties of hello certificate on hello page
            //
            protected void Page_Load(object sender, EventArgs e)
            {
                NameValueCollection headers = base.Request.Headers;
                certHeader = headers["X-ARR-ClientCert"];
                if (!String.IsNullOrEmpty(certHeader))
                {
                    try
                    {
                        byte[] clientCertBytes = Convert.FromBase64String(certHeader);
                        certificate = new X509Certificate2(clientCertBytes);
                        certSubject = certificate.Subject;
                        certIssuer = certificate.Issuer;
                        certThumbprint = certificate.Thumbprint;
                        certSignatureAlg = certificate.SignatureAlgorithm.FriendlyName;
                        certIssueDate = certificate.NotBefore.ToShortDateString() + " " + certificate.NotBefore.ToShortTimeString();
                        certExpiryDate = certificate.NotAfter.ToShortDateString() + " " + certificate.NotAfter.ToShortTimeString();
                    }
                    catch (Exception ex)
                    {
                        errorString = ex.ToString();
                    }
                    finally 
                    {
                        isValidCert = IsValidClientCertificate();
                        if (!isValidCert) Response.StatusCode = 403;
                        else Response.StatusCode = 200;
                    }
                }
                else
                {
                    certHeader = "";
                }
            }

            //
            // This is a SAMPLE verification routine. Depending on your application logic and security requirements, 
            // you should modify this method
            //
            private bool IsValidClientCertificate()
            {
                // In this example we will only accept hello certificate as a valid certificate if all hello conditions below are met:
                // 1. hello certificate is not expired and is active for hello current time on server.
                // 2. hello subject name of hello certificate has hello common name nildevecc
                // 3. hello issuer name of hello certificate has hello common name nildevecc and organization name Microsoft Corp
                // 4. hello thumbprint of hello certificate is 30757A2E831977D8BD9C8496E4C99AB26CB9622B
                //
                // This example does NOT test that this certificate is chained tooa Trusted Root Authority (or revoked) on hello server 
                // and it allows for self signed certificates
                //

                if (certificate == null || !String.IsNullOrEmpty(errorString)) return false;

                // 1. Check time validity of certificate
                if (DateTime.Compare(DateTime.Now, certificate.NotBefore) < 0 || DateTime.Compare(DateTime.Now, certificate.NotAfter) > 0) return false;

                // 2. Check subject name of certificate
                bool foundSubject = false;
                string[] certSubjectData = certificate.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certSubjectData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundSubject = true;
                        break;
                    }
                }
                if (!foundSubject) return false;

                // 3. Check issuer name of certificate
                bool foundIssuerCN = false, foundIssuerO = false;
                string[] certIssuerData = certificate.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certIssuerData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundIssuerCN = true;
                        if (foundIssuerO) break;
                    }

                    if (String.Compare(s.Trim(), "O=Microsoft Corp") == 0)
                    {
                        foundIssuerO = true;
                        if (foundIssuerCN) break;
                    }
                }

                if (!foundIssuerCN || !foundIssuerO) return false;

                // 4. Check thumprint of certificate
                if (String.Compare(certificate.Thumbprint.Trim().ToUpper(), "30757A2E831977D8BD9C8496E4C99AB26CB9622B") != 0) return false;

                // If you also want tootest if hello certificate chains tooa Trusted Root Authority you can uncomment hello code below
                //
                //X509Chain certChain = new X509Chain();
                //certChain.Build(certificate);
                //bool isValidCertChain = true;
                //foreach (X509ChainElement chElement in certChain.ChainElements)
                //{
                //    if (!chElement.Certificate.Verify())
                //    {
                //        isValidCertChain = false;
                //        break;
                //    }
                //}
                //if (!isValidCertChain) return false;

                return true;
            }
        }
    }
