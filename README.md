```
import heapq
import sys

def merge_blocos_ordenados(blocos, arquivo_saida):
    heap = [(bloco[0], i, 0) for i, bloco in enumerate(blocos)]
    heapq.heapify(heap)

    with open(arquivo_saida, 'w') as f:
        while heap:
            valor, indice_bloco, indice_elemento = heapq.heappop(heap)
            f.write(str(valor) + '\n')

            if indice_elemento + 1 < len(blocos[indice_bloco]):
                prox_elemento = blocos[indice_bloco][indice_elemento + 1]
                heapq.heappush(heap, (prox_elemento, indice_bloco, indice_elemento + 1))

def carrega_blocos_ordenados(arquivo, tamanho_bloco):
    blocos = []
    with open(arquivo, 'r', encoding='utf-8') as f:
        bloco = []
        for linha in f:
            bloco.append(int(linha.strip()))
            if len(bloco) == tamanho_bloco:
                bloco.sort()
                blocos.append(bloco)
                bloco = []

        if bloco:
            bloco.sort()
            blocos.append(bloco)

    return blocos

if __name__ == "__main__":
    if len(sys.argv) != 4:
        print("Uso: python script.py <arquivo_entrada> <arquivo_saida> <tamanho_bloco>")
        sys.exit(1)

    caminho_do_arquivo = sys.argv[1]
    caminho_saida = sys.argv[2]
    tamanho_bloco = int(sys.argv[3])

    fragmentos = carrega_blocos_ordenados(caminho_do_arquivo, tamanho_bloco)
    merge_blocos_ordenados(fragmentos, caminho_saida)
```
