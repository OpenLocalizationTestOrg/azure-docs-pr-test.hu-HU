---
title: "a Mobile Engagement - háttérrendszer integrációs aaaAzure"
description: "Csatlakozás Azure Mobile Engagement egy SharePoint SharePoint háttér toocreate kampányait"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 06297b43-579f-46e6-8a58-961a68f9aa09
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 89e1ef57db607d63c326a760b20260ad439f08b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement---api-integration"></a>Az Azure Mobile Engagement - API-integráció
Egy automatizált marketing rendszerben létrehozása és aktiválása hello marketingkampányok is automatikusan történik. Erre a célra - Azure Mobile Engagement lehetővé teszi, hogy az ilyen API-k használatával is automatizált marketingkampányok létrehozása. 

Általában ügyfelei a marketingkampányok részeként használhatják hello a Mobile Engagement előtér felület toocreate közlemények/szavazások stb. Azonban, hello marketingkampányok érett válik, hogy szükség van tooleverage hello adatok zárolva van (például a CRM vagy CMS rendszer például SharePoint) a hello háttérrendszerek, hogy egy teljesen automatizált folyamat is létrehozható, amely Mobile kampányok hoz létre A bevonási dinamikusan áramlanak hello háttérrendszerek hello adatok alapján. 

![][5]

Az oktatóanyag is megnőnek olyan forgatókönyvekben, ahol a SharePoint üzleti felhasználók és tölti fel az értékesítési adatokat egy SharePoint-lista egy automatikus folyamat szerzi be hello listában szereplő elemeket, és hello a Mobile Engagement rendszer használatával kapcsolódik a hello elérhető REST API-k toocreate egy marketingkampányt hello SharePoint adatokból. 

> [!IMPORTANT]
> Általában használható ez a minta kiindulási pontként megértése, hogyan toocall Mobile Engagement REST API-k, mert részletek hello két fő szempontjait hello API - hitelesítő és tompított paraméter hívja. 
> 
> 

## <a name="sharepoint-integration"></a>SharePoint-integráció
1. Ez milyen hello minta SharePoint-lista tűnik. **Cím**, **kategória**, **NotificationTitle**, **üzenet** és **URL-cím** hello hirdetmény létrehozásához használt. Nincs oszlopát **IsProcessed** amellyel hello minta automation folyamat konzol program hello formában. Vagy futtathatja, a konzol program egy Azure webjobs-feladat, hogy is ütemezheti, vagy közvetlenül használható hello SharePoint munkafolyamat tooprogram létrehozása és aktiválásával hello közlemény elem behelyezésekor a hello SharePoint-listát. Ezt a mintát használjuk hello console programot, amely kerül a listában hello SharePoint hello elemek között, és bejelentés létrehozása az Azure Mobile Engagement az egyes őket, és végül jelöli meg hello **IsProcessed** jelző toobe true sikeres közlemény létrehozása.
   
    ![][1]
2. Hello kódot hello mintát használjuk *távoli hitelesítés a SharePoint Online használata hello ügyfél objektummodell* [Itt](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) tooauthenticate hello SharePoint listájával.
3. Hitelesítését követően azt ismétlése hello lista elemek toofind ki az újonnan létrehozott elemeket (amely lesz **IsProcessed** = false). 
   
         static async void CreateCampaignFromSharepoint()
        {
            using (ClientContext clientContext = ClaimClientContext.GetAuthenticatedContext(targetSharepointSite))
            {
                // Using https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c for authentication     
                Web site = clientContext.Web;
                List list = site.Lists.GetByTitle("VideoContent");
                CamlQuery query = new CamlQuery();
                query.ViewXml = "<View/>";
                ListItemCollection items = list.GetItems(query);
   
                // Load hello SharePoint list
                clientContext.Load(list);
                clientContext.Load(items);
                clientContext.ExecuteQuery();
   
                // Loop through hello list toogo through all hello items which are newly added
                foreach (ListItem item in items)
                    if (item["IsProcessed"].ToString() != "Yes")
                    {
                        string name = item["Title"].ToString();
                        string notificationTitle = item["NotificationTitle"].ToString();
                        string notificationMessage = item["Message"].ToString();
                        string category = item["Category"].ToString();
                        string actionURL = ((FieldUrlValue)item["URL"]).Url;
   
                        // Send an HTTP request toocreate AzME campaign
                        int campaignId = CreateAzMECampaign
                            (name, notificationTitle, notificationMessage, category, actionURL).Result;
   
                        if (campaignId != -1)
                        {
                            // If creating campaign is successful then set this tootrue
                            item["IsProcessed"] = "Yes";
   
                            // Now try tooactivate hello campaign also
                            await ActivateAzMECampaign(campaignId);
                        }
                        else
                        {
                            item["IsProcessed"] = "Error";
                        }
                        item.Update();
                    }
                clientContext.ExecuteQuery();
            }
        }

## <a name="mobile-engagement-integration"></a>Mobile Engagement-integráció
1. Ha egy elem, amely feldolgozást igényel találtunk – azt bontsa ki a szükséges adatokat hello toocreate hello a közlemény listaelem, és hívja meg `CreateAzMECampaign` toocreate, és ezt követően `ActivateAzMECampaign` tooactivate azt. Ezek a lényegében REST API-hívások toohello Mobile Engagement háttérrendszeréhez hívása. 
2. hello Mobile Engagement REST API-k szükséges egy **alapszintű hitelesítési séma engedélyezési HTTP-fejléc** hello áll `ApplicationId` és hello `ApiKey` amely kap hello Azure-portálon. Győződjön meg arról, hogy a hello kulcs hello használ **api-kulcsokat** szakasz és *nem* a hello **sdk kulcsok** szakasz. 
   
   ![][2]
   
       static string CreateAuthZHeader()
       {
           string BasicAuthzHeaderString = "Basic " + EncodeTo64(ApplicationId + ":" + ApiKey);
           return BasicAuthzHeaderString;
       }
   
       static public string EncodeTo64(string toEncode)
       {
           byte[] toEncodeAsBytes = System.Text.ASCIIEncoding.ASCII.GetBytes(toEncode);
           string returnValue = System.Convert.ToBase64String(toEncodeAsBytes);
           return returnValue;
       }  
3. Hello közlemény típus létrehozása a kampány – tekintse meg a toohello [dokumentáció](https://msdn.microsoft.com/library/azure/mt683750.aspx). Van szüksége arról, hogy meg hello kampány toomake `kind` , *közlemény* és hello [hasznos](https://msdn.microsoft.com/library/azure/mt683751.aspx) és FormUrlEncodedContent, átadja azt. 
   
        static async Task<int> CreateAzMECampaign(string campaignName, string notificationTitle, 
            string notificationMessage, string notificationCategory, string actionURL)
        {
            string createURIFragment = "/reach/1/create";
   
            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
   
                // Create hello payload toosend hello content
                // Reference -> https://msdn.microsoft.com/library/dn913749.aspx
                string data =
                    @"{""name"":""" + campaignName + @"""," + 
                    @"""type"":""only_notif""," + 
                    @"""deliveryTime"":""any"","" + 
                    @""notificationTitle"":""" + notificationTitle + 
                    @""",""notificationMessage"":""" + notificationMessage + 
                    @""",""actionUrl"":""" + actionURL + @"""}";
   
                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("data", data),
                });
   
                // Send hello POST request
                var response = await client.PostAsync(url + createURIFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                int campaignId = -1;
                if (response.StatusCode.ToString() == "OK")
                {
                    // This is hello campaign id
                    campaignId = Convert.ToInt32(responseString);
                    Console.WriteLine("Campaign successfully created with id {0}", campaignId);
                }
                else
                {
                    Console.WriteLine("Campaign creation failed with error - '{0}'", responseString);
                }
                return campaignId;
            }
        }
4. Ha már létrehozott hello közlemény, látni fogja a hello Mobile Engagement portálra a következő hello hasonlót (vegye figyelembe, hogy hello állapot = Vázlat és aktív = N/A)
   
    ![][3]
5. `CreateAzMECampaign`létrehoz egy bejelentési kampányt, és visszaadja a azonosító toohello hívójához. `ActivateAzMECampaign`Ezt az azonosítót igényel, hello paraméter tooactivate hello kampány. 
   
        static async Task<bool> ActivateAzMECampaign(int campaignId)
        {
            string activateUriFragment = "/reach/1/activate";
            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
   
                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("id", campaignId.ToString()),
                });
   
                // Send hello POST request
                var response = await client.PostAsync(url + activateUriFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                if (response.StatusCode.ToString() == "OK")
                {
                    Console.WriteLine("Campaign successfully activated");
                    return true;
                }
                else
                {
                    Console.WriteLine("Campaign activation failed with error - '{0}'", responseString);
                    return false;
                }
            }
        }
6. Miután aktiválta hello közlemény, látni fogja valami hasonló hello hello a Mobile Engagement portál:
   
    ![][4]
7. Amint hello kampány aktiválása lekérdezi azokat az eszközöket, amelyek megfelelnek a hello kampány hello feltételnek indul el jelent meg értesítések. 
8. Is láthatja, hogy hello listaelem megjelölve a IsProcessed = FALSE értékre van beállítva tooTrue hello közlemény kampány létrehozása után.  

Ez a minta többnyire szükséges hello tulajdonságok megadása egyszerű közlemény a kampány létrehozása. Testre szabhatja ezt, mint amennyit hello információk segítségével hello portálról is [Itt](https://msdn.microsoft.com/library/azure/mt683751.aspx). 

<!-- Images. -->
[1]: ./media/mobile-engagement-sample-backend-integration-sharepoint/sharepointlist.png
[2]: ./media/mobile-engagement-sample-backend-integration-sharepoint/properties.png
[3]: ./media/mobile-engagement-sample-backend-integration-sharepoint/new-announcement.png
[4]: ./media/mobile-engagement-sample-backend-integration-sharepoint/activate-announcement.png
[5]: ./media/mobile-engagement-sample-backend-integration-sharepoint/diagram.png



