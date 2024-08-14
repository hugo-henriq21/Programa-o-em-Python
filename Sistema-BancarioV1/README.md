# Sistema Bancário em Python

Este é um simples sistema bancário em Python que permite ao usuário realizar operações básicas como saque, depósito e visualização de extrato. O sistema é projetado para funcionar com um único usuário e implementa restrições para saques diários.

## Funcionalidades

- **Saque**: O usuário pode realizar até 3 saques diários, com um limite máximo de R$ 500,00 por saque. O sistema verifica se há saldo suficiente antes de permitir o saque. Se o saldo for insuficiente, o usuário é informado e o saque não é realizado.
- **Depósito**: O usuário pode depositar qualquer valor positivo na conta. O saldo é atualizado imediatamente após o depósito.
- **Extrato**: Exibe todos os saques e depósitos realizados na conta, junto com o saldo atual. Se não houver movimentações, o sistema informa que nenhuma movimentação foi realizada.
- **Sair**: O usuário pode sair do sistema a qualquer momento escolhendo a opção de saída.

```python
sair = False
saldo = 0
saque_diario = 0
limite_maximo = 500.00
saques = []

while not sair:
    print("""
        Bem Vindo!
      
        - Escolha uma das opções abaixo para continuar
        1. Digite "s" para sacar
        2. Digite "d" para depositar
        3. Digite "v" para visualizar o extrato
        4. Digite "q" para sair
    """)

    opcao = input("Escolha uma opção: ")

    if opcao == 's':
        if saque_diario < 3:
            saque = float(input("Informe o valor que deseja sacar: "))

            if saque > 0 and saque <= saldo and saque <= limite_maximo:
                saldo -= saque
                saques.append(saque)
                saque_diario += 1
                print(f"Valor sacado: R$ {saque:.2f}")
            elif saque > saldo:
                print("Valor informado é maior que o saldo disponível.")
            elif saque > limite_maximo:
                print("Valor informado é maior que o limite máximo para saque diário (R$ 500,00).")
            else:
                print("Valor informado é inválido.")
        else:
            print("Você já realizou 3 saques hoje. Tente novamente amanhã.")

    elif opcao == 'd':
        deposito = float(input("Informe o valor para depósito: "))

        if deposito > 0:
            saldo += deposito
            print(f"Depósito realizado com sucesso: R$ {deposito:.2f}")
        else:
            print("Valor inválido para depósito.")

    elif opcao == 'v':
        print("\nExtrato de Movimentações")
        print("----------------------------")
        if saques or saldo > 0:
            for i, saque in enumerate(saques):
                print(f"Saque {i+1}: R$ {saque:.2f}")
            print(f"Saldo atual: R$ {saldo:.2f}")
        else:
            print("Não foram realizadas movimentações.")
        print("----------------------------\n")

    elif opcao == 'q':
        sair = True
        print("Obrigado por utilizar o sistema bancário.")

    else:
        print("Opção inválida. Tente novamente.")







```
