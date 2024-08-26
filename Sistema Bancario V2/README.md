# Sistema Bancário em Python(V2)

Esta é a segunda versão do sistema bancário em Python. Na Versão 2 do sistema bancário em Python, diversas melhorias foram implementadas, tornando o sistema mais robusto e completo.
Agora, o sistema suporta múltiplos usuários e a criação de contas correntes, além das funcionalidades básicas da versão anterior.

## Funcionalidades

- **Saque**: O usuário pode realizar até 3 saques diários, com um limite máximo de R$ 500,00 por saque. O sistema verifica se há saldo suficiente antes de permitir o saque. Se o saldo for insuficiente, o usuário é informado e o saque não é realizado.
- **Depósito**: O usuário pode depositar qualquer valor positivo na conta. O saldo é atualizado imediatamente após o depósito.
- **Extrato**: Exibe todos os saques e depósitos realizados na conta, junto com o saldo atual. Se não houver movimentações, o sistema informa que nenhuma movimentação foi realizada.
- **Sair**: O usuário pode sair do sistema a qualquer momento escolhendo a opção de saída.
- **Criação de Usuários**: O sistema agora permite a criação de múltiplos usuários, armazenando informações como nome, data de nascimento, CPF e endereço. O CPF é validado para garantir que não existam duplicatas, evitando o cadastro de dois usuários com o mesmo CPF.
- **Criação de Contas Correntes**: Além dos usuários, o sistema permite a criação de contas correntes. Cada conta corrente é associada a um usuário específico e possui um número de agência fixo ("0001") e um número de conta sequencial, que começa em 1. Um usuário pode ter mais de uma conta, mas cada conta pertence a um único usuário.
- **Operações Bancárias para Múltiplas Contas**: As operações de saque, depósito e visualização de extrato agora são realizadas em contas correntes específicas, garantindo que cada usuário possa gerenciar suas contas de forma independente.
- **Validação de Dados**: Implementação de verificações adicionais para assegurar que os dados inseridos, como CPF e endereço, estejam no formato correto.
- **Manutenção de Contas e Usuários**: As contas e os usuários são mantidos em listas, facilitando o gerenciamento e a consulta de informações.

```python
sair = False
saldo = 0
limite_maximo = 500.00
saques = []
usuarios = []  # Lista para armazenar os usuários
contas = []  # Lista para armazenar as contas
numero_conta_sequencial = 1  # Inicia o número da conta em 1
AGENCIA_FIXA = "0001"  # Número fixo da agência

def criar_usuario(nome, data_nascimento, cpf, endereco):
    cpf = ''.join(filter(str.isdigit, cpf))
    
    for usuario in usuarios:
        if usuario['cpf'] == cpf:
            return "Erro: Usuário com este CPF já está cadastrado."

    usuario = {
        'nome': nome,
        'data_nascimento': data_nascimento,
        'cpf': cpf,
        'endereco': endereco
    }
    usuarios.append(usuario)
    return "Usuário criado com sucesso!"

def criar_conta_corrente(cpf):
    global numero_conta_sequencial
    
    cpf = ''.join(filter(str.isdigit, cpf))
    usuario_existe = any(usuario['cpf'] == cpf for usuario in usuarios)

    if not usuario_existe:
        return "Erro: Usuário com este CPF não encontrado."

    conta = {
        'agencia': AGENCIA_FIXA,
        'numero_conta': numero_conta_sequencial,
        'cpf': cpf
    }
    contas.append(conta)
    numero_conta_sequencial += 1
    return f"Conta criada com sucesso! Agência: {conta['agencia']}, Número da Conta: {conta['numero_conta']}"

def realizar_saque(*, valor_saque):
    global saldo
    saque_diario = 0

    if saque_diario < 3:
        if valor_saque > 0 and valor_saque <= saldo and valor_saque <= limite_maximo:
            saldo -= valor_saque
            saques.append(valor_saque)
            saque_diario += 1
            return f"Valor sacado: R$ {valor_saque:.2f}"
        elif valor_saque > saldo:
            return "Valor informado é maior que o saldo disponível."
        elif valor_saque > limite_maximo:
            return "Valor informado é maior que o limite máximo para saque diário (R$ 500,00)."
        else:
            return "Valor informado é inválido."
    else:
        return "Você já realizou 3 saques hoje. Tente novamente amanhã."

def realizar_deposito(valor_deposito):
    global saldo
    if valor_deposito > 0:
        saldo += valor_deposito
        return f"Depósito realizado com sucesso: R$ {valor_deposito:.2f}"
    else:
        return "Valor inválido para depósito."

def vizualizer_saque():
    global saldo
    print("\nExtrato de Movimentações")
    print("----------------------------")
    if saques or saldo > 0:
        for i, valor_saque in enumerate(saques):
            return f"Saque {i+1}: R$ {valor_saque:.2f}"
    else:
        return "Não foram realizadas movimentações."
    print("----------------------------\n")

def vizualizer_saldo():
    global saldo
    print("\nExtrato de Movimentações")
    print("----------------------------")
    if saques or saldo > 0:
        for i, valor_saque in enumerate(saques):
            return f"Saldo atual: R$ {saldo:.2f}"
    else:
        return "Não foram realizadas movimentações."
    print("----------------------------\n")

def exit():
    global sair 
    sair = True
    return "Obrigado por utilizar o sistema bancário."

while not sair:
    print("""
        Bem Vindo!
      
        - Escolha uma das opções abaixo para continuar
        1. Digite "s" para sacar
        2. Digite "d" para depositar
        3. Digite "v" para visualizar o extrato
        4. Digite "c" para criar um novo usuário
        5. Digite "cc" para criar uma conta corrente
        6. Digite "q" para sair
    """)

    opcao = input("Escolha uma opção: ")

    if opcao == 's':
        saque = float(input("Informe o valor que deseja sacar: "))
        print(realizar_saque(valor_saque=saque))

    elif opcao == 'd':
        deposito = float(input("Informe o valor para depósito: "))
        print(realizar_deposito(deposito))

    elif opcao == 'v':
        print(vizualizer_saque())
        print(vizualizer_saldo())

    elif opcao == 'c':
        nome = input("Informe o nome: ")
        data_nascimento = input("Informe a data de nascimento (DD/MM/AAAA): ")
        cpf = input("Informe o CPF: ")
        endereco = input("Informe o endereço (logradouro, nro - bairro - cidade/sigla estado): ")
        print(criar_usuario(nome, data_nascimento, cpf, endereco))

    elif opcao == 'cc':
        cpf = input("Informe o CPF do usuário para criar uma conta corrente: ")
        print(criar_conta_corrente(cpf))

    elif opcao == 'q':
        print(exit())

    else:
        print("Opção inválida. Tente novamente.")

```
