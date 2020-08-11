#### Azure Data Warehouse Synapses Analytics
- __MPP__: Massive Parallel Process
- Run Complex Queries
- Arquitetura que distribui o processamento em vários nodes;
  - __Control Node:__ manage de parallel processing engine and optimizes the query by passing it through or distributing to multiples compute nodes. Após o processamento, o dado volta para o control node e é entregue ao usuário;
    - Os nodes trabalham em paralelo para acessar a storage e rodar a query
    - __Data Movement Service__: (between the computer nodes) this moves the datas across the nodes as needed;
