# ENTRADAS #######
# a0: endereço do vetor
# a1: tamanho do vetor
# a2: limiar desejado

# SAÍDA #######
# t0 : quantidade de elementos acima do limiar

.data
vetor: .word 200, 190, 340, 100 # exemplo

##### START MODIFIQUE AQUI START #####
# Definindo limiar
li a2, 200

# Tamanho do vetor
li a1, 4
##### END MODIFIQUE AQUI END #####

.text
jal ra, main

##### START INSTRUÇÃO DE CORREÇÃO/VERIFICAÇÃO #####
addi a4, x0, 3  # configurando a quantidade de salários acima do limiar de 200.
beq a4, t0, FIM  # Verifica a quantidade de salários acima do limiar.
##### END INSTRUÇÃO DE CORREÇÃO/VERIFICAÇÃO #####

main:
	
    ##### START MODIFIQUE AQUI START #####
    # Endereço do vetor
    la a0, vetor
    # Salvar o endereço de retorno (ra) na pilha
    addi sp, sp, -4
    sw ra, 0(sp)

    # Chama a subrotina aplica_reajuste
    jal ra, aplica_reajuste

    # Recupera o endereço de retorno da pilha
    lw ra, 0(sp)
    addi sp, sp, 4

    # Restaurando os argumentos para a instrução de correção/verificação
    #mv a0, a1
    #lw a1, threshold

    ##### END MODIFIQUE AQUI END #####

aplica_reajuste:
    ##### START MODIFIQUE AQUI START #####
    # Argumentos: a0 = endereço do vetor, a1 = tamanho do vetor

    # Inicialize o contador de salários acima do limiar
    li t0, 0

    # Percorrendo  vetor e aplicando o reajuste de 50%
    li t3, 0  # Índice inicial do vetor
loop:
    bge t3, a1, fim_loop  # Saiando do loop se atingir o tamanho do vetor

    # Carregando o valor atual do vetor
    lw t4, 0(a0)
    
    # para multiplicar por 1,5 ( Multiplica por 3 e divide por 2)
    li t5, 3  # Carregandoo valor 3 em t5
    li t6, 2  # Carregando o valor 2 em t5
    
    mul t4, t4, t5  # Multiplica t4 por 3, armazenando o resultado em t4
    divu t4, t4, t6  # Divida t4 por 2 (divisão sem sinal), armazenando o resultado em t4

    # Verificando se o salário está acima do limiar
    bge t4, a2, salario_acima_limiar

    # Se não estiver acima do limiar, continue para o próximo elemento
    j proximo_elemento

salario_acima_limiar:
    # Incremente o contador de salários acima do limiar
    addi t0, t0, 1

proximo_elemento:
    # Avance para o próximo elemento do vetor
    addi t3, t3, 1
    addi a0, a0, 4  # Avance para o próximo endereço do vetor
    j loop

fim_loop:
    ##### END MODIFIQUE AQUI END #####

FIM: addi x0, x0, 1
