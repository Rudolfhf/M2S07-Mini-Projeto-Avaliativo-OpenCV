# Pipeline de Pré-processamento de imagens com OpenCV
## Mini Projeto Avaliativo - Módulo 2:
 
 **Aluno:** Rudolf Hoffmann<br>
 **Professor:** Junior Prado<br>
 **Curso:** Machine Learning e Visão Computacional - Programa (SCTEC)

## 1. Descrição do Mini Projeto
Esse trabalho tem como objetivo aplicar técnicas de redução de ruídos, conversão de cores e detecção de bordas para padronizar as imagens que serão utilizadas por algoritmos para treinar um modelo preditivo. 

As imagens são parte do dataset que contém fotos reais de peças de fundição metálica, classificadas em duas categorias: 
com defeito (`def_front`) e sem defeito (`ok_front`).
Elas podem ser baixadas através do link: 
https://drive.google.com/file/d/1K5gNxQ7RXA-nb4boNzPYQTJlRvJyYBD1/view?usp=sharing

## 2. Bibliotecas e Tecnologias utilizadas
- Python 3.10.1
- OpenCV 5.0.0.93
- Numpy 2.2.6
- Pathlib
- os
- Editor: Vscode

## 3. Instalação
#### Estrutura:
1. Clone o repositório:

```bash
git clone "https://github.com/Rudolfhf/M2S07-Mini-Projeto-Avaliativo-OpenCV.git"
```
2. Dentro da pasta do projeto crie a pasta `data/raw`.
3. Extraia o dataset baixado e copie **as imagens** 
-> da pasta `/casting_512x512/def_front` 
-> para dentro da pasta `data/raw`.

#### Configuração Windows:
4. Crie e ative um ambiente virtual:

```bash
python -m venv .venv
.venv\Scripts\activate
```

5. Instale as dependências:
```bash
pip install -r requirements.txt
```
6. Abra o notebook `pipeline.ipynb` no VS Code ou Jupyter.
7. Execute o processamento em lote no segundo bloco:


## 4. Pipeline de pré-processamento
0. Leitura em lote das imagens (com extensão válida)
1. Conversão para Grayscale
2. Filtro para redução de ruído [Suavização Bilateral Blur]
3. Limiarização (Threshold) Adaptive [ADAPTIVE_THRESH_GAUSSIAN_C, cv2.THRESH_BINARY]
4. Destaque de característica com Canny [20,100]
5. Operação Morfológica morphologyEx [MORPH_CLOSE] com Kernel[5,5]
6. Padronização para formato 256x256

## 5. Estrutura de pastas
```
projeto/
├── data/
│ ├── raw/ # imagens originais do dataset
│ └── processed_images/ # imagens processadas
├── pipeline.ipynb
├── requirements.txt
├── .gitignore
└── readme.md
```

## 6. Sprints e Branches

 - Sprint 1:`development` **Configuração do repositório**
 - Sprint 2:`estruturacao-e-leitura` **leitura de imagens em lote**
 - Sprint 3:`preprocessamento-base` **Grayscale e suavização**
 - Sprint 4:`segmentacao-caracteristicas` **Limiarização e detecção de bordas**
 - Sprint 5:`refinamento-morfologico` **Operações morfológicas e resize**
 - Sprint 6:`gravacao-documentacao` **Gravação das imagens e documentação final**

### Decisões técnicas e justificativas

**Suavização — Bilateral em vez de Gaussian/Median:**  
Testamos os três filtros na mesma imagem de amostra. O Bilateral Filter, com parâmetros 
ajustados (`sigmaColor=20, sigmaSpace=20`), apresentou o melhor equilíbrio: suaviza a 
textura interna da peça sem borrar as bordas dos aros metálicos — algo que testes com 
`sigma` mais alto (75) não conseguiram (as bordas ficaram visivelmente borradas).

**Limiarização — Adaptive Threshold em vez de Otsu:**  
Testamos o método de Otsu, mas ele não separou adequadamente a peça do fundo: a imagem 
apresenta três zonas distintas de intensidade (fundo em cinza médio, aro metálico claro 
e região central escura), e o Otsu — por calcular um único limiar global — agrupou 
incorretamente o fundo com o aro claro. O Adaptive Threshold, por calcular o limiar 
localmente em blocos de vizinhança, lidou melhor com essa variação de iluminação/contraste.


