#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <limits.h>
#include <ctype.h>

#define MAX_CITIES 20

typedef struct {
    char name[50];
    int bus[MAX_CITIES];
    int flight[MAX_CITIES];
    int train[MAX_CITIES];
    int busCost[MAX_CITIES];
    int flightCost[MAX_CITIES];
    int trainCost[MAX_CITIES];
    int busTime[MAX_CITIES];
    int flightTime[MAX_CITIES];
    int trainTime[MAX_CITIES];
} City;

City cities[MAX_CITIES];
int cityCount = 0;

void toLowerCase(char *str) {
    for (int i = 0; str[i]; i++) {
        str[i] = tolower(str[i]);
    }
}

int findCityIndex(const char *cityName) {
    char lowerName[50];
    strcpy(lowerName, cityName);
    toLowerCase(lowerName);
    for (int i = 0; i < cityCount; i++) {
        char current[50];
        strcpy(current, cities[i].name);
        toLowerCase(current);
        if (strcmp(current, lowerName) == 0)
            return i;
    }
    return -1;
}

void printCities() {
    printf("\nAvailable cities:\n");
    for (int i = 0; i < cityCount; i++) {
        printf("%d. %s\n", i + 1, cities[i].name);
    }
}

void initializeCities() {
    
    strcpy(cities[0].name, "Dhaka");
    cities[0].bus[1] = 50; cities[0].busCost[1] = 100; cities[0].busTime[1] = 60;
    cities[0].flight[1] = 30; cities[0].flightCost[1] = 5000; cities[0].flightTime[1] = 30;
    cities[0].train[1] = 40; cities[0].trainCost[1] = 300; cities[0].trainTime[1] = 120;

    cities[0].bus[2] = 120; cities[0].busCost[2] = 200; cities[0].busTime[2] = 150;
    cities[0].flight[2] = 90; cities[0].flightCost[2] = 7000; cities[0].flightTime[2] = 45;
    cities[0].train[2] = 100; cities[0].trainCost[2] = 500; cities[0].trainTime[2] = 180;

    cities[0].bus[3] = 180; cities[0].busCost[3] = 350; cities[0].busTime[3] = 240;
    cities[0].flight[3] = 150; cities[0].flightCost[3] = 8000; cities[0].flightTime[3] = 60;
    cities[0].train[3] = 120; cities[0].trainCost[3] = 600; cities[0].trainTime[3] = 240;

    
    strcpy(cities[1].name, "Khulna");
    cities[1].bus[0] = 50; cities[1].busCost[0] = 100; cities[1].busTime[0] = 60;
    cities[1].flight[0] = 30; cities[1].flightCost[0] = 5000; cities[1].flightTime[0] = 30;
    cities[1].train[0] = 40; cities[1].trainCost[0] = 300; cities[1].trainTime[0] = 120;

    cities[1].bus[2] = 80; cities[1].busCost[2] = 150; cities[1].busTime[2] = 100;
    cities[1].flight[2] = 70; cities[1].flightCost[2] = 4000; cities[1].flightTime[2] = 60;
    cities[1].train[2] = 90; cities[1].trainCost[2] = 400; cities[1].trainTime[2] = 160;

    cities[1].bus[3] = 170; cities[1].busCost[3] = 300; cities[1].busTime[3] = 220;
    cities[1].flight[3] = 140; cities[1].flightCost[3] = 7500; cities[1].flightTime[3] = 75;
    cities[1].train[3] = 110; cities[1].trainCost[3] = 550; cities[1].trainTime[3] = 210;

    
    strcpy(cities[2].name, "Chittagong");
    cities[2].bus[0] = 120; cities[2].busCost[0] = 200; cities[2].busTime[0] = 150;
    cities[2].flight[0] = 90; cities[2].flightCost[0] = 7000; cities[2].flightTime[0] = 45;
    cities[2].train[0] = 100; cities[2].trainCost[0] = 500; cities[2].trainTime[0] = 180;

    cities[2].bus[1] = 80; cities[2].busCost[1] = 150; cities[2].busTime[1] = 100;
    cities[2].flight[1] = 70; cities[2].flightCost[1] = 4000; cities[2].flightTime[1] = 60;
    cities[2].train[1] = 90; cities[2].trainCost[1] = 400; cities[2].trainTime[1] = 160;

    cities[2].bus[3] = 150; cities[2].busCost[3] = 250; cities[2].busTime[3] = 180;
    cities[2].flight[3] = 130; cities[2].flightCost[3] = 7500; cities[2].flightTime[3] = 70;
    cities[2].train[3] = 110; cities[2].trainCost[3] = 600; cities[2].trainTime[3] = 200;

    
    strcpy(cities[3].name, "Rajshahi");
    cities[3].bus[0] = 180; cities[3].busCost[0] = 350; cities[3].busTime[0] = 240;
    cities[3].flight[0] = 150; cities[3].flightCost[0] = 8000; cities[3].flightTime[0] = 60;
    cities[3].train[0] = 120; cities[3].trainCost[0] = 600; cities[3].trainTime[0] = 240;

    cities[3].bus[1] = 170; cities[3].busCost[1] = 300; cities[3].busTime[1] = 220;
    cities[3].flight[1] = 140; cities[3].flightCost[1] = 7500; cities[3].flightTime[1] = 75;
    cities[3].train[1] = 110; cities[3].trainCost[1] = 550; cities[3].trainTime[1] = 210;

    cities[3].bus[2] = 150; cities[3].busCost[2] = 250; cities[3].busTime[2] = 180;
    cities[3].flight[2] = 130; cities[3].flightCost[2] = 7500; cities[3].flightTime[2] = 70;
    cities[3].train[2] = 110; cities[3].trainCost[2] = 600; cities[3].trainTime[2] = 200;

    cityCount = 4; 
}

void dijkstra(int src, int dest, int mode) {
    int dist[MAX_CITIES], parent[MAX_CITIES], visited[MAX_CITIES] = {0};
    int distances[MAX_CITIES][MAX_CITIES];
    int costs[MAX_CITIES][MAX_CITIES];
    int times[MAX_CITIES][MAX_CITIES];

    if (mode == 1) {
        memcpy(distances, cities[src].bus, sizeof(distances));
        memcpy(costs, cities[src].busCost, sizeof(costs));
        memcpy(times, cities[src].busTime, sizeof(times));
    } else if (mode == 2) {
        memcpy(distances, cities[src].flight, sizeof(distances));
        memcpy(costs, cities[src].flightCost, sizeof(costs));
        memcpy(times, cities[src].flightTime, sizeof(times));
    } else {
        memcpy(distances, cities[src].train, sizeof(distances));
        memcpy(costs, cities[src].trainCost, sizeof(costs));
        memcpy(times, cities[src].trainTime, sizeof(times));
    }

    for (int i = 0; i < cityCount; i++) {
        dist[i] = INT_MAX;
        parent[i] = -1;
    }
    dist[src] = 0;

    for (int count = 0; count < cityCount - 1; count++) {
        int min = INT_MAX, u = -1;
        for (int v = 0; v < cityCount; v++) {
            if (!visited[v] && dist[v] < min) {
                min = dist[v];
                u = v;
            }
        }
        if (u == -1) break;
        visited[u] = 1;
        for (int v = 0; v < cityCount; v++) {
            if (!visited[v] && distances[u][v] && dist[u] + distances[u][v] < dist[v]) {
                dist[v] = dist[u] + distances[u][v];
                parent[v] = u;
            }
        }
    }
    if (dist[dest] == INT_MAX) {
        printf("\nNo path exists between these cities!\n");
        return;
    }
    printf("\nDijkstra Shortest path distance: %d km\n", dist[dest]);
    printf("Cost: %d\n", costs[src][dest]);
    printf("Time: %d minutes\n", times[src][dest]);
}





void bellmanFord(int src, int dest, int mode) {
    int dist[MAX_CITIES], parent[MAX_CITIES];
    int distances[MAX_CITIES][MAX_CITIES];
    int costs[MAX_CITIES][MAX_CITIES];
    int times[MAX_CITIES][MAX_CITIES];

    if (mode == 1) {
        memcpy(distances, cities[src].bus, sizeof(distances));
        memcpy(costs, cities[src].busCost, sizeof(costs));
        memcpy(times, cities[src].busTime, sizeof(times));
    } else if (mode == 2) {
        memcpy(distances, cities[src].flight, sizeof(distances));
        memcpy(costs, cities[src].flightCost, sizeof(costs));
        memcpy(times, cities[src].flightTime, sizeof(times));
    } else {
        memcpy(distances, cities[src].train, sizeof(distances));
        memcpy(costs, cities[src].trainCost, sizeof(costs));
        memcpy(times, cities[src].trainTime, sizeof(times));
    }

    for (int i = 0; i < cityCount; i++) {
        dist[i] = INT_MAX;
        parent[i] = -1;
    }
    dist[src] = 0;

    for (int i = 0; i < cityCount - 1; i++) {
        for (int u = 0; u < cityCount; u++) {
            for (int v = 0; v < cityCount; v++) {
                if (dist[u] != INT_MAX && dist[u] + distances[u][v] < dist[v]) {
                    dist[v] = dist[u] + distances[u][v];
                    parent[v] = u;
                }
            }
        }
    }

    if (dist[dest] == INT_MAX) {
        printf("\nNo path exists between these cities!\n");
        return;
    }

    printf("\nBellman-Ford Shortest path distance: %d km\n", dist[dest]);
    printf("Cost: %d\n", costs[src][dest]);
    printf("Time: %d minutes\n", times[src][dest]);
}



void floydWarshall(int src, int dest, int mode) {
    int dist[MAX_CITIES][MAX_CITIES], next[MAX_CITIES][MAX_CITIES];
    int distances[MAX_CITIES][MAX_CITIES];
    int costs[MAX_CITIES][MAX_CITIES];
    int times[MAX_CITIES][MAX_CITIES];

    if (mode == 1) {
        memcpy(distances, cities[src].bus, sizeof(distances));
        memcpy(costs, cities[src].busCost, sizeof(costs));
        memcpy(times, cities[src].busTime, sizeof(times));
    } else if (mode == 2) {
        memcpy(distances, cities[src].flight, sizeof(distances));
        memcpy(costs, cities[src].flightCost, sizeof(costs));
        memcpy(times, cities[src].flightTime, sizeof(times));
    } else {
        memcpy(distances, cities[src].train, sizeof(distances));
        memcpy(costs, cities[src].trainCost, sizeof(costs));
        memcpy(times, cities[src].trainTime, sizeof(times));
    }

    for (int i = 0; i < cityCount; i++) {
        for (int j = 0; j < cityCount; j++) {
            dist[i][j] = distances[i][j];
        }
    }

    for (int k = 0; k < cityCount; k++) {
        for (int i = 0; i < cityCount; i++) {
            for (int j = 0; j < cityCount; j++) {
                if (dist[i][j] > dist[i][k] + dist[k][j]) {
                    dist[i][j] = dist[i][k] + dist[k][j];
                }
            }
        }
    }

    if (dist[src][dest] == INT_MAX) {
        printf("\nNo path exists between these cities!\n");
        return;
    }

    printf("\nFloyd-Warshall Shortest path distance: %d km\n", dist[src][dest]);
    printf("Cost: %d\n", costs[src][dest]);
    printf("Time: %d minutes\n", times[src][dest]);
}

void planTravel() {
    printCities();
    char src[50], dest[50];

    printf("\nEnter source city: ");
    fgets(src, 50, stdin);
    src[strcspn(src, "\n")] = 0;

    printf("Enter destination city: ");
    fgets(dest, 50, stdin);
    dest[strcspn(dest, "\n")] = 0;

    int srcIndex = findCityIndex(src);
    int destIndex = findCityIndex(dest);

    if (srcIndex == -1 || destIndex == -1) {
        printf("Invalid city names!\n");
        return;
    }

    printf("\nChoose algorithm:\n1. Dijkstra\n2. Bellman-Ford\n3. Floyd-Warshall\n");
    int algo;
    scanf("%d", &algo);
    getchar();

    printf("\nChoose mode of transport:\n1. Bus\n2. Flight\n3. Train\n");
    int mode;
    scanf("%d", &mode);
    getchar();

    switch (algo) {
        case 1:
            dijkstra(srcIndex, destIndex, mode);
            break;
        case 2:
            bellmanFord(srcIndex, destIndex, mode);
            break;
        case 3:
            floydWarshall(srcIndex, destIndex, mode);
            break;
        default:
            printf("Invalid choice of algorithm!\n");
    }
}

int main() {
    initializeCities(); 
    planTravel(); 
    return 0;
}
