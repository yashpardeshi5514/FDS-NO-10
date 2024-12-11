#include <iostream>
#include <stack>
#include <string>
 
using namespace std;
 
// Function to return precedence of operators
int precedence(char op) {
    if (op == '+' || op == '-') return 1;
    if (op == '*' || op == '/') return 2;
    return 0;
}
 
// Function to perform arithmetic operations
int applyOperator(int a, int b, char op) {
    if (op == '+') return a + b;
    if (op == '-') return a - b;
    if (op == '*') return a * b;
    if (op == '/') return a / b;
    return 0;
}
 
// Function to convert infix to postfix
string infixToPostfix(const string& infix) {
    stack<char> operators;
    string postfix;
 
    for (char c : infix) {
        // If character is an operand, add it to the postfix expression
        if (isalnum(c)) {
            postfix += c;
        }
        // If character is '(', push it to the stack
        else if (c == '(') {
            operators.push(c);
        }
        // If character is ')', pop from stack until '(' is found
        else if (c == ')') {
            while (!operators.empty() && operators.top() != '(') {
                postfix += operators.top();
                operators.pop();
            }
            operators.pop(); // Pop '('
        }
        // If character is an operator
        else if (c == '+' || c == '-' || c == '*' || c == '/') {
            while (!operators.empty() && precedence(operators.top()) >= precedence(c)) {
                postfix += operators.top();
                operators.pop();
            }
            operators.push(c); // Push current operator to stack
        }
    }
 
    // Pop remaining operators from the stack
    while (!operators.empty()) {
        postfix += operators.top();
        operators.pop();
    }
 
    return postfix;
}
 
// Function to evaluate postfix expression
int evaluatePostfix(const string& postfix) {
    stack<int> values;
 
    for (char c : postfix) {
        // If character is an operand, push it to the stack
        if (isalnum(c)) {
            values.push(c - '0'); // Convert char to int (assumes single digit)
        }
        // If character is an operator, pop two operands, apply the operator and push the result
        else if (c == '+' || c == '-' || c == '*' || c == '/') {
            int b = values.top(); values.pop();
            int a = values.top(); values.pop();
            int result = applyOperator(a, b, c);
            values.push(result);
        }
    }
 
    return values.top(); // The final result
}
 
int main() {
    string infix, postfix;
 
    // Input infix expression
    cout << "Enter infix expression (with single character operands): ";
    getline(cin, infix);
 
    // Convert infix to postfix
    postfix = infixToPostfix(infix);
    cout << "Postfix expression: " << postfix << endl;
 
    // Evaluate the postfix expression
    int result = evaluatePostfix(postfix);
    cout << "Evaluation result: " << result << endl;
 
    return 0;
}
