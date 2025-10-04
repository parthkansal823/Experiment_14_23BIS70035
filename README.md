# Student Management System Using Node.js, Express, and MongoDB (MVC Architecture)
## Objective
Learn how to design and build a Node.js application using MVC architecture to manage student data stored in MongoDB. This experiment teaches:
- Separation of concerns (Model-View-Controller)
- Backend code organization
- CRUD operations using Mongoose

## Technologies Used
- Node.js
- Express.js
- MongoDB
- Mongoose
- Postman (for API testing)

## Folder Structure
```
student-management/
│
├─ models/
│   └─ Student.js
├─ controllers/
│   └─ studentController.js
├─ routes/
│   └─ studentRoutes.js
├─ server.js
└─ package.json
```

## CODE

### models/Student.js
```js
const mongoose = require('mongoose');

const studentSchema = new mongoose.Schema({
    name: { type: String, required: true },
    age: { type: Number, required: true },
    course: { type: String, required: true }
});

module.exports = mongoose.model('Student', studentSchema);

```

## controllers/studentController.js
```js
const Student = require('../models/Student');

exports.getAllStudents = async (req, res) => {
    try {
        const students = await Student.find();
        res.status(200).json(students);
    } catch (err) {
        res.status(500).json({ message: err.message });
    }
};

exports.getStudentById = async (req, res) => {
    try {
        const student = await Student.findById(req.params.id);
        if (!student) return res.status(404).json({ message: 'Student not found' });
        res.status(200).json(student);
    } catch (err) {
        res.status(500).json({ message: err.message });
    }
};

exports.createStudent = async (req, res) => {
    const student = new Student({
        name: req.body.name,
        age: req.body.age,
        course: req.body.course
    });
    try {
        const newStudent = await student.save();
        res.status(201).json(newStudent);
    } catch (err) {
        res.status(400).json({ message: err.message });
    }
};

exports.updateStudent = async (req, res) => {
    try {
        const student = await Student.findByIdAndUpdate(req.params.id, req.body, { new: true });
        if (!student) return res.status(404).json({ message: 'Student not found' });
        res.status(200).json(student);
    } catch (err) {
        res.status(400).json({ message: err.message });
    }
};

exports.deleteStudent = async (req, res) => {
    try {
        const student = await Student.findByIdAndDelete(req.params.id);
        if (!student) return res.status(404).json({ message: 'Student not found' });
        res.status(200).json({ message: 'Student deleted', student });
    } catch (err) {
        res.status(500).json({ message: err.message });
    }
};
```

## routes/studentRoutes.js
