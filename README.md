class Student:

    def __init__(self, name, surname):
        self.name = name
        self.surname = surname
        self.finished_courses = []
        self.courses_in_progress = []
        self.grades = {}

    def __str__(self):
        some_student = f'Имя: {self.name}\nФамилия: {self.surname}\nСредняя оценка за домашнее задание: {round(self.average_st_grade(), 1)}\nКурсы в процессе изучения: {",".join(self.courses_in_progress)}\nЗавершенные курсы: {",".join(self.finished_courses)}'
        return some_student

    def rate_lecturer(self, lecturer, course, grade):
        if isinstance(lecturer,
                      Lecturer) and course in self.courses_in_progress and course in lecturer.l_courses_attached:
            if course in lecturer.l_grades:
                lecturer.l_grades[course] += [grade]
            else:
                lecturer.l_grades[course] = [grade]
        else:
            return 'Ошибка'

    def average_st_grade(self):
        total_st_grade = self.grades.values()
        average_st_grade = sum(sum(total_st_grade, [])) / len(sum(total_st_grade, []))
        return average_st_grade

    def __lt__(self, other):
        if not isinstance(other, Student):
            print("Оценки студента можно сравнить только с оценками другого студента")
            return
        return self.average_st_grade() < other.average_st_grade()


class Mentor:

    def __init__(self, name, surname):
        self.name = name
        self.surname = surname
        self.courses_attached = []


class Lecturer(Mentor):
    def __init__(self, name, surname):
        super().__init__(name, surname)
        self.l_courses_attached = []
        self.l_grades = {}

    def _l_average_score(self):
        total_grade = self.l_grades.values()
        average_grade = sum(sum(total_grade, [])) / len(sum(total_grade, []))
        return average_grade

    def __str__(self):
        some_lecturer = f'Имя: {self.name}\nФамилия: {self.surname}\nСредняя оценка за лекции: {round(self._l_average_score(), 1)}'
        return some_lecturer

    def __lt__(self, other):
        if not isinstance(other, Lecturer):
            print("Оценки лектора можно сравнить только с оценками другого лектора")
            return
        return self._l_average_score() < other._l_average_score()


class Reviewer(Mentor):

    def rate_hw(self, student, course, grade):
        if isinstance(student, Student) and course in self.courses_attached and course in student.courses_in_progress:
            if course in student.grades:
                student.grades[course] += [grade]
            else:
                student.grades[course] = [grade]
        else:
            return 'Ошибка'

    def __str__(self):
        some_reviewer = f'Имя: {self.name}\nФамилия: {self.surname}'
        return some_reviewer


lecturer_1 = Lecturer("Ivan", "Ivanov")
lecturer_1.l_courses_attached += ["Math"]

lecturer_2 = Lecturer("Mariya", "Mariyna")
lecturer_2.l_courses_attached += ["Physics"]

student_1 = Student("Alex", "Alexeev")
student_1.courses_in_progress += ["Math"]
student_1.finished_courses += ["Phyton"]
student_1.rate_lecturer(lecturer_1, "Math", 7)

student_2 = Student("Petr", "Petrov")
student_2.courses_in_progress += ["Physics"]
student_2.finished_courses += ["Java"]
student_2.rate_lecturer(lecturer_2, "Physics", 5)

reviewer_1 = Reviewer("Vasily", "Vasilyev")
reviewer_1.courses_attached += ["Math"]
reviewer_1.rate_hw(student_1, "Math", 9)

reviewer_2 = Reviewer("Ksenia", "Aksenova")
reviewer_2.courses_attached += ["Physics"]
reviewer_2.rate_hw(student_2, "Physics", 5)

print(student_1)
print(student_2)
print(student_1 < student_2)
print(lecturer_1 < student_1)
print(lecturer_1)
print(lecturer_2)
print(lecturer_1 < lecturer_2)
print(student_1 < lecturer_1)
print(reviewer_1)
print(reviewer_2)

students_list = [student_1, student_2]


def all_st_average():
    course_math_av_grade = []
    course_physics_av_grade = []
    for student in students_list:
        for course, grade in student.grades.items():
            if course == "Math":
                course_math_av_grade.append(grade)
            if course == "Physics":
                course_physics_av_grade.append(grade)
    average_math_grade = sum(sum(course_math_av_grade, [])) / len(course_math_av_grade)
    average_physics_grade = sum(sum(course_physics_av_grade, [])) / len(course_physics_av_grade)
    return f'Средняя оценка студентов по курсу "Math": {average_math_grade} \nСредняя оценка студентов по курсу "Physics": {average_physics_grade}'


lecturers_list = [lecturer_1, lecturer_2]


def all_lecturers_average():
    course_math_lecturer_av_grade = []
    course_physics_lecturer_av_grade = []
    for lecturer in lecturers_list:
        for course, grade in lecturer.l_grades.items():
            if course == "Math":
                course_math_lecturer_av_grade.append(grade)
            if course == "Physics":
                course_physics_lecturer_av_grade.append(grade)
    l_average_math_grade = sum(sum(course_math_lecturer_av_grade, [])) / len(course_math_lecturer_av_grade)
    l_average_physics_grade = sum(sum(course_physics_lecturer_av_grade, [])) / len(course_physics_lecturer_av_grade)
    return f'Средняя оценка лекторов по курсу "Math": {l_average_math_grade} \nСредняя оценка лекторов по курсу "Physics": {l_average_physics_grade}'


print(all_st_average())
print(all_lecturers_average())