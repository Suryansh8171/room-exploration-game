#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define MAX_ROOMS 15
#define MAX_ENEMIES 7

struct Room {
    int id;
    int isEnemyPresent;
    int isItemPresent;
};

struct Enemy {
    int id;
    int health;
};

int main() {
    srand(time(0));

    struct Room rooms[MAX_ROOMS];
    struct Enemy enemies[MAX_ENEMIES];

    int playerHealth = 100;
    int currentRoom = 0;

    for (int i = 0; i < MAX_ROOMS; i++) {
        rooms[i].id = i;
        rooms[i].isEnemyPresent = rand() % 2;
        rooms[i].isItemPresent = rand() % 2;
    }

    for (int i = 0; i < MAX_ENEMIES; i++) {
        enemies[i].id = i;
        enemies[i].health = rand() % 51 + 50;
    }

    printf("Welcome to the Text-Based RPG Game!\n");

    char choice;
    while (playerHealth > 0 && currentRoom < MAX_ROOMS) {
        printf("\nYou are in room %d. What do you want to do?\n", currentRoom + 1);
        printf("Options: (N)orth, (S)outh, (E)ast, (W)est, (I)gnore Enemy, (F)ight Enemy, (P)ick Up Item, (Q)uit: ");
        scanf(" %c", &choice);

        switch (choice) {
            case 'N':
            case 'n':
                currentRoom++;
                break;
            case 'S':
            case 's':
                currentRoom--;
                break;
            case 'E':
            case 'e':
                currentRoom += 2;
                break;
            case 'W':
            case 'w':
                currentRoom -= 2;
                break;
            case 'I':
            case 'i':
                if (rooms[currentRoom].isEnemyPresent) {
                    printf("You chose to ignore the enemy.\n");
                } else {
                    printf("There's no enemy in this room.\n");
                }
                break;
            case 'F':
            case 'f':
                if (rooms[currentRoom].isEnemyPresent) {
                    int enemyIndex = rand() % MAX_ENEMIES;
                    printf("You chose to fight the enemy in this room.\n");
                    printf("Enemy %d with %d health appeared.\n", enemies[enemyIndex].id + 1, enemies[enemyIndex].health);

                    while (playerHealth > 0 && enemies[enemyIndex].health > 0) {
                        playerHealth -= rand() % 21 + 10;
                        enemies[enemyIndex].health -= rand() % 31 + 10;
                    }

                    if (playerHealth > 0) {
                        printf("You defeated the enemy! Your health: %d\n", playerHealth);
                    } else {
                        printf("You were defeated. Game over!\n");
                        return 0;
                    }
                } else {
                    printf("There's no enemy in this room.\n");
                }
                break;
            case 'P':
            case 'p':
                if (rooms[currentRoom].isItemPresent) {
                    printf("You found a health potion! Your health is restored to 100.\n");
                    playerHealth = 100;
                    rooms[currentRoom].isItemPresent = 0; // Remove the item from the room
                } else {
                    printf("There's no item in this room.\n");
                }
                break;
            case 'Q':
            case 'q':
                printf("Thanks for playing! Goodbye!\n");
                return 0;
            default:
                printf("Invalid choice. Please try again.\n");
        }
    }

    printf("Congratulations! You explored all the rooms and survived!\n");
    return 0;
}
