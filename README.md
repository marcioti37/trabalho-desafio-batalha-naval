#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define TAM 5
#define AGUA '~'
#define NAVIO 'N'
#define TIRO_ACERTO 'X'
#define TIRO_ERRO 'O'

// Protótipos
void inicializarTabuleiro(char tabuleiro[TAM][TAM]);
void posicionarNavios(char tabuleiro[TAM][TAM], int qtd);
void exibirTabuleiro(char tabuleiro[TAM][TAM], int ocultar);
int atirar(char tabuleiro[TAM][TAM], int x, int y);
int venceu(char tabuleiro[TAM][TAM]);

int main() {
    char jogador[TAM][TAM];
    char computador[TAM][TAM];
    int linha, coluna;
    int navios = 3;

    srand(time(NULL));

    inicializarTabuleiro(jogador);
    inicializarTabuleiro(computador);

    printf("=== BATALHA NAVAL ===\n");

    printf("\nPosicione seus navios:\n");
    posicionarNavios(jogador, navios);
    posicionarNavios(computador, navios); // Computador posiciona aleatoriamente

    while (1) {
        printf("\nSeu tabuleiro:\n");
        exibirTabuleiro(jogador, 0);

        printf("\nTabuleiro do computador:\n");
        exibirTabuleiro(computador, 1);

        printf("\nSua vez de atirar! Informe linha e coluna (0 a %d): ", TAM - 1);
        scanf("%d %d", &linha, &coluna);

        if (atirar(computador, linha, coluna))
            printf("Você acertou!\n");
        else
            printf("Você errou!\n");

        if (venceu(computador)) {
            printf("Parabéns! Você venceu!\n");
            break;
        }

        // Turno do computador (aleatório)
        linha = rand() % TAM;
        coluna = rand() % TAM;
        printf("Computador atira em %d %d...\n", linha, coluna);

        if (atirar(jogador, linha, coluna))
            printf("Computador acertou!\n");
        else
            printf("Computador errou!\n");

        if (venceu(jogador)) {
            printf("Você perdeu! O computador venceu.\n");
            break;
        }
    }

    return 0;
}

void inicializarTabuleiro(char tabuleiro[TAM][TAM]) {
    for (int i = 0; i < TAM; i++)
        for (int j = 0; j < TAM; j++)
            tabuleiro[i][j] = AGUA;
}

void posicionarNavios(char tabuleiro[TAM][TAM], int qtd) {
    int i = 0;
    while (i < qtd) {
        int x = rand() % TAM;
        int y = rand() % TAM;
        if (tabuleiro[x][y] == AGUA) {
            tabuleiro[x][y] = NAVIO;
            i++;
        }
    }
}

void exibirTabuleiro(char tabuleiro[TAM][TAM], int ocultar) {
    printf("  ");
    for (int i = 0; i < TAM; i++)
        printf("%d ", i);
    printf("\n");

    for (int i = 0; i < TAM; i++) {
        printf("%d ", i);
        for (int j = 0; j < TAM; j++) {
            char c = tabuleiro[i][j];
            if (ocultar && c == NAVIO)
                printf("%c ", AGUA);
            else
                printf("%c ", c);
        }
        printf("\n");
    }
}

int atirar(char tabuleiro[TAM][TAM], int x, int y) {
    if (x < 0 || x >= TAM || y < 0 || y >= TAM)
        return 0;

    if (tabuleiro[x][y] == NAVIO) {
        tabuleiro[x][y] = TIRO_ACERTO;
        return 1;
    } else if (tabuleiro[x][y] == AGUA) {
        tabuleiro[x][y] = TIRO_ERRO;
    }
    return 0;
}

int venceu(char tabuleiro[TAM][TAM]) {
    for (int i = 0; i < TAM; i++)
        for (int j = 0; j < TAM; j++)
            if (tabuleiro[i][j] == NAVIO)
                return 0;
    return 1;
}
