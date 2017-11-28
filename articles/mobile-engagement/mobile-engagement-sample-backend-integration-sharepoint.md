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
# <a name="azure-mobile-engagement---api-integration"></a><span data-ttu-id="ba175-103">Az Azure Mobile Engagement - API-integráció</span><span class="sxs-lookup"><span data-stu-id="ba175-103">Azure Mobile Engagement - API integration</span></span>
<span data-ttu-id="ba175-104">Egy automatizált marketing rendszerben létrehozása és aktiválása hello marketingkampányok is automatikusan történik.</span><span class="sxs-lookup"><span data-stu-id="ba175-104">In an automated marketing system, creating and activating hello marketing campaigns also occur automatically.</span></span> <span data-ttu-id="ba175-105">Erre a célra - Azure Mobile Engagement lehetővé teszi, hogy az ilyen API-k használatával is automatizált marketingkampányok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ba175-105">For this purpose - Azure Mobile Engagement enables creating such automated marketing campaigns using APIs as well.</span></span> 

<span data-ttu-id="ba175-106">Általában ügyfelei a marketingkampányok részeként használhatják hello a Mobile Engagement előtér felület toocreate közlemények/szavazások stb.</span><span class="sxs-lookup"><span data-stu-id="ba175-106">Typically customers use hello Mobile Engagement front end interface toocreate announcements/polls etc as part of their marketing campaigns.</span></span> <span data-ttu-id="ba175-107">Azonban, hello marketingkampányok érett válik, hogy szükség van tooleverage hello adatok zárolva van (például a CRM vagy CMS rendszer például SharePoint) a hello háttérrendszerek, hogy egy teljesen automatizált folyamat is létrehozható, amely Mobile kampányok hoz létre A bevonási dinamikusan áramlanak hello háttérrendszerek hello adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="ba175-107">However as hello marketing campaigns become mature, there is a need tooleverage hello data locked in hello backend systems (like any CRM system or CMS system like SharePoint) so that a fully automated pipeline can be created which creates campaigns in Mobile Engagement dynamically based on hello data flowing in from hello backend systems.</span></span> 

![][5]

<span data-ttu-id="ba175-108">Az oktatóanyag is megnőnek olyan forgatókönyvekben, ahol a SharePoint üzleti felhasználók és tölti fel az értékesítési adatokat egy SharePoint-lista egy automatikus folyamat szerzi be hello listában szereplő elemeket, és hello a Mobile Engagement rendszer használatával kapcsolódik a hello elérhető REST API-k toocreate egy marketingkampányt hello SharePoint adatokból.</span><span class="sxs-lookup"><span data-stu-id="ba175-108">This tutorial goes through such a scenario where a SharePoint business user populates a SharePoint list with marketing data and an automated process picks up items from hello list and connects with hello Mobile Engagement system using hello available REST APIs toocreate a marketing campaign from hello SharePoint data.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="ba175-109">Általában használható ez a minta kiindulási pontként megértése, hogyan toocall Mobile Engagement REST API-k, mert részletek hello két fő szempontjait hello API - hitelesítő és tompított paraméter hívja.</span><span class="sxs-lookup"><span data-stu-id="ba175-109">In general, you can use this sample as a starting point for understanding how toocall any Mobile Engagement REST API as it details hello two key aspects of calling hello APIs - authenticating and passing parameters.</span></span> 
> 
> 

## <a name="sharepoint-integration"></a><span data-ttu-id="ba175-110">SharePoint-integráció</span><span class="sxs-lookup"><span data-stu-id="ba175-110">SharePoint integration</span></span>
1. <span data-ttu-id="ba175-111">Ez milyen hello minta SharePoint-lista tűnik.</span><span class="sxs-lookup"><span data-stu-id="ba175-111">Here is what hello sample SharePoint list looks like.</span></span> <span data-ttu-id="ba175-112">**Cím**, **kategória**, **NotificationTitle**, **üzenet** és **URL-cím** hello hirdetmény létrehozásához használt.</span><span class="sxs-lookup"><span data-stu-id="ba175-112">**Title**, **Category**, **NotificationTitle**, **Message** and **URL** are used for creating hello announcement.</span></span> <span data-ttu-id="ba175-113">Nincs oszlopát **IsProcessed** amellyel hello minta automation folyamat konzol program hello formában.</span><span class="sxs-lookup"><span data-stu-id="ba175-113">There is a column called **IsProcessed** which is used by hello sample automation process in hello form of a console program.</span></span> <span data-ttu-id="ba175-114">Vagy futtathatja, a konzol program egy Azure webjobs-feladat, hogy is ütemezheti, vagy közvetlenül használható hello SharePoint munkafolyamat tooprogram létrehozása és aktiválásával hello közlemény elem behelyezésekor a hello SharePoint-listát.</span><span class="sxs-lookup"><span data-stu-id="ba175-114">You can either run this console program as an Azure WebJob so that you can schedule it or you can directly use hello SharePoint workflow tooprogram creating and activating hello announcement when an item is inserted into hello SharePoint list.</span></span> <span data-ttu-id="ba175-115">Ezt a mintát használjuk hello console programot, amely kerül a listában hello SharePoint hello elemek között, és bejelentés létrehozása az Azure Mobile Engagement az egyes őket, és végül jelöli meg hello **IsProcessed** jelző toobe true sikeres közlemény létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ba175-115">In this sample we use hello console program which goes through hello items in hello SharePoint list and create announcement in Azure Mobile Engagement for each of them and then finally marks hello **IsProcessed** flag toobe true on successful announcement creation.</span></span>
   
    ![][1]
2. <span data-ttu-id="ba175-116">Hello kódot hello mintát használjuk *távoli hitelesítés a SharePoint Online használata hello ügyfél objektummodell* [Itt](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) tooauthenticate hello SharePoint listájával.</span><span class="sxs-lookup"><span data-stu-id="ba175-116">We are using hello code from hello sample *Remote Authentication in SharePoint Online Using hello Client Object Model* [here](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) tooauthenticate with hello SharePoint list.</span></span>
3. <span data-ttu-id="ba175-117">Hitelesítését követően azt ismétlése hello lista elemek toofind ki az újonnan létrehozott elemeket (amely lesz **IsProcessed** = false).</span><span class="sxs-lookup"><span data-stu-id="ba175-117">Once authenticated, we loop through hello list items toofind out any newly created items (which will have **IsProcessed** = false).</span></span> 
   
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

## <a name="mobile-engagement-integration"></a><span data-ttu-id="ba175-118">Mobile Engagement-integráció</span><span class="sxs-lookup"><span data-stu-id="ba175-118">Mobile Engagement integration</span></span>
1. <span data-ttu-id="ba175-119">Ha egy elem, amely feldolgozást igényel találtunk – azt bontsa ki a szükséges adatokat hello toocreate hello a közlemény listaelem, és hívja meg `CreateAzMECampaign` toocreate, és ezt követően `ActivateAzMECampaign` tooactivate azt.</span><span class="sxs-lookup"><span data-stu-id="ba175-119">Once we find an item which requires processing - we extract hello information required toocreate an announcement from hello list item and call `CreateAzMECampaign` toocreate it and subsequently `ActivateAzMECampaign` tooactivate it.</span></span> <span data-ttu-id="ba175-120">Ezek a lényegében REST API-hívások toohello Mobile Engagement háttérrendszeréhez hívása.</span><span class="sxs-lookup"><span data-stu-id="ba175-120">These are essentially REST API calls calling toohello Mobile Engagement backend.</span></span> 
2. <span data-ttu-id="ba175-121">hello Mobile Engagement REST API-k szükséges egy **alapszintű hitelesítési séma engedélyezési HTTP-fejléc** hello áll `ApplicationId` és hello `ApiKey` amely kap hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="ba175-121">hello Mobile Engagement REST APIs require a **Basic auth scheme authorization HTTP header** which is composed of hello `ApplicationId` and hello `ApiKey` which you get from hello Azure portal.</span></span> <span data-ttu-id="ba175-122">Győződjön meg arról, hogy a hello kulcs hello használ **api-kulcsokat** szakasz és *nem* a hello **sdk kulcsok** szakasz.</span><span class="sxs-lookup"><span data-stu-id="ba175-122">Make sure that you are using hello Key from hello **api keys** section and *not* from hello **sdk keys** section.</span></span> 
   
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
3. <span data-ttu-id="ba175-123">Hello közlemény típus létrehozása a kampány – tekintse meg a toohello [dokumentáció](https://msdn.microsoft.com/library/azure/mt683750.aspx).</span><span class="sxs-lookup"><span data-stu-id="ba175-123">For creating hello announcement type campaign - refer toohello [documentation](https://msdn.microsoft.com/library/azure/mt683750.aspx).</span></span> <span data-ttu-id="ba175-124">Van szüksége arról, hogy meg hello kampány toomake `kind` , *közlemény* és hello [hasznos](https://msdn.microsoft.com/library/azure/mt683751.aspx) és FormUrlEncodedContent, átadja azt.</span><span class="sxs-lookup"><span data-stu-id="ba175-124">You need toomake sure that you are specifying hello campaign `kind` as *announcement* and hello [payload](https://msdn.microsoft.com/library/azure/mt683751.aspx) and passing it as FormUrlEncodedContent.</span></span> 
   
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
4. <span data-ttu-id="ba175-125">Ha már létrehozott hello közlemény, látni fogja a hello Mobile Engagement portálra a következő hello hasonlót (vegye figyelembe, hogy hello állapot = Vázlat és aktív = N/A)</span><span class="sxs-lookup"><span data-stu-id="ba175-125">Once you have hello announcement created, you will see something like hello following on hello Mobile Engagement portal (note that hello State=Draft and Activated = N/A)</span></span>
   
    ![][3]
5. <span data-ttu-id="ba175-126">`CreateAzMECampaign`létrehoz egy bejelentési kampányt, és visszaadja a azonosító toohello hívójához.</span><span class="sxs-lookup"><span data-stu-id="ba175-126">`CreateAzMECampaign` creates an announcement campaign and returns its Id toohello caller.</span></span> <span data-ttu-id="ba175-127">`ActivateAzMECampaign`Ezt az azonosítót igényel, hello paraméter tooactivate hello kampány.</span><span class="sxs-lookup"><span data-stu-id="ba175-127">`ActivateAzMECampaign` requires this Id as hello parameter tooactivate hello campaign.</span></span> 
   
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
6. <span data-ttu-id="ba175-128">Miután aktiválta hello közlemény, látni fogja valami hasonló hello hello a Mobile Engagement portál:</span><span class="sxs-lookup"><span data-stu-id="ba175-128">Once you have hello announcement activated, you will see something like hello following on hello Mobile Engagement portal:</span></span>
   
    ![][4]
7. <span data-ttu-id="ba175-129">Amint hello kampány aktiválása lekérdezi azokat az eszközöket, amelyek megfelelnek a hello kampány hello feltételnek indul el jelent meg értesítések.</span><span class="sxs-lookup"><span data-stu-id="ba175-129">As soon as hello campaign gets activated, any devices which satisfy hello criterion on hello campaign will start seeing notifications.</span></span> 
8. <span data-ttu-id="ba175-130">Is láthatja, hogy hello listaelem megjelölve a IsProcessed = FALSE értékre van beállítva tooTrue hello közlemény kampány létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="ba175-130">You will also notice that hello list item marked with IsProcessed = false has been set tooTrue once hello announcement campaign is created.</span></span>  

<span data-ttu-id="ba175-131">Ez a minta többnyire szükséges hello tulajdonságok megadása egyszerű közlemény a kampány létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ba175-131">This sample created a simple announcement campaign specifying mostly hello required properties.</span></span> <span data-ttu-id="ba175-132">Testre szabhatja ezt, mint amennyit hello információk segítségével hello portálról is [Itt](https://msdn.microsoft.com/library/azure/mt683751.aspx).</span><span class="sxs-lookup"><span data-stu-id="ba175-132">You can customize this as much as you can from hello portal by using hello information [here](https://msdn.microsoft.com/library/azure/mt683751.aspx).</span></span> 

<!-- Images. -->
[1]: ./media/mobile-engagement-sample-backend-integration-sharepoint/sharepointlist.png
[2]: ./media/mobile-engagement-sample-backend-integration-sharepoint/properties.png
[3]: ./media/mobile-engagement-sample-backend-integration-sharepoint/new-announcement.png
[4]: ./media/mobile-engagement-sample-backend-integration-sharepoint/activate-announcement.png
[5]: ./media/mobile-engagement-sample-backend-integration-sharepoint/diagram.png



