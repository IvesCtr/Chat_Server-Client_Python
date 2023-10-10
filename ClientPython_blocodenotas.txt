"""
** INSTRUÇÕES DE USO **

Inicialmente, com os códigos do SERVIDOR e do CLIENTES em execução, o CLIENTE deve entrar com o comando

'/ENTRAR <IP DO SERVIDOR> <PORTA A SE CONECTAR> <NICK DO CLIENTE>'

Feito isso, o Cliente irá - caso coloque IP:PORTA corretamente, e o NICK esteja livre - se conectar ao Servidor. Uma mensagem
é exibida se a conexão for bem sucedida.

 - Se o IP ou a PORTA estiverem incorretos, o Cliente poderá usar o comando '/ENTRAR ...' novamente.
 - Se o NICK não estiver livre, o Cliente deverá colocar outro NICK.

Conectado ao Servidor, o Cliente terá 4 opções:

1. Enviar mensagens de forma livre e sem limite de mensagens sequidas, mensagens essas que serão enviadas ao Servidor,
que, por sua vez, as enviará a TODOS os Clientes conectados ao Servidor no momento;

2. Usar o comando '/USUARIOS' para listar os clientes conectados naquele momento ao servidor;

3. Usar o comando '/NICK <NOVO NICK>' para trocar de NICK no Servidor, desde que o novo NICK escolhido não já esteja em uso;

4. Usar o comando '/SAIR' para sair do Servidor, liberando o seu socket, a porta e o NICK.

"""


import socket
import threading

IP = ""  # IP do servidor
PORTA = 0  # Porta do servidor
client_name = ""

def entrar():
    global IP, PORTA, client_name
    while True:
        print("\n >> Comandos disponíveis:")
        print(" > Digite '/ENTRAR <IP> <PORTA> <APELIDO>' para se conectar a um servidor.")

        command = input()
        if command.startswith("/ENTRAR "):
            parts = command.split()
            if len(parts) == 4:
                IP = parts[1]
                PORTA = int(parts[2])
                client_name = parts[3]
                break
            else:
                print("\nUso: '/ENTRAR <IP> <PORTA> <APELIDO>'")

def receive_messages():
    while True:
        try:
            message = client_socket.recv(1024).decode()
            if not message:
                disconnect()
                break
            print(message)
        except ConnectionResetError:
            print("\nDesconectando do servidor...")
            disconnect()
            break
        except ConnectionAbortedError:
            print("\nDesconectando do servidor...")
            disconnect()
            break

def disconnect():
    client_socket.close()
    exit(0)

def main():
    global client_socket

    entrar()

    try:
        client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        if(IP == "127.0.0.1"):
            client_socket.connect((IP, PORTA))
            client_socket.send(client_name.encode())
        else:
            print("\nErro ao se conectar ao servidor. Verifique o IP ou a PORTA e tente novamente.\n")
            exit(0)

        lotado = client_socket.recv(1024).decode()
        if lotado == "1":
            print("\nServidor lotado. Tente novamente mais tarde.\n")
            exit(0)

        print("\n\nConectado ao servidor.\n")

        receive_thread = threading.Thread(target=receive_messages)
        receive_thread.start()

        while True:
            message = input()
            if message == "/SAIR":
                break
            else:
                client_socket.send(message.encode())

        disconnect()

    except Exception:
        print("\nErro ao se conectar ao servidor. Verifique o IP ou a PORTA e tente novamente.\n")
        disconnect()


if __name__ == "__main__":
    main()