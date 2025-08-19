saprse repe
#include <stdio.h>
#define R_MAX 100 
#define C_MAX 100 
typedef struct {
    int r, c, v; 
} se; 

int toSparse(int m[R_MAX][C_MAX], int r, int c, se sm[]) {
    int nzc = 0;
    sm[0].r = r;
    sm[0].c = c;
    for (int i = 0; i < r; i++) {
        for (int j = 0; j < c; j++) {
            if (m[i][j] != 0) {
                nzc++;
                sm[nzc].r = i;
                sm[nzc].c = j;
                sm[nzc].v = m[i][j];
            }
        }
    }
    sm[0].v = nzc; 
    return nzc;
}

void dispSparse(se sm[], int nzc) {
    printf("\n--- Sparse Matrix ---\n");
    printf("R\tC\tV\n");
    printf("---------------------\n");
    printf("%d\t%d\t%d\n", sm[0].r, sm[0].c, sm[0].v);
    printf("---------------------\n");
    for (int i = 1; i <= nzc; i++) {
        printf("%d\t%d\t%d\n", sm[i].r, sm[i].c, sm[i].v);
    }
}

int main() {
    int m[R_MAX][C_MAX];
    se sm[R_MAX * C_MAX + 1];
    int r, c;
    printf("Enter rows and columns: ");
    scanf("%d %d", &r, &c);
    printf("Enter matrix elements:\n");
    for (int i = 0; i < r; i++) {
        for (int j = 0; j < c; j++) {
            scanf("%d", &m[i][j]);
        }
    }
    int nzc = toSparse(m, r, c, sm);
    dispSparse(sm, nzc);
    return 0;
}
sparse transpose 
#include <stdio.h>
#define MAX 101 
typedef struct {
    int r, c, v;
} se;
void fastTranspose(se sm[], se tm[]) {
    int nzc = sm[0].v;
    int cols = sm[0].c;
    int colCount[cols];
    int startPos[cols];
    for (int i = 0; i < cols; i++) {
        colCount[i] = 0;
    }
    for (int i = 1; i <= nzc; i++) {
        colCount[sm[i].c]++;
    }
    startPos[0] = 1;
    for (int i = 1; i < cols; i++) {
        startPos[i] = startPos[i - 1] + colCount[i - 1];
    }
    tm[0].r = sm[0].c;
    tm[0].c = sm[0].r;
    tm[0].v = nzc;
    for (int i = 1; i <= nzc; i++) {
        int pos = startPos[sm[i].c]++;
        tm[pos].r = sm[i].c;
        tm[pos].c = sm[i].r;
        tm[pos].v = sm[i].v;
    }
}
void readSparse(se sm[]) {
    printf("Enter rows, cols, and number of non-zero elements: ");
    scanf("%d %d %d", &sm[0].r, &sm[0].c, &sm[0].v);
    printf("Enter the sparse matrix (row col val):\n");
    for (int i = 1; i <= sm[0].v; i++) {
        scanf("%d %d %d", &sm[i].r, &sm[i].c, &sm[i].v);
    }
}
void dispSparse(se sm[]) {
    printf("R\tC\tV\n");
    printf("---------------------\n");
    for (int i = 0; i <= sm[0].v; i++) {
        printf("%d\t%d\t%d\n", sm[i].r, sm[i].c, sm[i].v);
    }
}
int main() {
    se sm[MAX], tm[MAX];
    readSparse(sm);
    fastTranspose(sm, tm);
    printf("\nOriginal Matrix:\n");
    dispSparse(sm);
    printf("\nTranspose Matrix:\n");
    dispSparse(tm);
    return 0;
}
