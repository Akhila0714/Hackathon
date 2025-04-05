# Hackathon
htTeam Members:
2303A51417 � Akhila
2303A51335 � Karthik
2303A51359 � Abhilash
2303A51334 � Arshad
2303A51356 - Poojitha
[TeamMembers.txt](https://github.com/user-attachments/files/19611943/TeamMembers.txt)
tps://github.com/user-attachments/assets/b205a217-c050-4127-b57f-3d1abf70fd7d
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#define TOTAL_DISEASES 5
struct CropDisease {
    char diseaseName[50];
    char visibleSigns[120];
    char cureSteps[200];
};
struct CropDisease knowledgeBase[] = {
    {"Leaf Blight", "Yellow patches with brown rings", "Spray Mancozeb and remove affected leaves."},
    {"Powdery Mildew", "White powder-like texture on leaves", "Use sulfur-based spray and ensure good airflow."},
    {"Root Rot", "Drooping leaves and blackened roots", "Improve water drainage and use copper-based solution."},
    {"Rust", "Tiny red-orange spots underneath leaves", "Use Chlorothalonil and avoid water on leaves."},
    {"Healthy", "No noticeable symptoms", "No action required at this time."}
};
void xorMask(char *text, char key) {
    for (int i = 0; text[i] != '\0'; i++) {
        text[i] ^= key;
    }
}
int identifyDiseaseIndex(char filename[]) {
    if (strstr(filename, "blight")) return 0;
    else if (strstr(filename, "mildew")) return 1;
    else if (strstr(filename, "root")) return 2;
    else if (strstr(filename, "rust")) return 3;
    else return 4;
}
void saveReport(const char* cropType, const char* photo, struct CropDisease detected) {
    FILE *filePtr = fopen("reports.log", "a");
    if (!filePtr) {
        printf("❌ Could not save the scan details.\n");
        return;
    }
    char lineToWrite[400];
    sprintf(lineToWrite, "Crop: %s | Image: %s | Diagnosis: %s\n", cropType, photo, detected.diseaseName);
    xorMask(lineToWrite, 'S'); 
    fprintf(filePtr, "%s", lineToWrite);
    fclose(filePtr);
}
void showScanHistory() {
    FILE *filePtr = fopen("reports.log", "r");
    if (!filePtr) {
        printf("\n🗃 No scan history found yet.\n");
        return;
    }
    printf("\n🧾 Previous Scan Records:\n");
    char line[400];
    while (fgets(line, sizeof(line), filePtr)) {
        xorMask(line, 'S'); 
        printf("%s", line);
    }
    fclose(filePtr);
}
void beginScan() {
    char cropName[50];
    char imageName[100];
    printf("Enter the type of crop: ");
    scanf("%s", cropName);
    printf("Enter the image file name (e.g., rust_image.jpg): ");
    scanf("%s", imageName);
    int diseaseIndex = identifyDiseaseIndex(imageName);
    struct CropDisease match = knowledgeBase[diseaseIndex];
    printf("\n🔍 Scanning your crop image...\n");
    printf("📌 Diagnosis Result: %s\n", match.diseaseName);
    printf("🧪 Observed Signs: %s\n", match.visibleSigns);
    printf("💡 Suggested Action: %s\n", match.cureSteps);
    saveReport(cropName, imageName, match);
    printf("\n📁 This result has been securely recorded.\n");
}

int main() {
    int userChoice;
    printf("🌿 Welcome to CropGuard - Smart Plant Health Checker (C Edition)\n");
    do {
        printf("\n-------- MAIN MENU --------\n");
        printf("1. Scan a Crop Image\n");
        printf("2. View Previous Scans\n");
        printf("3. Exit Program\n");
        printf("Choose an option: ");
        scanf("%d", &userChoice);
        switch(userChoice) {
            case 1:
                beginScan();
                break;
            case 2:
                showScanHistory();
                break;
            case 3:
                printf("\n🌱 Thank you for using CropGuard. Wishing you a healthy harvest!\n");
                break;
            default:
                printf("⚠ Invalid choice. Please select from the menu.\n");
        }
    } while(userChoice != 3);
    return 0;
}
