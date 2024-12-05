# Criando um Modelo de Previsão no Azure Machine Learning pelo Portal Au ure

Este guia descreve como criar, treinar e implantar um modelo de previsão diretamente pelo Portal do Azure utilizando **Azure Machine Learning**, realizado como exercício parte do DIO Bootcamp Microsoft IA-900.

---

## 1. **Configurar o Workspace do Azure ML**

1. **Acesse o Portal do Azure:**
   - Faça login no [Portal do Azure](https://portal.azure.com).

2. **Crie um Workspace do Azure ML:**
   - No menu esquerdo, clique em **Criar um recurso**.
   - Pesquise por **Machine Learning** e selecione **Machine Learning Workspace**.
   - Preencha os detalhes:
     - Nome: `workspace-ml-predicao`.
     - Grupo de Recursos: Crie um novo ou escolha um existente.
     - Região: Escolha a mesma região dos seus dados.
   - Clique em **Revisar + Criar** e confirme.

---

## 2. **Criar e Configurar um Experimento**

1. **Abra o Workspace:**
   - Após a criação, acesse o workspace e clique em **Azure Machine Learning Studio** para abrir o estúdio no navegador.

2. **Prepare os dados:**
   - Vá para a aba **Datasets** e clique em **+ Criar Dataset**.
   - Escolha o tipo de dados (ex: JSON, CSV).
   - Faça o upload do arquivo de entrada (o JSON fornecido) ou conecte a um armazenamento existente.
   - Verifique e registre o dataset com um nome, como `dados-previsao`.

3. **Crie um pipeline de treinamento:**
   - Na aba **Designer**, clique em **+ Criar Pipeline**.
   - Arraste o dataset (`dados-previsao`) para a tela.
   - Adicione módulos como:
     - **Dividir Dados** para separar treinamento e teste.
     - **Treinar Modelo** (escolha um algoritmo, ex: Regressão Linear ou Florestas Aleatórias).
     - **Avaliar Modelo** para validar os resultados.
   - Conecte os módulos na sequência lógica e clique em **Executar**.
   - Nomeie o experimento e aguarde a execução.

---

## 3. **Registrar o Modelo Treinado**

1. **Acesse os resultados do experimento:**
   - Após o término do pipeline, clique no módulo de treinamento e baixe ou registre o modelo gerado.

2. **Registre o modelo:**
   - Vá para a aba **Modelos** no menu lateral.
   - Clique em **+ Registrar Modelo**.
   - Selecione o modelo gerado e preencha os detalhes:
     - Nome do modelo: `modelo-previsao`.
     - Versão: `1`.

---

## 4. **Implantar o Modelo**

1. **Crie um endpoint:**
   - Na aba **Implantações**, clique em **+ Criar Endpoint**.
   - Escolha **Ação de Inferência em Tempo Real**.
   - Preencha os detalhes:
     - Nome: `previsao-endpoint`.
     - Modelo: Selecione `modelo-previsao`.
     - Tipo de compute: Escolha **Aci (Azure Container Instances)** para testes simples.

2. **Configurar o script de inferência:**
   - O portal cria automaticamente um script baseado no modelo registrado. Verifique e ajuste se necessário.

3. **Implante:**
   - Clique em **Implantar** e aguarde até que o endpoint seja criado.

---

## 5. **Testar o Endpoint**

1. **Obtenha a URL de Inferência:**
   - Após a implantação, clique no endpoint criado.
   - Copie a **URL de Inferência** e a **Chave de Acesso**.

2. **Teste pelo portal:**
   - Na aba **Testar**, insira os dados de entrada:
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

---

## 6. **Monitore e Atualize**

1. **Acesse o monitoramento:**
   - Na aba **Implantações**, visualize logs e desempenho do endpoint.

2. **Atualize o modelo:**
   - Para melhorar o desempenho, atualize o modelo treinando-o com novos dados e publique uma nova versão.

---

Com isso, você terá configurado, treinado e implantado um modelo de previsão diretamente pelo Portal do Azure!
```
