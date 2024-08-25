ASDE Algorithm Test

Imagine Abhimanyu in Chakravyuha. There are 11 circles in the Chakravyuha surrounded by different enemies.
Abhimanyu is located in the innermost circle and has to cross all the 11 circles to reach Pandavas army back. 
 
Given:
  Each circle is guarded by different enemy where enemy is equipped with k1, k2……k11 powers
Abhmanyu start from the innermost circle with p power Abhimanyu has a boon to skip fighting enemy
a times 
 Abhmanyu can recharge himself with power b times 
 Battling in each circle will result in loss of the same power from Abhimanyu as the enemy. 
 If Abhmanyu enter the circle with energy less than the respective enemy, he will lose the battle
  k3 and k7 enemies are endured with power to regenerate themselves once with half of their initial
power and can attack Abhimanyu from behind if he is battling in iteratively next circle 
 
 
Write an algorithm to find if Abhimanyu can cross the Chakravyuh and test it with two sets of test cases.

#include <iostream>
#include <vector>
using namespace std;

bool can_cross_chakravyuh(int p, int a, int b, vector<int>& enemies) {
    int skipped_enemies = 0;
    int recharges_left = b;
    int abhimanyu_power = p;
    int n = enemies.size();
    
    // Iterate through each circle
    for (int i = 0; i < n; i++) {
        // Regenerating logic for k3 and k7
        if (i == 4 && enemies[2] > 0) {  // k3 attacks again after battling in circle 4
            abhimanyu_power -= enemies[2] / 2;  // Regenerates with half power
            if (abhimanyu_power <= 0) {
                return false;
            }
        }
        
        if (i == 8 && enemies[6] > 0) {  // k7 attacks again after battling in circle 8
            abhimanyu_power -= enemies[6] / 2;  // Regenerates with half power
            if (abhimanyu_power <= 0) {
                return false;
            }
        }

        // Check if Abhimanyu can fight
        if (abhimanyu_power >= enemies[i]) {
            abhimanyu_power -= enemies[i];
        } else {
            if (skipped_enemies < a) {  // Abhimanyu can skip this fight
                skipped_enemies++;
            } else if (recharges_left > 0) {  // Recharge if recharges are left
                recharges_left--;
                abhimanyu_power = p;  // Restoring power to initial state
                if (abhimanyu_power >= enemies[i]) {
                    abhimanyu_power -= enemies[i];
                } else {
                    return false;
                }
            } else {  // No skips or recharges left, and Abhimanyu doesn't have enough power
                return false;
            }
        }
    }

    // If Abhimanyu crossed all circles
    return true;
}

int main() {
    // Test Case 1: Abhimanyu can skip 2 fights and recharge once
    int p = 15;
    int a = 2;
    int b = 1;
    vector<int> enemies = {10, 20, 25, 15, 30, 10, 35, 5, 15, 20, 10};  // Enemy powers
    
    bool result = can_cross_chakravyuh(p, a, b, enemies);
    if (result) {
        cout << "Abhimanyu can cross the Chakravyuh!" << endl;
    } else {
        cout << "Abhimanyu cannot cross the Chakravyuh!" << endl;
    }

    // Test Case 2: Abhimanyu has no skips or recharges left
    p = 10;
    a = 0;
    b = 0;
    enemies = {5, 8, 12, 15, 18, 25, 30, 5, 7, 10, 12};
    
    result = can_cross_chakravyuh(p, a, b, enemies);
    if (result) {
        cout << "Abhimanyu can cross the Chakravyuh!" << endl;
    } else {
        cout << "Abhimanyu cannot cross the Chakravyuh!" << endl;
    }

    return 0;
}


