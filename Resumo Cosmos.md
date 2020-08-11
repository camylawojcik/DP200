### __AZURE COSMOS DB__
- É um container de um ou mais Banco de dados;
	- Um banco é um container com uma ou mais collections;
	- Uma collection contém documentos 
	- Um documento é um conjunto não estruturado de pares chave-valor, lidos e gravados em formato JSON;
- Globally distributed, multi-moldel database
- ##### You can deploy using:
	- SQL API (default, doc. db, non fixed API)
	- Mongo DB API (data and document)
	- Cassandra API
	- Gremlin API (define relationships or graph based view of data)
	- Table API (support fpr application, high availability and global distribution)
- ##### Quando usar:
	- Quando precisar de um banco de dados NoSQL; 
	- Compativel com modelo de API;
	- Escala Mundial
		- Baixa Latência
	- 99,999% uptime
- Failover regional
- Multimaster replication: obter tempo de resposta < 1s em qualquer lugar do mundo
- Leituras e gravações < 10ms
- #### __Níveis de Consistência:__ 
    73% dos cosmos tenant usam session consistency e 20% preferem bounded staleness
	- Strong:
	- Garante a leitura a versão mais recente de um item. Escrita vísivel apenas após o commit. Dado recplicado através de todos os nodes, quase simultaneamente; Custo da leitura é maior que no session, e igual ao staleness;
	- Bounded Staleness: prefixos consistentes. Reads lag behind writes by at most k prefixes or t interval;
	- Session: prefixos consistentes. Monotonic reads, monotonic writes, read-your-writes, write-follows-reads. Scoped to a client session. Predictable consistency for a session
	- Consistent prefix: updates returned are some prefix of all the updates, with no gaps.
		- Garante que o leitor não veja os dados fora de ordem;
	- Eventual: replicado com menos frequência que na strong consistency. Recomendado quando a aplicação não requer ordem na leitura;
- Ingestão: ADF
- Queries: Triggers, stored procedures and user-defined functions (UFDs) or use JavaScript query API;
- Security:
	- Encryption;
	- IP Firewall configurations and access from virtual networks;
	- Autenticação baseada em TOKEN;
	- AD provides role-based security;
- #### __Indexing:__
	- é automático mais pode ser sobreescrito;
	- Cada container tem uma politica que diz como os itens devem ser indexados;
	- Default: Consistent. (Toda propriedade de todos os itens é indexada e o indice tem atualização
	  sincrona, imediata, com as operações insert, update e delete)
	- Lazy: A atualização do indice tem baixa prioridade. A escrita é mais barata. Ingest now and query later;
	- None: Sem indice. As consultas são mais caras. Se estiver lendo dados diretamente, em vez de consultar a coleção, é possível evitar a sobrecarga da indexação;
	- Many as 20 failed domains
- Very elastic, horizontal scalability;
- #### __Throughput:__ 
    Is the amount of material or items passing through a system or process;
	- In cosmos, is the database operations. The cost is expressed by request units or hour use;
	- Distribuido igualmente entre todas as partições fisicas;
	- RU/s Request Units per second is the currency for throughput;
	- You pay the throughput you provision and the storage you consume;
	- Você pode compartilhar entre containers ou ter uma configuração dedicada para containers específicos (+ usado)
		- Se você não definir um throughput específico, não existe garantia para os containers caso tudo seja consumido
		a azure limitará suas solicitações e as operações terão que esperar para tentar novamente. Provavelmente,
		causando latência mais alta;
	- __Minimo de RU/s:__ 400 - max: 250.000, se precisar de mais precisar abrir ticket no portal
	- Data consistency: níveis de consistência robusta consomem aproximadamente 2x mais RUs 
	- __Query Patterns:__ garantia de que uma mesma query, com os mesmos dados, sempre custam o mesmo número de RU/s;
- __Partitioning:__
	- Logical and Physical;
	- Uma partição física é implementada por um grupo de réplicas, chamado de Replica Set.
	- Replica set: replicas dentro da mesma região;
	- Partition Set: replicas across regions;
	- 1 physical partition tem 1 ou mais logical partitions
	- O numero de partições fisicas depende: da quantidade de Throughput e da quantidade de dados armazenados
	- Cada logical armazena até 20gb
- __Partitions Key:__
	- Scale out or Horizontal Scale;
	- Definida quando o container é criado. Não é possível alterar;
	- Valor usado para organizar os dados em partições lógicas;
	- Usado para evitar um Hot Partition;
	- Quando uma nova partition é necessária, o cosmos cria automaticamente (Sem impacto);
	- O espaço de storage associado a cada partition não pode exceder 20 GB (é o tamanho da física)
		- se sua partição for exceder esse tamanho, considere composite keys;
- O código HTTP 429 indica que há um número excessivo de solicitações. 
	- Ao adicionar uma região nova, o azure garante disponibilização em 30 minutos (até 100TB) 
- Multi-Mater Support
- __Conflict Resolution:__ 
	- Last-Writer-Wins (LWW)
	- Custom-User-Defined functions
	- Custom-Async: cosmos excludes all conflicts from being commited 
		and registers them in the read-onl conflicts feed for deferred 
		resolution by the user's application;
- Multi-model: document, key-value, wide-column or graph-based data;
- Failover automático quando vocÊ tem seus dados em várias regiões;
  Aplicável somente a uma cnta de gravação de região única.
- Falha na região de leitura: automaticamente desconectada. Redirecionamento para outra na lista de preferenciais. 
- Falha na Região de Escrita: marcada como offline. Região alternativa promovida a região de gravação.
- __Monitoring:__
	- Performance metrics on the metrics page;
	- Performance metrics using Azure Monitoring;
	- Performance Metrics on the Account Page - Total requests for the current day; storage used;
- __Pricing:__ 
	- Taxa de Transferência (padrão ou dimensionamento automático);
	- Armazenamento consumido por hora (Duas cópias de backup são fornecidas gratuitamente, com cópias adicionais cobradas como 
		o total de GBs de dados armazenados.); 
