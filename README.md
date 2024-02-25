# Classificação de Falhas Elétricas

## Contexto do Projeto

A linha de transmissão é um componente crucial do sistema de energia. À medida que a demanda por energia aumentou exponencialmente na era moderna, o papel central da linha de transmissão é transportar eletricidade da área de origem para a rede de distribuição. O sistema elétrico é composto por diversos elementos complexos e dinâmicos, sujeitos a perturbações e falhas.

Usinas de geração elétrica de alta capacidade e redes de usinas sincronizadas, geograficamente dispersas, requerem detecção rápida de falhas e operação eficiente dos equipamentos de proteção. Detectar e classificar corretamente as falhas nas linhas de transmissão é essencial para manter a estabilidade do sistema. Além disso, o sistema de proteção pode acionar relés adicionais para proteger o sistema contra interrupções. A aplicação de tecnologia de reconhecimento de padrões pode ajudar a distinguir sistemas elétricos saudáveis de defeituosos e a identificar as fases afetadas em sistemas trifásicos com falhas.

Nesse sentido, O modelo supervisionado **Random Forest** é uma técnica de aprendizado de máquina que pode ser aplicada para classificar falhas elétricas em linhas de transmissão. 

## Sobre o Modelo

Random Forest (Floresta Aleatória) é um algoritmo de classificação baseado em árvores de decisão. Consiste em criar várias árvores de decisão (a “floresta”) e combinar suas previsões para obter uma classificação final. Cada árvore é treinada em uma amostra aleatória dos dados de treinamento. As árvores individuais votam na classe mais provável, e a classe com mais votos é a previsão final.

O Random Forest pode ser usado para classificar falhas elétricas nas linhas de transmissão com base em dados históricos. Aqui estão algumas maneiras pelas quais ele pode ajudar: 

- Uma vez detectada uma anomalia, o modelo pode classificar a falha em categorias específicas. 
- Por exemplo, distinguir entre uma falha de isolamento e um curto-circuito. 
- Isso ajuda os operadores a tomar medidas corretivas adequadas. c. Tempo de Resposta Rápido: 
- O Random Forest é eficiente em termos de tempo de processamento. - Pode fornecer resultados quase em tempo real, permitindo uma resposta rápida a falhas.

Treinamento e Avaliação:

- O modelo é treinado com dados históricos rotulados (exemplos de falhas conhecidas).
- Os recursos (medidas elétricas) são usados como entradas, e as classes de falhas são os rótulos.
- A precisão do modelo é avaliada usando dados de teste não vistos.

## Sobre o Projeto

Este projeto emprega o algoritmo **RandomForestClassifier**, uma poderosa ferramenta de aprendizado de máquina do pacote scikit-learn, com o objetivo de classificar diversos tipos de falhas elétricas. Uma característica distintiva deste projeto é a introdução de ruído gaussiano aos dados. O ruído gaussiano, também conhecido como ruído branco, é uma forma de variação aleatória que é adicionada aos dados para simular incertezas ou variações naturais. Neste contexto, o ruído gaussiano é usado para testar a robustez do modelo em face de pequenas perturbações nos dados. Ao adicionar esse ruído, podemos observar o impacto direto na acurácia do modelo, proporcionando uma visão valiosa sobre a capacidade do modelo de generalizar a partir de dados ruidosos. Isso é particularmente relevante em ambientes do mundo real, onde os dados podem ser sujeitos a várias fontes de ruído e incerteza. Portanto, a análise da relação entre o ruído gaussiano e a acurácia do modelo é um aspecto crucial deste projeto.

### Dados

Os dados usados neste projeto estão contidos em um arquivo Excel chamado [dataset_electrical_fault.xlsx](dataset_electrical_fault.xlsx). Cada linha no conjunto de dados representa um conjunto de medições elétricas, e a coluna 'fault_type' representa o tipo de falha elétrica.

### Dicionário das Colunas

|Nome da Coluna|Papel|Tipo|Descrição|
|--------------|-----|----|---------|
|fault_location|Recurso|Discreto|Valores representando a posição onde ocorreu a falta na linha de transmissão|
|rms_phase_A_current|Recurso|Discreto|Raiz quadrada média, fase A para o sinal de corrente|
|rms_phase_B_current|Recurso|Discreto|Raiz quadrada média, fase B para o sinal de corrente|
|rms_phase_C_current|Recurso|Discreto|Raiz quadrada média, fase C para o sinal de corrente|
|rms_phase_A_voltage|Recurso|Discreto|Raiz quadrada média, fase A para o sinal de tensão|
|rms_phase_B_voltage|Recurso|Discreto|Raiz quadrada média, fase B para o sinal de tensão|
|rms_phase_C_current|Recurso|Discreto|Raiz quadrada média, fase C para o sinal de tensão|
|energy_phase_A_current|Recurso|Discreto|Energia absoluta, fase A para o sinal de corrente|
|energy_phase_B_current|Recurso|Discreto|Energia absoluta, fase B para o sinal de corrente|
|energy_phase_C_current|Recurso|Discreto|Energia absoluta, fase C para o sinal de corrente|
|rms_phase_C_current|Recurso|Discreto|Raiz quadrada média, fase C para o sinal de tensão|
|fault_type|Alvo|Categórico|AG, BG, CG, AB, AC, BC, ABG, ACG, BCG, ABC|

### Tipos de Falhas

|Tipo de Falha|Descrição|
|--------------|--------|
|AG|fase A está em curto com a terra|
|BG|fase B está em curto com a terra|
|CG|fase C está em curto com a terra|
|AB|fase A está em curto com a fase B|
|AC|fase A está em curto com a fase C|
|BC|fase B está em curto com a fase C|
|BC|fase B está em curto com a fase C|
|ABG|fase A e B estão em curto com a terra|
|ACG|fase A e C estão em curto com a terra|
|BCG|fase B e C estão em curto com a terra|
|ABC|fase A, B e C estão em curto entre si (falha trifásica)|

### Pré-processamento

O pré-processamento dos dados envolve a substituição de vírgulas por pontos e a conversão dos valores para números. Além disso, os dados são divididos em recursos (X) e rótulos (y). Os recursos são todas as colunas, exceto a coluna 'fault_type', que é o rótulo.

## Modelo 

O modelo pode ser vizualizado em [classificacao_falhas_eletricas.ipynb](electrical_fault_classification.ipynb)

## Conclusão

Adicionar ruído branco aos meus dados e observar uma diminuição na acurácia do modelo à medida que o desvio padrão do ruído aumenta pode levar a algumas possíveis conclusões:

- Robustez do Modelo: Meu modelo pode não ser robusto a variações nos dados. Em um cenário do mundo real, os dados podem ter algum nível de ruído. Se o desempenho do modelo diminui significativamente com a adição de ruído, isso pode indicar que o modelo pode não funcionar bem quando exposto a dados do mundo real que contêm ruído.

- Sobreajuste: Se o modelo tem um desempenho muito bom nos dados sem ruído, mas o desempenho diminui com a adição de ruído, isso pode ser um sinal de sobreajuste. O modelo pode estar aprendendo a representar os dados de treinamento muito bem, mas falha ao generalizar para novos dados ou dados com pequenas variações (ruído).

- Importância da Limpeza de Dados: A adição de ruído e a observação do impacto no desempenho do modelo destacam a importância da limpeza de dados e do pré-processamento. Em muitos casos, a limpeza de dados e a remoção de ruído podem ser tão importantes quanto a escolha do algoritmo de aprendizado de máquina.

- Necessidade de Regularização: Se o seu modelo é sensível ao ruído, pode ser útil explorar técnicas de regularização. A regularização adiciona uma penalidade à complexidade do modelo na função de perda, o que pode ajudar a prevenir o sobreajuste e tornar o modelo mais robusto ao ruído.

## Tecnologias Utilizadas

- Python 3.10.9;
- Jupyter Notebook;
- Numpy;
- Pandas;
- Scikit-Learn;
- Matplotlib.

## Contribuições 

Contribuições são bem vindas. Sinta-se a vontade para sugerir melhorias e possíveis correções no código.

## Autor 

Jadeson Bruno Albuquerque da Silva

[![Linkedin](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/jadeson-silva/)