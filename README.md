# APLIKASI PENGGUNAAN STACK

Stack menggunakan prinsip **LIFO (Last In First Out)** di mana elemen terakhir yang dimasukkan ke dalam wadah merupakan elemen pertama yang akan keluar ketika stack
diakses. Perintah dasar dalam stack yaitu `push()`, `top()`, dan `pop()`, di mana masing-masing perintah berarti menambahkan elemen ke atas stack, melihat nilai dari elemen
yang berada paling atas pada stack, dan menghapus data paling atas pada stack. Contoh dasar penggunaan Stack yaitu pada pengaplikasian faktorial. Contoh kodenya:

```cpp
int faktorial (int n) {
  if(n==1) return 1;
  else return n * faktorial(n-1);
}

int main () { cout << faktorial(5); }
```

## 1. Konversi Infix to Postfix
Contoh kasus yang sangat efektif jika diselesaikan menggunakan stack yaitu untuk penerjemah matematika, yaitu
mengonversi ekspresi infix menjadi expresi postfix (reverse polish notation). Reverse Polish Notation merupakan 
ekspresi matematika di mana operator berada di belakang operand. Contoh kode unutk melakukan konversi dari invfix
ke postfix:

```cpp
#include <iostream>
#include <stack>
#include <string>
#include <cctype>
using namespace std;

// Fungsi untuk menentukan prioritas operator
int precedence(char op) {
    if (op == '^')
        return 3;
    else if (op == '*' || op == '/')
        return 2;
    else if (op == '+' || op == '-')
        return 1;
    else
        return 0;
}

// Fungsi untuk cek apakah karakter adalah operator
bool isOperator(char c) {
    return (c == '+' || c == '-' || c == '*' || c == '/' || c == '^');
}

// Fungsi utama konversi infix ke postfix
string infixToPostfix(string infix) {
    stack<char> st;
    string postfix = "";

    for (int i = 0; i < infix.length(); i++) {
        char c = infix[i];

        // Jika operand (huruf/angka)
        if (isalnum(c)) {
            postfix += c;
        }
        // Jika '('
        else if (c == '(') {
            st.push(c);
        }
        // Jika ')'
        else if (c == ')') {
            while (!st.empty() && st.top() != '(') {
                postfix += st.top();
                st.pop();
            }
            if (!st.empty())
                st.pop(); // hapus '('
        }
        // Jika operator
        else if (isOperator(c)) {
            while (!st.empty() && precedence(st.top()) >= precedence(c)) {
                postfix += st.top();
                st.pop();
            }
            st.push(c);
        }
    }

    // Pop semua operator tersisa
    while (!st.empty()) {
        postfix += st.top();
        st.pop();
    }

    return postfix;
}

// Main function
int main() {
    string infix;

    cout << "Masukkan ekspresi infix: ";
    cin >> infix;

    string postfix = infixToPostfix(infix);

    cout << "Postfix: " << postfix << endl;

    return 0;
}
```

Output yang dihasilkan yaitu:<br>
<img width="376" height="45" alt="image" src="https://github.com/user-attachments/assets/fee38786-b06d-4077-bf5f-503571b334fe" />

## 2. Evaluasi Ekspresi Postfix
Selain mengubah ekspresi infix menjadi postfix, stack juga bisa digunakan untuk mengevaluasi hasil dari operasi
yang diberikan dalam bentuk infix. Contoh kodenya:
```cpp
#include <iostream>
#include <stack>
#include <cctype>
using namespace std;

// Fungsi evaluasi postfix
int evaluatePostfix(string exp)
{
    stack<int> st;

    for (char c : exp)
    {

        // Jika operand (angka)
        if (isdigit(c))
        {
            st.push(c - '0'); // konversi char ke int
        }
        // Jika operator
        else
        {
            int val2 = st.top();
            st.pop();
            int val1 = st.top();
            st.pop();

            switch (c)
            {
            case '+':
                st.push(val1 + val2);
                break;
            case '-':
                st.push(val1 - val2);
                break;
            case '*':
                st.push(val1 * val2);
                break;
            case '/':
                st.push(val1 / val2);
                break;
            }
        }
    }

    return st.top();
}

// Main
int main()
{
    string postfix;

    cout << "Masukkan ekspresi postfix: ";
    cin >> postfix;

    cout << "Hasil evaluasi: " << evaluatePostfix(postfix) << endl;

    return 0;
}
```

Output yang dihasilkan:<br>
<img width="367" height="45" alt="image" src="https://github.com/user-attachments/assets/7186c11f-6c64-485b-9605-7818ff11306f" />

## 3. Multidigit Postfix
Stack juga bisa digunakan untuk operasi matematika postfix yang melibatkan lebih dari 1 digit, contoh kodenya:
```cpp
#include <iostream>
#include <stack>
#include <sstream>
using namespace std;

int evaluatePostfix(string exp)
{
    stack<int> st;
    stringstream ss(exp);
    string token;

    while (ss >> token)
    {
        // Jika operator
        if (token == "+" || token == "-" || token == "*" || token == "/")
        {
            int val2 = st.top();
            st.pop();
            int val1 = st.top();
            st.pop();

            if (token == "+")
                st.push(val1 + val2);
            else if (token == "-")
                st.push(val1 - val2);
            else if (token == "*")
                st.push(val1 * val2);
            else if (token == "/")
                st.push(val1 / val2);
        }
        // Jika operand
        else
        {
            st.push(stoi(token));
        }
    }

    return st.top();
}

int main()
{
    string postfix;

    cout << "Masukkan postfix (pisahkan dengan spasi): ";
    getline(cin, postfix);

    cout << "Hasil: " << evaluatePostfix(postfix) << endl;

    return 0;
}
```

Untuk output yang dihasilkan:<br>
<img width="543" height="44" alt="image" src="https://github.com/user-attachments/assets/63be3ace-58b4-4bc1-a456-f9735d9bafde" />
