# Update CollectionView after Screen Pop
This guide will show you how to update any **ObservableCollection** after poping a screen. <br>
I often use this when I have a Create Button on a Query Page.

### 1st Page:
This a Trip List, with a **ObservableCollection** populating a query and a Button at the end, to create a New Trip.

#### Trip List:

XAML Code:
````xaml
<!-- Button UI -->
<Button Text="CREATE" BackgroundColor="#35AE2A" VerticalOptions="End" x:Name="BtnCreate" Clicked="BtnCreate_Clicked"/>
````
C# Code:
````csharp
// On Button Click
async void BtnCreate_Clicked(System.Object sender, System.EventArgs e) {
    ContentPage navTo = new TripCreate01();

    MessagingCenter.Subscribe<TripCreate01>(this, "PageClosed", (sender) => {
        // Will call this function when receiving the message
        UpdateData();
    });

    await Navigation.PushAsync(navTo);
}
````

### Trip Create:

> Take attention to make sure that **OnDisappearingHandler** is sending the same message as requested.

C# Code:
````csharp
public TripCreate01() {
    InitializeComponent();
    this.Disappearing += OnDisappearingHandler;
}

private async void OnDisappearingHandler(object sender, EventArgs e) {
    /* Code here to check if something before closing */
    // return;

    MessagingCenter.Send(this, "PageClosed");
    this.Disappearing -= OnDisappearingHandler;
}
````
