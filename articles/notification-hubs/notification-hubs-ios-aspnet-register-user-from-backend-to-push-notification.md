---
title: "aaaRegister hello aktuális felhasználó a leküldéses értesítések webes API használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan toorequest leküldéses értesítés regisztrálása az Azure Notification Hubs iOS-alkalmazást a regisztrálás ASP.NET Web API végrehajtásakor."
services: notification-hubs
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
ms.assetid: 4e3772cf-20db-4b9f-bb74-886adfaaa65d
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: f859feb436093e703d7e1db38354dd356fff8efe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="register-hello-current-user-for-push-notifications-by-using-aspnet"></a>ASP.NET segítségével hello aktuális felhasználó a leküldéses értesítések regisztrálása
> [!div class="op_single_selector"]
> * [iOS](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
> 
> 

## <a name="overview"></a>Áttekintés
Ez a témakör bemutatja, toorequest a leküldéses értesítés regisztrálása az Azure Notification Hubs regisztrációs ASP.NET Web API végrehajtásakor. Ez a témakör bővíti hello oktatóanyag [értesítse a felhasználókat a Notification hubs használatával]. Már végrehajtotta a oktatóanyag toocreate hitelesített hello mobilszolgáltatásban hello szükséges lépéseket. További információt a hello felhasználók forgatókönyv értesíti, lásd: [értesítse a felhasználókat a Notification hubs használatával].

## <a name="update-your-app"></a>Az alkalmazás frissítése
1. Adja hozzá a MainStoryboard_iPhone.storyboard hello összetevők hello objektum könyvtárból a következő:
   
   * **Címke**: "Push Notification Hubs szolgáltatással tooUser"
   * **Címke**: "Végrehajtott"
   * **Címke**: "User"
   * **Szövegmező**: "User"
   * **Címke**: "Password"
   * **Szövegmező**: "Password"
   * **Gomb**: "Bejelentkezés"
     
     Ezen a ponton a storyboard következőhöz hasonló hello:
     
      ![][0]
2. Hello Segéd-szerkesztőben hozzon létre az összes kapcsolt hello vezérlők kimeneteket, keresheti őket, hello szövegmezők kapcsolattartásnak hello nézetvezérlő (delegált), és hozzon létre egy **művelet** a hello **bejelentkezési** gombra.
   
       ![][1]
   
       Your BreakingNewsViewController.h file should now contain hello following code:
   
        @property (weak, nonatomic) IBOutlet UILabel *installationId;
        @property (weak, nonatomic) IBOutlet UITextField *User;
        @property (weak, nonatomic) IBOutlet UITextField *Password;
   
        - (IBAction)login:(id)sender;
3. Hozzon létre egy osztályt **deviceinfo információja**, és a következő kód másolása hello hello fájl DeviceInfo.h hello felület szakaszba:
   
        @property (readonly, nonatomic) NSString* installationId;
        @property (nonatomic) NSData* deviceToken;
4. Másolja a következő kód hello implementation szakaszban hello DeviceInfo.m fájl hello:
   
            @synthesize installationId = _installationId;
   
            - (id)init {
                if (!(self = [super init]))
                    return nil;
   
                NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
                _installationId = [defaults stringForKey:@"PushToUserInstallationId"];
                if(!_installationId) {
                    CFUUIDRef newUUID = CFUUIDCreate(kCFAllocatorDefault);
                    _installationId = (__bridge_transfer NSString *)CFUUIDCreateString(kCFAllocatorDefault, newUUID);
                    CFRelease(newUUID);
   
                    //store hello install ID so we don't generate a new one next time
                    [defaults setObject:_installationId forKey:@"PushToUserInstallationId"];
                    [defaults synchronize];
                }
   
                return self;
            }
   
            - (NSString*)getDeviceTokenInHex {
                const unsigned *tokenBytes = [[self deviceToken] bytes];
                NSString *hexToken = [NSString stringWithFormat:@"%08X%08X%08X%08X%08X%08X%08X%08X",
                                      ntohl(tokenBytes[0]), ntohl(tokenBytes[1]), ntohl(tokenBytes[2]),
                                      ntohl(tokenBytes[3]), ntohl(tokenBytes[4]), ntohl(tokenBytes[5]),
                                      ntohl(tokenBytes[6]), ntohl(tokenBytes[7])];
                return hexToken;
            }
5. PushToUserAppDelegate.h adja hozzá a következő tulajdonság egypéldányos hello:
   
        @property (strong, nonatomic) DeviceInfo* deviceInfo;
6. A hello **didFinishLaunchingWithOptions** metódus a PushToUserAppDelegate.m, adja hozzá a következő kód hello:
   
        self.deviceInfo = [[DeviceInfo alloc] init];
   
        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
   
    első sor hello inicializálja hello **deviceinfo információja** egypéldányos. hello második sor indítása hello regisztráció a leküldéses értesítések, amelyek már jelen van az hello már befejeződött [Ismerkedés a Notification Hubs] oktatóanyag.
7. A PushToUserAppDelegate.m, valósítja meg hello **didRegisterForRemoteNotificationsWithDeviceToken** a AppDelegate a, és adja hozzá a következő kód hello:
   
        self.deviceInfo.deviceToken = deviceToken;
   
    Ez a beállítás hello eszköz jogkivonatát hello kérelem.
   
   > [!NOTE]
   > Ezen a ponton nem lehetnek többi kód az ezzel a módszerrel. Ha már rendelkezik egy hívás toohello **registerNativeWithDeviceToken** hello befejezésekor hozzáadott metódus [Ismerkedés a Notification Hubs](/manage/services/notification-hubs/get-started-notification-hubs-ios/) kell Megjegyzés kimenő, vagy távolítsa el, amely útmutató hívja meg.
   > 
   > 
8. Hello PushToUserAppDelegate.m fájlban adja hozzá a következő leíró metódus hello:
   
   * (a "void") alkalmazás:(UIApplication *) alkalmazás didReceiveRemoteNotification:(NSDictionary *) userInfo {NSLog (@"% @", userInfo);   UIAlertView * riasztás = [[UIAlertView foglalási] initWithTitle:@"Notification" üzenetet: [userInfo objectForKey:@"inAppMessage"] delegált: üres cancelButtonTitle: @"OK" otherButtonTitles:nil, üres];   [riasztás megjelenítése]; }
   
   Ezt a módszert a felhasználói felület hello megjelenít egy riasztást, amikor az alkalmazás futtatása értesítéseket fogad.
9. Nyissa meg a végrehajtása a következő hello hello PushToUserViewController.m fájlt, és visszatérési hello billentyűzet:
   
        - (BOOL)textFieldShouldReturn:(UITextField *)theTextField {
            if (theTextField == self.User || theTextField == self.Password) {
                [theTextField resignFirstResponder];
            }
            return YES;
        }
10. A hello **viewDidLoad** hello PushToUserViewController.m fájlban metódus inicializálása hello végrehajtott címke az alábbiak szerint:
    
         DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
         Self.installationId.text = deviceInfo.installationId;
11. Adja hozzá a következő felületen a PushToUserViewController.m tulajdonságai hello:
    
        @property (readonly) NSOperationQueue* downloadQueue;
        - (NSString*)base64forData:(NSData*)theData;
12. Adja hozzá a következő megvalósítási hello:
    
            - (NSOperationQueue *)downloadQueue {
                if (!_downloadQueue) {
                    _downloadQueue = [[NSOperationQueue alloc] init];
                    _downloadQueue.name = @"Download Queue";
                    _downloadQueue.maxConcurrentOperationCount = 1;
                }
                return _downloadQueue;
            }
    
            // base64 encoding
            - (NSString*)base64forData:(NSData*)theData
            {
                const uint8_t* input = (const uint8_t*)[theData bytes];
                NSInteger length = [theData length];
    
                static char table[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";
    
                NSMutableData* data = [NSMutableData dataWithLength:((length + 2) / 3) * 4];
                uint8_t* output = (uint8_t*)data.mutableBytes;
    
                NSInteger i;
                for (i=0; i < length; i += 3) {
                    NSInteger value = 0;
                    NSInteger j;
                    for (j = i; j < (i + 3); j++) {
                        value <<= 8;
    
                        if (j < length) {
                            value |= (0xFF & input[j]);
                        }
                    }
    
                    NSInteger theIndex = (i / 3) * 4;
                    output[theIndex + 0] =                    table[(value >> 18) & 0x3F];
                    output[theIndex + 1] =                    table[(value >> 12) & 0x3F];
                    output[theIndex + 2] = (i + 1) < length ? table[(value >> 6)  & 0x3F] : '=';
                    output[theIndex + 3] = (i + 2) < length ? table[(value >> 0)  & 0x3F] : '=';
                }
    
                return [[NSString alloc] initWithData:data encoding:NSASCIIStringEncoding];
            }
13. Másolás hello következő hello kódot **bejelentkezési** eseménykezelő metódus hozta létre xcode-ban:
    
            DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
    
            // build JSON
            NSString* json = [NSString stringWithFormat:@"{\"platform\":\"ios\", \"instId\":\"%@\", \"deviceToken\":\"%@\"}", deviceInfo.installationId, [deviceInfo getDeviceTokenInHex]];
    
            // build auth string
            NSString* authString = [NSString stringWithFormat:@"%@:%@", self.User.text, self.Password.text];
    
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"http://nhnotifyuser.azurewebsites.net/api/register"]];
            [request setHTTPMethod:@"POST"];
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
            [request addValue:[@([json lengthOfBytesUsingEncoding:NSUTF8StringEncoding]) description] forHTTPHeaderField:@"Content-Length"];
            [request addValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
            [request addValue:[NSString stringWithFormat:@"Basic %@",[self base64forData:[authString dataUsingEncoding:NSUTF8StringEncoding]]] forHTTPHeaderField:@"Authorization"];
    
            // connect with POST
            [NSURLConnection sendAsynchronousRequest:request queue:[self downloadQueue] completionHandler:^(NSURLResponse* response, NSData* data, NSError* error) {
                // add UIAlert depending on response.
                if (error != nil) {
                    NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
                    if ([httpResponse statusCode] == 200) {
                        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Back-end registration" message:@"Registration successful" delegate:nil cancelButtonTitle: @"OK" otherButtonTitles:nil, nil];
                        [alert show];
                    } else {
                        NSLog(@"status: %ld", (long)[httpResponse statusCode]);
                    }
                } else {
                    NSLog(@"error: %@", error);
                }
            }];
    
    Ez a módszer egy telepítési Azonosítót és a csatorna lekérdezi a leküldéses értesítések, és elküldi azt, és hello eszköztípus, toohello hitelesítése egy regisztrációs létrehozó a Notification Hubs módszer a webes API. A Web API lett definiálva [értesítse a felhasználókat a Notification hubs használatával].

Most, hogy hello ügyfélalkalmazás frissítették, térjen vissza a toohello [értesítse a felhasználókat a Notification hubs használatával] és frissítési hello mobilszolgáltatás toosend értesítések a Notification Hubs használatával.

<!-- Anchors. -->

<!-- Images. -->
[0]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios1.png
[1]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios2.png

<!-- URLs. -->
[értesítse a felhasználókat a Notification hubs használatával]: /manage/services/notification-hubs/notify-users-aspnet

[Ismerkedés a Notification Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-ios
