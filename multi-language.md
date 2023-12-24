# Multiple Languages with .NET MAUI

Let's create multiple languages for our .NET MAUI - This time, I'll be using ENGLISH for default language and PT-BR for secondary.

For more informations, follow [this link](https://learn.microsoft.com/en-us/dotnet/maui/fundamentals/localization?view=net-maui-8.0).

## 01. Default App Language:

To set our *'default'* language, we need to create a file called **AppResources.resx**. <br>
 - I've created mine on the folder ````Resources\Languages\AppResources.resx````

## 02. Other Languages:

These languages files follow the device language.

Some of the supported file-name:

````
'de' - Deutsch
'es' - Spanish
'fr' - French
'ja' - Japanese
'pt' or 'pt-BR' - Portuguese Brazil
'pt-PT' - Portuguese Portugal
'ru' - Russian
````

In my case, I've created the ````Resources\Languages\AppResources.pt-BR.resx````

## 03. Create Texts:

When oppening the RESX file, add new lines with the necessary texts:

#### AppResources.resx:
````xaml
<data name="Access" xml:space="preserve">
  <value>LOGIN</value>
</data>
<data name="User" xml:space="preserve">
  <value>User</value>
</data>
<data name="DontHaveAccount" xml:space="preserve">
    <value>Don't have an account?</value>
</data>
<data name="SignUp" xml:space="preserve">
  <value>Sign up!</value>
</data>
````

#### AppResources.pt-BR.resx:
````xaml
<data name="Access" xml:space="preserve">
    <value>ACESSAR</value>
</data>
<data name="User" xml:space="preserve">
    <value>Usuário</value>
</data>
<data name="DontHaveAccount" xml:space="preserve">
    <value>Não tem uma conta?</value>
</data>
<data name="SignUp" xml:space="preserve">
    <value>Cadastre-se!</value>
</data>
````

## 04. Code Implementation:

#### XAML File:

In the ContentPage *(or what type you're using)* contents, add a reference from the language folder.

````xaml
xmlns:lang="clr-namespace:MeuCarro.Resources.Languages"
````

The full tag should be something like this:

````xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:lang="clr-namespace:MyApp.Resources.Languages"
             x:Class="MeuCarro.CreateAccountFinish"
             Title="MyPage">

    <!-- MORE CONTENT... -->

</ContentPage>
````

And, to implement this on a text:

````xaml
    <!-- LABEL -->
    <Label Text="{x:Static lang:AppResources.Access}"/>

    <!-- BUTTON -->
    <Button Text="{x:Static lang:AppResources.BtnReturnMenu}"/>

    <!-- ENTRY -->
    <Entry Placeholder="{x:Static lang:AppResources.User}"/>

    <!-- LABEL w/ SPAN -->
    <Label>
        <Label.FormattedText>
            <FormattedString>
                <Span Text="{x:Static lang:AppResources.LicensePlate}"/>
            </FormattedString>
        </Label.FormattedText>
    </Label>

    <!-- AND MANY OTHERS... -->
````

#### C# File:

You need to create a variable that shows the code where the language files are.

I have a static script with the localization, if any changes are required, I need to replace in a single file only.

````csharp
using System.Reflection;
using System.Resources;

public static readonly ResourceManager RM = new ResourceManager("MyApp.Resources.Languages.AppResources", Assembly.GetExecutingAssembly());
````

And to get the text on the code:

````csharp
DisplayAlert(RM.GetString("ErrorHeader"), RM.GetString("ErrorAccountNotExists"), "OK");
````
