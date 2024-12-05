# Criando um Modelo de Previsão no Azure Machine Learning pelo Portal Azure

Este guia descreve como criar, treinar e implantar um modelo de previsão diretamente pelo Portal do Azure utilizando **Azure Machine Learning**, realizado como exercício parte do DIO Bootcamp Microsoft IA-900.


## 1. **Configurar o Workspace do Azure ML**

1. **Acesse o Portal do Azure:**
   - Faça login no [Portal do Azure](https://portal.azure.com).

2. **Crie um Workspace do Azure ML:**
   - No menu esquerdo, clique em **Criar um recurso**.
   - Pesquise por **Machine Learning** e selecione e crie o **Workspace Machine Learning**.
   - Selecione a Assinatura
   - Crie um Grupo de Recursos
   - Preencha os detalhes:
     - Nome: `workspace-ml-predicao`.
     - Grupo de Recursos: Crie um novo ou escolha um existente.
     - Região: Escolha a mesma região dos seus dados.
   - Clique em **Revisar + Criar** e confirme.


## 2. **Criar e Configurar um Experimento**

1. **Abra o Workspace:**
   - Após a criação, acesse o workspace e clique em **Azure Machine Learning Studio** para abrir o estúdio no navegador.

2. **Prepare os dados:**
   - Vá para a aba **Dados** e clique em **+ Criar**.
   - Insira Nome e Descrição nos respectivos campos e escolha o tipo de dados (ex: urifile, mltable, Tabular, Arquivo, JSON, CSV), e clique em **Avançar**
   - Faça o upload do arquivo de entrada (o JSON fornecido), conecte a um armazenamento existente, ou forneça um link da Web.
   - Verifique e registre o Dado com um nome, como `bike-rentals`.

3. **Use aprendizado de máquina automatizado para treinar um modelo:**
   No Azure Machine Learning Studio , visualize a página ML automatizado (em Criação ).

   - Crie um novo trabalho de ML automatizado com as seguintes configurações, usando Avançar conforme necessário para avançar na interface do usuário:

   - Configurações básicas :

      - Nome do trabalho : O campo Nome do trabalho já deve estar preenchido previamente com um nome exclusivo. Mantenha-o como está.
      - Novo nome do experimento :mslearn-bike-rental
      - Descrição : Aprendizado de máquina automatizado para previsão de aluguel de bicicletas
      - Tags : nenhuma
      - Tipo de tarefa e dados :
   
     - Selecione o tipo de tarefa : Regressão
     - Selecionar conjunto de dados : Crie um novo conjunto de dados com as seguintes configurações:
     - Tipo de dados :
     - Nome :bike-rentals
     - Descrição :Historic bike rental data
     - Tipo : Tabela (mltable)
     - Fonte de dados :
     - Selecione De arquivos locais
     - Tipo de armazenamento de destino :
     - Tipo de armazenamento de dados : Azure Blob Storage
     - Nome : workspaceblobstore
     - Seleção de MLtable :
     - Pasta de upload : Baixe e descompacte a pasta que contém os  arquivos.
     - Selecione Criar . Após a criação do conjunto de dados, selecione o conjunto de dados bike-rentals para continuar a enviar o trabalho de ML automatizado.


## 3. **Avaliar Modelo**

1. Na guia Visão geral do trabalho de aprendizado de máquina automatizado, observe o melhor resumo do modelo.

2. Selecione o texto em Nome do algoritmo para o melhor modelo para visualizar seus detalhes.

3. Selecione a guia Métricas e selecione os gráficos residuais e prediction_true, caso ainda não estejam selecionados.


## 4. **Implantar o Modelo**

1. **Crie um endpoint:**
      - Na aba **Implantações**, clique em **+ Criar Ponto de Extremidade**.
      - Escolha **Ação de Inferência em Tempo Real**.
      - Preencha os detalhes:
         Máquina virtual : Standard_DS3_v2
         Contagem de instâncias : 3
         Ponto final : Novo
         Nome do ponto de extremidade : deixe o padrão ou certifique-se de que seja globalmente exclusivo
         Nome da implantação : Deixe o padrão
         Inferência de coleta de dados : Desativado
         Modelo de pacote : Desativado
   
2. **Implante:**
   - Aguarde até que o status Deploy mude para Succeeded . Isso pode levar de 5 a 10 minutos.


## 5. **Testar o Endpoint**

- No Azure Machine Learning Studio, no menu à esquerda, selecione Endpoints e abra o endpoint em tempo real predict-rentals .

- Na página do endpoint em tempo real do predict-rentals , visualize a guia Teste .

- No painel Dados de entrada para testar o ponto de extremidade , substitua o JSON do modelo pelos seguintes dados de entrada:


     ```json
     {
       "input_data": {
         "columns": [
           "day",
           "mnth",
           "year",
           "season",
           "holiday",
           "weekday",
           "workingday",
           "weathersit",
           "temp",
           "atemp",
           "hum",
           "windspeed"
         ],
         "index": [0],
         "data": [[1,1,2022,2,0,1,1,2,0.3,0.3,0.3,0.3]]
       }
     }
     ```

3. **Verifique o resultado:**
   - O resultado esperado será algo como:
     ```json
     [361.41500445154400]
     ```

4. **Teste em sua aplicação:**
   - Use a URL e a chave para realizar requisições via **cURL**, Postman ou outro cliente HTTP.


## 6. **Monitore e Atualize**

1. **Acesse o monitoramento:**
   - Na aba **Implantações**, visualize logs e desempenho do endpoint.

2. **Atualize o modelo:**
   - Para melhorar o desempenho, atualize o modelo treinando-o com novos dados e publique uma nova versão.



## Limpar
O serviço web que você criou está hospedado em uma *Azure Container Instance*. Se você não pretende usa-lo, você pode excluir o **ponto de extremidade** para evitar acumular uso desnecessário do Azure.
No Azure Machine Learning Studio, acesse a guia **ponto de extremidade**  e em seguida, selecione 'Delete' e confirme que deseja exclui-lo.
