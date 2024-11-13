#include <iostream>
#include <vector>
#include <cmath>

using namespace std;

// Структура для представления точки в 2D пространстве
struct Point {
    double x;
    double y;
};

// Структура для представления тела
struct Body {
    double mass;
    Point position;
    Point velocity;
    Point acceleration;
};

// Структура для представления пружины
struct Spring {
    double stiffness;
    Point anchor1;
    Point anchor2;
};

// Функция для вычисления силы пружины
Point calculateSpringForce(const Spring& spring, const Body& body) {
    // Расстояние между телом и точкой крепления пружины
    double dx = body.position.x - spring.anchor2.x;
    double dy = body.position.y - spring.anchor2.y;
    double distance = sqrt(dx * dx + dy * dy);

    // Сила пружины
    double forceMagnitude = spring.stiffness * (distance - spring.anchor1.x);
    double forceX = forceMagnitude * dx / distance;
    double forceY = forceMagnitude * dy / distance;

    return {forceX, forceY};
}

// Функция для обновления состояния тела
void updateBody(Body& body, const Point& force, double dt) {
    // Ускорение = сила / масса
    body.acceleration.x = force.x / body.mass;
    body.acceleration.y = force.y / body.mass;

    // Обновление скорости
    body.velocity.x += body.acceleration.x * dt;
    body.velocity.y += body.acceleration.y * dt;

    // Обновление положения
    body.position.x += body.velocity.x * dt;
    body.position.y += body.velocity.y * dt;
}

int main() {
    // Создание тела
    Body body = {1.0, {0.0, 0.0}, {0.0, 0.0}, {0.0, 0.0}}; // Масса = 1.0, начальное положение (0, 0), начальная скорость (0, 0)

    // Создание пружины
    Spring spring = {10.0, {0.0, 0.0}, {5.0, 0.0}}; // Жесткость = 10.0, точка крепления 1 (0, 0), точка крепления 2 (5, 0)

    // Моделирование движения тела
    double dt = 0.01; // Шаг времени
    for (int i = 0; i < 1000; i++) {
        // Вычисление силы пружины
        Point force = calculateSpringForce(spring, body);

        // Обновление состояния тела
        updateBody(body, force, dt);

        // Вывод положения тела
        cout << "Положение тела: (" << body.position.x << ", " << body.position.y << ")" << endl;
    }

    return 0;
}
