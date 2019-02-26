# CUSTOM PAGE TRANSITIONS
A Simple Custom Animated Page Transitions Demonstration for Xamarin Forms.

## INTRODUCTION
Using the [AnimationNavPage Component](https://components.xamarin.com/view/customnavpage) we can demonstrate how to create a custom transition between different pages.

![demo](https://github.com/bbl-Laobu/AnimatedTransitionNavPageDemo/raw/master/demo.gif)

## DESCRIPTION
To demonstrate and achieve different types of transitions between navigation pages, we are using the excellent ‘AnimationNavPage Component’ written by AlexandrNikulin (free trial or priced at $26). 

Setting-up and using the component happens in 3 steps:	
1.	Install and reference the DLL files for PCL, IOS and Android
2.	Declare AnimationNavigationPage
3.	Create and Animation page


## INSTALL
Download the ‘customnavpage’ package from https://github.com/AlexandrNikulin/AnimationNavigationPage. You can easily donwload a trial version from the site. Copy the included lib folder into your solution and link the following DLL’s to your different projects (PCL, Andoid and IOS) as shown below:

![PCL](https://github.com/bbl-Laobu/AnimatedTransitionNavPageDemo/raw/master/PCLRef.png) ![Android](https://github.com/bbl-Laobu/AnimatedTransitionNavPageDemo/raw/master/AndroidRef.png) ![IOS](https://github.com/bbl-Laobu/AnimatedTransitionNavPageDemo/raw/master/IOSRef.png)

Next, add 'FormsControls.Touch.Main.Init()' into AppDelegate.cs of your Xamarin iOS Project:
```csharp
public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
    {
        public override bool FinishedLaunching(UIApplication app, NSDictionary options)
        {
            global::Xamarin.Forms.Forms.Init();

            FormsControls.Touch.Main.Init();

            LoadApplication(new App());

            return base.FinishedLaunching(app, options);
        }
    }
```

Finaly, add 'FormsControls.Droid.Main.Init()' into MainActivity.cs of your Xamarin Droid Project:
```csharp
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            TabLayoutResource = Resource.Layout.Tabbar;
            ToolbarResource = Resource.Layout.Toolbar;

            base.OnCreate(bundle);

            global::Xamarin.Forms.Forms.Init(this, bundle);

            FormsControls.Droid.Main.Init(this);

            LoadApplication(new App());
        }
    }
```

## DECLARE ANIMATIONNAVIGATIONPAGE
In your App, declare your new main page as follows:
```csharp  
public class App : Application
{
        public App()
        {
            InitializeComponent();
            MainPage = new AnimationNavigationPage(new StartPage());
        }
}
```
## CREATE AND ANIMATION PAGE
There are 3 ways to create an Animation Page:
1.	Implement the IAnimationPage interface
2.	Use XAML Tags - No Binding
3. 	Use XAML Tags - With Binding

## OPTION 1 - Implement the IAnimationPage interface
Firstly, add the interface declaration to your class definition as follows:
```csharp  
public partial class InterfaceImplementedPage : ContentPage, IAnimationPage 
```

Next, add the following code to the class:
```csharp  
public IPageAnimation PageAnimation { get; } = new FlipPageAnimation { Duration = AnimationDuration.Long, Subtype = AnimationSubtype.FromTop };

public void OnAnimationStarted(bool isPopAnimation)
{
	// Put your code here but leaving empty works just fine
}

public void OnAnimationFinished(bool isPopAnimation)
{
	// Put your code here but leaving empty works just fine
}
```

Note that depending on the type of transition you want, you can change FlipPageAnimation into SlidePageAnimation, FadePageAnimation… or whichever animation you require. Further configuration such as Duration and Subtype can be easily done.

## OPTION 2 - Use XAML no Binding 
Firstly, make sure that the code behind inherits from AnimationPage:
```csharp  
public partial class XamlNoBindingPage : AnimationPage
```

Next, use the 'controls:AnimationPage' tag instead of the 'ContentPage' tag as follows: 
```xaml  
<controls:AnimationPage xmlns="http://xamarin.com/schemas/2014/forms" 
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" 
    xmlns:controls="clr-namespace:FormsControls.Base;assembly=FormsControls.Base"
    x:Class="AnimatedTransitionNavPageDemo.Pages.XamlNoBindingPage" 
    Title="XAML No Binding">
```

Lastly, we must declare a tag in the page to specify which transition we would like to use:
```xaml
<controls:AnimationPage.PageAnimation>
   <controls:RotatePageAnimation Duration="Medium" Subtype="FromLeft" />
</controls:AnimationPage.PageAnimation>
```

Notice that here we are declaring a RotatePageAnimation but again we can pick whatever Animation we want. Properties are also available to further customize the animation (duration, subtype…).

After this you are now able to configure your page as you would in a normal content page.


## OPTION 3 - Use XAML with BINDING
The component allows you to bind the PageAnimation property. To do this declare the 'controls:AnimationPage' as follows:
```xaml 
<controls:AnimationPage xmlns="http://xamarin.com/schemas/2014/forms" 
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" 
    xmlns:controls="clr-namespace:FormsControls.Base;assembly=FormsControls.Base"
    x:Class="AnimatedTransitionNavPageDemo.Pages.XamlWithBindingPage" 
    NavigationPage.BackButtonTitle="Back"
    Title="XAML With Binding"
    PageAnimation="{Binding MyPageAnimation}">
```

In this case Your ViewModel should then have a ‘MyPageAnimation’ property defined. We are using [Fody](https://github.com/Fody/PropertyChanged) so our simplified code looks like this:
```csharp 
public IPageAnimation MyPageAnimation { get; set; }
```

This property can then be set in the following way:
```csharp 
MyPageAnimation = new SlidePageAnimation()
  {
  	Duration = AnimationDuration.Long,
	Subtype = AnimationSubtype.FromTop
  };
```

Again, we can use different types of animations and further configure the properties.

So, there you have it. A simple example on how to set-up Custom Transitions using 3 different implementation techniques. 

Enjoy and any question or improvements, please let me know.
Laobu!

## REFERENCES
- ‘AnimationNavPage’ By AlexandrNikulin @ [https://components.xamarin.com/view/customnavpage   ](https://components.xamarin.com/view/customnavpage ) 
- 'Fody/PropertyChanged' by SimonCropp @ [https://github.com/Fody/PropertyChanged] (https://github.com/Fody/PropertyChanged)


## AUTHOR
Laobu – Bernard Blanckaert
