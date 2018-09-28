## Objective

Main objective of this blog post is to give you an idea about how to Share Image and Text on Twitter in iOS Using Native Code.


## Step 1 : Introduction

Once, you’ve built the x-code project from unity editor, you have to add two frameworks: Accounts and Social as shown in the image below. 

Here, we share our data using the native code so you don’t need to create any app in twitter account or download any SDK or any other stuff related to third party. **Share Text and image on Twitter in iOS**


## Step 2 : Implementation

- First create viewController.mm file and put it in Assets->Plugins->IOS folder in unity.
- This file contains the actual code of sharing the content on twitter.
- Using the following .m file you can share textual information, images and promotional images on twitter in IOS.

```swift
#import <Social/Social.h>
#import <Accounts/Accounts.h>
@implementation ViewController : UIViewController
-(void) shareTextOnTwitter: (const char *) shareMessage
{
 SLComposeViewController *mySLComposerSheet;
 NSString *message = [NSString stringWithUTF8String:shareMessage];
 if([SLComposeViewController isAvailableForServiceType:SLServiceTypeTwitter])
 {
 mySLComposerSheet = [[SLComposeViewController alloc] init]; //initiate the Social Controller
 mySLComposerSheet = [SLComposeViewController composeViewControllerForServiceType:SLServiceTypeTwitter]; //Tell him with what social plattform to use it, e.g. facebook or twitter
 [mySLComposerSheet setInitialText:[NSString stringWithFormat:message,mySLComposerSheet.serviceType]]; //the message you want to post
 [[UIApplication sharedApplication].keyWindow.rootViewController presentViewController:mySLComposerSheet animated:YES completion:nil];
 }
 [mySLComposerSheet setCompletionHandler:^(SLComposeViewControllerResult result) {
 NSString *output;
 switch (result) {
 case SLComposeViewControllerResultCancelled:
 output = @"Action Cancelled";
 break;
 case SLComposeViewControllerResultDone:
 output = @"Post Successfull";
 break;
 default:
 break;
 } //check if everything worked properly. Give out a message on the state.
 UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Twitter" message:output delegate:nil cancelButtonTitle:@"Ok" otherButtonTitles:nil];
 [alert show];
 }];
}
-(void) postImageWithMessageOnTwitter: (const char *) path: (const char *) shareMessage
{
 NSString *imagePath = [NSString stringWithUTF8String:path];
 UIImage *image = [UIImage imageWithContentsOfFile:imagePath];
 SLComposeViewController *mySLComposerSheet;
 NSString *message = [NSString stringWithUTF8String:shareMessage];
 if([SLComposeViewController isAvailableForServiceType:SLServiceTypeTwitter])
 {
 mySLComposerSheet = [[SLComposeViewController alloc] init]; //initiate the Social Controller
 mySLComposerSheet = [SLComposeViewController composeViewControllerForServiceType:SLServiceTypeTwitter]; //Tell him with what social plattform to use it, e.g. facebook or twitter
 [mySLComposerSheet setInitialText:[NSString stringWithFormat:message,mySLComposerSheet.serviceType]]; //the message you want to post
 [mySLComposerSheet addImage:image];
 [[UIApplication sharedApplication].keyWindow.rootViewController presentViewController:mySLComposerSheet animated:YES completion:nil];
 }
 [mySLComposerSheet setCompletionHandler:^(SLComposeViewControllerResult result) {
 NSString *output;
 switch (result) {
 case SLComposeViewControllerResultCancelled:
 output = @"Action Cancelled";
 break;
 case SLComposeViewControllerResultDone:
 output = @"Post Successfull";
 break;
 default:
 break;
 } //check if everything worked properly. Give out a message on the state.
 UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Twitter" message:output delegate:nil cancelButtonTitle:@"Ok" otherButtonTitles:nil];
 [alert show];
 }];
}
@end
extern "C"{
 void TwitterTextSharingMethod(const char * message){
 ViewController *vc = [[ViewController alloc] init];
 [vc shareTextOnTwitter: message];
 [vc release];
 }
}
extern "C"{
 void TwitterImageSharing(const char * path, const char * message){
 ViewController *vc = [[ViewController alloc] init];
 [vc postImageWithMessageOnTwitter: path: message];
 [vc release];
 }
}
```

- The above two methods “TwitterTextSharingMethod” and “TwitterImageSharing” are invoked in your unity script. You just need to import these methods in your scripts as shown below:

```swift
[DllImport("__Internal")]
 privatestaticexternvoid TwitterTextSharingMethod (string message);
[DllImport("__Internal")]
 privatestaticexternvoid TwitterImageSharing (string iosPath, string message);
 
```

- Now, you have to call these methods. For this purpose follow the steps given below.

```swift
TwitterTextSharingMethod(“Twitter Text Sharing Demo”);
string path = Application.persistentDataPath + "/MyImage.png";
TwitterImageSharing(path, “Twitter Image Sharing Demo” );
```
