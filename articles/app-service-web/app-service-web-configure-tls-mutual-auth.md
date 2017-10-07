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
# <a name="how-tooconfigure-tls-mutual-authentication-for-web-app"></a>Hogyan tooConfigure TLS a kölcsönös hitelesítés a webalkalmazás számára
## <a name="overview"></a>Áttekintés
Korlátozhatja a hozzáférést tooyour Azure-webalkalmazásban különböző hitelesítési az engedélyezésével. Egyirányú toodo ezért nem használ tanúsítványt, a TLS/SSL hello kérés esetén tooauthenticate. Ez az eljárás meghívása TLS kölcsönös hitelesítés vagy ügyféltanúsítvány-alapú hitelesítés, és ez a cikk részletesen ismerteti hogyan toosetup a webes alkalmazás toouse ügyféltanúsítvány-alapú hitelesítés.

> **Megjegyzés:** a webhely HTTP és HTTPS protokollt használó nem fér hozzá, addig nem kap minden ügyfél-tanúsítványt. Így ha az alkalmazás ügyfél-tanúsítványok nem engedélyezze kérelmek tooyour alkalmazás HTTP Protokollon keresztül.
> 
> 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="configure-web-app-for-client-certificate-authentication"></a>Ügyféltanúsítvány-alapú hitelesítés a webalkalmazás konfigurálása
a webes alkalmazás toorequire ügyféltanúsítványok tooadd van szüksége a webalkalmazás beállítását, majd állítsa be tootrue clientCertEnabled hely hello toosetup. Ez a beállítás már nem érhető el hello kezelési élményt hello portálon keresztül, és a REST API hello szüksége lesz rá használt toobe tooaccomplish.

Használhatja a hello [ARMClient eszköz](https://github.com/projectkudu/ARMClient) toomake azt könnyen toocraft hello REST API-hívás. Hello eszköz bejelentkezés után a következő parancs tooissue hello lesz szüksége:

    ARMClient PUT subscriptions/{Subscription Id}/resourcegroups/{Resource Group Name}/providers/Microsoft.Web/sites/{Website Name}?api-version=2015-04-01 @enableclientcert.json -verbose

cseréje mindent megkeresése és a webalkalmazás vonatkozó információkat, és a fájl létrehozásakor a következő nevű enableclientcert.json a következő JSON hello tartalom:

    {
        "location": "My Web App Location",
        "properties": {
            "clientCertEnabled": true
        }
    }

Győződjön meg arról a webalkalmazás "hely" toowherever toochange hello értékének például északi középső Régiójában, vagy nyugati USA stb.

Is használhatja a https://resources.azure.com tooflip hello `clientCertEnabled` tulajdonság túl`true`.

> **Megjegyzés:** ARMClient Powershell futtatja, ha szüksége lesz tooescape hello @ jel hello JSON-fájl, a háttérben osztásjelek ".
> 
> 

## <a name="accessing-hello-client-certificate-from-your-web-app"></a>Hello ügyfél tanúsítvány a a webes alkalmazás elérése
Ha ASP.NET használ, és konfigurálja az alkalmazás toouse ügyféltanúsítvány-alapú hitelesítés, a hello tanúsítvány hello érhető el **HttpRequest.ClientCertificate** tulajdonság. Más alkalmazás verem hello ügyféltanúsítványt keresztül hello "X-ARR-ClientCert" fejléc base64-kódolású értéket az alkalmazás elérhető lesz. Az alkalmazás hozzon létre egy tanúsítványt az ezt az értéket, és majd a hitelesítés és engedélyezés céljából az alkalmazás használatával.

## <a name="special-considerations-for-certificate-validation"></a>Különleges szempontok a tanúsítvány érvényesítése
hello ügyféltanúsítvány toohello alkalmazás küldött nem halad át egyetlen ellenőrzési hello Azure Web Apps platform. Ez a tanúsítvány érvényesítése feladata hello hello webalkalmazás. Itt található ASP.NET mintakód, amely ellenőrzi a tanúsítvány tulajdonságai hitelesítési célokra.

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
