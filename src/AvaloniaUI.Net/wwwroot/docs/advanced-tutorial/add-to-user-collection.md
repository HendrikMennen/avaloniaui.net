---
Title: Add Items to Users Collection
Order: 10
---

## Adding Albums to the Users Collection

Ok so now the user can find albums to purchase, it would be nice if the user could see which albums are in their collection. To do this we can add a similar UI to the MainWindow.

Modify the `MainWindow.axaml` so that it places the existing `Button` inside a panel. We can then add a `TextBlock` that we can show when the user has no music, to prompt them to buy some.

We can then use an `ItemsControl` instead of a `ListBox` as we did before. An `ItemsControl` is the exact same as a `ListBox` except it doesnt allow the user to select anything.

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:vm="using:Avalonia.MusicStore.ViewModels"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:views="clr-namespace:Avalonia.MusicStore.Views"
        mc:Ignorable="d" d:DesignWidth="800" d:DesignHeight="450"
        x:Class="Avalonia.MusicStore.Views.MainWindow"
        Icon="/Assets/avalonia-logo.ico"
        Title="Avalonia.MusicStore"
        TransparencyLevelHint="AcrylicBlur"
        Background="Transparent"
        ExtendClientAreaToDecorationsHint="True"
        WindowStartupLocation="CenterScreen">

    <Design.DataContext>
        <vm:MainWindowViewModel />
    </Design.DataContext>

    <Panel>
        <ExperimentalAcrylicBorder IsHitTestVisible="False">
            <ExperimentalAcrylicBorder.Material>
                <ExperimentalAcrylicMaterial
                    BackgroundSource="Digger"
                    TintColor="Black"
                    TintOpacity="1"
                    MaterialOpacity="0.65" />
            </ExperimentalAcrylicBorder.Material>
        </ExperimentalAcrylicBorder>
        
        <Panel Margin="40">
            <Button HorizontalAlignment="Right" VerticalAlignment="Top" Command="{Binding BuyMusicCommand}">
                <PathIcon Data="{StaticResource store_microsoft_regular}" />
            </Button>
            
            <TextBlock IsVisible="{Binding CollectionEmpty}" Text="Its lonely in here. Purchase some music to get your collection started." HorizontalAlignment="Center" VerticalAlignment="Center" />
            
            <ItemsControl Margin="0 40 0 0" Items="{Binding Albums}">
                <ItemsControl.ItemsPanel>
                    <ItemsPanelTemplate>
                        <WrapPanel />
                    </ItemsPanelTemplate>
                </ItemsControl.ItemsPanel>
            </ItemsControl>
        </Panel>
    </Panel>
</Window>
```



Now open `MainWindowViewModel` and add the following properties.

```c#
private bool _collectionEmpty;

public bool CollectionEmpty
{
    get => _collectionEmpty;
    set => this.RaiseAndSetIfChanged(ref _collectionEmpty, value);
}
        
public ObservableCollection<AlbumViewModel> Albums { get; } = new();
```

Notice again we use an `ObservableCollection` for the `Albums` property, and we add a simple `CollectionEmpty` property to tell us when the users music collection is empty.

In order to have the `CollectionEmpty` property updated we can add the following code to the constructor.

```c#
this.WhenAnyValue(x => x.Albums.Count)
    .Subscribe(x => CollectionEmpty = x == 0);
```

Then in the constructor we can modify the BuyMusicCommand so that it adds the purchased Album to the list of `Albums`.

```c#
BuyMusicCommand = ReactiveCommand.CreateFromTask(async () =>
{
    var store = new MusicStoreViewModel();

    var result = await ShowDialog.Handle(store);

    if (result != null)
    {
        Albums.Add(result);
    }
});
```

Notice we check the result for `null`, this is because the `user` may have cancelled the dialog without selecting anything, in which case nothing should be added. If the dialog returns a result then we simply add it to the list of Albums.

Lets run the program and see if it works.

![image-20210310175949319](/docs/advanced-tutorial/images/image-20210310175949319.png)

For the finishing touch we simply need to add persistance to the application.
