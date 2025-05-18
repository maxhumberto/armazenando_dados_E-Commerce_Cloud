# armazenando_dados_E-Commerce_Cloud
Etapas do Projeto

### 1️⃣ Criar um Resource Group
1. Acesse o portal [Azure Portal](https://portal.azure.com).
2. Vá em **"Resource Groups"** e clique em **"Create"**.
3. Defina:
   - Subscription: sua assinatura ativa
   - Resource Group Name: `meu-grupo-produtos`
   - Region: ex: `Brazil South`
4. Clique em **Review + Create** e depois em **Create**.

---

### 2️⃣ Criar um SQL Database
1. Vá em **"SQL Databases"** e clique em **"Create"**.
2. Preencha os campos:
   - Database name: `ProdutosDB`
   - Server: criar um novo (`sql-server-produtos`)
   - Authentication: SQL authentication (defina usuário e senha)
   - Elastic Pool: No
   - Compute + Storage: escolha o tamanho básico (ex: 5 DTUs)
3. Selecione o resource group criado.
4. Clique em **Review + Create** e depois em **Create**.

---

### 3️⃣ Criar uma Storage Account (Blob)
1. Vá em **"Storage Accounts"** e clique em **"Create"**.
2. Preencha:
   - Storage account name: `storageprodutos`
   - Region: mesma do resource group
   - Performance: Standard
   - Redundancy: LRS (Local Redundant Storage)
3. Selecione o mesmo resource group.
4. Clique em **Review + Create** e depois em **Create**.

---

### 4️⃣ Configurar o Banco de Dados e Criar Tabela
1. Conecte-se ao SQL Server usando SSMS ou Azure Data Studio.
2. Execute o script para criar a tabela de produtos:

```sql
CREATE TABLE Produtos (
    Id INT PRIMARY KEY IDENTITY(1,1),
    Nome NVARCHAR(100),
    Descricao NVARCHAR(255),
    Preco DECIMAL(10,2),
    ImagemURL NVARCHAR(500)
);

5️⃣ Implementar Upload de Imagens no Blob Storage
Obtenha a Connection String da sua storage account no Azure.

Crie um container chamado imagens-produtos.

No backend, implemente o upload de arquivos para o Blob Storage e armazene a URL na tabela SQL.

### Exemplo.

from azure.storage.blob import BlobServiceClient

connection_string = "sua-connection-string"
blob_service_client = BlobServiceClient.from_connection_string(connection_string)
container_name = "imagens-produtos"
container_client = blob_service_client.get_container_client(container_name)

def upload_imagem(caminho_arquivo, nome_arquivo):
    with open(caminho_arquivo, "rb") as data:
        container_client.upload_blob(name=nome_arquivo, data=data)
    return f"https://storageprodutos.blob.core.windows.net/{container_name}/{nome_arquivo}"
