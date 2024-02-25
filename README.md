{
 "cells": [
  {
   "cell_type": "markdown",
   "id": "4112f22f-ab01-4bdd-92fb-b193d32a2c7a",
   "metadata": {},
   "source": [
    "# Classificação de Falhas Elétricas"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "4770be96-30ad-42ab-b792-906004bc1f06",
   "metadata": {},
   "source": [
    "## Contexto do Projeto"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "1336a4c2-ef77-439b-8baf-32124ed538fd",
   "metadata": {},
   "source": [
    "A linha de transmissão é um componente crucial do sistema de energia. À medida que a demanda por energia aumentou exponencialmente na era moderna, o papel central da linha de transmissão é transportar eletricidade da área de origem para a rede de distribuição. O sistema elétrico é composto por diversos elementos complexos e dinâmicos, sujeitos a perturbações e falhas.\n",
    "\n",
    "Usinas de geração elétrica de alta capacidade e redes de usinas sincronizadas, geograficamente dispersas, requerem detecção rápida de falhas e operação eficiente dos equipamentos de proteção. Detectar e classificar corretamente as falhas nas linhas de transmissão é essencial para manter a estabilidade do sistema. Além disso, o sistema de proteção pode acionar relés adicionais para proteger o sistema contra interrupções. A aplicação de tecnologia de reconhecimento de padrões pode ajudar a distinguir sistemas elétricos saudáveis de defeituosos e a identificar as fases afetadas em sistemas trifásicos com falhas.\n",
    "\n",
    "Nesse sentido, O modelo supervisionado **Random Forest** é uma técnica de aprendizado de máquina que pode ser aplicada para classificar falhas elétricas em linhas de transmissão. "
   ]
  },
  {
   "cell_type": "markdown",
   "id": "1947730e-fdc4-413f-b9bd-08dfdcbc9ea7",
   "metadata": {},
   "source": [
    "## Sobre o Modelo"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "2890f213-403b-48b4-b2b3-bc6a2f521d25",
   "metadata": {},
   "source": [
    "Random Forest (Floresta Aleatória) é um algoritmo de classificação baseado em árvores de decisão. Consiste em criar várias árvores de decisão (a “floresta”) e combinar suas previsões para obter uma classificação final. Cada árvore é treinada em uma amostra aleatória dos dados de treinamento. As árvores individuais votam na classe mais provável, e a classe com mais votos é a previsão final.\n",
    "\n",
    "O Random Forest pode ser usado para classificar falhas elétricas nas linhas de transmissão com base em dados históricos. Aqui estão algumas maneiras pelas quais ele pode ajudar: \n",
    "\n",
    "- Uma vez detectada uma anomalia, o modelo pode classificar a falha em categorias específicas. \n",
    "- Por exemplo, distinguir entre uma falha de isolamento e um curto-circuito. \n",
    "- Isso ajuda os operadores a tomar medidas corretivas adequadas. c. Tempo de Resposta Rápido: \n",
    "- O Random Forest é eficiente em termos de tempo de processamento. - Pode fornecer resultados quase em tempo real, permitindo uma resposta rápida a falhas.\n",
    "\n",
    "Treinamento e Avaliação:\n",
    "\n",
    "- O modelo é treinado com dados históricos rotulados (exemplos de falhas conhecidas).\n",
    "- Os recursos (medidas elétricas) são usados como entradas, e as classes de falhas são os rótulos.\n",
    "- A precisão do modelo é avaliada usando dados de teste não vistos."
   ]
  },
  {
   "cell_type": "markdown",
   "id": "64ee7637-f853-440f-9c79-0db9d1c8c5ce",
   "metadata": {},
   "source": [
    "## Sobre o Projeto"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "d03229cf-25cd-4135-a076-e9342653ba42",
   "metadata": {},
   "source": [
    "Este projeto emprega o algoritmo **RandomForestClassifier**, uma poderosa ferramenta de aprendizado de máquina do pacote scikit-learn, com o objetivo de classificar diversos tipos de falhas elétricas. Uma característica distintiva deste projeto é a introdução de ruído gaussiano aos dados. O ruído gaussiano, também conhecido como ruído branco, é uma forma de variação aleatória que é adicionada aos dados para simular incertezas ou variações naturais. Neste contexto, o ruído gaussiano é usado para testar a robustez do modelo em face de pequenas perturbações nos dados. Ao adicionar esse ruído, podemos observar o impacto direto na acurácia do modelo, proporcionando uma visão valiosa sobre a capacidade do modelo de generalizar a partir de dados ruidosos. Isso é particularmente relevante em ambientes do mundo real, onde os dados podem ser sujeitos a várias fontes de ruído e incerteza. Portanto, a análise da relação entre o ruído gaussiano e a acurácia do modelo é um aspecto crucial deste projeto."
   ]
  },
  {
   "cell_type": "markdown",
   "id": "f5d955de-4ba1-45f6-9991-ef45f64c196b",
   "metadata": {},
   "source": [
    "### Dados"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "a57d0d33-337d-4862-9d34-ef533aca3e4b",
   "metadata": {},
   "source": [
    "Os dados usados neste projeto estão contidos em um arquivo Excel chamado [dataset_electrical_fault.xlsx](dataset_electrical_fault.xlsx). Cada linha no conjunto de dados representa um conjunto de medições elétricas, e a coluna 'fault_type' representa o tipo de falha elétrica."
   ]
  },
  {
   "cell_type": "markdown",
   "id": "15420826-271a-4037-aac8-88af207f10db",
   "metadata": {},
   "source": [
    "### Dicionário das Colunas"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "3900e1f8-41e7-4157-9493-f00142e87066",
   "metadata": {},
   "source": [
    "|Nome da Coluna|Papel|Tipo|Descrição|\n",
    "|--------------|-----|----|---------|\n",
    "|fault_location|Recurso|Discreto|Valores representando a posição onde ocorreu a falta na linha de transmissão|\n",
    "|rms_phase_A_current|Recurso|Discreto|Raiz quadrada média, fase A para o sinal de corrente|\n",
    "|rms_phase_B_current|Recurso|Discreto|Raiz quadrada média, fase B para o sinal de corrente|\n",
    "|rms_phase_C_current|Recurso|Discreto|Raiz quadrada média, fase C para o sinal de corrente|\n",
    "|rms_phase_A_voltage|Recurso|Discreto|Raiz quadrada média, fase A para o sinal de tensão|\n",
    "|rms_phase_B_voltage|Recurso|Discreto|Raiz quadrada média, fase B para o sinal de tensão|\n",
    "|rms_phase_C_current|Recurso|Discreto|Raiz quadrada média, fase C para o sinal de tensão|\n",
    "|energy_phase_A_current|Recurso|Discreto|Energia absoluta, fase A para o sinal de corrente|\n",
    "|energy_phase_B_current|Recurso|Discreto|Energia absoluta, fase B para o sinal de corrente|\n",
    "|energy_phase_C_current|Recurso|Discreto|Energia absoluta, fase C para o sinal de corrente|\n",
    "|rms_phase_C_current|Recurso|Discreto|Raiz quadrada média, fase C para o sinal de tensão|\n",
    "|fault_type|Alvo|Categórico|AG, BG, CG, AB, AC, BC, ABG, ACG, BCG, ABC|"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "c3b12fd9-c3ec-4262-9632-f413d6b2b2f5",
   "metadata": {},
   "source": [
    "### Tipos de Falhas"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "86ef9e17-a67d-46b9-b690-e491b0c00590",
   "metadata": {},
   "source": [
    "|Tipo de Falha|Descrição|\n",
    "|--------------|--------|\n",
    "|AG|fase A está em curto com a terra|\n",
    "|BG|fase B está em curto com a terra|\n",
    "|CG|fase C está em curto com a terra|\n",
    "|AB|fase A está em curto com a fase B|\n",
    "|AC|fase A está em curto com a fase C|\n",
    "|BC|fase B está em curto com a fase C|\n",
    "|BC|fase B está em curto com a fase C|\n",
    "|ABG|fase A e B estão em curto com a terra|\n",
    "|ACG|fase A e C estão em curto com a terra|\n",
    "|BCG|fase B e C estão em curto com a terra|\n",
    "|ABC|fase A, B e C estão em curto entre si (falha trifásica)|"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "50d015f6-77cb-4a4c-86b9-ab01e354bb48",
   "metadata": {},
   "source": [
    "### Pré-processamento"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "9ade878b-08fb-4107-97a9-efe608a2b8cf",
   "metadata": {},
   "source": [
    "O pré-processamento dos dados envolve a substituição de vírgulas por pontos e a conversão dos valores para números. Além disso, os dados são divididos em recursos (X) e rótulos (y). Os recursos são todas as colunas, exceto a coluna 'fault_type', que é o rótulo."
   ]
  },
  {
   "cell_type": "markdown",
   "id": "dc997592-38bf-4624-bac2-0acbbff6cc95",
   "metadata": {},
   "source": [
    "## Modelo "
   ]
  },
  {
   "cell_type": "markdown",
   "id": "bee5f0e4-8dd9-46de-aba5-7bab7a195746",
   "metadata": {},
   "source": [
    "O modelo pode ser vizualizado em [classificacao_falhas_eletricas.ipynb](electrical_fault_classification.ipynb)"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "fb1f8ab2-edfc-4b0f-8073-d63b86b7a52d",
   "metadata": {},
   "source": [
    "## Conclusão"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "a6293018-f1bc-45dc-bd7c-1a5c9cdb408b",
   "metadata": {},
   "source": [
    "Adicionar ruído branco aos meus dados e observar uma diminuição na acurácia do modelo à medida que o desvio padrão do ruído aumenta pode levar a algumas possíveis conclusões:\n",
    "\n",
    "- Robustez do Modelo: Meu modelo pode não ser robusto a variações nos dados. Em um cenário do mundo real, os dados podem ter algum nível de ruído. Se o desempenho do modelo diminui significativamente com a adição de ruído, isso pode indicar que o modelo pode não funcionar bem quando exposto a dados do mundo real que contêm ruído.\n",
    "\n",
    "- Sobreajuste: Se o modelo tem um desempenho muito bom nos dados sem ruído, mas o desempenho diminui com a adição de ruído, isso pode ser um sinal de sobreajuste. O modelo pode estar aprendendo a representar os dados de treinamento muito bem, mas falha ao generalizar para novos dados ou dados com pequenas variações (ruído).\n",
    "\n",
    "- Importância da Limpeza de Dados: A adição de ruído e a observação do impacto no desempenho do modelo destacam a importância da limpeza de dados e do pré-processamento. Em muitos casos, a limpeza de dados e a remoção de ruído podem ser tão importantes quanto a escolha do algoritmo de aprendizado de máquina.\n",
    "\n",
    "- Necessidade de Regularização: Se o seu modelo é sensível ao ruído, pode ser útil explorar técnicas de regularização. A regularização adiciona uma penalidade à complexidade do modelo na função de perda, o que pode ajudar a prevenir o sobreajuste e tornar o modelo mais robusto ao ruído."
   ]
  },
  {
   "cell_type": "markdown",
   "id": "2e761f08-83b6-4c9e-a31b-372abd006411",
   "metadata": {},
   "source": [
    "## Tecnologias Utilizadas\n",
    "\n",
    "- Python 3.10.9\n",
    "- Jupyter Notebook\n",
    "- Numpy\n",
    "- Pandas\n",
    "- Scikit-Learn\n",
    "- Matplotlib"
   ]
  },
  {
   "cell_type": "markdown",
   "id": "e4c79051-0b68-4113-b191-0e62e65161c4",
   "metadata": {},
   "source": [
    "## Contribuições \n",
    "\n",
    "Contribuições são bem vindas. Sinta-se a vontade para sugerir melhorias e possíveis correções no código."
   ]
  },
  {
   "cell_type": "markdown",
   "id": "7d0ae73c-1b8c-4d22-8859-aaa330d77d60",
   "metadata": {},
   "source": [
    "## Autor \n",
    "\n",
    "Jadeson Bruno Albuquerque da Silva\n",
    "\n",
    "[![Linkedin](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/jadeson-bruno-228450101/)"
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3 (ipykernel)",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.10.9"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 5
}
