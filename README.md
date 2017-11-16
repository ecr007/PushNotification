# PushNotification

## Register and get RemoteNotification in ios 10 above and below ##

#### import UserNotifications and add UserNotifications.framework into general settings ####
```
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
        if #available(iOS 10, *)
        { // iOS 10 support
            //create the notificationCenter
            let center = UNUserNotificationCenter.current()
            center.delegate = self
            // set the type as sound or badge
            center.requestAuthorization(options: [.sound,.alert,.badge]) { (granted, error) in
                if granted {
                    print("Notification Enable Successfully")
                }else{
                    print("Some Error Occure")
                }
            }
            application.registerForRemoteNotifications()
        }
        else if #available(iOS 9, *)
        {
            // iOS 9 support
            UIApplication.shared.registerUserNotificationSettings(UIUserNotificationSettings(types: [.badge, .sound, .alert], categories: nil))
            UIApplication.shared.registerForRemoteNotifications()
        }
        else if #available(iOS 8, *)
        {
            // iOS 8 support
            UIApplication.shared.registerUserNotificationSettings(UIUserNotificationSettings(types: [.badge, .sound,
                                                                                                     .alert], categories: nil))
            UIApplication.shared.registerForRemoteNotifications()
        }
        else
        { // iOS 7 support
            application.registerForRemoteNotifications(matching: [.badge, .sound, .alert])
        }
        return true
    }
    
    
    //get device token here
    func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken
        deviceToken: Data)
    {
        var token = ""
        for i in 0..<deviceToken.count {
            token = token + String(format: "%02.2hhx", arguments: [deviceToken[i]])
        }
        print("Registration succeeded!")
        print("Token: ", token)
    }
    
    //get error here
    func application(_ application: UIApplication, didFailToRegisterForRemoteNotificationsWithError error:
        Error) {
        print("Registration failed!")
    }
    
    //get Notification Here below ios 10
    func application(_ application: UIApplication, didReceiveRemoteNotification data: [AnyHashable : Any]) {
        // Print notification payload data
        print("Push notification received: \(data)")
    }
    
    //This is the two delegate method to get the notification in iOS 10..
    //First for foreground
    @available(iOS 10.0, *)
    func userNotificationCenter(_ center: UNUserNotificationCenter, willPresent notification: UNNotification, withCompletionHandler completionHandler: @escaping (_ options:UNNotificationPresentationOptions) -> Void)
    {
        print("Handle push from foreground")
        // custom code to handle push while app is in the foreground
        print("\(notification.request.content.userInfo)")
    }
    //Second for background and close
    @available(iOS 10.0, *)
    func userNotificationCenter(_ center: UNUserNotificationCenter, didReceive response:UNNotificationResponse, withCompletionHandler completionHandler: @escaping () -> Void)
    {
        print("Handle push from background or closed")
        // if you set a member variable in didReceiveRemoteNotification, you will know if this is from closed or background
        print("\(response.notification.request.content.userInfo)")
    }
```
