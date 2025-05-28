# XP Inc. - Cloud com Inteligência Artificial

## Criação e Configuração de uma Máquina Virtual no Azure

### 1. Criando uma Máquina Virtual
- **Acesse ao Portal do Azure**: Faça login no [Portal do Azure](https://portal.azure.com).
  - Se você não tiver uma assinatura do Azure, crie uma conta gratuita antes de começar.
- **Navegação**:
  - No topo do portal, em Azure services, selecione **Virtual machines**
  - Na página **Compute infrastructure | Virtual machines**, clique em *Create* e selecione **Azure virtual machine**.
- **Configuração Inicial**:
  - Em **Instance details**, insira um nome para a máquina virtual e escolha *Windows Server 2022 Datacenter: Azure Edition - x64 Gen 2* em **Image**. Deixe os outros padrões.
  - Em **Administrator account**, forneça um nome de usuário e uma senha. 
  - Em **Inbound port rules**, escolha *Allow selected ports* e, em seguida, selecione **RDP (3389)** e **HTTP (80)** na lista suspensa.
  - Deixe o resto como padrão e clique no botão **Review + create** no final da página.
  - Após a execução da validação, selecione o botão **Create** na parte inferior da página.
  - Após a conclusão da implantação, selecione **Go to resource**

---
#### 2. Conectando-se à Máquina Virtual
- Inicie uma conexão da área de trabalho remota para a máquina virtual:
  - Selecione **Connect > RDP** na página de visão geral de sua máquina virtual.
  - Na aba **Connect with RDP**, mantenha as opções padrão para se conectar por endereço IP pela porta 3389 e clique em **Download RDP file**.
  - Abra o arquivo RDP baixado e clique em **Connect** quando solicitado.
  - Na janela Segurança do Windows, selecione Mais opções e Usar uma conta diferente. Digite o nome de usuário como localhost\/*username*, insira a senha que você criou para a máquina virtual e clique em **OK**.
    - Você pode receber um aviso do certificado durante o processo de logon. Clique em Sim ou em Continuar para criar a conexão.

---
#### 3. Instalar servidor Web
- **Instalação do Servidor Web IIS**:
  - Execute o comando no **PowerShell**:
    ```powershell
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    ```
- **Testando a Instalação**:
  - Copie o **endereço IP** da VM e cole no navegador para visualizar a página de boas-vindas do IIS.

---
#### 4. Gerenciamento e Boas Práticas
- **Desligamento Automático**: Configure para evitar cobranças desnecessárias.
  - Na seção **Operations**, selecione a opção **Auto-shutdown**.
  - Selecione a opção **On** para habilitar e, em seguida, defina uma hora que seja adequada para você.
  - Depois de definir a hora, selecione **Save** na parte superior para habilitar a configuração de Desligamento automático.
- **Excluir recursos**: Quando o grupo de recursos, a máquina virtual e todos os recursos relacionados não forem mais necessários.
  - Na página Overview, selecione o link **Resource group**.
  - Selecione **Delete resource group** na parte superior da página do grupo de recursos.
  - Digite o nome do grupo de recursos e selecione **Delete** para concluir a exclusão dos recursos e do grupo de recursos.
- **Segurança**: Utilize **Grupos de Segurança de Rede (NSG)** para controlar acessos.
- **Backup e Monitoramento**: Configure alertas e snapshots para proteção dos dados.

---
## Criar Instância Gerenciada de SQL do Azure
#### Criar uma Azure SQL Managed Instance

Usando o portal do Azure, siga estas etapas:

1. **Acesse ao Portal do Azure**: Faça login no [Portal do Azure](https://portal.azure.com). 
2. No menu esquerdo do portal do Azure, selecione **Azure SQL**.
3. Selecione **+ Create** para abrir a página **Select SQL deployment option**.
4. Escolha _Singe instance_ na lista suspensa e depois selecione **Criar** para abrir a página **Create Azure SQL Managed Instance**.

### 1. Aba 'Basics'
- **Subscription**: Sua assinatura. Uma assinatura que concede a você permissão para criar recursos.
- **Resource group**: Um grupo de recursos novo ou existente. Para ver os nomes do grupo de recursos válidos, consulte [Regras e restrições de nomenclatura](https://learn.microsoft.com/pt-br/azure/architecture/best-practices/resource-naming).
- **Managed Instance name**: Qualquer nome válido. Para ver os nomes válidos, consulte [Regras e restrições de nomenclatura](https://learn.microsoft.com/pt-br/azure/architecture/best-practices/resource-naming).
- **Region**: A região na qual você deseja criar a instância gerenciada. Para obter mais informações sobre as regiões, confira [Regiões do Azure](https://azure.microsoft.com/regions/).
- Em **Managed Instance details**, clique em **Configure Managed Instance** na seção **Compute + storage** para abrir a página.

| Configuração                  | Valor sugerido                                                                                                                                                                                                                                                                                                                                                                            |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Service Tier**              | A camada **General Purpose** atende à maioria das cargas de trabalho de produção e é a opção padrão.                                                                                                                                                                                                                                                                                      |
| **Hardware generation**       | A **Standard-series (Gen5)** é o padrão, que define os limites de computação e de memória.                                                                                                                                                                                                                                                                                                |
| **vCores**                    | Os vCores representam a quantidade exata de recursos de computação que sempre são provisionados para a carga de trabalho. **Eight vCores** é o padrão.                                                                                                                                                                                                                                    |
| **Storage in GB**             | Tamanho do armazenamento em GB. Selecione-o com base no tamanho de dados esperado.                                                                                                                                                                                                                                                                                                        |
| **SQL Server License**        | Use o pagamento conforme o uso, uma licença SQL existente com o [Benefício Híbrido do Azure](https://learn.microsoft.com/pt-br/azure/azure-sql/azure-hybrid-benefit?view=azuresql) ou habilite os [direitos de failover híbrido](https://learn.microsoft.com/pt-br/azure/azure-sql/managed-instance/managed-instance-link-feature-overview?view=azuresql#license-free-passive-dr-replica) |
| **Backup storage redundancy** | O **Geo-redundant backup storage** é o padrão e o recomendado.                                                                                                                                                                                                                                                                                                                            |

- **Belongs to an instance pool?**: Selecione **Sim** para que essa instância seja criada dentro de um pool de [instâncias](https://learn.microsoft.com/pt-br/azure/azure-sql/managed-instance/instance-pools-configure?view=azuresql).
- **Authentication method**: Usar a autenticação do SQL. Para fins deste guia de início rápido, use a autenticação SQL.
	- Você também pode optar por usar a autenticação do SQL e do [Microsoft Entra](https://learn.microsoft.com/pt-br/azure/azure-sql/database/authentication-aad-overview?view=azuresql). 
	- Qualquer nome de usuário válido. Para ver os nomes válidos, consulte [Regras e restrições de nomenclatura](https://learn.microsoft.com/pt-br/azure/architecture/best-practices/resource-naming). 
	- Qualquer senha válida. A senha deve ter no mínimo 16 caracteres e atender a [requisitos de complexidade definidos](https://learn.microsoft.com/pt-br/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm-).

Depois de designar sua configuração de **Compute + Storage**, use **Apply** para salvar suas configurações e navegar de volta para a página **Create Azure SQL Managed Instance**. Selecione **Next** para ir para a **Aba 'Networking'**

---
### 2. Aba 'Networking'

Preencha as informações opcionais na guia **Networking**. Se você omitir essas informações, o portal aplicará as configurações padrão.

| Configuração                                                                        | Valor sugerido                          |
| ----------------------------------------------------------------------------------- | --------------------------------------- |
| **Rede virtual / sub-rede**                                                         | Crie ou use uma rede virtual existente. |
| **Tipo de conexão**                                                                 | Escolha um tipo de conexão adequado.    |
| **Ponto de extremidade público**                                                    | Selecione **Desabilitar**.              |
| **Permitir o acesso de** (se o **Ponto de extremidade público** estiver habilitado) | Selecione **Sem Acesso**                |

Selecione **Review + create** para revisar suas escolhas antes de criar uma instância gerenciada. Ou defina as configurações de segurança selecionando **Next: Security settings**.

---
### 3. Aba 'Security'

Para fins deste guia de início rápido, deixe as configurações na aba **Security** em seus valores padrão.

Selecione **Review + create** para revisar suas escolhas antes de criar uma instância gerenciada. Ou defina mais configurações personalizadas selecionando **Next: Additional settings**.

---
### 4. Aba 'Additional settings'

Preencha as informações opcionais na aba **Additional settings**. Se você omitir essas informações, o portal aplicará as configurações padrão.

| Configuração           | Valor sugerido                                                   |
| ---------------------- | ---------------------------------------------------------------- |
| **Collation**          | Escolha a Collation que deseja usar para a instância gerenciada. |
| **Time zone**          | Selecione o fuso horário que sua instância gerenciada observará. |
| **Geo-Replication**    | Selecione **No**.                                                |
| **Maintenance window** | Escolha uma janela de manutenção adequada.                       |

Configure as Tags do Azure selecionando **Next: Tags**.

---
### 5. Aba 'Tags'

As [TAGS](https://learn.microsoft.com/pt-br/azure/azure-resource-manager/management/tag-resources) ajudam você a organizar logicamente seus recursos. As TAGS são mostradas nos relatórios de custo e permitem outras atividades de gerenciamento.

Marque com uma TAG sua nova *SQL Managed Instance* com uma TAG de Proprietário para identificar quem a criou e uma TAG de Environment para identificar se esse sistema é de produção, desenvolvimento etc.

Selecione **Review + create** para prosseguir.

---
### 6. Aba 'Review + create'

Selecione a guia **Review + create**, examine suas escolhas e selecione **Create** para implantar sua instância gerenciada.


---
### 7. Conclusão

O status da implantação pode ser conferido no ícone de notificações. Selecione **Deployment in progress** na notificação para abrir a janela da SQL Managed Instance e monitorar mais detalhadamente o progresso da implantação.


Após a conclusão da implantação, navegue até o grupo de recursos para exibir a instância gerenciada:

![Captura de tela dos recursos da Instância Gerenciada de SQL no portal do Azure.](https://learn.microsoft.com/pt-br/azure/azure-sql/managed-instance/media/instance-create-quickstart/azure-sql-managed-instance-resources.png?view=azuresql)


---
#### Criar banco de dados

Você pode criar um novo banco de dados usando o portal do Azure, o PowerShell ou a CLI do Azure.

Para criar um novo banco de dados para a sua instância no portal do Azure, siga estas etapas:
1. Acesse a [SQL Managed Instance](https://portal.azure.com/#view/HubsExtension/BrowseResource/resourceType/Microsoft.Sql%2FmanagedInstances) no portal do Azure.
2. Na página **Overview**, escolha **+ New database** da sua instância gerenciada de SQL para abrir a página **Create Azure SQL Managed Database**.
	- Dê um nome para o banco de dados na aba **Basics**.
	- Na aba **Data source**, selecione **Nenhuma** para um banco de dados vazio ou restaure um banco de dados a partir do backup.
	- Defina as configurações restantes nas guias restantes e selecione **Review + create** para validar suas escolhas.
3. Use **Create** para implantar seu banco de dados.


---
##### Sources
- [What is cloud computing](https://azure.microsoft.com/pt-br/resources/cloud-computing-dictionary/what-is-cloud-computing/)
- [Quickstart: Create a Windows virtual machine in the Azure portal](https://learn.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-portal)
- [Quickstart: Create Azure SQL Managed Instance - Azure SQL Managed Instance](https://learn.microsoft.com/en-us/azure/azure-sql/managed-instance/instance-create-quickstart?view=azuresql&tabs=azure-portal)
