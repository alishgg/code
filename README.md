#include <stdio.h>
#include <string.h>

#define MAX_ITEMS 10

struct Item {
    char name[20];
    char id;
    float cost;
    int quantity;
    int sold;
    float totalProfit;
};

void saveInventory(struct Item inventory[]);
void loadInventory(struct Item inventory[]);
void myMenu(struct Item inventory[]); // pass inventory array to myMenu function
void showInventory(struct Item inventory[]);
void sellItem(struct Item inventory[]);
void showSales(struct Item inventory[]);
void orderItem(struct Item inventory[]);
void addItem(struct Item inventory[]);

int main() {
    struct Item inventory[MAX_ITEMS];
    loadInventory(inventory);
    myMenu(inventory); // pass inventory array to myMenu function

    return 0;
}

void saveInventory(struct Item inventory[]) {
    FILE *invFile = fopen("inventory.txt", "w");
    if (invFile != NULL) {
        for (int i = 0; i < MAX_ITEMS; i++) {
            fprintf(invFile, "%s %d %.2f %d %d %.2f\n", inventory[i].name, inventory[i].id, inventory[i].cost,
                inventory[i].quantity, inventory[i].sold, inventory[i].totalProfit);
        }
        fclose(invFile);
    } else {
        printf("Failed to save inventory!\n");
    }
}

void loadInventory(struct Item inventory[]) {
    FILE *invFile = fopen("inventory.txt", "r");
    if (invFile != NULL) {
        for (int i = 0; i < MAX_ITEMS; i++) {
            fscanf(invFile, "%s %d %f %d %d %f\n", inventory[i].name, &inventory[i].id, &inventory[i].cost,
                &inventory[i].quantity, &inventory[i].sold, &inventory[i].totalProfit);
        }
        fclose(invFile);
    } else {
        printf("Inventory file not found, starting with empty inventory.\n");
    }
}
void myMenu(struct Item inventory[]) { // pass inventory array
    char choice;
    do {
        puts("Inventory Control System");
        puts("To choose a function, enter its letter label:");
        puts("A) Show the name, identification number and number of each item in stock,");
        puts("   including the cost of each item and total value of each item in stock.");
        puts("B) Allow the owner to enter the sale of an item.");
        puts("C) Show the number of units sold, the profit for each item and the total store");
        puts("   profit.");
        puts("D) Allow the owner to order more of an existing item.");
        puts("E) Allow the owner to order new items.");
        puts("F) Quit.");
        printf("Enter your choice: ");
        scanf(" %c", &choice);
      choice = toupper(choice);

        switch (choice)) { // use toupper function to convert choice to uppercase
            
            case 'A':
                showInventory(inventory);
                break;
            case 'B':
                sellItem(inventory);
                saveInventory(inventory);
                break;
            case 'C':
                showSales(inventory);
                break; // add break statement
            case 'D':
                orderItem(inventory);
                saveInventory(inventory);
                break;
            case 'E':
                addItem(inventory);
                saveInventory(inventory);
                break;
            case 'F':
                printf("Exiting program...\n");
                break;
            default:
                printf("Invalid choice! Please
                break;
} 
}
 printf("\n"); }
 void showInventory(struct Item inventory[]) {
int index;
    printf("%-20s %-5s %-10s %-10s %-10s %-10s\n", "Name", "ID", "Cost", "Quantity", "Value", "Profit");
    for (int index = 0; index < MAX_ITEMS; index++) {
        if (inventory[index].quantity > 0) {
            float value = inventory[index].cost * inventory[index].quantity;
            printf("%-20s %-5d $%-9.2f %-10d $%-9.2f $%-9.2f\n", inventory[index].name, inventory[index].id,
                inventory[index].cost, inventory[index].quantity, value, inventory[index].profit);
        }
    }
}
void sellItem(struct Item inventory[]) {
    int itemID, quantity;
    float cost, totalPrice, totalProfit;

    printf("Enter the ID of the item you want to sell: ");
    scanf("%d", &itemID);

    // Find the item in the inventory
    int index;
    for (index = 0; index < MAX_ITEMS; index++) {
        if (inventory[index].id == itemID) {
            break;
        }
    }

    // Check if the item was found
    if (index == MAX_ITEMS) {
        printf("Item not found!\n");
        return;
    }

    // Get the selling quantity and check if it's available in the inventory
    printf("Enter the quantity of the item you want to sell: ");
    scanf("%d", &quantity);

    if (inventory[index].quantity < quantity) {
        printf("Insufficient quantity!\n");
        return;
    }

    // Calculate the total price and profit of the sold items
    cost = inventory[index].cost;
    totalPrice = cost * quantity;
    totalProfit = (cost * (inventory[index].sold + quantity)) - inventory[index].totalProfit;

    // Update the inventory
    inventory[index].quantity -= quantity;
    inventory[index].sold += quantity;
    inventory[index].totalProfit += totalProfit;

    printf("Sold %d units of %s for $%.2f, making a profit of $%.2f\n", quantity, inventory[index].name, totalPrice, totalProfit);
}
