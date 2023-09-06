#include <stdio.h>
#include <stdlib.h>

// Estrutura de um nó da fila
struct Node {
    int data;
    struct Node* next;
};

// Função para criar um novo nó
struct Node* createNode(int data) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    newNode->data = data;
    newNode->next = NULL;
    return newNode;
}

// Estrutura da fila
struct Queue {
    struct Node* front;
    struct Node* rear;
};

// Função para inicializar a fila
void initializeQueue(struct Queue* queue) {
    queue->front = queue->rear = NULL;
}

// Função para inserir um elemento na fila
void enqueue(struct Queue* queue, int data) {
    struct Node* newNode = createNode(data);

    if (queue->rear == NULL) {
        queue->front = queue->rear = newNode;
        return;
    }

    queue->rear->next = newNode;
    queue->rear = newNode;
}

// Função para remover um elemento da fila
void dequeue(struct Queue* queue) {
    if (queue->front == NULL) {
        printf("A fila está vazia.\n");
        return;
    }

    struct Node* temp = queue->front;
    queue->front = queue->front->next;

    free(temp);
}

// Função para exibir os elementos da fila
void displayQueue(struct Queue* queue) {
    struct Node* current = queue->front;

    if (current == NULL) {
        printf("A fila está vazia.\n");
        return;
    }

    printf("Elementos na fila: ");
    while (current != NULL) {
        printf("%d ", current->data);
        current = current->next;
    }
    printf("\n");
}

// Função recursiva para contar os elementos da fila
int countElements(struct Node* node) {
    if (node == NULL) {
        return 0;
    }
    return 1 + countElements(node->next);
}

// Função recursiva para buscar uma chave na fila e retornar seu valor
int searchKey(struct Node* node, int key) {
    if (node == NULL) {
        return -1; // Chave não encontrada
    }
    if (node->data == key) {
        return node->data; // Chave encontrada
    }
    return searchKey(node->next, key);
}

int main() {
    struct Queue queue;
    initializeQueue(&queue);

    int choice, data, key;

    while (1) {
        printf("\nOperações da fila:\n");
        printf("1. Inserir elemento\n");
        printf("2. Excluir elemento\n");
        printf("3. Exibir fila\n");
        printf("4. Contar elementos\n");
        printf("5. Buscar por chave\n");
        printf("6. Sair\n");

        printf("Escolha uma opção: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Digite o elemento a ser inserido na fila: ");
                scanf("%d", &data);
                enqueue(&queue, data);
                break;
            case 2:
                dequeue(&queue);
                break;
            case 3:
                displayQueue(&queue);
                break;
            case 4:
                printf("A fila contém %d elementos.\n", countElements(queue.front));
                break;
            case 5:
                printf("Digite a chave a ser buscada: ");
                scanf("%d", &key);
                int result = searchKey(queue.front, key);
                if (result != -1) {
                    printf("Chave encontrada: %d\n", result);
                } else {
                    printf("Chave não encontrada na fila.\n");
                }
                break;
            case 6:
                exit(0);
            default:
                printf("Opção inválida. Tente novamente.\n");
        }
    }

    return 0;
}
