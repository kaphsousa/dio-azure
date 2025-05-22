# XP Inc. - Cloud com Inteligência Artificial

### **Guia Prático: Criação e Configuração de uma Máquina Virtual no Azure**

#### **1. Introdução**
Computação em Nuvem é como um grande drive virtual que você pode acessar de qualquer lugar. Em vez de guardar arquivos e programas no seu computador, tudo fica salvo em servidores espalhados pelo mundo, prontos para serem usados quando você precisar.
- Vantagens de utilizar **Máquinas Virtuais (VMs)** no Azure:
  - ✅ Não ocupa espaço no seu computador.
  - ✅ Você pode acessar de qualquer lugar.
  - ✅ Mais segurança para os seus dados.
  - ✅ Facilidade para empresas e desenvolvedores criarem sistemas.

---
#### **2. Criando uma Máquina Virtual**
- **Acesso ao Portal do Azure**: Faça login no [Portal do Azure](https://portal.azure.com).
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
#### **3. Conectando-se à Máquina Virtual**
- Inicie uma conexão da área de trabalho remota para a máquina virtual:
  - Selecione **Connect > RDP** na página de visão geral de sua máquina virtual.
  - Na aba **Connect with RDP**, mantenha as opções padrão para se conectar por endereço IP pela porta 3389 e clique em **Download RDP file**.
  - Abra o arquivo RDP baixado e clique em **Connect** quando solicitado.
  - Na janela Segurança do Windows, selecione Mais opções e Usar uma conta diferente. Digite o nome de usuário como localhost\/*username*, insira a senha que você criou para a máquina virtual e clique em **OK**.
    - Você pode receber um aviso do certificado durante o processo de logon. Clique em Sim ou em Continuar para criar a conexão.

---
#### **4. Instalar servidor Web**
- **Instalação do Servidor Web IIS**:
  - Execute o comando no **PowerShell**:
    ```powershell
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    ```
- **Testando a Instalação**:
  - Copie o **endereço IP** da VM e cole no navegador para visualizar a página de boas-vindas do IIS.

---
#### **5. Gerenciamento e Boas Práticas**
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
##### Sources
- [What is cloud computing](https://azure.microsoft.com/pt-br/resources/cloud-computing-dictionary/what-is-cloud-computing/)
- [Quickstart: Create a Windows virtual machine in the Azure portal](https://learn.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-portal)
