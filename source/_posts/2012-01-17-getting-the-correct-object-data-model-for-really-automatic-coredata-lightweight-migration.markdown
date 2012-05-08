---
layout: post
title: "Getting the correct object data model for really automatic CoreData lightweight migrations"
date: 2012-01-17 09:31
comments: true
categories: [ios, objective-c, coredata, migration]

---
If you're using CoreData for your iOS application, you may have the need sometimes **to update your data model**. If your application is already live (e.g. shipped on the AppStore), you will have to **migrate your existing data when updating the model**.

## Lightweight migration

As long as your migration is not too complicated, **CoreData provides a simple way to do this**, using what they call the **lightweight migration process**. This process is not-that-bad documented in Apple documentation's *(ref 1)*, but... there is a piece of code lacking, and making the whole thing "automatic" was not so easy...

## Automatic migration

The automatic-nature of the process is given by the creation of a new version of your Data Model in XCode, and telling it which version is the current one (see *ref 2* for Apple's documentation on this matter).

Now that you've created your second version, you may want to use documentation's code sample to build your lightweight migration process:

``` objc
NSError *error;
NSURL *storeURL = <#The URL of a persistent store#>;
NSPersistentStoreCoordinator *psc = <#The coordinator#>;
NSDictionary *options = [NSDictionary dictionaryWithObjectsAndKeys:
    [NSNumber numberWithBool:YES], NSMigratePersistentStoresAutomaticallyOption,
    [NSNumber numberWithBool:YES], NSInferMappingModelAutomaticallyOption, nil];
if (![psc addPersistentStoreWithType:<#Store type#> configuration:<#Configuration or nil#> URL:storeURL options:options error:&error]) {
  // Handle the error.
}
```

At this stage, most of us already have a way of getting the `NSManagedObjectModel` instance required to build a `NSPersistentCoordinator` instance. This generally involves this code (often put in the application's delegate):

``` objc
- (NSManagedObjectModel *)managedObjectModel {
  if (managedObjectModel != nil) {
    return managedObjectModel;
  }
  managedObjectModel = [[NSManagedObjectModel mergedModelFromBundles:nil] retain];
  return managedObjectModel;
}
```

The issue with this is that you're gonna get a "`Can't merge models with two different entities named 'EntityName'`" exception, because you're in fact merging the two versions of your model... **And here broke the automaticity!**

You indeed can't merge your two model versions. What you want is your current model version to get loaded into the `NSPersistentStoreCoordinator`. Here you have two choices:

1. Everything I could find on the web was giving me a solution requiring to manually enter the name of the new model version so that I could load the correct model version. But hey! I'm telling XCode what the current version is, shouldn't it be enough?
2. Your second solution, the truly automatic one is here, just one line away...

``` objc Load the current NSManagedObjectModel using XCode generated VersionInfo property list
- (NSManagedObjectModel *)currentManagedObjectModel {
  // Find and open the .plist file with the current version name
  // Path to the plist (in the application bundle)
  NSString *pathToDataModelVersionInfoPlist = [[NSBundle mainBundle] pathForResource:@"VersionInfo" ofType:@"plist" inDirectory:@"DataModel.momd"];
  NSDictionary *dataModelVersionInfo = [[NSDictionary alloc] initWithContentsOfFile:pathToDataModelVersionInfoPlist];
  NSString *currentModelName = [dataModelVersionInfo objectForKey:@"NSManagedObjectModel_CurrentVersionName"];
  NSString *modelPath = [[NSBundle mainBundle] pathForResource:currentModelName ofType:@"mom" inDirectory:@"DataModel.momd"];
  NSURL *modelURL = [NSURL fileURLWithPath:modelPath];
  return [[[NSManagedObjectModel alloc] initWithContentsOfURL:modelURL] autorelease];
}
```

With this small piece of code, you will read the `VersionInfo.plist` file maintained by XCode which contains the reference to the current data model. So now, each time you set a new data model version, it will get chosen automatically and migrated without having to update you code!

**NB: This has not been tested a lot.** It works with iOS 5 and should be tested on different versions before releasing any code using this trick. It is dependent on a file built and added to the bundle by XCode, so you may have to check with updates that it is not changed or removed.

## iOS documentation references

1. iOS x.x Library > Data Management > Core Data Model Versioning and Data Migration Programming Guide > Lightweight Migration
2. iOS x.x Library > Data Management > Core Data Model Versioning and Data Migration Programming Guide > Versioning > Model Versions