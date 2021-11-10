# Projeto Semantix - Big Data Engineer
Atividades e desafio Semantix - Big Data Engineer



O Projeto e Desafio Semantix consiste em aplicar os conhecimentos adquiridos usando uma base de dados do Governo Federal (https://covid.saude.gov.br/) que remonta as informações sobre a COVID-19.

**Fase 1 - Nível Básico**

Os dados deverão inicialmente de maneira estática, cortada em um determinada data, serem carregadas em um Cluster e que permitirá a análise.

Neste primeiro momento o ideal é conhecer a base de dados que será analisada e o tipo de informação que se pretende investigar.

------

1º Atividade - Subir os dados brutos para o HDFS.

- Aqui foi criado um diretório dentro do /user o /covid

  ​	`hdfs dfs -mkdir /user/covid`

- Após copiado os arquivos com a extensão .csv para o HDFS e diretório covid

  ​	`[hdfs dfs -put /input/HIST*.csv /user/covid]`

- Verificado se ocorreu sucesso e se os arquivos estão no local correto

  ​	`hdfs dfs -ls /user/covid`

  ![img1](https://github.com/ecardozo/ProjetoSemantix/blob/main/CapturaDeTela/EnvioDeDadosBrutosHDFS.png)

------

2º Atividade - Otimizar os dados do HDFS particionando por munícipio em uma tabela Hive.

- Criação do Banco de Dados no Hive, acessando o hive pelo hive-server e após o beeline abrir a conexão.

![img2](https://github.com/ecardozo/ProjetoSemantix/blob/main/CapturaDeTela/CriacaoBDCovid.png)

- Criação da tabela no Hive

`create external table covidmunicipio(
regiao string,
estado string,
coduf int,
codmun int,
codRegiaoSaude int,
nomeRegiaoSaude string,
data string,
semanaEpi int,
populacaoTCU2019 bigint,
casosAcumulado bigint,
casosNovos bigint,
obitosAcumulado bigint,
obitosNovos int,
Recuperadosnovos bigint,
emAcompanhamentoNovos bigint,  `

`interiormetropolitana int
)
comment "Mapeamento da Base de Dados COVID-19 particionada por municipio"
partitioned by (municipio string)
row format delimited
fields terminated by '\t'
lines terminated by '\n'
stored as textfile
location 'hdfs://namenode:8020/user/covid'
tblproperties("skip.header.line.count"="1");`

Com o objetivo de realizar a otimização das tabelas, será necessário avaliar os atuais dados coletados neste arquivo .csv que será usado para diversas análises.

A tabela bruta (raw) neste caso possui os campos 

regiao;

estado;

municipio;

coduf;

codmun;

codRegiaoSaude;

nomeRegiaoSaude;

data;

semanaEpi;

populacaoTCU2019;

casosAcumulado;

casosNovos;

obitosAcumulado;

obitosNovos;

Recuperadosnovos;

emAcompanhamentoNovos;

interior/metropolitana

Por se tratar de dados coletados de todo Brasil, podemos entender que há valores que se caso não sejam bem definidos pode dar uma falsa e errada interpretação.



- Dados acumulados do munícipio relacionados a última semana de envio de informações:

`dadosGerais = spark.read.csv("/user/covid", header="true", sep=";")`

​			Carregados os dado pelo Jupyter Notebook

​			Foi observado os seguintes dados

`casosAcumulados = dadosCovidBrasil.groupBy("municipio").agg(last("casosAcumulado").alias("Casos Acumulado"), last("casosNovos").alias("Novos Casos") )`

`casosAcumulados.show(15)`

![img4](https://github.com/ecardozo/ProjetoSemantix/blob/main/CapturaDeTela/TelaDadosAcumulado.png)

`#salvando os dados`
`casosAcumulados.write.orc("/user/covid/relatorio", compression="zlib")`



