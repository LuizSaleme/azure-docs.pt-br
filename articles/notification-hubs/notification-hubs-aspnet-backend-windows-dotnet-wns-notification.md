---
title: Enviar notificações para usuários específicos usando Hubs de Notificação do Azure | Microsoft Docs
description: Saiba como enviar notificações para usuários específicos usando aplicativos da UWP (Plataforma Universal do Windows).
documentationcenter: windows
author: dimazaid
manager: kpiteira
editor: spelluru
services: notification-hubs
ms.assetid: 012529f2-fdbc-43c4-8634-2698164b5880
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/04/2018
ms.author: dimazaid
ms.openlocfilehash: ee0e78402515a733731e6faf21b09886674cda51
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38452406"
---
# <a name="tutorial-send-notifications-to-specific-users-by-using-azure-notification-hubs"></a>Tutorial: Enviar notificações para usuários específicos usando o Hubs de Notificação do Azure
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a>Visão geral
Este tutorial mostra como usar os Hubs de Notificação do Azure para enviar notificações por push a um usuário específico do aplicativo em um dispositivo específico. Um back-end de WebAPI do ASP.NET é usado para autenticar clientes. Quando o back-end autentica um usuário de aplicativo do cliente, ele adiciona automaticamente uma marca para o registro de notificação. O back-end usa essa marca para enviar notificações para o usuário específico. 

> [!NOTE]
> O código completo deste tutorial pode ser encontrado no [GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers). 

Neste tutorial, você deve executar as seguintes etapas:

> [!div class="checklist"]
> * Criar o projeto WebAPI
> * Autenticar clientes para o back-end da WebAPI
> * Registrar para receber notificações usando o back-end da WebAPI
> * Enviar notificações do back-end da WebAPI
> * Publicar o novo back-end da API Web
> * Atualizar o código para o projeto cliente
> * Testar o aplicativo


## <a name="prerequisites"></a>pré-requisitos
Este tutorial baseia-se no hub de notificação e no projeto do Visual Studio que você criou no [Tutorial: Enviar notificações a aplicativos de Plataforma Universal do Windows usando o tutorial Hubs de Notificação do Azure](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md). Portanto, conclua-o antes de iniciar este tutorial. 

> [!NOTE]
> Se você estiver usando Aplicativos Móveis no Serviço de Aplicativo do Azure como serviço de back-end, consulte a [versão dos Aplicativos Móveis](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md) deste tutorial.


&nbsp;

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="update-the-code-for-the-client-project"></a>Atualizar o código para o projeto cliente
Nesta seção, você atualiza o código no projeto que você concluiu para o [Tutorial: Enviar notificações a aplicativos de Plataforma Universal do Windows usando Hubs de Notificação do Azure](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md). O projeto já deve estar associado com a Windows Store. Ele também deve ser configurado para usar seu hub de notificação. Nesta seção, você adiciona código para chamar o novo back-end da WebAPI e o usará para se registrar e enviar notificações.

1. No Visual Studio, abra a solução que você criou para o [Tutorial: Enviar notificações a aplicativos de Plataforma Universal do Windows usando Hubs de Notificação do Azure](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).
2. No Gerenciador de Soluções, clique com o botão direito do mouse no projeto **WindowsApp**, em seguida, clique em **Gerenciar Pacotes NuGet**.
3. No lado esquerdo, clique em **Online**.
4. Na caixa **Pesquisar**, digite **Http Client**.
5. Na lista de resultados, clique em **System**e clique em **Instalar**. Conclua a instalação.
6. De volta à caixa **Pesquisar** do NuGet, digite **Json.net**. Instale o pacote **Newtonsoft.json** e feche a janela do Gerenciador de Pacotes NuGet.
8. No Gerenciador de Soluções, no projeto **WindowsApp**, clique duas vezes em **MainPage.xaml** para abri-lo no editor do Visual Studio.
9. No código XML do **MainPage.XAML**, substitua a seção `<Grid>` pelo código a seguir: esse código adiciona uma caixa de texto de nome de usuário e senha com os quais o usuário se autentica. Ele também adiciona caixas de texto para a mensagem de notificação e a marca de nome de usuário que deve receber a notificação:

    ```xml   
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>

        <TextBlock Grid.Row="0" Text="Notify Users" HorizontalAlignment="Center" FontSize="48"/>

        <StackPanel Grid.Row="1" VerticalAlignment="Center">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition></ColumnDefinition>
                    <ColumnDefinition></ColumnDefinition>
                    <ColumnDefinition></ColumnDefinition>
                </Grid.ColumnDefinitions>
                <TextBlock Grid.Row="0" Grid.ColumnSpan="3" Text="Username" FontSize="24" Margin="20,0,20,0"/>
                <TextBox Name="UsernameTextBox" Grid.Row="1" Grid.ColumnSpan="3" Margin="20,0,20,0"/>
                <TextBlock Grid.Row="2" Grid.ColumnSpan="3" Text="Password" FontSize="24" Margin="20,0,20,0" />
                <PasswordBox Name="PasswordTextBox" Grid.Row="3" Grid.ColumnSpan="3" Margin="20,0,20,0"/>

                <Button Grid.Row="4" Grid.ColumnSpan="3" HorizontalAlignment="Center" VerticalAlignment="Center"
                            Content="1. Login and register" Click="LoginAndRegisterClick" Margin="0,0,0,20"/>

                <ToggleButton Name="toggleWNS" Grid.Row="5" Grid.Column="0" HorizontalAlignment="Right" Content="WNS" IsChecked="True" />
                <ToggleButton Name="toggleGCM" Grid.Row="5" Grid.Column="1" HorizontalAlignment="Center" Content="GCM" />
                <ToggleButton Name="toggleAPNS" Grid.Row="5" Grid.Column="2" HorizontalAlignment="Left" Content="APNS" />

                <TextBlock Grid.Row="6" Grid.ColumnSpan="3" Text="Username Tag To Send To" FontSize="24" Margin="20,0,20,0"/>
                <TextBox Name="ToUserTagTextBox" Grid.Row="7" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                <TextBlock Grid.Row="8" Grid.ColumnSpan="3" Text="Enter Notification Message" FontSize="24" Margin="20,0,20,0"/>
                <TextBox Name="NotificationMessageTextBox" Grid.Row="9" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                <Button Grid.Row="10" Grid.ColumnSpan="3" HorizontalAlignment="Center" Content="2. Send push" Click="PushClick" Name="SendPushButton" />
            </Grid>
        </StackPanel>
    </Grid>
    ```
11. No Gerenciador de Soluções, abra o arquivo **MainPage.xaml.cs** para os projetos **(Windows 8.1)** e **(Windows Phone 8.1)**. Adicione as seguintes instruções `using` na parte superior dos dois arquivos:

    ```csharp    
    using System.Net.Http;
    using Windows.Storage;
    using System.Net.Http.Headers;
    using Windows.Networking.PushNotifications;
    using Windows.UI.Popups;
    using System.Threading.Tasks;
    ```
12. Em **MainPage.xaml.cs** para o projeto **WindowsApp**, adicione o membro a seguir para a classe `MainPage`. Certifique-se de substituir `<Enter Your Backend Endpoint>` pelo ponto de extremidade de back-end real obtido anteriormente. Por exemplo, `http://mybackend.azurewebsites.net`.
    
    ```csharp
    private static string BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";
    ```
13. Adicione o código a seguir à classe MainPage em **MainPage.xaml.cs** para os projetos **(Windows 8.1)** e **(Windows Phone 8.1)**.
    
    O método `PushClick` é o manipulador de cliques para o botão **Enviar por Push** . Ele chama o back-end para disparar uma notificação para todos os dispositivos com uma marca de nome de usuário que corresponde ao parâmetro `to_tag` . A mensagem de notificação é enviada como conteúdo JSON no corpo da solicitação.
    
    O método `LoginAndRegisterClick` é o manipulador de cliques para o botão **Fazer logon e registrar**. Ele armazena o token de autenticação básica (representa qualquer token usado pelo seu esquema de autenticação) no armazenamento local, em seguida, utiliza `RegisterClient` para o registro de notificações usando o back-end.

    ```csharp
    private async void PushClick(object sender, RoutedEventArgs e)
    {
        if (toggleWNS.IsChecked.Value)
        {
            await sendPush("wns", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);
        }
        if (toggleGCM.IsChecked.Value)
        {
            await sendPush("gcm", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);
        }
        if (toggleAPNS.IsChecked.Value)
        {
            await sendPush("apns", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);

        }
    }

    private async Task sendPush(string pns, string userTag, string message)
    {
        var POST_URL = BACKEND_ENDPOINT + "/api/notifications?pns=" +
            pns + "&to_tag=" + userTag;

        using (var httpClient = new HttpClient())
        {
            var settings = ApplicationData.Current.LocalSettings.Values;
            httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

            try
            {
                await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                    System.Text.Encoding.UTF8, "application/json"));
            }
            catch (Exception ex)
            {
                MessageDialog alert = new MessageDialog(ex.Message, "Failed to send " + pns + " message");
                alert.ShowAsync();
            }
        }
    }

    private async void LoginAndRegisterClick(object sender, RoutedEventArgs e)
    {
        SetAuthenticationTokenInLocalStorage();

        var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

        // The "username:<user name>" tag gets automatically added by the message handler in the backend.
        // The tag passed here can be whatever other tags you may want to use.
        try
        {
            // The device handle used is different depending on the device and PNS. 
            // Windows devices use the channel uri as the PNS handle.
            await new RegisterClient(BACKEND_ENDPOINT).RegisterAsync(channel.Uri, new string[] { "myTag" });

            var dialog = new MessageDialog("Registered as: " + UsernameTextBox.Text);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
            SendPushButton.IsEnabled = true;
        }
        catch (Exception ex)
        {
            MessageDialog alert = new MessageDialog(ex.Message, "Failed to register with RegisterClient");
            alert.ShowAsync();
        }
    }

    private void SetAuthenticationTokenInLocalStorage()
    {
        string username = UsernameTextBox.Text;
        string password = PasswordTextBox.Password;

        var token = Convert.ToBase64String(System.Text.Encoding.UTF8.GetBytes(username + ":" + password));
        ApplicationData.Current.LocalSettings.Values["AuthenticationToken"] = token;
    }
    ```
1. Abra o arquivo **App.xaml.cs**. Localize a chamada para `InitNotificationsAsync()` in the `OnLaunched()` . Comentar ou excluir a chamada para `InitNotificationsAsync()`. O manipulador do botão inicializa os registros de notificação.

    ```csharp
    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        //InitNotificationsAsync();
    ```
1. Clique com o botão direito do mouse no projeto **WindowsApp**, clique em **Adicionar** e clique em **Classe**. Nomeie a classe como **RegisterClient.cs** e clique em **OK** para gerar a classe.
   
   Essa classe encapsula as chamadas do REST necessárias para entrar em contato com o back-end do aplicativo de modo a se registrar para as notificações por push. Ele também armazena localmente os *registrationIds* criados pelo Hub de Notificação, conforme detalhado em [Registrando-se por meio do back-end do aplicativo](http://msdn.microsoft.com/library/dn743807.aspx). Ele usa um token de autorização armazenado localmente quando você clica no botão **Fazer logon e registrar-se**.
2. Adicione as seguintes instruções `using` à parte superior do arquivo RegisterClient.cs:

    ```csharp   
    using Windows.Storage;
    using System.Net;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using Newtonsoft.Json;
    using System.Threading.Tasks;
    using System.Linq;
    ```
3. Adicione o código a seguir à definição de classe `RegisterClient` .
   
    ```csharp
    private string POST_URL;

    private class DeviceRegistration
    {
        public string Platform { get; set; }
        public string Handle { get; set; }
        public string[] Tags { get; set; }
    }

    public RegisterClient(string backendEndpoint)
    {
        POST_URL = backendEndpoint + "/api/register";
    }

    public async Task RegisterAsync(string handle, IEnumerable<string> tags)
    {
        var regId = await RetrieveRegistrationIdOrRequestNewOneAsync();

        var deviceRegistration = new DeviceRegistration
        {
            Platform = "wns",
            Handle = handle,
            Tags = tags.ToArray<string>()
        };

        var statusCode = await UpdateRegistrationAsync(regId, deviceRegistration);

        if (statusCode == HttpStatusCode.Gone)
        {
            // regId is expired, deleting from local storage & recreating
            var settings = ApplicationData.Current.LocalSettings.Values;
            settings.Remove("__NHRegistrationId");
            regId = await RetrieveRegistrationIdOrRequestNewOneAsync();
            statusCode = await UpdateRegistrationAsync(regId, deviceRegistration);
        }

        if (statusCode != HttpStatusCode.Accepted && statusCode != HttpStatusCode.OK)
        {
            // log or throw
            throw new System.Net.WebException(statusCode.ToString());
        }
    }

    private async Task<HttpStatusCode> UpdateRegistrationAsync(string regId, DeviceRegistration deviceRegistration)
    {
        using (var httpClient = new HttpClient())
        {
            var settings = ApplicationData.Current.LocalSettings.Values;
            httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string) settings["AuthenticationToken"]);

            var putUri = POST_URL + "/" + regId;

            string json = JsonConvert.SerializeObject(deviceRegistration);
                            var response = await httpClient.PutAsync(putUri, new StringContent(json, Encoding.UTF8, "application/json"));
            return response.StatusCode;
        }
    }

    private async Task<string> RetrieveRegistrationIdOrRequestNewOneAsync()
    {
        var settings = ApplicationData.Current.LocalSettings.Values;
        if (!settings.ContainsKey("__NHRegistrationId"))
        {
            using (var httpClient = new HttpClient())
            {
                httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                var response = await httpClient.PostAsync(POST_URL, new StringContent(""));
                if (response.IsSuccessStatusCode)
                {
                    string regId = await response.Content.ReadAsStringAsync();
                    regId = regId.Substring(1, regId.Length - 2);
                    settings.Add("__NHRegistrationId", regId);
                }
                else
                {
                    throw new System.Net.WebException(response.StatusCode.ToString());
                }
            }
        }
        return (string)settings["__NHRegistrationId"];

    }
    ```
4. Salve todas as alterações.

## <a name="test-the-application"></a>Testar o aplicativo
1. Inicie o aplicativo no Windows.
2. Insira um **Nome de Usuário** e **Senha** conforme mostrado na tela abaixo. Eles devem diferir do nome de usuário e senha que você insere no Windows Phone.
3. Clique em **Fazer logon e registrar** e verifique se um diálogo mostra que você fez logon. Esse código também habilita o botão **Enviar por Push**.
   
    ![][14]
5. Em seguida, no campo **Marca de Nome de Usuário do Destinatário**, insira o nome de usuário registrado. Digite uma mensagem de notificação e clique em **Enviar notificação por push**.
6. Apenas os dispositivos que foram registrados com o nome de usuário correspondente recebem a mensagem de notificação.
   
    ![][15]

## <a name="next-steps"></a>Próximas etapas
Neste tutorial, você aprendeu como enviar notificações por push para usuários específicos que têm tags associadas seus registros. Para saber como enviar notificações por push, vá para o tutorial a seguir: 

> [!div class="nextstepaction"]
>[Notificações por push com base no local](notification-hubs-push-bing-spartial-data-geofencing-notification.md)

[9]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push9.png
[10]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push10.png
[11]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push11.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-ui.png
[14]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-windows-instance-username.png
[15]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-notification-received.png
[16]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-send-message.png



<!-- URLs. -->
[Get started with Notification Hubs]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Secure Push]: notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md
[Use Notification Hubs to send breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
