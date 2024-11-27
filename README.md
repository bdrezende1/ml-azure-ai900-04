# ml-azure-ai900-04

Aplicaremos técnicas de organização de documentos e conduziremos pesquisas eficientes através da ingestão de dados, seguindo três passos essenciais: ingestão de conhecimento de IA, criação do índice correspondente e exploração dessas funcionalidades. 

---

## Laboratório: Busca com Inteligência Artificial no Azure

**Passo 1: Criar uma Conta no Azure**
Se você ainda não tem uma conta no Azure, o primeiro passo é criar uma. Acesse [o site do Azure](https://azure.microsoft.com/) e clique em "Iniciar gratuitamente". Siga as instruções para criar sua conta.

**Passo 2: Criar um Recurso do Azure Cognitive Search**
Agora que você tem uma conta no Azure, vamos criar o recurso necessário para o laboratório:

1. No portal do Azure, clique em `Criar um recurso`.
2. Digite "Azure Cognitive Search" na barra de pesquisa e selecione a opção que aparecer.
3. Clique em `Criar`.

**Preenchendo o Formulário de Criação:**

- **Assinatura**: Selecione a assinatura que você está usando.
- **Grupo de Recursos**: Você pode criar um novo grupo de recursos ou usar um existente.
- **Nome do Serviço de Busca**: Escolha um nome exclusivo para o seu serviço de busca.
- **Localização**: Escolha a região mais próxima de você.
- **Plano de Preços**: Selecione um plano que atenda às suas necessidades. Para fins de aprendizado, o plano gratuito é uma boa opção.
- **Partições e Réplicas**: Para este laboratório, podemos deixar as configurações padrão.

Após preencher os campos, clique em `Revisar + Criar`, e depois em `Criar`.

**Passo 3: Obter as Chaves e o Endpoint do Serviço de Busca**
Uma vez criado o recurso, você precisará das chaves de acesso e do endpoint:

1. Vá para seu recurso de Azure Cognitive Search no portal do Azure.
2. No menu à esquerda, clique em `Chaves e Endpoint`.
3. Anote as chaves de administração e o endpoint que serão exibidos.

**Passo 4: Configurar seu Ambiente de Desenvolvimento**
Para interagir com o serviço de busca, você precisará configurar seu ambiente de desenvolvimento. Se estiver usando Python, instale a biblioteca necessária:

```bash
pip install azure-search-documents
```

**Passo 5: Criar um Índice de Busca**
Um índice de busca é uma estrutura onde seus dados de busca são armazenados e organizados. Veja como criar um índice usando Python:

```python
from azure.core.credentials import AzureKeyCredential
from azure.search.documents import SearchClient
from azure.search.documents.indexes import SearchIndexClient
from azure.search.documents.indexes.models import SearchIndex, SearchField, SimpleField, SearchableField

# Substitua pelos valores do seu serviço
service_endpoint = "SEU_ENDPOINT_AQUI"
admin_key = "SUA_CHAVE_DE_ADMINISTRACAO_AQUI"

search_client = SearchIndexClient(endpoint=service_endpoint, credential=AzureKeyCredential(admin_key))

fields = [
    SimpleField(name="id", type="Edm.String", key=True),
    SearchableField(name="content", type="Edm.String", analyzer_name="en.lucene"),
]

index = SearchIndex(name="meu-indice", fields=fields)
result = search_client.create_index(index)

print(f"Índice '{result.name}' criado com sucesso.")
```

**Passo 6: Carregar Dados no Índice**
Com o índice criado, podemos adicionar documentos a ele. Aqui está um exemplo de como fazer isso:

```python
from azure.search.documents import SearchClient
from azure.core.credentials import AzureKeyCredential

search_client = SearchClient(endpoint=service_endpoint, index_name="meu-indice", credential=AzureKeyCredential(admin_key))

documents = [
    {"id": "1", "content": "Este é um exemplo de documento para o Azure Cognitive Search."},
    {"id": "2", "content": "Aprender a usar o Azure Search pode facilitar muitas tarefas."}
]

result = search_client.upload_documents(documents=documents)
print(f"Documentos adicionados: {result}")
```

**Passo 7: Executar uma Consulta de Busca**
Agora que os dados estão indexados, podemos executar consultas de busca. Veja como buscar documentos com base em termos:

```python
response = search_client.search("exemplo")

for doc in response:
    print(f"ID: {doc['id']} | Conteúdo: {doc['content']}")
```

![image](https://github.com/user-attachments/assets/6357ebb3-45fb-409a-ab4c-e820eb32caa0)

![image](https://github.com/user-attachments/assets/2cbc0bac-02eb-4d4f-ae43-e66e63aa4b5f)

