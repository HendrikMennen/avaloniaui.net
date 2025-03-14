---
Title: Creating a Modern looking Window
Order: 3
---

## Use Dark Mode and Add a little Acrylic

Lets try and make this look a little more modern by applying `Dark` mode and some `Acrylic` styling to the Window.

1. Open App.axaml 

   Change the FluentTheme Mode from Light to Dark.

```xml
   <FluentTheme Mode="Dark"/>
   ```

2. Open `MainWindow.axaml` notice that its still showing the Light mode version in the previewer.

   This is because the previewer actually runs your application in a special mode. 

   

3. Using the menu click `Build` -> `Build Startup Project`

   Notice that the preview now changes to the dark mode.

   The previewer knows about changes you make to the file your editing, but it doesnt know about changes in other files. This is why you need to build the project if another file was changed.

   

   ![dark-mode-preview](/docs/advanced-tutorial/images/dark-mode-preview.png)

   

4. After where it says `Title="Avalonia.MusicStore"` add the following code:

```xml
   <Window xmlns="https://github.com/avaloniaui"
           xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
           xmlns:vm="using:Avalonia.MusicStore.ViewModels"
           xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
           xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
           mc:Ignorable="d" d:DesignWidth="800" d:DesignHeight="450"
           x:Class="Avalonia.MusicStore.Views.MainWindow"
           Icon="/Assets/avalonia-logo.ico"
           Title="Avalonia.MusicStore"
           
           TransparencyLevelHint="AcrylicBlur"
           Background="Transparent">
```

   This will make the Window Transparent and apply a Blur.

   

   To apply acrylic to the window, that we can tint and customize for a modern look, replace the `<TextBlock>` with the following code:

```xml
   <Window xmlns="https://github.com/avaloniaui"
           xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
           xmlns:vm="using:Avalonia.MusicStore.ViewModels"
           xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
           xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
           mc:Ignorable="d" d:DesignWidth="800" d:DesignHeight="450"
           x:Class="Avalonia.MusicStore.Views.MainWindow"
           Icon="/Assets/avalonia-logo.ico"
           Title="Avalonia.MusicStore"
           TransparencyLevelHint="AcrylicBlur"
           Background="Transparent">
   
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
       </Panel>
   </Window>
```

Now click the `Debug` `Button` to run the application again.

Notice we have a nice acrylic window effect. Shame about the titlebar though. Lets see how we can make that blend in a bit more.

![acrylic-material](/docs/advanced-tutorial/images/acrylic-material.png)

*Note, Linux users can not yet take advantage of this due to limitations of X11. The code will run and the window will still work on Linux, but the full effect will not be realised.



5. It is possible to have Avalonia render into the `titlebar`. Allowing us to create a more blended look. Modern web browsers tend to render tabs into the titlebar with this technique. 

   This works by extending the `Windows` client area into the `non-client` area.

   To enable this mode on the `Window` element  set the `ExtendClientAreaToDecorationsHint` property to `True`.

```xml
   <Window xmlns="https://github.com/avaloniaui"
           xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
           xmlns:vm="using:Avalonia.MusicStore.ViewModels"
           xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
           xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
           mc:Ignorable="d" d:DesignWidth="800" d:DesignHeight="450"
           x:Class="Avalonia.MusicStore.Views.MainWindow"
           Icon="/Assets/avalonia-logo.ico"
           Title="Avalonia.MusicStore"
           TransparencyLevelHint="AcrylicBlur"
           Background="Transparent"
           
           ExtendClientAreaToDecorationsHint="True">
```

   Press the `Debug` button again to run.

![full-acrylic-window](/docs/advanced-tutorial/images/full-acrylic-window.png)

Perfect, a modern looking Window, Avalonia is able to render every pixel.
