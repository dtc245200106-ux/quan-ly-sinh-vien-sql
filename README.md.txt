# Bài thực hành: Tạo CSDL Quản Lý Sinh Viên

## 📋 Thông tin
- **Database:** QuanLySinhVien
- **Số bảng:** 4
- **Công cụ:** MySQL Workbench

## 📊 Cấu trúc các bảng

### 1. Bảng Class
| Trường | Kiểu | Ràng buộc |
|--------|------|-----------|
| ClassID | INT | PK, AUTO_INCREMENT |
| ClassName | VARCHAR(60) | NOT NULL |
| StartDate | DATETIME | NOT NULL |
| Status | BIT | |

### 2. Bảng Student
| Trường | Kiểu | Ràng buộc |
|--------|------|-----------|
| StudentId | INT | PK, AUTO_INCREMENT |
| StudentName | VARCHAR(30) | NOT NULL |
| Address | VARCHAR(50) | |
| Phone | VARCHAR(20) | |
| Status | BIT | |
| ClassId | INT | FK → Class |

### 3. Bảng Subject
| Trường | Kiểu | Ràng buộc |
|--------|------|-----------|
| SubId | INT | PK, AUTO_INCREMENT |
| SubName | VARCHAR(30) | NOT NULL |
| Credit | TINYINT | DEFAULT 1, CHECK >= 1 |
| Status | BIT | DEFAULT 1 |

### 4. Bảng Mark
| Trường | Kiểu | Ràng buộc |
|--------|------|-----------|
| MarkId | INT | PK, AUTO_INCREMENT |
| SubId | INT | FK → Subject |
| StudentId | INT | FK → Student |
| Mark | FLOAT | DEFAULT 0, CHECK 0-100 |
| ExamTimes | TINYINT | DEFAULT 1 |

## 📝 Câu lệnh SQL

```sql
CREATE DATABASE QuanLySinhVien;
USE QuanLySinhVien;

CREATE TABLE Class(
    ClassID INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    ClassName VARCHAR(60) NOT NULL,
    StartDate DATETIME NOT NULL,
    Status BIT
);

CREATE TABLE Student(
    StudentId INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    StudentName VARCHAR(30) NOT NULL,
    Address VARCHAR(50),
    Phone VARCHAR(20),
    Status BIT,
    ClassId INT NOT NULL,
    FOREIGN KEY (ClassId) REFERENCES Class(ClassID)
);

CREATE TABLE Subject(
    SubId INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    SubName VARCHAR(30) NOT NULL,
    Credit TINYINT NOT NULL DEFAULT 1 CHECK (Credit >= 1),
    Status BIT DEFAULT 1
);

CREATE TABLE Mark(
    MarkId INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    SubId INT NOT NULL,
    StudentId INT NOT NULL,
    Mark FLOAT DEFAULT 0 CHECK (Mark BETWEEN 0 AND 100),
    ExamTimes TINYINT DEFAULT 1,
    UNIQUE (SubId, StudentId),
    FOREIGN KEY (SubId) REFERENCES Subject(SubId),
    FOREIGN KEY (StudentId) REFERENCES Student(StudentId)
);