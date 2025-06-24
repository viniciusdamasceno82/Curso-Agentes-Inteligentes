# Curso-Agentes-Inteligentes

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
      responder_filtros(tipo, categoria), criar_grafico_linha(), criar_grafico_pizza(), criar_tabela_fornecedores().
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
          # Adicionar sessões para gráficos: btn_grafico.click(fn=criar_grafico_linha, outputs=...)
      demo.launch()
    ```
