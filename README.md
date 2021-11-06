# Projeto Semantix - Big Data Engineer
Atividades e desafio Semantix - Big Data Engineer



O Projeto e Desafio Semantix consiste em aplicar os conhecimentos adquiridos usando uma base de dados do Governo Federal (https://covid.saude.gov.br/) que remonta as informações sobre a COVID-19.

**Fase 1 - Nível Básico**

Os dados deverão inicialmente de maneira estática, cortada em um determinada data, serem carregadas em um Cluster e que permitirá a análise.

Neste primeiro momento o ideal é conhecer a base de dados que será analisada e o tipo de informação que se pretende investigar.

------

1º Atividade - Subir os dados brutos para o HDFS.

- Aqui foi criado um diretório dentro do /user o /covid

  ​	hdfs dfs -mkdir /user/covid

- Após copiado os arquivos com a extensão .csv para o HDFS e diretório covid

  ​	hdfs dfs -put /input/HIST*.csv /user/covid

- Verificado se ocorreu sucesso e se os arquivos estão no local correto

  ​	hdfs dfs -ls /user/covid

  ![img1](https://github.com/ecardozo/ProjetoSemantix/blob/main/CapturaDeTela/EnvioDeDadosBrutosHDFS.png)

------

2º Atividade - Otimizar os dados do HDFS particionando por munícipio em uma tabela Hive.

Criação do Banco de Dados no Hive, acessando o hive pelo hive-server e após o beeline abrir a conexão.

