# Prevent App Themes to Change

This tutorial, is going to show how to keep the app from changing colors when changing cellphone to Dark/Light Mode.

In this example, I'm gonna use Light Mode only on my app.

### Steps:

1. Open your App.xaml.cs and add this line before setting the Main Page:

````csharp
UserAppTheme = AppTheme.Light;
````

So, it should be something like this:

````csharp
public partial class App : Application {
    public App() {
        InitializeComponent();

        UserAppTheme = AppTheme.Light;

        MainPage = new NavigationPage(new LoginPage());
    }
}
````

2. Add this line inside the MainActivity.cs for Android:

````csharp
// Overrides OnCreate:
AppCompatDelegate.DefaultNightMode = AppCompatDelegate.ModeNightNo;
````

Staying something like this:

````csharp
[Activity(Theme = "@style/Maui.SplashTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation | ConfigChanges.UiMode | ConfigChanges.ScreenLayout | ConfigChanges.SmallestScreenSize | ConfigChanges.Density)]
public class MainActivity : MauiAppCompatActivity {

    protected override void OnCreate(Bundle savedInstanceState) {
        AppCompatDelegate.DefaultNightMode = AppCompatDelegate.ModeNightNo;
        base.OnCreate(savedInstanceState);
    }

    // ... other methods ...
}
````

3. Add this content inside iOS's info.plist:

````xaml
<key>UIUserInterfaceStyle</key>
<string>Light</string>
````

### End:

Recompile your app and it should be ready to go.
