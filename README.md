# Visao-geral-sobre-o-Apache-Spark
Apache Spark características

Visão geral de alto nível
De um alto nível, o serviço do Azure Databricks inicia e gerencia os clusters do Apache Spark em sua assinatura do Azure. Os clusters do Apache Spark são grupos de computadores tratados como um único computador e lidam com a execução de comandos emitidos por notebooks. Os clusters permitem que o processamento de dados seja paralelizado em vários computadores para aprimorar a escala e o desempenho. Eles consistem em nós de driver e de trabalho do Spark. O nó do driver envia trabalho para os nós de trabalho e os instrui a efetuar pull de dados de uma fonte de dados especificada.

No Databricks, a interface do notebook é normalmente o programa de driver. Esse programa de driver contém o loop principal para o programa e cria conjuntos de dados distribuídos no cluster, depois aplica operações a esses conjuntos de dados. Os programas de driver acessam o Apache Spark por meio de um objeto SparkSession, independentemente do local de implantação.


![driver spark 2023-11-21 ](https://github.com/Nol75/Vis-o-geral-sobre-o-Apache-Spark/assets/25771016/e277969e-182e-4841-a6df-67db9f6fbcdd)


O Microsoft Azure gerencia o cluster e o dimensiona automaticamente conforme necessário com base no seu uso e na configuração definida ao configurar o cluster. O término automático também pode ser habilitado, o que permite que o Azure encerre o cluster após um número especificado de minutos de inatividade.

Trabalhos do Spark em detalhes
O trabalho enviado para o cluster é dividido em quantos trabalhos independentes forem necessários. É assim que o trabalho é distribuído entre os nós de cluster. Os trabalhos são subdivididos em tarefas. A entrada para um trabalho é particionada em uma ou mais partições. Essas partições são a unidade de trabalho para cada slot. Entre tarefas, as partições podem precisar ser organizadas novamente e compartilhadas pela rede.

O segredo do alto desempenho do Spark é o paralelismo. A escala vertical (com a adição de recursos a um computador) é limitada a uma quantidade finita de velocidades de CPU, RAM e threads, mas os clusters são escalonados horizontalmente, adicionando novos nós ao cluster conforme necessário.

O Spark paraleliza trabalhos em dois níveis:

O primeiro nível de paralelização é o executor, uma JVM (Máquina Virtual Java) em execução em um nó de trabalho, normalmente, uma instância por nó.
O segundo nível de paralelização é o slot, cujo número é determinado pelo número de núcleos e CPUs de cada nó.
Cada executor tem vários slots aos quais tarefas paralelizadas podem ser atribuídas.

![jvm spark 2023-11-23](https://github.com/Nol75/Vis-o-geral-sobre-o-Apache-Spark/assets/25771016/44f055fc-8427-47f4-a730-12b336c73e9c)


A JVM tem naturalmente vários threads, mas uma única JVM, como a que coordena o trabalho no driver, tem um limite superior finito. Ao dividir o trabalho em tarefas, o driver pode atribuir unidades de trabalho a *slots nos executores em nós de trabalho para execução paralela. Além disso, o driver determina como particionar os dados para que eles possam ser distribuídos para processamento paralelo. Portanto, o driver atribui uma partição de dados a cada tarefa para que cada tarefa saiba qual parte dos dados deve ser processada. Depois de iniciada, cada tarefa buscará a partição dos dados atribuídos a ela.
