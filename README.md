# Curso Agentes Autônomos com Redes Generativas

<p align="center">
    <picture>
        <image src="/i2a2-instituto-inteligencia-artificial-aplicada.png" alt="Logo I2A2" width="30%">  
    </picture>
</p>

**Institut d'Intelligence Artificielle Appliquée**

**Grupo de Trabalho Agentes Inteligentes**

Relatório da Atividade: Agente de Análise de Notas Fiscais

Prof: Celso Azevedo

Alunos:

-   Adriano Silva
-   Alan Batessoco
-   Carlos Santos
-   Gabriel Brognoli
-   Isabel Maito
-   Nicholas Lemos
-   Rogério Chaves
-   Vanessa Léda
-   Vinicius Damasceno

## 📝 Project Structure

```
agente_csv/
│
├── data/
│ ├── 202401_NFs.zip # Arquivo ZIP original com CSVs
│ └── temp_extracted/ # Onde o ZIP é descompactado em runtime
│ ├── 202401_NFs_Cabecalho.csv
│ └── 202401_NFs_Itens.csv
│
├── .requirements.txt # Contém: gradio, pandas, plotly, etc.
├── .README.md # Descrição do projeto, instruções de execução
├── iniciar.py # (Opcional) launcher para desktop/Tkinter
├── agente_csv_app.py # Script principal com interface Gradio
├── executar.bat / executar.sh # Scripts para iniciar a aplicação
└── outros scripts auxiliares # Por exemplo, scripts para build (PyInstaller)
```

<br>

## Fluxo de Execução

1. Descompactação

    - O agente carrega o arquivo 202401_NFs.zip (espera estar em data/202401_NFs.zip ou em um caminho relativo) e descompacta para data/temp_extracted/.
    - Dentro do ZIP, valida-se a existência dos dois CSVs:
        - 202401_NFs_Cabecalho.csv
        - 202401_NFs_Itens.csv

2. Carregamento dos dados

    - Usando Pandas para ler CSVs e converter datas se necessário.
    - Validações básicas: tipos de coluna, ausência de valores nulos críticos.

3. Preparação das funções de análise

    - Exemplos:

    ```
      responder_filtros(tipo, categoria),
      criar_grafico_linha(),
      criar_grafico_pizza(),
      criar_tabela_fornecedores().
    ```

    - Uso de operações

    ```
     Pandas: groupby, sum, value_counts, describe, etc.
    ```

4. Interface com Gradio

    - Componentes: Dropdowns, Botões, Áreas de texto, Áreas de exibição de gráficos/tabelas.
    - Exemplo:

    ```
    with gr.Blocks() as demo:
          tipo_dropdown = gr.Dropdown(choices=["Fornecedor", "Item", ...], label="Tipo de análise")

          categoria_dropdown = gr.Dropdown(choices=["Maior valor total", "Menor valor total", ...], label="Categoria")

          output_text = gr.Textbox(label="Resultado")

          btn = gr.Button("Gerar Resposta")

          btn.click(fn=responder_filtros, inputs=[tipo_dropdown, categoria_dropdown], outputs=output_text)

          # Adicionar sessões para gráficos:
          btn_grafico.click(fn=criar_grafico_linha, outputs=...)

      demo.launch()

    ```

5. Tratamento de erros / mensagens ao usuário

    - Mensagens claras se o ZIP não estiver presente ou estiver corrompido.
    - Verificar existência de colunas esperadas; em caso negativo, log/alerta.
    - Validação dos inputs: categorias compatíveis com o tipo selecionado.

6. Build e Deploy

    - .requirements.txt com dependências exatas (gradio, pandas, plotly).
    - Scripts de inicialização: executar.sh, executar.bat, iniciar.py.
    - Instruções no README: instalação e execução.
    - Observação sobre variáveis de ambiente para chaves (caso use APIs externas).

7. Exemplos de Perguntas e Respostas

-   Pergunta: Qual é o fornecedor que teve maior montante recebido?
-   Como foi obtida:

    ```
    fornecedor_maior = cabecalho.groupby("RAZÃO SOCIAL EMITENTE")["VALOR NOTA FISCAL"].sum().sort_values(ascending=False).head(1)

    Resposta (exemplo):

    RAZÃO SOCIAL EMITENTE
    CHEMYUNION LTDA    1292418.75

    ```

-   Pergunta: Qual item (produto/serviço) teve maior volume entregue (em quantidade)?
-   Como foi obtida:

    ```
    item_maior = itens.groupby("DESCRIÇÃO DO PRODUTO/SERVIÇO")["QUANTIDADE"].sum().sort_values(ascending=False).head(1)

    Resposta (exemplo):

    DESCRIÇÃO DO PRODUTO/SERVIÇO
    DIPIFARMA INJETÁVEL    5000.0

    ```

-   Pergunta: Quantas notas fiscais existem de cada fornecedor (top 10)?
-   Como foi obtida:

    ```
    notas_por_fornecedor = cabecalho["RAZÃO SOCIAL EMITENTE"].value_counts().head(10)

    Resposta (exemplo):

    FORNECEDOR A    12
    FORNECEDOR B     9

    ```

-   Pergunta: Qual a distribuição de valores das notas (mínimo, máximo e média)?
-   Como foi obtida:

    ```
    stats = cabecalho["VALOR NOTA FISCAL"].describe()[["min", "max", "mean"]]

    Resposta (exemplo):

    Mínimo: 20.00
    Máximo: 1.292.418,75
    Média: 3.371,XX

    ```
