# .NET MAUI - Tips and Tricks

Just like I did with [Progress 4GL](https://github.com/raphaelfrei/open_edge-guides) - I've decided to create an repo to store some tips for .NET.

I know that this tool is currently under development, but I'm using it at a commercial level and I've encontered some issues in the path and decided to help by taking notes what I did to fix it.

## What is .NET MAUI?

> .NET Multi-platform App UI (.NET MAUI) is a cross-platform framework for creating native mobile and desktop apps with C# and XAML.
>
> Using .NET MAUI, you can develop apps that can run on Android, iOS, MacOS, and Windows from a single shared code-base.

## 📝 Project Basics:

All my work is currently (2023/24 least) being developed on a MacBook Air M1 and Visual Studio for MacOS.

In every project that I create - I've installed these Nugets:

- [Community Toolkit for MAUI](https://github.com/CommunityToolkit/Maui)
- [FingerPrint](https://github.com/smstuebe/xamarin-fingerprint) - *(Only if biometrics is necessary)*
- [Microsoft.Data.Sqlite](https://www.nuget.org/packages/Microsoft.Data.Sqlite/) - *(Only if using SQLite for DB)*

## 💻 Projects with MAUI:

*Some commercial projects I can't share*

- [Financial Manager](https://github.com/raphaelfrei/financial-manager)
- [Password Generator](https://github.com/raphaelfrei/xaml_password-generator)

## 📦 Content:

- [SQLite Implementation](https://github.com/raphaelfrei/maui-guides/blob/main/sqlite.md)
- [Device Biometrics](https://github.com/raphaelfrei/maui-guides/blob/main/user_biometrics.md)
- [Multiple Languages](https://github.com/raphaelfrei/maui-guides/blob/main/multi-language.md)
- [Prevent Light/Dark Mode from Change](https://github.com/raphaelfrei/maui-guides/blob/main/fix-app-themes.md)
