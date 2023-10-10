# Procedimento contador
# Recebe:
#   t2 (x6): endereço do vetor
#   t3 (x7): tamanho do vetor
# Retorna:
#   x10: quantidade de números pares
#   x11: quantidade de números ímpares

.data
vetor: .word 0 0 0 0

##### START MODIFIQUE AQUI START #####
    
# Inicialize um registrador para o índice do vetor
li x13, 0  # Inicialize o índice com 0
li x16, 4  # Valor imediato (4 bytes por palavra)
  
##### END MODIFIQUE AQUI END #####

.text
jal x1, contador
addi x14, x0, 2
beq x14, x10, FIM
beq x14, x11, FIM

##### START MODIFIQUE AQUI START #####

contador:
# Endereço do vetor 
	la t2, vetor   
# Tamanho do vetor 
	li t3, 4 

# Inicialize contadores
    li x10, 0       # Inicialize contador de pares (x10) com 0
    li x11, 0       # Inicialize contador de ímpares (x11) com 0
loop:
	bge x13, t3, fim_loop  # Saia do loop se o índice for maior ou igual ao tamanho do vetor (t3)
    # Calcule o deslocamento (offset) para acessar o elemento atual do vetor
    mul x18, x13, x16  # Supondo que o vetor contenha palavras de 4 bytes cada (32 bits)

    # Calcule o endereço do elemento atual do vetor somando o endereço base com o deslocamento
    add x15, t2, x18

    # Carregue o valor do vetor para x17 
    lw x17, 0(x15)

    # Verifique se o número é par ou ímpar
    andi x12, x17, 1  # Verifique o bit menos significativo (LSB) para determinar se é ímpar (1) ou par (0)

    # Se x12 for igual a 0, o número é par; caso contrário, é ímpar
    beqz x12, numero_par  # Salte para a etiqueta numero_par se o número for par

    # Se chegou aqui, o número é ímpar
    addi x11, x11, 1  # Incrementa o contador de ímpares
    j proximo_elemento  # Salta para a etiqueta proximo_elemento

numero_par:
    # Se chegou aqui, o número é par
    addi x10, x10, 1  # Incrementa o contador de pares

proximo_elemento:
    # Incremento o índice para acessar o próximo elemento do vetor
    addi x13, x13, 1
    j loop  # Continue o loop

fim_loop:
	nop
    # Agora, x10 e x11 contêm a contagem de números pares e ímpares, respectivamente
##### END MODIFIQUE AQUI END #####

FIM:
addi x0, x0, 1