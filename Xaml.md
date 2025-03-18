#XAML

## イベント

### Loaded、UnLoaded、IsVisibleChangedイベント
>Loaded、UnLoaded、IsVisibleChangedの動作順は異なる。

- IsVisibleChangedはいつも最初にCall
* Loadedする場合の順番：IsVisibleChanged → Loaded in .xaml → Loaded in .xaml.cs
- UnLoadedする場合の順番：IsVisibleChanged → UnLoaded in .xaml.cs → UnLoaded in .xaml

.xaml.csファイル内
```
    public partial class CameraSettingWindow : UserControl
    {
        public CameraSettingWindow(BindableBase pViewModel)
        {
            this.DataContext = pViewModel;
            InitializeComponent();
            this.Loaded += CameraSettingWindow_Loaded;
            this.IsVisibleChanged += CameraSettingWindow_IsVisibleChanged;
            this.Unloaded += CameraSettingWindow_Unloaded;
        }
        void CameraSettingWindow_Loaded(object sender, System.Windows.RoutedEventArgs e)
        {
            Console.WriteLine("CameraSettingWindow_Loaded"); //ロード時最後呼ばれる、ViewModelが先
        }
        void CameraSettingWindow_IsVisibleChanged(object sender, System.Windows.DependencyPropertyChangedEventArgs e)
        {
            Console.WriteLine("CameraSettingWindow_Loaded"); //最初呼ばれる
        }
        void CameraSettingWindow_Unloaded(object sender, System.Windows.RoutedEventArgs e)
        {
            Console.WriteLine("CameraSettingWindow_Unloaded"); // UnLoaded時ViewModelより先呼ばれる
        }
    }
```
.xaml内
```
<UserControl x:Class="MimakiPrinterController.Pages.CameraSettingWindow"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008" 
             xmlns:i="http://schemas.microsoft.com/expression/2010/interactivity"
             xmlns:custom="clr-namespace:MimakiPrinterController.Custom"
             xmlns:local="clr-namespace:MimakiPrinterController.local"
             mc:Ignorable="d" 
             d:DesignHeight="450" d:DesignWidth="800">
    <i:Interaction.Triggers>
        <i:EventTrigger EventName="Loaded">
            <i:InvokeCommandAction Command="{Binding Path=LoadingCamera}" /> <!-- ロード時優先で呼ばれる-->
        </i:EventTrigger>
        <i:EventTrigger EventName="Unloaded">
            <i:InvokeCommandAction Command="{Binding Path=UnLoadingCamera}" /> <!-- アンロード時最後で呼ばれる-->
        </i:EventTrigger>
    </i:Interaction.Triggers>
```
