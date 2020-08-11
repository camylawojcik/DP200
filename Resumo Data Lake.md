### __AZURE DATA LAKE STORAGE__
Projetado para volume de dados em escala de exabytes, manipulando centenas de GB de taxa de transferência
- Tempo real ou lote;
- Gen2: armazena no blob, com acesso pela api do blob ou do Data Lake;
	- query by Azure Blob Storage API ou the Azure Data Lake System API
- Gen1: também pode atuar como camada de armazenamento para várias plataformas
    - query by using U-SQL
- __Onde usar:__ grandes quantidades de dados; reduz tempo de processamento, tornando a pesquisa mais rápida;
- Escalabilidade ilimitada, compativel com Hadoop, 
- __Segurança:__ 
	- Compátivel com AD e com RBAC;
	- Auditoria real-time no Storage Analytics Service
	- Suporte ACLs (Access Control List);
	- conformidade com POSIX, Diver Azure Blob File System (ABFS) 
	- Suporte a CORS: sinalizador opcional que utiliza cabeçalhos HTTP;
	- Todos os dados são encriptados no rest; Não é possível desabilitar e não tem custo extra;
	- Possível habilitar criptografia em trânsito;
	- Azure Threat Protection (serviço do Blob);
- __Otimização e Performance:__
	- Organiza os dados em diretórios e subdiretórios, reduzindo recursos computacionais, tempo e custo;
	- Problemas normalmente ocorrem na entrada ou saída dos dados;
		- Possível solução: Express Route
	- Mantenha os arquivos com tamanhos entre 256MB e 100 GB
	- Organize file/folder structure para otimizar para "larger files"
	- Configure as ferramentas de ingestão para máxima paralelização
		- ADF: parallelCopies, Sqoop: fs.azure.block.size, -m (mapper)
		- Data Lake Storage Gen2 can scale to provide the necessary throughput for all analytics scenario. 
		  By default, a Data Lake Storage Gen2 account provides automatically enough throughput to meet the needs of 
		  a broad category of use cases. For the cases where customers run into the default limit, the Data Lake Storage Gen2 
		  account can be configured to provide more throughput by contacting Azure Support.
	- Otimizado para Big Data Analytics
- __Zone and Geo Redundant (LRS) (GRS)__
- Hierarchical Namespaces: 
	- organiza o blob em diretórios e armazena metadados sobre cada diretório e arquivos. Permitindo renomear e deletar em uma única operação atômica;
	- (desabiltar a opção de hierarchical namespace se não for executar operações analíticas)
- Stages:
	- Ingestion, Store, Prep and Train, Model and Serve;
- __Storage Account Keys:__
  - Chave inserida no cabeçalho http
  - Chave primária secundária que só deve ser usada em aplicativos confiáveis. Dão acesso a tudo na conta. 
			Devem ser protegidas e regeradas periodicamente;
    - Shared Access Signature (sas): String que contem um token de segurança que pode ser colocada na URL
    - Service Level Access Sginature: recursos especificos da storage account;
	    - Account Level Shared: tudo do anterior + habilidades
- __Cobrado__ por armazenamento (tiers), transação (https://azure.microsoft.com/pt-br/pricing/details/storage/data-lake/)
- __Monitoramento:__ Azure Monitor (tem mais algum?)
