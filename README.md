<b> INSTRUÇÕES DE USO <b/>

Inicialmente, com os códigos do SERVIDOR e do CLIENTES em execução, o CLIENTE deve entrar com o comando

/ENTRAR 'IP DO SERVIDOR' 'PORTA A SE CONECTAR' 'NICK DO CLIENTE'

Feito isso, o Cliente irá - caso coloque IP:PORTA corretamente, e o NICK esteja livre - se conectar ao Servidor. Uma mensagem
é exibida se a conexão for bem sucedida.

 - Se o IP ou a PORTA estiverem incorretos, o Cliente não conseguirá se conectar.
 - Se o NICK não estiver livre, o Cliente deverá colocar outro NICK.

Conectado ao Servidor, o Cliente terá 4 opções:

1. Enviar mensagens de forma livre e sem limite de mensagens sequidas, mensagens essas que serão enviadas ao Servidor,
que, por sua vez, as enviará a TODOS os Clientes conectados ao Servidor no momento;

2. Usar o comando '/USUARIOS' para listar os clientes conectados naquele momento ao servidor;

3. Usar o comando '/NICK <NOVO NICK>' para trocar de NICK no Servidor, desde que o novo NICK escolhido não já esteja em uso;

4. Usar o comando '/SAIR' para sair do Servidor, liberando o seu socket, a porta e o NICK.

<b> Lembre-se de executar primeiro o código do Servidor! </b>
