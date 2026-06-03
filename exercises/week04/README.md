# Tuần 4: Sắp Xếp Cơ Bản — Bài tập

## 🎯 Mục tiêu tuần này
Cài đặt và hiểu Bubble Sort, Selection Sort, Insertion Sort.

---

### Bài 1: Cài đặt 3 thuật toán ⭐⭐
Bubble Sort, Selection Sort, Insertion Sort. Mỗi cái in từng bước thay đổi mảng.
-----Câu trả lời:-------
#include <iostream>
#include <vector>

using namespace std;

void printArray(const vector<int>& arr) {
    for (int x : arr) cout << x << " ";
    cout << endl;
}

// 1. Bubble Sort (Sắp xếp nổi bọt)
void bubbleSort(vector<int> arr) {
    cout << "\n--- BUBBLE SORT O ĐANG CHẠY ---\nMảng gốc: ";
    printArray(arr);
    int n = arr.size();
    
    for (int i = 0; i < n - 1; ++i) {
        bool swapped = false;
        for (int j = 0; j < n - i - 1; ++j) {
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
                swapped = true;
                cout << "Hoán vị (" << arr[j+1] << ", " << arr[j] << "): ";
                printArray(arr);
            }
        }
        if (!swapped) break; // Mảng đã mượt, dừng sớm
    }
}

// 2. Selection Sort (Sắp xếp chọn)
void selectionSort(vector<int> arr) {
    cout << "\n--- SELECTION SORT ĐANG CHẠY ---\nMảng gốc: ";
    printArray(arr);
    int n = arr.size();
    
    for (int i = 0; i < n - 1; ++i) {
        int min_idx = i;
        for (int j = i + 1; j < n; ++j) {
            if (arr[j] < arr[min_idx]) {
                min_idx = j;
            }
        }
        if (min_idx != i) {
            swap(arr[i], arr[min_idx]);
            cout << "Đưa phần tử nhỏ nhất (" << arr[i] << ") về vị trí " << i << ": ";
            printArray(arr);
        }
    }
}

// 3. Insertion Sort (Sắp xếp chèn)
void insertionSort(vector<int> arr) {
    cout << "\n--- INSERTION SORT ĐANG CHẠY ---\nMảng gốc: ";
    printArray(arr);
    int n = arr.size();
    
    for (int i = 1; i < n; ++i) {
        int key = arr[i];
        int j = i - 1;
        
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
        cout << "Chèn phần tử " << key << " vào vị trí thích hợp: ";
        printArray(arr);
    }
}

int main() {
    vector<int> sample = {23, 8, 15, 4, 12};
    
    bubbleSort(sample);
    selectionSort(sample);
    insertionSort(sample);
    
    return 0;
}
### Bài 2: Đếm số phép so sánh và hoán vị ⭐⭐
Với cùng 1 mảng 100 phần tử: đếm số lần so sánh và số lần hoán vị của 3 thuật toán. In bảng so sánh.
-------------Câu trả lời:------------
#include <iostream>
#include <vector>
#include <random>
#include <iomanip>

using namespace std;

// Cấu trúc lưu kết quả đếm
struct Counter {
    long long comparisons = 0;
    long long swaps = 0;
};

Counter runBubbleSort(vector<int> arr) {
    Counter c;
    int n = arr.size();
    for (int i = 0; i < n - 1; ++i) {
        bool swapped = false;
        for (int j = 0; j < n - i - 1; ++j) {
            c.comparisons++;
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
                c.swaps++;
                swapped = true;
            }
        }
        if (!swapped) break;
    }
    return c;
}

Counter runSelectionSort(vector<int> arr) {
    Counter c;
    int n = arr.size();
    for (int i = 0; i < n - 1; ++i) {
        int min_idx = i;
        for (int j = i + 1; j < n; ++j) {
            c.comparisons++;
            if (arr[j] < arr[min_idx]) {
                min_idx = j;
            }
        }
        if (min_idx != i) {
            swap(arr[i], arr[min_idx]);
            c.swaps++;
        }
    }
    return c;
}

Counter runInsertionSort(vector<int> arr) {
    Counter c;
    int n = arr.size();
    for (int i = 1; i < n; ++i) {
        int key = arr[i];
        int j = i - 1;
        
        while (j >= 0) {
            c.comparisons++;
            if (arr[j] > key) {
                arr[j + 1] = arr[j];
                c.swaps++; // Hành động dịch chuyển mảng coi như 1 lần đổi trạng thái
                j--;
            } else {
                break;
            }
        }
        arr[j + 1] = key;
    }
    return c;
}

int main() {
    int N = 100;
    vector<int> original_arr(N);
    
    // Tạo 100 phần tử ngẫu nhiên
    random_device rd; mt19937 g(rd());
    for (int i = 0; i < N; ++i) original_arr[i] = g() % 1000;

    // Chạy và đếm
    Counter b_res = runBubbleSort(original_arr);
    Counter s_res = runSelectionSort(original_arr);
    Counter i_res = runInsertionSort(original_arr);

    // In bảng so sánh công phu
    cout << "====================================================\n";
    cout << "    BẢNG ĐẾM PHÉP TÍNH TRÊN MẢNG NGẪU NHIÊN N = 100  \n";
    cout << "====================================================\n";
    cout << left << setw(20) << "| Thuật toán" << setw(18) << "| Số phép so sánh" << setw(18) << "| Số phép hoán vị" << "|\n";
    cout << "----------------------------------------------------\n";
    cout << left << setw(20) << "| Bubble Sort" << "| " << setw(16) << b_res.comparisons << "| " << setw(16) << b_res.swaps << "|\n";
    cout << left << setw(20) << "| Selection Sort" << "| " << setw(16) << s_res.comparisons << "| " << setw(16) << s_res.swaps << "|\n";
    cout << left << setw(20) << "| Insertion Sort" << "| " << setw(16) << i_res.comparisons << "| " << setw(16) << i_res.swaps << "|\n";
    cout << "====================================================\n";

    return 0;
}
### Bài 3: Best / Worst / Average Case ⭐⭐
Với mảng đã sắp xếp, ngược chiều, và ngẫu nhiên — đo thời gian chạy 3 thuật toán. Kết luận khi nào nên dùng cái nào.
------Câu trả lời:--------
#include <iostream>
#include <vector>
#include <chrono>
#include <algorithm>
#include <random>
#include <iomanip>

using namespace std;

// --- Các hàm sort chạy ngầm để đo thời gian ---
void bubbleSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; ++i) {
        bool swapped = false;
        for (int j = 0; j < n - i - 1; ++j) {
            if (arr[j] > arr[j + 1]) { swap(arr[j], arr[j + 1]); swapped = true; }
        }
        if (!swapped) break;
    }
}

void selectionSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; ++i) {
        int min_idx = i;
        for (int j = i + 1; j < n; ++j) {
            if (arr[j] < arr[min_idx]) min_idx = j;
        }
        if (min_idx != i) swap(arr[i], arr[min_idx]);
    }
}

void insertionSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 1; i < n; ++i) {
        int key = arr[i], j = i - 1;
        while (j >= 0 && arr[j] > key) { arr[j + 1] = arr[j]; j--; }
        arr[j + 1] = key;
    }
}

// Hàm đo thời gian đa năng
double measureTime(void (*sortFunc)(vector<int>&), vector<int> arr) {
    auto start = chrono::high_resolution_clock::now();
    sortFunc(arr);
    auto end = chrono::high_resolution_clock::now();
    chrono::duration<double, milli> duration = end - start;
    return duration.count();
}

int main() {
    int N = 3000; // Tăng N lên để nhìn rõ thời gian chạy tính bằng mili-giây
    cout << "Đang đo thời gian thực tế với N = " << N << " phần tử...\n";

    // 1. Tạo mảng Best Case (Đã sắp xếp)
    vector<int> best_arr(N);
    for (int i = 0; i < N; ++i) best_arr[i] = i;

    // 2. Tạo mảng Worst Case (Sắp xếp ngược chiều)
    vector<int> worst_arr(N);
    for (int i = 0; i < N; ++i) worst_arr[i] = N - i;

    // 3. Tạo mảng Average Case (Ngẫu nhiên)
    vector<int> avg_arr(N);
    random_device rd; mt19937 g(rd());
    for (int i = 0; i < N; ++i) avg_arr[i] = g() % 10000;

    // Đo đạc thời gian
    double b_best = measureTime(bubbleSort, best_arr);
    double b_avg  = measureTime(bubbleSort, avg_arr);
    double b_worst= measureTime(bubbleSort, worst_arr);

    double s_best = measureTime(selectionSort, best_arr);
    double s_avg  = measureTime(selectionSort, avg_arr);
    double s_worst= measureTime(selectionSort, worst_arr);

    double i_best = measureTime(insertionSort, best_arr);
    double i_avg  = measureTime(insertionSort, avg_arr);
    double i_worst= measureTime(insertionSort, worst_arr);

    // In bảng kết quả
    cout << "\n====================================================================\n";
    cout << "             BẢNG ĐO THỜI GIAN CHẠY THỰC TẾ (đơn vị: ms)            \n";
    cout << "====================================================================\n";
    cout << left << setw(18) << "| Thuật toán" << setw(18) << "| Đã xếp (Best)" << setw(18) << "| Ngẫu nhiên (Avg)" << setw(18) << "| Ngược (Worst)" << "|\n";
    cout << "--------------------------------------------------------------------\n";
    cout << left << setw(18) << "| Bubble Sort" << "| " << setw(14) << fixed << setprecision(2) << b_best << "| " << setw(14) << b_avg << "| " << setw(14) << b_worst << "|\n";
    cout << left << setw(18) << "| Selection Sort" << "| " << setw(14) << s_best << "| " << setw(14) << s_avg << "| " << setw(14) << s_worst << "|\n";
    cout << left << setw(18) << "| Insertion Sort" << "| " << setw(14) << i_best << "| " << setw(14) << i_avg << "| " << setw(14) << i_worst << "|\n";
    cout << "====================================================================\n";

    // KẾT LUẬN
    cout << "\n=== KẾT LUẬN KHI NÀO NÊN DÙNG CÁI NÀO? ===\n";
    cout << "1. Insertion Sort: Khuyên dùng khi mảng ĐÃ GẦN NHƯ SẮP XẾP XONG hoặc mảng liên tục nhận thêm dữ liệu mới. Lúc này thời gian chạy đạt xấp xỉ Tuyến tính $O(N)$, siêu nhanh.\n";
    cout << "2. Selection Sort: Khuyên dùng khi dung lượng bộ nhớ ghi hạn chế và CHI PHÍ HOÁN VỊ ĐẮT ĐỎ (Ví dụ: Thẻ nhớ Flash cũ, hệ thống nhúng) vì số lần hoán vị dữ liệu của nó luôn tối ưu ở mức $O(N)$ mặc dù so sánh nhiều.\n";
    cout << "3. Bubble Sort: Không nên dùng trong thực tế vì quá chậm, cấu trúc nhiều vòng lặp lồng nhau sinh ra nhiều lệnh thừa, chủ yếu dùng học tập.\n";

    return 0;
}
### Bài 4: 🔥 Dự Án Mini — Sorting Visualizer (Console) ⭐⭐⭐
> **Cảm hứng:** [Sorting — algorithm-visualizer.org](https://algorithm-visualizer.org/simple-recursive/bubble-sort)
> 
Hiển thị từng bước sắp xếp trực quan bằng thanh ASCII:
```
Bubble Sort — Bước 3/8:
█████████████████ 17
████████████ 12
██████████████████████ 22
████████ 8
███████████████ 15

Đang so sánh: vị trí [1] và [2] ← đánh dấu màu
Số bước còn lại: 5
```
-------Câu trả lời:--------
#include <iostream>
#include <vector>
#include <chrono>
#include <thread>
#include <random>

using namespace std;

// Mã màu ANSI định hình giao diện
const string RESET   = "\033[0m";
const string RED     = "\033[31m";
const string GREEN   = "\033[32m";
const string CYAN    = "\033[36m";
const string YELLOW  = "\033[33m";

// Hàm dựng hình Visualizer lên màn hình
void render(const vector<int>& arr, string algo_name, int current_step, int idx1, int idx2, string note) {
    // Xóa sạch console để vẽ khung hình mới
#ifdef _WIN32
    system("cls");
#else
    cout << "\033[2J\033[1;1H";
#endif

    cout << CYAN << "=== SORTING VISUALIZER (CONSOLE MINI PROJECT) ===" << RESET << "\n";
    cout << YELLOW << algo_name << " — Bước đang chạy: " << current_step << RESET << "\n\n";

    for (int i = 0; i < arr.size(); ++i) {
        // Đánh dấu đỏ cho cặp phần tử đang trực tiếp tương tác
        if (i == idx1 || i == idx2) cout << RED;
        else cout << GREEN;

        // Vẽ độ dài thanh dựa trên giá trị số
        for (int j = 0; j < arr[i]; ++j) cout << "█";
        cout << " " << arr[i] << RESET << "\n";
    }

    cout << "\n" << CYAN << note << RESET << "\n";
    this_thread::sleep_for(chrono::milliseconds(200)); // Delay 200ms tạo hiệu ứng chuyển động
}

void bubbleSortVisual(vector<int> arr) {
    int n = arr.size(), step = 0;
    for (int i = 0; i < n - 1; ++i) {
        for (int j = 0; j < n - i - 1; ++j) {
            render(arr, "Bubble Sort", ++step, j, j + 1, "Đang so sánh vị trí [" + to_string(j) + "] và [" + to_string(j+1) + "]");
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
            }
        }
    }
    render(arr, "Bubble Sort", step, -1, -1, "-> Hoàn thành sắp xếp!");
}

void selectionSortVisual(vector<int> arr) {
    int n = arr.size(), step = 0;
    for (int i = 0; i < n - 1; ++i) {
        int min_idx = i;
        for (int j = i + 1; j < n; ++j) {
            render(arr, "Selection Sort", ++step, j, min_idx, "Tìm Min: So sánh phần tử [" + to_string(j) + "] với Min hiện tại [" + to_string(min_idx) + "]");
            if (arr[j] < arr[min_idx]) min_idx = j;
        }
        if (min_idx != i) swap(arr[i], arr[min_idx]);
    }
    render(arr, "Selection Sort", step, -1, -1, "-> Hoàn thành sắp xếp!");
}

void insertionSortVisual(vector<int> arr) {
    int n = arr.size(), step = 0;
    for (int i = 1; i < n; ++i) {
        int key = arr[i];
        int j = i - 1;
        render(arr, "Insertion Sort", ++step, j, i, "Lấy giá trị " + to_string(key) + " chuẩn bị đem chèn ngược về trước.");
        
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            render(arr, "Insertion Sort", ++step, j, j + 1, "Đẩy phần tử lớn hơn sang phải.");
            j--;
        }
        arr[j + 1] = key;
    }
    render(arr, "Insertion Sort", step, -1, -1, "-> Hoàn thành sắp xếp!");
}

int main() {
#ifdef _WIN32
    system("chcp 65001 > nul"); // Hỗ trợ hiển thị thanh █ không bị lỗi font trên Windows CMD
#endif

    // Tạo mảng gồm 15 số ngẫu nhiên thích hợp với màn hình console
    int size = 15;
    vector<int> arr(size);
    random_device rd; mt19937 g(rd());
    for (int i = 0; i < size; ++i) arr[i] = g() % 25 + 5; // Giới hạn giá trị từ 5 đến 30

    cout << "====================================\n";
    cout << "    CHỌN THUẬT TOÁN ĐỂ XEM ĐỒ HỌA   \n";
    cout << "====================================\n";
    cout << "1. Bubble Sort\n2. Selection Sort\n3. Insertion Sort\nLựa chọn của bạn: ";
    int choice; cin >> choice;

    if (choice == 1) bubbleSortVisual(arr);
    else if (choice == 2) selectionSortVisual(arr);
    else if (choice == 3) insertionSortVisual(arr);
    else cout << "Lựa chọn không hợp lệ!\n";

    return 0;
}






**Yêu cầu:** dùng ANSI color codes để tô màu thanh đang so sánh, delay 200ms giữa các bước, cho phép chọn thuật toán.

---
📁 Tham khảo: `Chuong2_TimKiem_SapXep/Chuong2_TimKiem_SapXep.cpp`
