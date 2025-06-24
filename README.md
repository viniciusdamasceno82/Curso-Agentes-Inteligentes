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
