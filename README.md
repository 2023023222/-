
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

class Student {
private:
    string id;
    string name;
    char gender;
    float chinese;
    float math;
    float english;

public:
    Student(string id, string name, char gender, float chinese, float math, float english)
        : id(id), name(name), gender(gender), chinese(chinese), math(math), english(english) {}

    float getAverage() {
        return (chinese + math + english) / 3.0;
    }

    string getId() {
        return id;
    }

    string getName() {
        return name;
    }

    void display() {
        cout << "学号: " << id << "\t姓名: " << name << "\t性别: " << gender << "\t语文: " << chinese << "\t数学: " << math << "\t英语: " << english << endl;
    }
};

class StudentManager {
private:
    vector<Student> students;

public:
    void addStudent(Student student) {
        students.push_back(student);
    }

    void displayAllStudents() {
        for (auto& student : students) {
            student.display();
        }
    }

    void deleteStudentById(string id) {
        students.erase(remove_if(students.begin(), students.end(), [&](const Student& s) { return s.getId() == id; }), students.end());
    }

    void deleteStudentByName(string name) {
        students.erase(remove_if(students.begin(), students.end(), [&](const Student& s) { return s.getName() == name; }), students.end());
    }

    Student* findStudentById(string id) {
        for (auto& student : students) {
            if (student.getId() == id) {
                return &student;
            }
        }
        return nullptr;
    }

    Student* findStudentByName(string name) {
        for (auto& student : students) {
            if (student.getName() == name) {
                return &student;
            }
        }
        return nullptr;
    }

    void updateStudentById(string id, Student newStudent) {
        for (auto& student : students) {
            if (student.getId() == id) {
                student = newStudent;
                break;
            }
        }
    }

    void updateStudentByName(string name, Student newStudent) {
        for (auto& student : students) {
            if (student.getName() == name) {
                student = newStudent;
                break;
            }
        }
    }

    void sortByAverage() {
        sort(students.begin(), students.end(), [](const Student& a, const Student& b) { return a.getAverage() > b.getAverage(); });
    }

    void displayFailingStudents() {
        for (auto& student : students) {
            if (student.getAverage() < 60.0) {
                student.display();
            }
        }
    }
};

int main() {
    StudentManager manager;

    // 添加学生记录
    manager.addStudent(Student("1001", "张三", 'M', 75, 80, 85));
    manager.addStudent(Student("1002", "李四", 'F', 65, 70, 55));
    manager.addStudent(Student("1003", "王五", 'M', 85, 90, 95));

    // 显示所有学生记录
    cout << "所有学生记录：" << endl;
    manager.displayAllStudents();

    // 删除学生记录
    manager.deleteStudentById("1002");

    // 查询学生记录
    cout << "按学号查询学生记录：" << endl;
    Student* student = manager.findStudentById("1001");
    if (student != nullptr) {
        student->display();
    }

    // 修改学生记录
    manager.updateStudentByName("张三", Student("1001", "张三", 'M', 80, 85, 90));

    // 统计排序
    manager.sortByAverage();
    cout << "按平均成绩从高到低排名：" << endl;
    manager.displayAllStudents();

    // 列出不及格学生清单
    cout << "不及格学生清单：" << endl;
    manager.displayFailingStudents();

    return 0;
}
