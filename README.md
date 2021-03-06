# prmToolkit

# Log
Pacote responsável por gerenciar os logs da aplicação.

Suporte a gravação de log:
- File
- EventViewer
- Database (MySql, SqlServer e Firebird)

Recursos
Todo o comportamento do log deve ser informado diretamente no arquivo de configuração de seu projeto, com isso caso seja necessário mudar a estratégia de gravação de log, você não precisa recompilar sua aplicação.


### Installation - Log

Para instalar, abra o prompt de comando Package Manager Console do seu Visual Studio e digite o comando abaixo:

Para adicionar a referencia a dll
```sh
Install-Package prmToolkit.Log
```

### Exemplo de como configurar o log

Após adicionar o pacote prmToolkit.Log em seu projeto, configure seu (WebConfig ou AppConfig) conforme abaixo:


```sh
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <appSettings>
    <!--EnumDatabaseType = SqlServer = 0, MySql = 1, Firebird = 2-->
    <!--EnumExceptionDetail = Simple = 0, Synthetic = 1, Analytical = 2-->
    <!--EnumLogType = SaveToFile = 0, SaveToDatabase = 1, SaveToEventViewer = 2-->
    <!--EnumMessageType = Information = 0, Warning = 1, Error = 2 -->

    <!--Irá salvar o log em todos lugares definidos aqui-->
    <add key="Log_SaveAll_Set_Sequence_EnumLogType" value="1, 0, 2"/>

    <!--
    Irá salvar o log no modo de contigência, caso o log principal de algum erro o contigência é acionado. 
    Ele irá gravar para o primeiro tipo de log informado, caso não consiga ele tenta o segundo tipo de log informado e assim sucessivamente
    -->
    <add key="Log_TrySave_Set_Sequence_EnumLogType_Contingency" value="0"/>

    <!--Define que tipo de banco de dados você quer armazenar o log MySql, SqlServer ou Firebird-->
    <add key="Log_Database_Set_EnumDatabaseType" value="1"/>

    <!--Define o nível de detalhamento do log, podendo ser log simples, sintético ou analitico-->
    <add key="Log_Detail_Set_EnumExceptionDetail" value="0"/>

    <!--Aqui definimos que tipo de mensagem tem permissão de ser armazenado, podendo ser Information, Warning e Error-->
    <add key="Log_MessageType_View" value="0, 1, 2"/>

    <!--Define o nome da aplicação que está sendo logado-->
    <add key="Log_ApplicationName" value="prmToolkit"/>

    <!--Define onde será gravado o log em arquivo-->
    <add key="Log_File_Set_FolderPath" value="C:\_Paulo\Logs"/>
  </appSettings>

  <connectionStrings>
    <clear />
    <!--Local-->
    <add name="Log_ConnectionString" providerName="SQLOLEDB" connectionString="Server=localhost; Database=samich_log; Port=3306; Uid=root; Pwd=MySQL@dmin;" />
  </connectionStrings>

  <startup>
    <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
  </startup>
</configuration>

```
### Estrutura da tabela no banco de dados

#### MYSQL
```sh
CREATE TABLE `log` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `Application` varchar(100) DEFAULT NULL,
  `MessageType` varchar(50) DEFAULT NULL,
  `Message` varchar(255) DEFAULT NULL,
  `CurrentDate` datetime DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=105 DEFAULT CHARSET=utf8;
```

### SQL SERVER
```sh
CREATE TABLE [dbo].[Log](
	[Id] [bigint] IDENTITY(1,1) NOT NULL,
	[Application] [varchar](100) NULL,
	[MessageType] [varchar](50) NULL,
	[Message] [varchar](255) NULL,
	[CurrentDate] [datetime] NULL,
 CONSTRAINT [PK_Log] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]
```

### Exemplo de como usar

```sh
 
 LogManager.Save("Minha mensagem", EnumMessageType.Information)
 
```
